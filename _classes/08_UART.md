---
layout: "class"
course: "disruptive"
section:
    name: "Protocolos M2M"
    order: 3
class:
    title: "UART"
    order: 1
---
# UART **Universal asynchronous receiver/transmitter**

O UART é um protocolo de comuncação serial mais simples e fundamentais. Ele
trabalha com uma ponto-a-ponto, ou seja, apenas dois dispositivos conectados
diretamente.

### Conexão Física

Para implementar esse comunicação utilizamos os pinos `TX`, transmissor, e `RX`,
receptor de cada disposítivo. Essa conexão deve ocorrer de forma cruzada, ou
seja, dados os dipositicos D1 e D2 a conxão seria TX<sub>D1</sub>
$$\Leftrightarrow$$ RX<sub>D2</sub> e TX<sub>D2</sub>
$$\Leftrightarrow$$ RX<sub>D1</sub>. Lembre-se também de conectar os pinos `GND`
de ambas os dispositivos para garantir uma transmissão de dados correta. 

Veja o esquema: 

![](attachments/uart_generico.png)

### Programando

No ambiente do Arduino,  utilizamos a Classe `Serial` para fazer essa
comunicação. Sim o mesmo que utilizamos para enviar as mensagens de debug via
USB. 

Placas com um numero maior de GPIO como o Arduino Mega possuem multiplos
conjuntos de pinos/portas serial, `TX1`/`RX1`, `TX2`/`RX2`, enfim. E para
acessalos utilizamos as variantes `Serial1`, `Serial2` que gerênciam a
comunicação das portas seriais correspondentes. 

> No Arduino Uno, temos apenas um conjunto de pinos TX/RX são compartilhados
> pela porta USB. O que pode difícultar o uso desse protocolo nessa placa
> específicamente, pois a conexão USB "sequestra" a comunicação serial desses
> pinos. 

Ao configurar a comunicação serial, é necessário que ambos os dispositívos
estejam se comunicando no masma frequência de transimissão (**baud rate**).

```cpp
/**
 * DISPOSITIVO 1
 **/
 
 void setup(){
    Serial.begin(9600); //Inicializo a conexão serial com baud rate 9600
 }

 void loop(){
    Serial.print("Info"); //Enviando a informação via comunicação serial. 
    Serial.print('\n');   //Indico o fim da mensagem
    delay(1000);
 }
```

```cpp
/**
 * DISPOSITIVO 2
 **/

char    mensagem[128] = {0}; //Buffer para receber a mensagem
int     t_mensagem = 0;      //Tamanho da mensagem recebida
bool    isReading = true;

void setup(){
    Serial.begin(9600); //Lembre-se de configurar a velocidade igual o
                        //controller
}

void loop(){
    if (Serial.available() > 0 && //Verifico se existe dados a serem recebidos
        t_mensagem < 128 ){    //Verifico se não estourei meu buffer&

        char c = Serial.read(); //Leitura byte-por-byte

        mensagem[t_mensagem] = c; 
        t_mensagem++;

        if(c == '\n'){
            mensagem[t_mensagem] = '\0'; 
            isReading = false;
        }

        if(!isReading){
            //TODO: Seu código utilizando mensagem
            t_mensagem = 0;
            isReading = true;
        }
    }
}
```


## Software Serial

Existe uma possibilidade para quando queremos utilizar uma comunicação serial
Ponto-a-ponto que utilizando a Classe `SoftwareSerial`, que nos permitira
utilizar qualquer dupla de pinos GPIO para simular a conexão que seria feita
pelo hardware dedicado.

Mas ela tem seus pontos negativos, por não utilizar as portas de hardware
dedicadas para isso, acaba consumindo mais processamento, e funciona apenas em
comunicações de baixa velocidade.

Apesar disso é uma boa solução para quando estamos simulando circuitos pelo
TinkerCad. 

### Programando
A programação será similar ao que já vimos com o *Hardware Serial*, mas
precisamos definir os pinos que iremos utilizar. 

