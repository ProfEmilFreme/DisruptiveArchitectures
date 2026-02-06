---
layout: "class"
course: "disruptive"
section:
    name: "Backend"
    order: 3
class:
    title: "Python"
    order: 1
---

### Por que Python?

Python é conhecido por sua simplicidade e por uma comunidade gigante que leva
facilidade a projetos complexos. Para IoT, ele tem uma série de bibliotecas
prontas:  
- **FastAPI** ou **aiohttp** para criar endpoints HTTP rápidos  
- **gmqtt** ou **hbmqtt** para conexão MQTT  
- **websockets** ou suporte embutido em FastAPI para WebSocket  

Tudo isso funciona bem junto, na mesma thread, usando `asyncio`, o coração do
Python para lidar com várias tarefas ao mesmo tempo — uma verdadeira orquestra
de comunicação.

### Como funciona essa integração

A ideia é montar um servidor que seja o centro de tudo:  
- Um endpoint HTTP para receber comandos, configurar dispositivos ou retornar
  dados.  
- Uma conexão MQTT que envia e recebe mensagens, podendo publicar em tópicos ou
  escutar eventos de sensores.  
- Um WebSocket que mantém conexão aberta com o navegador ou outro dispositivo,
  enviando novidades em tempo real.  

Tudo rodando na mesma aplicação, trocando informações entre os protocolos de
forma rápida e sincronizada.

### Uma aproximação prática

Vamos imaginar um fluxo básico:  
1. Usuário envia uma requisição HTTP para alterar o estado de um sensor; o
   servidor processa e publica uma mensagem MQTT para o dispositivo.  
2. Dispositivo responde com uma mensagem MQTT; o servidor capta essa mensagem e,
   através do WebSocket, manda essa atualização para o painel na tela do
   usuário.  
3. O painel, com conexão WebSocket aberta, recebe essa informação
   instantaneamente, sem precisar recarregar a página.



