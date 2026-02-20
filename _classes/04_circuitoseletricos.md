---
layout: "class"
course: "disruptive"
section:
    name: "IoT"
    order: 1
class:
    title: "Circuítos (eletro)eletrônicos"
    order: 1
---

Antes de iniciar os trabalhos com os controladores, é bastante interessante que
você consiga olhar para um esquema (eletro)eletrônicos e entender minimamente o
que está acontecendo, e conseguir montar o circuito.

Você consegue entender o esquema abaixo?

![circuito 1](figuras/circuito1.png){: style="width:49%"}


Esse é um dos circuitos mais simples que podemos montar, ele é composto por uma
bateria, que é o ícone ao lado da inscrição 3V, um led eum resistor de 330 Ohms
e por fim o que liga todos esses componentes é algum tipo de conector, que pode
ser de fato fios de cobre, ou jumpers, pode ser um sistema de *protoboard*, ou
ainda uma placa de circuíto impresso (ou feita em casa).

Assim podemos criar o circuíto da seguinte forma: 

![circuito 1](figuras/circuito1.png){: style="width:49%"}
![foto do circuito 1](figuras/circuito1_foto.jpg){: style="width:49%"}

É uma possibilidade, mas para facilitar o processo de prototipagem é comum
utilizarmos as *protoboards* (ou *breadboard* em inglês).

![circuito 1](figuras/circuito1.png){: style="width:49%"}
![foto do circuito 1 melhorado](figuras/circuito1_foto2.jpg){: style="width:50%"}

## A *Protoboard*

Esse equipamentos facilita a parte de prototipagem, pois ele conta com a
estrutura mostrada na imagem abaixo. Cada trilha (linha) pode ser conectada por
diversos pinos, e todos que estiverem na mesma trilha compartilham do mesmo sinal
e/ou alimentação elétrica

![Protoboard](attachments/protoboard01.png){: style="width:49%"}

No exemplo desse circuito, veja como as trilhas ficam energizadas: 

![](attachments/protoboard02.png){: style="width:49%"}

Na figura acima, as trilhas energizadas estão destacadas em amarelo, enquanto os
componentes e jumpers aparecem em roxo. É possível observar que o circuito se
fecha com a bateria, permitindo seu funcionamento.

Note também que há diversos outros pinos já energizados disponíveis para a
montagem de novos circuitos. É importante sempre lembrar do 'caminho' da
eletricidade: ela parte do polo positivo (geralmente representado pela cor
vermelha) em direção ao polo negativo ou terra (representado pela cor preta).

Com tudo isso em mente já conseguimos olhar para outros componentes eletrônicos
comuns. Na figura abaixo é possível ver alguns desses componentes, e seus
equivalentes simbólicos para que consigamos ler e interpretar um diagrama
eletrônico.

![icones de circuitos](figuras/iconesCircuitos.png){: style="width:100%"}

# Componentes Eletrônicos

## Os básicos

### Chaves e Botões
<svg width="340" height="220">
  <defs>
    <clipPath id="chaves-clip">
      <rect x="0" y="0" width="340" height="220"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#chaves-clip)" 
    width="1128" 
    x="-550" 
    y="-25">
  </image>
</svg>

São dispositivos mecânicos ou eletrônicos que permitem abrir ou fechar um
circuito, interrompendo ou permitindo a passagem de corrente elétrica. São
usados para controlar o funcionamento de circuitos.

**Exemplos de uso:**  
- Interruptores para ligar e desligar luzes ou motores.  
- Botões de pressão em controles remotos, campainhas, ou painéis de controle.

**Tipos comuns:**  
- Chave liga/desliga (interruptor basculante).  
- Botão de pressão (momentâneo).  
- Chave seletora (permite selecionar entre várias posições).  
- Chave DIP (usada para configurações em placas eletrônicas).

### Resistor

<svg width="270" height="170">
  <defs>
    <clipPath id="resistor-clip">
      <rect x="0" y="0" width="270" height="170"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#resistor-clip)" 
    width="1128" 
    x="-590" 
    y="-480">
  </image>
</svg>

Limita a corrente elétrica que passa em um circuito, protegendo componentes
sensíveis e ajustando níveis de tensão e corrente.

**Exemplos de uso:**  
- Proteger LEDs para que não queimem.  
- Definir divisores de tensão para sensores.  
- Controlar corrente em circuitos de áudio.

**Tipos comuns:**  
- Resistor fixo (valores padrão como 220Ω, 1kΩ).  
- Resistor variável (trimpot ou potenciômetro).  
- Resistor de fio (para altas potências).  
- Resistor SMD (montagem em superfície para placas de circuito impresso).

### Diodos

<svg width="220" height="140">
  <defs>
    <clipPath id="diodo-clip">
      <rect x="0" y="0" width="220" height="140"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#diodo-clip)" 
    width="1128" 
    x="0" 
    y="-590">
  </image>
</svg>

Permitem que a corrente passe somente em uma direção. Usados para proteção ou
controle da direção da corrente.

**Exemplos de uso:**  
- Retificação em fontes de alimentação (converter corrente alternada em contínua).  
- Proteção contra polaridade invertida em circuitos.  
- Controle de sinais em circuitos digitais.

**Tipos comuns:**  
- Diodo retificador (1N4001).  
- Diodo zener (usado para regulação de tensão).  
- Diodo de alta velocidade.  


#### LED: Um diodo especial

<svg width="240" height="160">
  <defs>
    <clipPath id="led-clip">
      <rect x="0" y="0" width="240" height="160"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#led-clip)" 
    width="1128" 
    x="-250" 
    y="-350">
  </image>
</svg>

É um diodo que emite luz visível ou infravermelha quando a corrente elétrica
passa por ele.

