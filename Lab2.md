# Lab 2

## Task 1: IMU Setup

First, I connected the IMU to the Artemis board, as shown below, using QWIIC connectors. 

<p align="center">
<img width="400" src="photos/Lab2/Artemis_IMU_connected.png">
</p>
<br>

In order to interact with the IMU, I installed the “SparkFun 9DOF IMU Breakout_ICM 20948_Arduino Library.” After the library was installed, I ran the Example1_Basics file from the library to get the output shown in the video below. In the script, I kept AD0_VAL defined as 1, because the AD0 pin on the IMU is not connected to the ADR (address) based off the Sparkfun schematic provided. So, since the loop is not closed, the value should remain the default, 1. 

<p align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/yLPr0NABWfo?si=_Kv3WqxDgtZNJuPX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</p>
<br>

EXPLAIN WHAT IS HAPPENING AS THE IMU IS FLIPPED/ROTATED -> should i add pics?

NEED TO MAKE IT BLINK

## Task 2: Accelerometer

### Accelerometer Accuracy 

I was able to calculate the pitch and roll from the accelerometer output. I used the equations shown below, which were given in class. 

ADD IN SCREENSHOT

The following video shows the accelerometer output as the IMU is positioned at pitch angles of 0, 90, and then -90 degrees. I used the edges of the desk to get the most accurate measurements possible. The accelerometer pitch values were quite accurate, not differing more than about 4 degrees from the expected values. 

ADD IN PITCH VALUES VIDEO

In addition, I tested the accuracy of the accelerometer roll angles. The video below shows the accelerometer output at roll angles of 0, -90, and then 90 degrees. Similar to before, the values were quite accurate, differing no more than about 5 degrees from the expected angles. The rapid change in values occuring on screen happens when I reposition the IMU, which was unfortunately done off screen for part of the video.

ADD IN ROLL VALUES VIDEO 

Since I found the accelerometer output values to be quite accurate, as explained above, I did not think it was necessary to perform a two-point calibration.

### Noise in the Data

Even when the IMU is stationary, flat against my desk, there is noise present in the system. This noise can be seen in the graph below of roll and pitch graphed over time. Although both values are centered around 0, the measurements vary by about 1.5 degrees. Even a discrepancy this small could lead to further error propogation down the line. 

STATIONARY NOISE PIC

### Fourier Transform

In order to reduce noise in the accelerometer output, a low pass filter can be applied to the data. First, however, the frequency cutoff value needs to be determined. In order to determine the ideal frequency cutoff, I performed a Fourier Transform of both the roll and pitch data. I used the equations shown below to calculate the Fourier Transform.

  FOURIER TRANSFORM EQUATIONS

To test a system with a greater prevalence of noise, I gently hit the table while the accelerometer was collecting data. This created more vibrations in the system. The resulting pitch and roll output can be seen below.

PITCH ROLL VIBRATION PIC

After inducing this increased vibration, I generated the following plots, which display the Fourier Transforms of pitch and roll. On top, the full Fourier Transform plots are displayed. Below displays only a zoomed in portion of the plots. These sections were used to determine the frequency cutoff value.  

PITCH ROLL FOURIER TRANSFORM PLOTS (FUll and zoomed)

Based off the Fourier Transform plots, I chose my frequency cutoff value to be 8 Hz. Most of the significant data appears to occur at or below this frequency in both the plots of pitch and roll. 

### Low Pass Filter

Using my chosen frequency cutoff of 8 Hz, I implemented a lowpass filter on my accelerometer data. Using the equations given in lecture, I calculated an alpha value of 0.52. The equations I used are displayed below. Note that this alpha value was a bit high, due in part to my low sample rate of 45.5 samples/second. My sample rate was low due to delays and a print statement I implemented in order to better control the type of roll and pitch data I was collecting during testing. I will touch upon the impacts of increasing this sample rate more in tasks 3 and 4. 

ADD IN ALPHA Calc PICTURE

I first implemented the lowpass filter in Jupyter lab, seen below. I later added it to my Arduino code in order to calculate the complementary filter, discussed in Task 3. 

ADD LOWPASS FILTER CODE








## Task 3: Gyroscope

## Task 4: Sample Data

## Task 5: Record a Stunt


