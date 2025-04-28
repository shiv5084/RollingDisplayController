# RollingDisplayController
Project Description:
Design a RollingDisplayController system using the Nucleo C031C6 board that integrates a push button, keypad, and an LCD display to perform the following tasks:
Push Button Functionality:
● When the push button is pressed, it toggles the display on and off.
● If the push button is pressed continuously three times within a 3-second interval, the display should be cleared.
Keypad Functionality
● The keypad will be used to input numbers (or characters).
● The characters entered on the keypad will be displayed on the LCD.
● Only the first 20 characters entered should be visible on the display at any given time. Once 20 characters are reached, any new input should replace the oldest character on the screen, maintaining a rolling display of the latest 20 characters.
Functional Requirements:
Button Press Detection:
● A debounce mechanism should be implemented to ensure accurate detection of button presses.
● The system should be able to detect three button presses within a 3-second period to trigger the display clearing function.
Keypad Input Display:
● Each key press on the keypad should display the corresponding number or character on the screen.
● The system should maintain a circular buffer of up to 20 characters for the display.
Display Control:
● The LCD display should toggle on/off using the push button.
● The keypad input should be visible in real-time, and the first 20 characters should always be visible.
● When the push button is pressed three times within 3 seconds, the screen should be cleared

Online Emulator wokwi link:
https://wokwi.com/projects/429006322794364929
