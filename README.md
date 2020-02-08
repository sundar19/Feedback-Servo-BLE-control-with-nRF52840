# Feedback-Servo-BLE-control-with-nRF52840
Control a Feedback Servo with nRF52 over BLE

Use Nordic ToolBox UART to recieve values(available in Play Store)

The Feedback Servo used was http://www.feetechrc.com/product/continuous-rotation-servo/two-working-mode-digital-programmable-metal-gears-servo-fr5311m/

## General instructions
The following instructions are written for Segger Embedded Studio download here https://www.segger.com/downloads/embedded-studio/
1. Go to the following website http://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/ and download v15.2.0 
2. After which go to D:\nRF5_SDK_15.2.0_9412b96\examples\ble_peripheral\ble_app_uart\pca10056\s140\ses\ and open .emproject , then automatically segger studio will open along with main.c file
3. Change the main.c file to the attached file.

## Merging BLE with ADC
Go to sdk_config.h and search 

1. nrf_drv_timer   TIMER_ENABLED 1 (enable the timer by replacing 0 to 1),
                   TIMER3_ENABLED 1 (We have enabled timer instance 3 in our main.c)


2. nrfx_timer      NRFX_TIMER_ENABLED 1(enable the timer by replacing 0 to 1),
                   NRFX_TIMER3_ENABLED 1 (We have enabled timer instance 3 in our main.c)


3. nrfx_ppi        NRFX_PPI_ENABLED 1


4. nrf_drv_ppi     PPI_ENABLED 1


5. nrf_drv_saadc   SAADC_ENABLED 1


6. nrfx_saadc      NRFX_SAADC_ENABLED 1


Then include the following libraries in your main.c , #include""(already in program)

nrf_drv_saadc.h , nrf_drv_timer.h , nrf_drv_ppi.h , nrfx_ppi.h , nrfx_timer.h , nrfx_saadc.h


In your nRF_Drivers (A tab in your left)
Search and add the following files 

1.nrf_drv_ppi.c

2.nrfx_ppi.c

3.nrfx_timer.c

4.nrfx_saadc.c

## Merging PWM with BLE

1. In the segger studio towards the left a tab with "nRF_Libraries" will be present. Right click and add components --> libraries --> PWM --> app_pwm.c (add this file to nRF_Libraries)


2.Add "app_pwm.h" file in as a header in the main file. Note: We should specify the full path where the file is present( Already in Program)

(OR) 

press the "Project" tab --> "options" --> change the dropdown "release" tab to "common" --> go to "preprocessor" --> go to "user include directories" and click [...] and add app_pwm.h file there (nRF5_SDK_15.2.0_9412b96\components\libraries\pwm\app_pwm.h)


3. Compare the nRF_Drivers and add all the necessary driver files (nrf_nvic.c , nrf_soc.c) and add the necessary headers in the main file


4. Search for app_pwm in sdk_config.h and enable it
 

#ifndef APP_PWM_ENABLED

#define APP_PWM_ENABLED 0 --> 1

#endif


5." APP_PWM_INSTANCE(PWM1,1);"  Since we have enable timer instance 1 , enable the nrf_drv_timer and nrfx_timer for timer instance 1( Step 1 & 2 in Merging ADC with BLE)


6. enable nrf_drv_gpiote 


#ifndef GPIOTE_ENABLED

#define GPIOTE_ENABLED 1

#endif


and enable nrfx_gpiote


#ifndef NRFX_GPIOTE_ENABLED

#define NRFX_GPIOTE_ENABLED 1

#endif


7. while merging BLE,ADC,PWM


Enable SAADC first and PWM next ( in the "void main()" loop)
