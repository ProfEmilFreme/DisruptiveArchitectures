#### 1. Microcontroladores (MCUs)

Representando a base da IoT, os microcontroladores são circuítos integrados
que reunem processador, memória e entrada e saida (I/O) em um único chip,
projetados para executar tarefas específicas em sistemas embarcados. Eles
executam um programa único (firmware) em um loop contínuo, sem um sistema
operacional complexo.

Tem como caracteristicas o controle em tempo real, baixo consumo de energia e
acesso direto ao hardware. São ideais para ler dados de sensores, controlar
motores, gerenciar LEDs e comunicar-se através de protocolos de baixa velocidade
(como I2C, SPI, UART). Modelos mais avançados integram nativamente módulos de
conectividade sem fio (Wi-Fi, Bluetooth), tornando-os soluções completas para
nós de IoT.

Por seu baixo custo, simplicidade e eficiência energética, os MCUs são a escolha
predominante para dispositivos de ponta na IoT de Consumo, como wearables,
sensores domésticos inteligentes e eletrônicos de baixo custo.

 - *Exemplos:* Arduino Uno, ESP32

#### 2. Computadores de Placa Única (SBCs)

Os SBCs representam um aumento significativo no poder de processamento e na
flexibilidade, atuando como um intermediário entre dispositivos simples e
sistemas industriais complexos. São computadores totalmente funcionais
construídos em uma única placa de circuito. Diferente dos MCUs, eles possuem
processadores muito mais potentes (baseados em arquiteturas como ARM ou x86),
mais memória RAM e a capacidade de executar sistemas operacionais completos,
como Linux.

A presença de um sistema operacional permite multitarefa, gerenciamento de rede
avançado e a execução de softwares complexos. Eles podem processar dados
localmente (edge computing), hospedar servidores, gerenciar bancos de dados e
até mesmo executar algoritmos de inteligência artificial. Oferecem conectividade
padrão de desktop, como USB, Ethernet e HDMI.

Os SBCs são ideais para aplicações que exigem mais processamento do que um MCU
pode oferecer, como gateways de IoT (que agregam dados de múltiplos sensores),
centrais de automação residencial, sistemas de vigilância por vídeo e quiosques
interativos.

As SBCs podem aparecer em ambos os lados do espectro do IoT, assim como os
computadores comuns que temos em casa, suas específicações que regem seu valor
final e aplicação.  

- *Exemplos*: Raspbery Pi, Orange Pi

#### 3. Hardware Programável (FPGAs)

Os FPGAs (Field-Programmable Gate Arrays) oferecem uma abordagem
fundamentalmente diferente: em vez de programar software, o próprio hardware é
configurado para realizar uma tarefa.

São circuitos integrados compostos por blocos de lógica configuráveis e
interconexões programáveis. O desenvolvedor descreve o comportamento do hardware
usando uma Linguagem de Descrição de Hardware (HDL). Essa descrição é então
sintetizada e carregada no FPGA, criando um circuito digital personalizado.

A principal vantagem é o paralelismo massivo e a latência extremamente baixa.
Tarefas que seriam lentas em um processador sequencial podem ser executadas
simultaneamente em hardware, resultando em um desempenho excepcional para
processamento de sinais, visão computacional de alta velocidade e comunicação de
protocolos customizados.

Devido ao seu custo mais elevado e à necessidade de conhecimento especializado,
os FPGAs são usados em aplicações de nicho de alto desempenho, tanto em produtos
de consumo avançados quanto em equipamentos industriais e de telecomunicações
que exigem processamento em tempo real com determinismo absoluto.

#### 4. Controladores Lógicos Programáveis (PLCs)

No extremo industrial do espectro, os PLCs são a espinha dorsal da automação
industrial, projetados para confiabilidade e operação ininterrupta em ambientes
hostis.

São computadores industriais robustos, projetados para suportar condições
severas de temperatura, vibração, umidade e interferência eletromagnética. Sua
arquitetura é modular, permitindo a fácil expansão de módulos de entrada e saída
para conectar-se a uma vasta gama de sensores e atuadores industriais.

Otimizados para controle em tempo real e execução determinística. São
programados usando linguagens padrão da indústria (como a Lógica Ladder),
focadas na robustez e na facilidade de diagnóstico por técnicos de chão de
fábrica. Sua principal função não é o processamento de dados em massa, mas sim
garantir que os processos de controle ocorram de forma previsível e segura,
repetidamente.

Os PLCs são a escolha padrão para o controle de máquinas, linhas de montagem,
processos robóticos e infraestrutura crítica. Seu alto custo é justificado pela
sua extrema confiabilidade, segurança e longo ciclo de vida, requisitos
indispensáveis no mundo da IoT Industrial.

