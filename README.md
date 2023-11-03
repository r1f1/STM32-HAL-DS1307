# STM32-HAL-HD44780

## Overview

This is a simple library for [STM32 32-bit Arm Cortex MCUs](https://www.st.com/en/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus.html) to work with the [DS1307 RTC module](https://www.analog.com/media/en/technical-documentation/data-sheets/ds1307.pdf).

To develop this library it was used [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) and [HAL APIs](https://www.st.com/resource/en/user_manual/um1725-description-of-stm32f4-hal-and-lowlayer-drivers-stmicroelectronics.pdf) to interact with peripherals and low-level resources.    

In this repository you will find the RTC library, ready to attach to your new project and a project example, tested with the STM32F4DISCOVERY board.

## Examples

**SMT32F4DISCOVERY**

Brief description of the example attached

```c
#include "rtc.h"
#include <stdio.h"
int main(){
  RTC_HandleTypeDef rtc;
  RTC_time_t current_time;
  RTC_date_t current_date;

  if (rtc_init(&rtc, &hi2c2)){
	printf("RTC init has failed\n");
	while(1);
  }

  current_date.day = SUNDAY;
  current_date.date = 31;
  current_date.month = 12;
  current_date.year = 15;

  current_time.hours = 11;
  current_time.minutes = 59;
  current_time.seconds = 41;
  current_time.time_format = TIME_FORMAT_12HRS_PM;

  rtc_set_current_time(&rtc, &current_time);
  rtc_set_current_date(&rtc, &current_date);

  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
	  rtc_get_current_date(&rtc, &current_date);
	  rtc_get_current_time(&rtc, &current_time);

	  char *am_pm;
	  if (current_time.time_format != TIME_FORMAT_24HRS){
	  	am_pm = (current_time.time_format) ? "PM" : "AM";
	  	printf("Current time = %s %s\n", time_to_string(&current_time), am_pm); // 04:25:41 PM
	  }else {
		  printf("Current_time = %s\n", time_to_string(&current_time)); // 04:25:41
	  }

	  printf("Current date: %s <%s>\n", date_to_string(&current_date), DAY_OF_WEEK(current_date.day));
	  HAL_Delay(1000);
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */

}
```

In this example it's shown how to set an initial time and date and then print the results formatted every second.

**Print characters using ITM feature**

To see the results of this example it's neccesary to implement the `printf` like feature using ARM Cortex M3/M4/ ITM functionality. To achieve this, it's necessary to follow the following procedure: https://medium.com/@g.bharathraj19/getting-started-with-instrumentation-trace-macrocell-in-stm32cubeide-4af179eb0034

## Documentation

A brief documentation of the structures and functions provided by the library are shown below. For detailed instructions, refer to the doxygen compliant documentation within the `rtc.c` file.

### Macro definitions

List of important macro definitions with a brief description is shown below.

| Name | Description |
|     :---:    |     ---      |
| `RTC_I2C_ADDRESS`   | Device address.     | 

### Structure

List of structures provided with the library with a brief description is shown below.

| Name | Description |
|     :---:    |     ---      |
| `RTC_date_t`   | Struct to handle the parameters related to the date     | 
| `RTC_time_t`   | Struct to handle the parameters related to the time    | 
| `RTC_HandleTypeDef`   | Struct to handle the I2C device address and the I2C Handler     | 

### Functions

List of functions provided with the library with a brief description is shown below.

| Name | Description |
|     :---:    |     ---      |
| `rtc_init`   |  Initialize the RTC module. | 
| `rtc_set_current_time`   |   Set the initial time provided by the user | 
| `rtc_get_current_time`   |   Get the current time |
| `rtc_set_current_date`   |  Set the initial date provided by the user   | 
| `rtc_get_current_date`   |  Get the current date  |

