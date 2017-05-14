title: LCD通过按键显示自定义图标
date: 2017-5-8 12:58:36
categories: 我学编程
tags: 
 - Raspberry
toc: false
comments: true
---

在[HD44780 LCD User-Defined Graphics][1]这个网站上我们可以查看自定义个图标的编码值。如下图所示。
![自定义图标][2]

作业1
编写一个小程序，当按向上的button时，LCD第一行显示向上的箭头，并在第二行显示UP，以此类推显示4个方向键。

```Python
#!/usr/bin/python3
import time
import Adafruit_CharLCD as LCD

# Initialize the LCD using the pins
lcd = LCD.Adafruit_CharLCDPlate()

# create some custom characters
lcd.create_char(1, [0, 0, 4, 14, 31, 0, 0, 0])
lcd.create_char(2, [0, 0, 31, 14, 4, 0, 0, 0])
lcd.create_char(3, [0, 2, 6, 14, 6, 2, 0, 0])
lcd.create_char(4, [0, 8, 12, 14, 12, 8, 0, 0])

# Show button state.
lcd.clear()
lcd.message('Press buttons...')

# Make list of button value, text, and backlight color.
buttons = ( (LCD.LEFT,   '\x03\nLeft'  , (1,0,0)),
            (LCD.UP,     '\x01\nUp'    , (0,0,1)),
            (LCD.DOWN,   '\x02\nDown'  , (0,1,0)),
            (LCD.RIGHT,  '\x04\nRight' , (1,0,1)) )

print('Press Ctrl-C to quit.')
while True:
	# Loop through each button and check if it is pressed.
	for button in buttons:
		if lcd.is_pressed(button[0]):
			# Button is pressed, change the message and backlight.
			lcd.clear()
			lcd.message(button[1])
			lcd.set_color(button[2][0], button[2][1], button[2][2])

```
说明：
- 通过`lcd_create`函数我们可以自定义LCD要显示的字符
- 自定义字符通过`\x01`来访问，其中`\x01`代表第一个自定义字符，可以设置8个字符，编号从0到7
- `\n`表示换行


[1]: https://www.quinapalus.com/hd44780udg.html
[2]: /images/lcd_icon.png
