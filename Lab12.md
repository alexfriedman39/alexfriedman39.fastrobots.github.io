# Lab 12: Path Planning and Execution

## Open Loop Control 

To navigate through the map, I started with open loop control. I made 8 commands, MOVE_1 through MOVE_8, which each corresponded to a movement from one waypoint to the next. For example, MOVE_1 caused the robot to move from point 1 to 2, which required the robot to turn left 45 degrees and then move forward approximately 2.8 feet. 

*** ADD IN DRAWN DIAGRAM HERE ?? INCLUDES NAMES OF MOVE_1 ETC?? ***

The code for MOVE_1 through MOVE_8 can be seen below. 

<div style="height:650px; overflow:auto;">
<pre><code class="language-cpp">
case MOVE_1:
delay(1000);
//turn left 45 deg 
analogWrite(1, 0);
analogWrite(5, 0);
delay(2);
analogWrite(2, right_PWM*1.3); //right wheel forward
analogWrite(0, left_PWM*1.3); //left wheel backward
delay(turn_time);
analogWrite(2, 0);
analogWrite(0, 0);
delay(50);
//move forward
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;

case MOVE_2:
//turn right back to 0 deg, move to (1,-1)
delay(1000);
//turn left 45 deg 
analogWrite(2, 0);
analogWrite(0, 0);
delay(2);
analogWrite(5, right_PWM*1.3); //right wheel backward
analogWrite(1, left_PWM*1.3); //left wheel forward
delay(turn_time);
analogWrite(5, 0);
analogWrite(0, 0);
delay(100);
//move forward
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;

case MOVE_3:
//turn right to -45 deg and move to (2, -3)
delay(1000);
//turn left 45 deg 
analogWrite(2, 0);
analogWrite(0, 0);
delay(2);
analogWrite(5, right_PWM*1.3); //right wheel backward
analogWrite(1, left_PWM*1.3); //left wheel forward
delay(turn_time);
analogWrite(5, 0);
analogWrite(0, 0);
delay(100);
//move forward
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;

case MOVE_4:
//turn back to 0 deg and move to (5, -3)
delay(1000);
//turn left 45 deg 
analogWrite(1, 0);
analogWrite(5, 0);
delay(2);
analogWrite(2, right_PWM*1.3); //right wheel forward
analogWrite(0, left_PWM*1.3); //left wheel backward
delay(turn_time);
analogWrite(2, 0);
analogWrite(0, 0);
delay(50);
//move forward
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;

case MOVE_5:
//turn left to 90 degrees and move to (5, -2)
delay(1000);
//turn left 45 deg 
analogWrite(1, 0);
analogWrite(5, 0);
delay(2);
analogWrite(2, right_PWM*1.3); //right wheel forward
analogWrite(0, left_PWM*1.3); //left wheel backward
delay(turn_time);
analogWrite(2, 0);
analogWrite(0, 0);
delay(50);
//move forward
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;

case MOVE_6: 
//move forward to (5, 3)
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;

case MOVE_7:
//turn left to 180 deg and move to (0, 3)
delay(1000);
//turn left 90 deg 
analogWrite(1, 0);
analogWrite(5, 0);
delay(2);
analogWrite(2, right_PWM*1.3); //right wheel forward
analogWrite(0, left_PWM*1.3); //left wheel backward
delay(turn_time);
analogWrite(2, 0);
analogWrite(0, 0);
delay(50);
//move forward
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;

case MOVE_8:
//turn left to 270 deg and move to (0,0)
//turn left to 180 deg and move to (0, 3)
delay(1000);
//turn left 90 deg 
analogWrite(1, 0);
analogWrite(5, 0);
delay(2);
analogWrite(2, right_PWM*1.3); //right wheel forward
analogWrite(0, left_PWM*1.3); //left wheel backward
delay(turn_time);
analogWrite(2, 0);
analogWrite(0, 0);
delay(50);
//move forward
analogWrite(1, left_PWM); //left wheel forward
analogWrite(2, right_PWM); //right wheel forward
delay(forward_time);
analogWrite(1, 0);
analogWrite(2, 0);
delay(15);
break;
</code></pre>
</div>

As seen in the command code above, the delays were not hardcoded. Instead, I created the CHANGE_DELAY command, shown below, in order to easily change the delays via bluetooth while testing my robot in the map. 

<div style="height:500px; overflow:auto;">
<pre><code class="language-cpp">
case CHANGE_DELAY:
int change_for_delay, change_turn_delay;
  success = robot_cmd.get_next_value(change_for_delay);
  //Serial.println(right_PWM);
  if (!success)
    return;
  success = robot_cmd.get_next_value(change_turn_delay);
  if (!success)
    return;
  forward_time = change_for_delay;
  turn_time = change_turn_delay;
  //Serial.println(right_PWM);
  // Serial.println(right_PWM);
break;
 }
}
</code></pre>
</div>

After testing each leg of the robot's journey through the map, I found numerical values for the delay times that worked the best. The resulting Jupyter lab code is shown below. 

For 1 -> 2
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#First move from (-4, -3) to (-2, -1)
ble.send_command(CMD.CHANGE_DELAY, "500|230")
time.sleep(2)
ble.send_command(CMD.MOVE_1, "")
</code></pre>
</div>

For 2 -> 3
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#Second move from (-2, -1) to (1,-1)
ble.send_command(CMD.CHANGE_DELAY, "500|190")
time.sleep(2)
ble.send_command(CMD.MOVE_2, "")
</code></pre>
</div>

For 3 -> 4
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#Third move from (1, -1) to (2,-3)
ble.send_command(CMD.CHANGE_DELAY, "500|190")
time.sleep(2)
ble.send_command(CMD.MOVE_3, "")
</code></pre>
</div>

For 4 -> 5
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#Fourth move from (2, -3) to (5,-3)
ble.send_command(CMD.CHANGE_DELAY, "600|230")
time.sleep(2)
ble.send_command(CMD.MOVE_4, "")
</code></pre>
</div>

For 5 -> 6
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#Fifth move from (5, -3) to (5,-2)
ble.send_command(CMD.CHANGE_DELAY, "350|350")
time.sleep(2)
ble.send_command(CMD.MOVE_5, "")
</code></pre>
</div>

For 6 -> 7
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#Sixth move from (5, -2) to (5,3)
ble.send_command(CMD.CHANGE_DELAY, "1000|360")
time.sleep(2)
ble.send_command(CMD.MOVE_6, "")
</code></pre>
</div>

For 7 -> 8
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#Seventh move from (5, 3) to (0, 3)
ble.send_command(CMD.CHANGE_DELAY, "800|300")
time.sleep(2)
ble.send_command(CMD.MOVE_7, "")
</code></pre>
</div>

For 8 -> 9
<div style="height:200px; overflow:auto;">
<pre><code class="language-python">
#Eighth move from (0, 3) to (0, 0)
ble.send_command(CMD.CHANGE_DELAY, "500|300")
time.sleep(2)
ble.send_command(CMD.MOVE_7, "")
</code></pre>
</div>