```cpp
/**
 * DISPOSITIVO 1
 **/
#include <SoftwareSerial.h>

int pinoTx = 8;
int pinoRx = 7;

SoftwareSerial sSerial = SoftwareSerial(pinoRx, pinoTx);

void setup()
{
  pinMode(pinoTx, OUTPUT);
  pinMode(pinoRx, INPUT);
  
  sSerial.begin(9600);
}

void loop()
{
  sSerial.print("Info"); //Enviando a informação via comunicação serial. 
  sSerial.print('\n');   //Indico o fim da mensagem
  delay(1000);
}
```
```cpp
/**
 * DISPOSITIVO 2
 **/
#include<SoftwareSerial.h>

char    mensagem[128] = {0}; //Buffer para receber a mensagem
int     t_mensagem = 0;      //Tamanho da mensagem recebida
bool    isReading = true;

int pinoTx = 8;
int pinoRx = 7;

SoftwareSerial sSerial = SoftwareSerial(pinoRx, pinoTx);

void setup(){
    pinMode(pinoTx, OUTPUT);
    pinMode(pinoRx, INPUT);
  
    sSerial.begin(9600); //Lembre-se de configurar a velocidade igual o
                        //controller
    Serial.begin(9600);
}

void loop(){
    sSerial.listen();
    if (sSerial.available() > 0 && //Verifico se existe dados a serem recebidos
        t_mensagem < 128 ){    //Verifico se não estourei meu buffer&

        char c = sSerial.read(); //Leitura byte-por-byte

        mensagem[t_mensagem] = c; 
        t_mensagem++;

        if(c == '\n'){
            mensagem[t_mensagem] = '\0'; 
            isReading = false;
        }

        if(!isReading){
            //TODO: Seu código utilizando mensagem
            Serial.println(mensagem);
            t_mensagem = 0;
            isReading = true;
        }
    }
}
```

# $$I^2C$$ **Inter-Integrated Circuit**

O I2C é um protocolo de comunicação serial que permite a conexão de múltiplos
dispositivos periféricos (escravos) a um ou mais dispositivos controladores
(mestres). Diferente do UART que é ponto-a-ponto, o I2C utiliza um barramento
compartilhado, o que simplifica significativamente a fiação em projetos com
vários sensores e atuadores.

## Conexão Física

Para a comunicação, o I2C utiliza apenas duas linhas de sinal, além da
referência de aterramento (`GND`). Os pinos são dedicados e conhecidos como:

  * `SDA` (**Serial Data Line**): Linha por onde os dados são enviados e
    recebidos.
  * `SCL` (**Serial Clock Line**): Linha que carrega o sinal de clock,
    sincronizando a comunicação.


![](attachments/i2c_esq.png)

A conexão é feita em paralelo: todos os pinos `SDA` são conectados juntos e
todos os pinos `SCL` são conectados juntos. Lembre-se também de conectar os
pinos `GND` de todos os dispositivos para garantir uma referência de tensão
comum.

No I2C, um dispositivo atua como **controlador** (mestre), responsável por
iniciar a comunicação e gerar o sinal de clock na linha `SCL`. Os outros
dispositivos, os **periféricos** (escravos), "escutam" o barramento e só
respondem quando são chamados pelo controlador através de um endereço único.


![](attachments/i2c_multi.png)

> Cada dispositivo em um barramento I2C precisa ter um **endereço** único, que
> geralmente é um número de 7 bits. É através desse endereço que o controlador
> consegue se comunicar com um periférico específico, mesmo que vários outros
> estejam conectados no mesmo barramento. A colisão de endereços é um problema
> comum e deve ser verificada no datasheet dos componentes.

## Programando

No ambiente do Arduino, utilizamos a biblioteca `Wire.h`, que se encarrega de
abstrair e gerenciar toda a comunicação de baixo nível do protocolo I2C.

### Cenário 1: Controlador requisita e periférico responde

Neste exemplo, o controlador solicita 6 bytes de dados do periférico com o
endereço 2. O periférico, por sua vez, aguarda uma requisição e responde com a
mensagem "hello2".

```cpp
/**
 * CONTROLADOR
 * Fazendo requisição
 */
 #include <Wire.h> //Incluo a biblioteca de comunicação I2C

 void setup() {
   Wire.begin();        // Inicializo o *bus* I2C como controlador
   Serial.begin(9600);  // Inicializo o serial para debug
}

void loop() {
   // Faço uma requisição para o periférico de endereço 2, esperando 6 bytes
   Wire.requestFrom(2, 6);

   while (Wire.available()) { // Enquanto a comunicação estiver ativa:
     char c = Wire.read();    // Recebo a informação byte a byte
     Serial.print(c);
   }
   Serial.println();
   delay(500);
}
```

