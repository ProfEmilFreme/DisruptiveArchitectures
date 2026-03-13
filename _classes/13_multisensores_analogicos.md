---
layout: "class"
course: "disruptive"
section:
    name: "Sensores, Atuadores e IO"
    order: 4
class:
    title: "Sensores analógicos"
    order: 0
---

Os sensores analógicos convertem grandezas físicas (como a umidade do solo, a
inclinação ou a força) em uma voltagem variável. O Arduino lê essa voltagem e a
transforma em um número que conseguimos ler via entrada analógica.

O Arduino possui um conversor de 10 bits. Isso significa que ele divide os 5V da alimentação em 1024 partes iguais.

- Valor 0: O sensor está enviando 0V.
- Valor 1023: O sensor está enviando 5V.
- Valores intermediários: Representam a intensidade da grandeza medida (ex:
  mais força, mais luz ou mais calor).


Muitos dos sensosre analogicos funcionam dessa forma, abaixo temos o exemplo de
alguns deles: 

- Sensor de Gas;
- Sensor de Temperatura;
- Sensor de Rotação(Tilt);
- Sensor de Torção (Flex);
- Sensor de Força; 
- Sensor de Úmidade do solo;





![](assets/sensoresdiversos.png)


```cpp
/*******************************************************************************
 * @file            SensoresAnalogicos.ino
 * @brief           Demonstração básica de leitura de diversos sensores
 *                  analógicos
 * @author          Prof. Emil Freme
 * @license         Creative Commons Atribuição 4.0 (CC BY)
 *******************************************************************************/
const int pinUmid = A5;
const int pinTilt = A4;
const int pinFlex = A3;
const int pinForca = A2;
const int pinTemp = A1;
const int pinGas = A0;


void setup()
{
  pinMode(pinGas, INPUT);
  pinMode(pinTemp, INPUT);
  pinMode(pinForca, INPUT);
  pinMode(pinFlex, INPUT);
  pinMode(pinTilt, INPUT);
  pinMode(pinUmid, INPUT);
  Serial.begin(9600);
}

void loop()
{
  Serial.print("Gas: ");
  Serial.println(analogRead(pinGas));
  
  Serial.print("Temp: ");
  Serial.println(analogRead(pinTemp));
  
  Serial.print("Forca: ");
  Serial.println(analogRead(pinForca));
  
  Serial.print("Flex: ");
  Serial.println(analogRead(pinFlex));
  
  Serial.print("Tilt: ");
  Serial.println(analogRead(pinTilt));
  
  Serial.print("Solo: ");
  Serial.println(analogRead(pinUmid));

  delay(50);
}
```
[Projeto no Tinkecad](https://www.tinkercad.com/things/1Kg4O9sOt7e-sensore-analogicos?sharecode=Ud0U6Rl68C13ORO7_eEUoVq6MTJg5C6uwJwxPMdL8i8)

Um ponto fundamental no uso de sensores analógicos é que, raramente, usaremos o valor bruto de 0 a 1023 em nosso projeto final. Para que a informação faça sentido para o usuário, precisamos realizar o Mapeamento.

Mapear significa converter a escala de leitura do Arduino para uma escala que conhecemos na vida real. Por exemplo:

- Transformar 0-1023 em 0-100% de umidade para um sensor de solo.
- Transformar 0-1023 em 0-180 graus para controlar a posição de um servo motor.
- Transformar 0-1023 em 0-5 Volts para monitorar uma bateria.

No Arduino, fazemos isso facilmente com a função map():

`valorMapeado = map(valorLido, 0, 1023, minimo, maximo);`