**Exemplos de uso:**  
- Indicadores de estado em painéis eletrônicos.  
- Luzes de notificação em dispositivos.  
- Comunicação via infravermelho (controle remoto).

**Tipos comuns:**  
- LED padrão (vermelho, verde, azul, amarelo).  
- LED RGB (contém LEDs vermelho, verde e azul para gerar várias cores).  
- LED infravermelho (usado para comunicação remota).  
- LED de alta potência para iluminação.


### Potenciômetros

<svg width="230" height="170">
  <defs>
    <clipPath id="pot-clip">
      <rect x="0" y="0" width="230" height="170"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#pot-clip)" 
    width="1128" 
    x="-600" 
    y="-270">
  </image>
</svg>

Resistores ajustáveis que permitem alterar a resistência manualmente,
controlando a quantidade de corrente que passa.

**Exemplos de uso:**  
- Ajustar o volume em rádios e amplificadores.  
- Controlar a intensidade luminosa de LEDs.  
- Ajustar a sensibilidade de sensores em projetos de IoT.

**Tipos comuns:**  
- Potenciômetro rotativo (padrão, ajustado girando um eixo).  
- Potenciômetro deslizante.  
- Trimpot (ajuste fino, geralmente usado em circuitos para calibração).


### Capacitores

<svg width="280" height="110">
  <defs>
    <clipPath id="cap-clip">
      <rect x="0" y="0" width="280" height="110"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#cap-clip)" 
    width="1128" 
    x="-240" 
    y="-590">
  </image>
</svg>
Armazenam energia elétrica temporariamente e liberam de forma controlada. Ajuda
na filtragem de ruídos, estabilização de sinais e temporização.

**Exemplos de uso:**  
- Filtrar ruídos em fontes de alimentação.  
- Criar temporizadores em circuitos eletrônicos.  
- Acoplar sinais em amplificadores para bloquear DC.

**Tipos comuns:**  
- Capacitor cerâmico (ideais para filtrar sinais em alta frequência).  
- Capacitor eletrolítico (maiores capacidades, usados em fontes).  
- Capacitor de tantálio.  
- Capacitor de filme plástico.

### Motores DC Simples

Convertem energia elétrica em movimento rotacional, essenciais para projetos que
envolvem movimento.

**Exemplos de uso:**  
- Robôs que se movimentam.  
- Ventiladores e bombas pequenas.  
- Modelismo e automação simples.

**Tipos comuns:**  
- Motor DC com escovas.  
- Motor brushless (sem escovas, mais eficiente).  
- Motor com engrenagem (para aumento de torque).


### Buzzers

<svg width="226" height="226">
  <defs>
    <clipPath id="buz-clip">
      <rect x="0" y="0" width="226" height="226"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#buz-clip)" 
    width="1128" 
    x="-900" 
    y="-250">
  </image>
</svg>

Geram sons para alertas sonoros em dispositivos eletrônicos.

**Exemplos de uso:**  
- Alarmes e sinalizações em dispositivos.  
- Acerte/notifique erros ou eventos.  
- Partes de sistemas de feedback tátil/sonoro.

**Tipos comuns:**  
- Buzzer passivo (precisa de sinal oscilante para emitir som).  
- Buzzer ativo (gera som com apenas a aplicação da tensão).

### Transistores

<svg width="340" height="290">
  <defs>
    <clipPath id="trans-clip">
      <rect x="0" y="0" width="340" height="290"></rect>
    </clipPath>
  </defs>

  <image 
    href="figuras/iconesCircuitos.png" 
    clip-path="url(#trans-clip)" 
    width="1128" 
    x="-220" 
    y="0">
  </image>
</svg>

Atuam como amplificadores de sinal ou interruptores eletrônicos, usados para
controlar correntes maiores com sinais menores.

**Exemplos de uso:**  
- Amplificação de sinais de áudio.  
- Controle de motores e LEDs com microcontroladores.  
- Circuitos de chaveamento e reguladores de tensão.

**Tipos comuns:**  
- Transistor Bipolar de Junção (NPN e PNP).  
- Transistor de Efeito de Campo (MOSFET).  
- Transistor Darlington (alto ganho de corrente).



## Algumas relações importantes!

Para finalizar nossa discussão de eletrônica básica, vamos ver algumas relações
que ajudam a montar circuítos que funcionem:

### Lei de Ohm.

$$
V = { {R}\over{I} }
$$


Onde:

* **V** = Tensão (medida em volts, V)
* **R** = Resistência (medida em ohms, Ω)
* **I** = Corrente elétrica (medida em ampères, A)

Ou seja, a tensão aplicada em um componente é igual à resistência dele
multiplicada pela corrente que passa por ele.

A partir dessa relação, podemos isolar as variáveis conforme a necessidade:

$$
R = \frac{V}{I} \quad \quad I = \frac{V}{R}
$$

Essas fórmulas permitem, por exemplo, calcular qual resistor usar para limitar a
corrente de um LED.

### Potência Elétrica 

$$
P = V \times I
$$

* **P** = Potência (medida em watts, W)
* **V** = Tensão (medida em volts, V)
* **I** = Corrente elétrica (medida em ampères, A)

Ela indica quanta energia elétrica está sendo consumida ou dissipada em um
componente. Isso é útil, por exemplo, para garantir que um resistor não aqueça
além do limite ou para dimensionar a fonte de alimentação de um circuito.

### Em resumo:

* Use a **Lei de Ohm** para relacionar tensão, corrente e resistência.
* Use a **fórmula da potência** para saber o quanto de energia está sendo
  consumido.

Com essas duas relações básicas, já é possível interpretar e projetar circuitos
simples com mais segurança e eficiência.