```cpp
/**
 * PERIFÉRICO
 * Respondendo requisição do controlador
 */
 #include <Wire.h>

void setup() {
   Wire.begin(2);                // Se conecta ao "bus" I2C com o endereço 2
   Wire.onRequest(requestEvent); // Registro a função que será chamada em uma requisição
}

void loop() {
   delay(100);
}

// Esta função é executada toda vez que o controlador requisitar dados
// para o endereço registrado (2).
void requestEvent() {
   Wire.write("hello2"); // Respondo a mensagem com 6 bytes.
}
```

### Cenário 2: Controlador envia e periférico recebe

Aqui, o controlador envia continuamente uma string e um valor numérico para o
periférico de endereço 2. O periférico está configurado para receber esses dados
e exibi-los em seu próprio monitor serial.

```cpp
/**
 * CONTROLADOR
 * Enviando dados
 */
#include <Wire.h>

void setup()
{
  Wire.begin(); // Inicializa como controlador
}

byte x = 0;

void loop()
{
  Wire.beginTransmission(2); // Inicia transmissão para o periférico de endereço 2
  Wire.write("x is ");        // Envia uma string (5 bytes)
  Wire.write(x);             // Envia o valor da variável x (1 byte)
  Wire.endTransmission();    // Finaliza a transmissão

  x++;
  delay(500);
}
```

```cpp
/**
 * PERIFÉRICO
 * Consome informação do controller
 */
#include <Wire.h>

void setup()
{
  Wire.begin(2);                 // Se conecta ao "bus" I2C com o endereço 2
  Wire.onReceive(receiveEvent);  // Registro a função que será chamada ao receber dados
  Serial.begin(9600);            // Inicializo o serial para debug
}

void loop()
{
  delay(100);
}

// Esta função é executada toda vez que o controlador enviar dados
// para o endereço registrado (2). O parâmetro 'howMany' indica
// quantos bytes foram recebidos.
void receiveEvent(int howMany)
{
  while(Wire.available())
  {
    char c = Wire.read();   // recebe o byte como um caractere
    Serial.print(c);        // imprime o caractere
  }
  Serial.println(); // Pula uma linha ao final da mensagem
}
```

# SPI **Serial Peripheral Interface**

Para além do I2C, temos também o SPI, um protocolo de comunicação serial
síncrono que se destaca pela sua alta velocidade e pela comunicação
*full-duplex*, o que significa que os dados podem ser enviados e recebidos
simultaneamente. Ele é amplamente utilizado em aplicações que exigem maior taxa
de transferência de dados, como cartões SD, displays e alguns tipos de sensores.
A principal desvantagem em relação ao I2C é a necessidade de um número maior de
fios.

> Historicamente, a documentação do SPI utilizava os termos *Master* (Mestre) e
> *Slave* (Escravo). No entanto, a comunidade de tecnologia tem se movido para
> substituir essa terminologia por alternativas mais inclusivas e descritivas.
> Por isso, adotaremos os termos **Controlador** e **Periférico**,
> respectivamente. O termo "Controlador" descreve com mais precisão o papel do
> dispositivo que gerencia o barramento e inicia a comunicação, enquanto
> "Periférico" define adequadamente os dispositivos que respondem às
> solicitações.

## Conexão Física

A comunicação SPI exige, no mínimo, quatro linhas para conectar um controlador a
um periférico. Lembre-se também de conectar o `GND` entre os dispositivos.

  * `SCLK` (**Serial Clock**): Linha que transporta o sinal de clock gerado pelo
    Controlador para sincronizar a transmissão de dados.
  * `COPI/MOSI` (**Controler Out Peripheral In**): Linha de saída de dados do
    **C**ontrolador para a entrada do **P**eriférico.
  * `CIPO/MISO` (**Controler In Peripheral Out**): Linha de entrada de dados no
    **C**ontrolador, vindo da saída do **P**eriférico.
  * `CS/PS/SS` (**Chip/Peripheral Select**): Pino no controlador que seleciona com qual
    periférico ele deseja se comunicar. Cada periférico no barramento precisa de
    uma linha `SS` dedicada. Quando o nível lógico dessa linha vai para baixo
    (`LOW`), o periférico é ativado.


