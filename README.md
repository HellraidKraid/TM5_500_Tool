# TM5_700_Tool
Разработка и создание инструмента для робота-манипулятора TM5 500

**План работы:**
1. Разработка концепта и функционала инструмента;
2. Обработка имеющихся данных и поиск оптимальных решений;
3. Разработка инструмента при помощи 3D принтера и электроники;
4. Программирование электронных частей инструмента;
5. Тестирование инструмента;
6. Разработка компьютерной симуляции.

## Исследование
Данный проект заключается в разработке и создании инструмента для робота-манипулятора TM5-700. Его характеристики представленны на рисунке 1:

![TM5_500_characteristic](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/6f396b29-1776-473e-8806-e85c1bc7486a)

**Рисунок 1 - Характеристики робота-манипулятора TM5-700**

Было решено изготовить и разработать разливщик напитков с клапаном управляемым при помощи Arduino. Концепт инструмента представлен на рисунке 2:

![IMG_20240604_143517_099](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/95977101-43bb-4a89-8921-4fc6ef23658a)

**Рисунок 2 - Концепт инструмента робота-манипулятора**

## Разработка инструмента

*Оборудование - 3D-принтер Prusa i3 MK3*

*Материал - PLA*

*Программное обеспечение - PrusaSlicer, Компас 3D*

После нескольких неудачных вариантов было принято решение использовать клапон работающий по принципу зажима силиконовой трубки при помощи сервопривода. 
https://www.thingiverse.com/thing:4876444
На рисунке 3 представлена внутренняя конструкция клапона:

![valve](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/a0defe09-a3f0-4717-8711-5ca6bdfdb1e2)

**Рисунок 3 - Внутренняя конструкция клапона**

Основа инструмента, корпус и насадка для трубки были разработаны в программе "Компас 3D" и напечатаны на 3D принтере.
Составные части инструмента представлены на рисунках 4, 5 и 6:

![Основа](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/92e037b7-e3b0-41f2-bc6d-71e6adbd9a19)

**Рисунок 4 - Основа инструмента**

![Насадка для трубки](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/541c6aae-1168-4fa3-9b5a-cf2c18c8f4a3)

**Рисунок 5 - Насадка для трубки**

![Корпус трубки](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/8622f9e6-fa6b-4bbd-bff1-9c52ea4d77e1)

**Рисунок 6 - Корпус трубки**

Итоговый вариант инструмента в собранном виде представлен на рисунке 7:

![IMG_20240613_181222_896](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/a0173dd5-02ac-40b3-8744-1703d9ae2e58)

**Рисунок 7 - Готовый вариант инструмента робота-манипулятора**

## Программирование электронных частей

*Оборудование - DOIT ESP32, сервопривод SG90, аккумулятор*

*Программное обеспечение - Arduino IDE*

При помощи точки доступа телефона ESP32 дистанционно подключается к телефону. Угол поворота сервопривода SG90 управляется при помощи ползунка на сайте.

Сайт ESP32 представлен на рисунке 8:

![Сайт](https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/26cc47ac-7c50-4294-bdcf-81d140810097)


**Рисунок 7 - Сайт ESP32 для управления поворотом сервопривода**

**Код ESP32:**

```C++
#include <IRremote.hpp>
```

**Тестирование инструмента**

https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/6ae779d8-4f51-4b52-98ec-5e61a9a96450

**Компьютерная симуляция в RoboDK:**

https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/00bac153-b6c9-4542-8475-b3e31350d999
