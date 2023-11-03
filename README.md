# STM32-HAL-HD44780

## Overview

This is a simple library for [STM32 32-bit Arm Cortex MCUs](https://www.st.com/en/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus.html) to work with the [HD44780 16x2 Lcd module](https://www.sparkfun.com/datasheets/LCD/HD44780.pdf).

To develop this library it was used [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) and [HAL APIs](https://www.st.com/resource/en/user_manual/um1725-description-of-stm32f4-hal-and-lowlayer-drivers-stmicroelectronics.pdf) to interact with peripherals and low-level resources.    

In this repository you will find the LCD library, ready to attach to your new project and a project example, tested with the STM32F4DISCOVERY board.

In this library the HD44780 controller is used in 4-bits mode and driven by the PCF8574 I2C I/O expander.

## Examples

**SMT32F4DISCOVERY**

Brief description of the example attached

```c
#include "lcd.h"
#include <stdio.h"
int main(){
  LCD_HandleTypeDef lcd;

  lcd_init_I2C(&lcd, &hi2c1);

  lcd_print_string(&lcd, "LCD library");
  HAL_Delay(2000);

  lcd_set_cursor(&lcd, DISPLAY_ROW_2, DISPLAY_COLUMN_1);

  lcd_send_command(&lcd, LCD_CMD_Don_nC_BL);

  lcd_print_string(&lcd, "LCD library");
  HAL_Delay(2000);

  lcd_display_clear(&lcd);
  lcd_display_return_home(&lcd);
  lcd_send_command(&lcd, LCD_CMD_Don_nC_nBL);

  uint16_t date_num[3] = {3, 11, 2023};
  char date_char[20];
  sprintf(date_char, "0%d/%d/%d", date_num[0], date_num[1], date_num[2]);

  lcd_print_string(&lcd, date_char);
  HAL_Delay(2000);

  lcd_set_cursor(&lcd, DISPLAY_ROW_2, DISPLAY_COLUMN_4);
  lcd_print_string(&lcd, "HD44780");

  while(1){

  }
}
```

In this example it's shown how to use differents APIs to print several messages as well as a set of _integers_.

## Documentation

A brief documentation of the structures and functions provided by the library are shown below. For detailed instructions, refer to the doxygen compliant documentation within the `lcd.c` file.

### Macro definitions

List of important macro definitions with a brief description is shown below.

| Name | Description |
|     :---:    |     ---      |
| `SLAVE_ADDRESS_PCF`   | PCF8574 I2C I/O expander device address.     | 

### Structure

List of structures provided with the library with a brief description is shown below.

| Name | Description |
|     :---:    |     ---      |
| `LCD_HandleTypeDef`   | Struct to handle the I2C device addres and the I2C Handler.     | 

### Functions

List of functions provided with the library with a brief description is shown below.

| Name | Description |
|     :---:    |     ---      |
| `lcd_init_I2C`   |  Initialize the `LCD_HandleTypeDef` struct with the information to work with the I2C communication bus.    | 
| `lcd_init`   |  Initialize HD44780 controller following the steps provided by the manufacturer.  | 
| `lcd_send_command`   |   Set the display in a particular state by sending a command. | 
| `lcd_print_char`   |  In char of print a character on the LCD  | 
| `lcd_display_clear`   |  Clear the display   | 
| `lcd_display_return_home`   |  Move the cursor to the position (1, 1)  | 
| `lcd_print_string`   |  Split the incoming string into individual characters and send them one by one  | 
| `lcd_set_cursor`   |   Move the cursor to the desire position  | 