![](attachments/spi_generico.png)

De forma similar ao I2C, o protocolo SPI também suporta a comunicação com
múltiplos periféricos em um único barramento. A principal diferença, no entanto,
está no método de seleção: em vez de um endereço via software, o SPI utiliza uma
linha de hardware dedicada, o Chip Select (CS), para cada periférico que se
deseja controlar. 


![](attachments/spi_multi.png)


## Programando

No ambiente do Arduino, a comunicação SPI é gerenciada pela biblioteca padrão
`SPI.h`. Ela fornece as funções necessárias para configurar o microcontrolador
como controlador ou periférico.

### Exemplo de Comunicação

No cenário a seguir, o controlador envia o caractere `'H'` para o periférico. O
periférico, ao receber o dado, envia de volta o caractere `'W'` na mesma
transação.

**Código do Controlador**

```cpp
/*
 * Descrição:
 * Este código configura a placa como controlador SPI enviando um caractere
 * para o periférico e recebendo a resposta. Um LED integrado indica o momento
 * da transmissão para feedback visual, enquanto as mensagens são exibidas no monitor serial.
 */

 #include <SPI.h>

// Define o pino que será usado para o Chip Select (CS).
// O pino 10 é o padrão para SS (Slave Select) na maioria das placas Arduino,
// mas você pode usar qualquer outro pino digital para controlar múltiplos periféricos.
const int pinoCS = 10;

// Configura os parâmetros da comunicação SPI:
// - Velocidade de 8MHz
// - Ordem de bits MSB (Most Significant Bit First)
// - Modo SPI 0
SPISettings mySPISettings(8000000, MSBFIRST, SPI_MODE0);

void setup() {
  // Inicializa a comunicação serial para exibir mensagens de debug.
  Serial.begin(9600);

  // Configura o pino de Chip Select como saída.
  pinMode(pinoCS, OUTPUT);
  
  // Garante que o Periférico comece desativado, mantendo a linha CS em nível ALTO.
  digitalWrite(pinoCS, HIGH);
  
  // Configura o LED integrado como saída para dar feedback visual.
  pinMode(LED_BUILTIN, OUTPUT);
  
  // Inicializa o hardware SPI da placa.
  SPI.begin();
}

void loop() {
  Serial.println("Controlador: Ativando periférico...");

  // --- Início da Transação SPI ---

  // Acende o LED para indicar que a comunicação está prestes a começar.
  digitalWrite(LED_BUILTIN, HIGH); 
  
  // Aplica as configurações SPI definidas anteriormente.
  SPI.beginTransaction(mySPISettings);
  
  // Ativa o periférico, colocando a linha CS em nível BAIXO.
  digitalWrite(pinoCS, LOW);
  
  // Envia o caractere 'H' e, ao mesmo tempo, lê o byte retornado pelo periférico.
  char receivedChar = SPI.transfer('H');
  
  // Desativa o periférico, retornando a linha CS para o nível ALTO.
  digitalWrite(pinoCS, HIGH);
  
  // Libera o barramento SPI para outros dispositivos.
  SPI.endTransaction();

  // Apaga o LED para indicar que a transação terminou.
  digitalWrite(LED_BUILTIN, LOW);

  // --- Fim da Transação SPI ---

  // Exibe o caractere recebido no monitor serial.
  Serial.print("Controlador: Recebido de volta -> ");
  Serial.println(receivedChar);
  
  // Aguarda 1 segundo antes de iniciar a próxima comunicação.
  delay(1000);
}
```

**Código do Periférico**

Como o SPI depende de uma resposta imediata e síncrona ao clock do controlador,
a programação do periférico geralmente depende de **interrupções** de hardware.
A forma de configurar isso pode variar entre diferentes arquiteturas de
microcontroladores, como AVR (Arduino Uno) e ESP32.

**Periférico em Arduino (AVR)**

