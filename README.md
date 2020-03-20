
# 18650 SPOT WELDER

> Проект сварочного аппарата для аккумуляторов 18650.

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
### Туториал по настройке контроллера в STM32CubeMX
> **1**. Открываем **STM32CubeMX**, создаем новый проект нажатием на кнопку **ACCESS TO MCU SELECTOR**.
> **2**. Находим и выбираем используемый микроконтроллер **STM32F103C8**.
<p align="center">
  <img src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_2.PNG">
</p>

------------

> **3**. Переходим во вкладку **Project Manager**. Задаем **Name** и **Project Location** к проекту. в **Toolchain/IDE** необходимо выбрать **MDK-ARM**.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_3.JPG">
</p>

------------

> **4**. Переходим во вкладку **Pinout & Configuration**, раздел **System Core**, пункт **GPIO**. В окне **Pinout view** выбираем вывод **PA1** и настраиваем режим внешнего прерывания **GPIO_EXTI1**.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_4.JPG">
</p>

------------

> **5**. После выбора вывода и прерывания в окне **Configuration** можно настроить вывод **PA1**. В окне **PA1 Configuration** необходимо настроить вызов прерывания по спаду - **Falling** и подтянуть к питанию - **Pull-up**.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_5.JPG">
</p>

------------

> **6**. Во вкладке **NVIC** для этого вывода нужно включить прерывание для** EXTI line1 interrupt**.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_6.JPG">
</p>

------------

> **7**. Аналогичным образом добавим остальные выводы и настроим их, согластно таблице.
<p align="center">
  <img width="75%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_7.JPG">
</p>

------------

> **8**. И включим прерывания.
<p align="center">
  <img width="75%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_8.JPG">
</p>

------------

> **9**. Переходим в пункт **RCC**. Так как на плане разведены оба кварцевых резонатора, указываем это в окне **Mode**.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_9.JPG">
</p>

------------

> **10**. В пункте **SYS** необходимо указать режим **Debug** - **Serial Wire** для работы с **St-Link v2**.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_10.JPG">
</p>

------------

> **11**. В разделе **Connectivity** включим интерфейс **I2C1** и настроим его на соседние ножки **PB8** и **PB9** указав это в выпадающем списке.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_11.JPG">
</p>

------------

> **12**. Осталось настроить таймеры. Перейдем в раздел **Timers** и выберем таймер **TIM1**. Источник тактирования - **Clock Source** указать **Internal Clock**. Так же необходимо включить **One Pulse Mode**. 
> В Настройках таймера укжем **Prescaler** - **799** (что будет соответствовать шагу в **0.1 мс**), **Counter Period** - **89** (**9 мс** для каждой полуволны синусоиды продолжительностью **10 мс**), **Pulse - 45**. Эту велечину мы будем менять в программе.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_13.JPG">
</p>

------------

>**13**. Для таймера **TIM1** во вкладке **NVIC Settings** необходимо включить прерывание по сравнению - **TIM1 capture compare interrupt**.
<p align="center">
  <img width="80%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_14.JPG">
</p>

------------

> **14**. Аналогичным образом настроим второй таймер для мониторинга системы в режиме ожидания.
<p align="center">
  <img width="85%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_15.JPG">
</p>
<p align="center">
  <img width="80%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_16.JPG">
</p>

------------

> **15**. Проект готов. Можно нажать кнопку **GENERATE CODE** и запустить **Keil**.
<p align="center">
  <img width="90%" src="https://github.com/faster-light/18650_Spot_Welder/blob/master/tutorial/tutorial_17.JPG">
</p>
