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
#include <ESP32Servo.h>
#include <WiFi.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

// GPIO the servo is attached to
static const int servoPin = 13;

// Replace with your network credentials
const char* ssid     = "Your wifi access point name";
const char* password = "Your password";

// Set web server port number to 80
WiFiServer server(80);

// Variable to store the HTTP request
String header;

// Decode HTTP GET value
String valueString = String(5);
int pos1 = 0;
int pos2 = 0;

void setup() {
  Serial.begin(115200);

  myservo.attach(servoPin);  // attaches the servo on the servoPin to the servo object

  // Connect to Wi-Fi network with SSID and password
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  // Print local IP address and start web server
  Serial.println("");
  Serial.println("WiFi connected.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
  server.begin();
}

void loop() {
  WiFiClient client = server.available();   // Listen for incoming clients

  if (client) {                             // If a new client connects,
    Serial.println("New Client.");          // print a message out in the serial port
    String currentLine = "";                // make a String to hold incoming data from the client
    while (client.connected()) {            // loop while the client's connected
      if (client.available()) {             // if there's bytes to read from the client,
        char c = client.read();             // read a byte, then
        Serial.write(c);                    // print it out the serial monitor
        header += c;
        if (c == '\n') {                    // if the byte is a newline character
          // if the current line is blank, you got two newline characters in a row.
          // that's the end of the client HTTP request, so send a response:
          if (currentLine.length() == 0) {
            // HTTP headers always start with a response code (e.g. HTTP/1.1 200 OK)
            // and a content-type so the client knows what's coming, then a blank line:
            client.println("HTTP/1.1 200 OK");
            client.println("Content-type:text/html");
            client.println("Connection: close");
            client.println();

            // Display the HTML web page
            client.println("<!DOCTYPE html><html>");
            client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
            client.println("<link rel=\"icon\" href=\"data:,\">");
            // CSS to style the on/off buttons
            // Feel free to change the background-color and font-size attributes to fit your preferences
            client.println("<style>body { text-align: center; font-family: \"Trebuchet MS\", Arial; margin-left:auto; margin-right:auto;}");
            client.println(".slider { width: 300px; }</style>");
            client.println("<script src=\"https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js\"></script>");

            // Web Page
            client.println("</head><body><h1>RAZLIVATOR 3000</h1>");
            client.println("<p>Position: <span id=\"servoPos\"></span></p>");
            client.println("<input type=\"range\" min=\"0\" max=\"180\" class=\"slider\" id=\"servoSlider\" onchange=\"servo(this.value)\" value=\"" + valueString + "\"/>");

            client.println("<script>var slider = document.getElementById(\"servoSlider\");");
            client.println("var servoP = document.getElementById(\"servoPos\"); servoP.innerHTML = slider.value;");
            client.println("slider.oninput = function() { slider.value = this.value; servoP.innerHTML = this.value; }");
            client.println("$.ajaxSetup({timeout:1000}); function servo(pos) { ");
            client.println("$.get(\"/?value=\" + pos + \"&\"); {Connection: close};}</script>");

            client.println("</body></html>");

            //GET /?value=180& HTTP/1.1
            if (header.indexOf("GET /?value=") >= 0) {
              pos1 = header.indexOf('=');
              pos2 = header.indexOf('&');
              valueString = header.substring(pos1 + 1, pos2);

              //Rotate the servo
              myservo.write(valueString.toInt());
              Serial.println(valueString);
            }
            // The HTTP response ends with another blank line
            client.println();
            // Break out of the while loop
            break;
          } else { // if you got a newline, then clear currentLine
            currentLine = "";
          }
        } else if (c != '\r') {  // if you got anything else but a carriage return character,
          currentLine += c;      // add it to the end of the currentLine
        }
      }
    }
    // Clear the header variable
    header = "";
    // Close the connection
    client.stop();
    Serial.println("Client disconnected.");
    Serial.println("");
  }
}
```

**Тестирование инструмента**

https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/6ae779d8-4f51-4b52-98ec-5e61a9a96450

**Компьютерная симуляция в RoboDK:**

https://github.com/HellraidKraid/TM5_700_Tool/assets/144485874/00bac153-b6c9-4542-8475-b3e31350d999