```cpp
/*
 * Descrição:
 * Código para configurar o Arduino como periférico SPI. Ele usa uma rotina de
 * interrupção (ISR) para responder instantaneamente quando recebe um dado.
 **/
#include <SPI.h>
#include <stdint.h>

volatile char receivedChar;
volatile bool newDataAvailable = false;

// ISR (Interrupt Service Routine) - Executada quando a transferência SPI é completada
ISR (SPI_STC_vect) {
  receivedChar = SPDR;     // Lê o byte que acabou de ser recebido
  newDataAvailable = true;   // Marca flag indicando que um novo dado chegou
  SPDR = 'W';                // Prepara o próximo byte para ser enviado de volta
}

void setup() {
  Serial.begin(9600);
  pinMode(MISO, OUTPUT);
  // Importante: SS deve ser entrada para ser controlado pelo controlador
  pinMode(SS, INPUT);

  SPCR |= _BV(SPE);   // Habilita o hardware SPI
  SPCR |= _BV(SPIE);  // Habilita a interrupção SPI
  Serial.println("Arduino SPI Periférico inicializado");
}

void loop() {
  if (newDataAvailable) {
    Serial.print("Periférico recebeu: ");
    Serial.print(receivedChar);
    Serial.println(", enviou de volta: W");
    newDataAvailable = false; // Reseta a flag
  }
}
```

**Periférico em ESP32**

No ESP32, bibliotecas específicas podem abstrair a complexidade do hardware,
tornando a configuração mais simples.

```cpp
/*
 * Descrição:
 * Código para configurar a ESP32 como periférico SPI. A biblioteca
 * abstrai a configuração da interrupção através de uma função de callback.
 **/
#include <esp32SPI.h>

esp32SPI spi;
volatile char receivedChar = 0;
volatile bool newDataAvailable = false;

// Função de callback que será chamada quando dados forem recebidos via SPI
void onSPIReceive() {
  receivedChar = spi.read();
  spi.write('W'); // Prepara a resposta para o controlador
  newDataAvailable = true;
}

void setup() {
  Serial.begin(9600);
  spi.onReceive(onSPIReceive); // Registra a função de callback
  spi.begin();
  Serial.println("ESP32 SPI Periférico inicializado");
}

void loop() {
  if (newDataAvailable) {
    Serial.print("Periférico recebeu: ");
    Serial.print(receivedChar);
    Serial.println(", enviou de volta: W");
    newDataAvailable = false;
  }
  delay(10);
}
```



<style type="text/css"> .ritz .waffle a { color: inherit; }.ritz .waffle .s11{border-right:1px SOLID #000000;background-color:#ffffff;}.ritz .waffle .s14{border-bottom:1px SOLID #000000;background-color:#ffffff;}.ritz .waffle .s5{border-right:1px SOLID #000000;background-color:#ffffff;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s12{border-bottom:1px SOLID #000000;background-color:#ffffff;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s13{border-bottom:1px SOLID #000000;border-right:1px SOLID #000000;background-color:#ffffff;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s19{background-color:#e0f7fa;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s20{border-right:1px SOLID #000000;background-color:#e0f7fa;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s8{background-color:#e8e7fc;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s3{background-color:#ffffff;text-align:center;font-style:italic;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:nowrap;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s9{border-right:1px SOLID #000000;background-color:#e8e7fc;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s6{background-color:#e8f0fe;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s2{border-right:1px SOLID #000000;background-color:#8989eb;text-align:center;font-weight:bold;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:nowrap;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s0{border-right:1px SOLID #000000;background-color:#ffffff;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:nowrap;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s4{border-right:1px SOLID #000000;background-color:#ffffff;text-align:center;font-style:italic;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:nowrap;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s15{border-right:1px SOLID #000000;background-color:#63d297;text-align:center;font-weight:bold;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:nowrap;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s18{border-right:1px SOLID #000000;background-color:#e7f9ef;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s7{border-right:1px SOLID #000000;background-color:#e8f0fe;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s17{background-color:#e7f9ef;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:middle;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s16{border-right:1px SOLID #000000;background-color:#4dd0e1;text-align:center;font-weight:bold;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:nowrap;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s10{background-color:#ffffff;text-align:left;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:normal;overflow:hidden;word-wrap:break-word;direction:ltr;padding:2px 3px 2px 3px;}.ritz .waffle .s1{border-right:1px SOLID #000000;background-color:#5b95f9;text-align:center;font-weight:bold;color:#000000;font-family:"docs-Google Sans Text",Arial;font-size:14pt;vertical-align:bottom;white-space:nowrap;direction:ltr;padding:2px 3px 2px 3px;}</style>
