---
layout: "class"
course: "disruptive"
section:
    name: "Sistemas Embarcados"
    order: 2
class:
    title: "Introdução"
    order: 0
---

# O que é um sistema embarcado

Quando falamos de sistemas embarcados, imagine dispositivos “espertos” criados
para cumprir uma tarefa específica dentro de algum aparelho – pode ser desde o
controle do micro-ondas, a central eletrônica de um carro, até aquele sensor de
luz que ativa a iluminação automática. Diferente do computador ou notebook, que
servem para uma infinidade de funções, os sistemas embarcados existem para fazer
exatamente aquilo para o que foram projetados – e fazer muito bem.

Em termos práticos, eles são pequenos computadores construídos para rodar um
software (o chamado “firmware”), geralmente em cima de um hardware enxuto, com
microcontroladores e sensores pensados para otimizar espaço, energia e
estabilidade. Toda essa combinação é desenhada para atender a requisitos como
resposta rápida, confiabilidade e baixo custo — itens fundamentais para boa
parte dos eletrônicos ao nosso redor.

Sistemas embarcados podem ser super simples, como um controle remoto, ou
incrivelmente complexos, como os computadores de bordo de aviões e carros
autônomos. Seja qual for o caso, todos têm como característica central a união
entre hardware e software para dar conta de uma tarefa específica, nem mais, nem
menos.


# E como se desenvolve para sistemas embarcados?

Na bancada, o processo normalmente envolve três grandes etapas: escrever o
código que define o comportamento do sistema, compilar esse código para o
microcontrolador escolhido, e então gravar (“flashear”) esse código na placa.
Por isso, escolher bem as ferramentas de desenvolvimento pode facilitar (e
muito) o dia a dia:

- **Arduino IDE**: Extremamente popular, especialmente para quem está começando,
  pela simplicidade da interface e quantidade absurda de projetos e exemplos
  prontos. Com ela, dá para usar tanto placas Arduino como aquelas baseadas em
  ESP32, tornando o início do aprendizado menos complicado.
- **Espressif/ESP-IDF**: Esta é a ferramenta oficial para quem quer tirar o
  máximo proveito do ESP32. Ela fornece acesso a recursos avançados do chip e
  permite criar projetos mais robustos ou profissionais. Vai um pouco além da
  “zona de conforto” do Arduino, mas é super poderosa.
- **PlatformIO**: Um ambiente multiplataforma que integra várias placas e
  microcontroladores diferentes, além de se conectar facilmente a IDEs modernas
  como o VS Code. Para quem já está familiarizado com as opções básicas e busca
  produtividade e escalabilidade, é um prato cheio.

Essas plataformas ajudam a “traduzir” as ideias do papel para o hardware,
oferecendo desde suporte para conexões físicas até ferramentas de depuração e
gerenciamento de bibliotecas.


Dando esse passo a mais, entender sistemas embarcados vai muito além do código:
é pensar em como cada parte do circuito tem um papel para garantir que seu
projeto funcione direitinho na prática. 
