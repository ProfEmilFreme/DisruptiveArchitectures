---
layout: "class"
course: "disruptive"
section:
    name: "Sensores, Atuadores e IO"
    order: 4
class:
    title: "Output: LCD 16x2 I2C"
    order: 1
---

![](assets/render_lcdonly.png){:height="150px"}

O display LCD com o módulo "backpack" I2C é uma solução eficiente para exibir
informações, pois reduz a necessidade de 12 pinos do Arduino para apenas 2 pinos
de comunicação (SDA e SCL). Ele é classificado como um dispositivo de saída
(Output) na interface homem-máquina.

Na sequência temos o esquema e o desenho de como fazer a conexão dele:



![](assets/schem_lcdi2c.png)


![](assets/render_lcdi2c.png)

O funcionamento baseia-se no protocolo I2C, onde o Arduino atua como mestre
enviando pacotes de dados para o endereço do display (geralmente 0x27). O módulo
acoplado ao LCD converte esses dados seriais em sinais paralelos para controlar
os caracteres e o backlight.


```cpp
/*******************************************************************************
 * @file            EscreveLCDI2C.ino
 * @brief           Demonstração básica de uso da biblioteca Adafruit_LiquidCrystal
 * @author          Prof. Emil Freme
 * @adapted_from    https://github.com/adafruit/Adafruit_LiquidCrystal/blob
 * /master/examples/HelloWorld_i2c/HelloWorld_i2c.ino
 * @license         Creative Commons Atribuição 4.0 (CC BY)
 *******************************************************************************/
#include "Adafruit_LiquidCrystal.h"

// Inicializa o LCD no endereço I2C 32 (comum no Tinkercad)
Adafruit_LiquidCrystal lcd(32); 

void setup() {
  Serial.begin(9600); // Inicia comunicação serial para debug
  
  // Configura o LCD com 16 colunas e 2 linhas
  if (!lcd.begin(16, 2)) {
    Serial.println("Não foi possível inicializar o LCD");
    while(1); // Trava o código se houver erro na conexão
  }

  lcd.print("Mensagem NoLCD"); // Escreve na primeira linha
}

void loop() {
  lcd.setCursor(0, 1);       // Move o cursor para a segunda linha
  lcd.print("Segunda linha"); // Escreve na segunda linha

  // Efeito de piscar a luz de fundo (backlight)
  lcd.setBacklight(HIGH);    // Liga a luz
  delay(500);                // Espera 0.5s
  lcd.setBacklight(LOW);     // Desliga a luz
  delay(500);                // Espera 0.5s
}
```
