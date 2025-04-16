# Lab 9: Mapping

## Control

In order to complete Lab 9, I decided to use orientation control. As in previous labs, I activated my mapping code through a bluetooth command that set the flag, RECORD_MAP, to true. I implemented orientation control using the IMU's gyroscope. As seen in the code included below, the robot turned in increments of 10 degrees, effectively recording a total of 36 distance measurements. Yaw angle was measured continuously in order to accurately track the robot's movement. The desired PWM value was calculated based off yaw, using the same orientation PID function from Lab 6. If the resulting PWM value was positive, the robot was directed to turn right. Otherwise, the robot would turn left. To prevent the robot from unnecessary oscillation, I also directed it to stop moving if the yaw value obtained using the gyroscope was within 2 degrees of the target angle. Once the robot had stabilized at the correct angle, a distance measurement was recorded and the desired angle was updated. 

<div style="height:200px; overflow:auto;">
<pre><code class="language-cpp">
if(RECORD_MAP)
       {
        icm_20948_DMP_data_t data;
        myICM.readDMPdataFromFIFO(&data);

        int dist;
        float yaw_tot;
        float cur_time;
        float dt_map;
        int pwm;
        float yaw_0;
        float yaw_prev;
        float yaw_g;
        //Initial distance measurement (robot doesn't need to start moving)
        if(FIRST_ANGLE)
        {
          //Serial.println("First distance recorded");
            if (distanceSensor_1.checkForDataReady()) {
            meas_dist = distanceSensor_1.getDistance();
            yaw_tot = 0;

            time_stamps[map_meas] = millis();
            dist1[map_meas]=meas_dist;
            yaw[map_meas] = yaw_tot;


            map_meas++;
            distanceSensor_1.clearInterrupt();
            distanceSensor_1.stopRanging();
            distanceSensor_1.startRanging();
            FIRST_ANGLE = false;
            des_angle = 10;
          }
        }
        else{
          if(map_meas < 36)
          {
            float pid_t0 = millis();
            while(millis()-pid_t0 < 1000)
            {
             cur_time = millis();
             dt_map = (cur_time-prev_time_ori)/1000;
             prev_time_ori = cur_time;

             if(myICM.dataReady())
             {
              myICM.getAGMT();
              double yaw_g = myICM.gyrZ();
              yaw_tot = yaw_tot + yaw_g*dt_map;

              pwm = calc_speed_ori(yaw_tot, des_angle, dt_map);            

              if(abs(yaw_tot-des_angle) < 2 || yaw_tot > des_angle)
              {
                analogWrite(1, 0); 
                analogWrite(2, 0); 
                analogWrite(0, 0);
                analogWrite(5,0);
                delay(50);
              }
              else if(pwm>0)
              {
               if(pwm > 130)
               {
                pwm = 130;
               }
               turn_right(pwm);
              }
              else
              {
                pwm=abs(pwm);
                if(pwm > 130)
                {
                  pwm = 130;
                }
                turn_left(pwm);
              }
                }
             }
            
            stopMoving();
            delay(5);

           if (distanceSensor_1.checkForDataReady()) {
             meas_dist = distanceSensor_1.getDistance();
             distanceSensor_1.clearInterrupt();
             distanceSensor_1.stopRanging();
             distanceSensor_1.startRanging();

             time_stamps[map_meas] = millis();
             dist1[map_meas]=meas_dist;
             yaw[map_meas] = yaw_tot;

             Serial.print("yaw meas");
             Serial.println(yaw_tot);
             
             des_angle = des_angle+10;
             Serial.println(des_angle);
             Serial.println(".");
             
             map_meas++;
             prev_err_ori = 0;
             int_error_ori = 0;
           }
        }
         else
         {
          RECORD_MAP = false;
        }
        }
       }
</code></pre>
</div>




