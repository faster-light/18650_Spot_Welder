# 18650 SPOT WELDER
Проект сварочного аппарата для аккумуляторов 18650.

## Железо
Точечная сварка изготовлена из трансформатора СВЧ-печи, с переметанной вторичной обмоткой (2.5 витка) [медным проводом](https://www.chipdip.ru/product0/9000299899 "медным проводом") площадью поперечного сечения 35 кв. мм. 

### Силовая честь

Для управления первичной обмоткой использован симистр [BTA-16](https://www.chipdip.ru/product/bta16-600b "BTA-16"). Для управления симистором использована оптопара с симисторным выходом [MOC3063](https://www.chipdip.ru/product/moc3063m "MOC3063"). Для обнаружения перехода через ноль применена оптопара с транзисторным выходом [4N25](https://www.chipdip.ru/product/4n25-vishay "4N25") (подключена к сети через диодный мост [DF06M](https://www.chipdip.ru/product/df06m-vishay "DF06M")).

![Shematic High Volt](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/Shematic_High_Volt.PNG "Shematic High Volt")

### Система управления
В данном проекте использован микроконтроллер [STM32F103C8T6](https://www.chipdip.ru/product/stm32f103c8t6 "STM32F103C8T6") ([Manual](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/STM32F103C8T6_Manual.pdf "Manual"). [Reference manual](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/STM32F103C8T6_Reference_manual.pdf "Reference manual")). Система управления - энкодер EC11, индикации - OLED 0.91"с контроллером[ SSD1306](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/SSD1306_Manual.pdf " SSD1306") и разрешением 32x128 пикселей.

![Shematic Low Volt](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/Shematic_Low_Volt.PNG "Shematic Low Volt")

## Начало работы с STM32 в Keil и  STM32CubeMX

[STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html "STM32CubeMX")  - мощный генератор проектов от STMicroelectronics, позволяющий настроить порты ввода/вывода, таймеры, прерывания и т.п. и создать проект для нескольких сред разработки.
[Keil uVision](https://www.keil.com/download/product/ "Keil uVision") MDK-Arm - среда разработки для контроллеров с ядрами прхитектуры ARM (напр. Cortex-M).
### Туториал по насттройке контроллера в STM32CubeMX
![](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_1.PNG)
> Открываем STM32CubeMX, создаем новый проект.

![](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_2.PNG)
> Находим и выбираем используемый микроконтроллер

![](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_3.PNG)
> Переходим во вкладку Project Manager. Задаем Name и Project Location к проекту. в Toolchain/IDE необходимо выбрать MDK-ARM

![](https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_4.PNG)
> Переходим во вкладку Pinout & Configuration, раздел System Core, пункт GPIO.
