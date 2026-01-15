---
layout: "class"
course: "disruptive"
section:
    name: "Protocolos TCP/IP (Aplicação)"
    order: 5
class:
    title: "WebSocket"
    order: 2
---

O protocolo WebSocket surge para superar algumas limitações do HTTP,
especialmente em aplicações que exigem comunicação em tempo real entre cliente e
servidor. Diferentemente do HTTP, que funciona no modelo requisição-resposta, o
WebSocket estabelece uma conexão persistente e bidirecional, permitindo que
ambas as partes enviem dados a qualquer momento, sem a necessidade de reiniciar
a comunicação a cada troca.

Essa característica faz do WebSocket um protocolo ideal para aplicações IoT que
demandam atualizações rápidas e contínuas, como monitoramento, controle remoto,
jogos online, chats e dashboards dinâmicos. Além disso, o WebSocket reduz a
sobrecarga de cabeçalhos e conexões repetidas, otimizando o uso da banda e do
processamento.


# Servidor WebSocket com ESP32

Vamos ver um exemplo simples onde o ESP32 atua como servidor WebSocket,
aceitando conexões e enviando mensagens para os clientes conectados.

```cpp
#include <WiFi.h>
#include <WebSocketsServer.h>

// Credenciais Wi-Fi
const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";

WebSocketsServer webSocket = WebSocketsServer(81); // Porta 81 para o WebSocket

void onWebSocketEvent(uint8_t num, WStype_t type, uint8_t * payload, size_t length) {
  switch(type) {
    case WStype_DISCONNECTED:
      Serial.printf("Cliente %u desconectado\n", num);
      break;
    case WStype_CONNECTED: {
      IPAddress ip = webSocket.remoteIP(num);
      Serial.printf("Cliente %u conectado, IP: %s\n", num, ip.toString().c_str());
      webSocket.sendTXT(num, "Bem-vindo ao WebSocket ESP32!");
      break;
    }
    case WStype_TEXT:
      Serial.printf("Recebido do cliente %u: %s\n", num, payload);
      // Ecoa a mensagem para todos os clientes
      webSocket.broadcastTXT(payload, length);
      break;
    default:
      break;
  }
}

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  
  Serial.print("Conectando ao Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nConectado!");
  Serial.print("IP: ");
  Serial.println(WiFi.localIP());

  webSocket.begin();
  webSocket.onEvent(onWebSocketEvent);

  Serial.println("Servidor WebSocket iniciado.");
}

void loop() {
  webSocket.loop();
}
```

# Cliente WebSocket Simples em JavaScript

Para testar o servidor, pode-se usar uma página HTML simples que cria uma
conexão WebSocket e mostra as mensagens enviadas e recebidas:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Cliente WebSocket ESP32</title>
</head>
<body>
  <h1>Cliente WebSocket</h1>
  <textarea id="log" cols="50" rows="10" readonly></textarea><br>
  <input id="message" type="text" placeholder="Digite uma mensagem...">
  <button onclick="sendMessage()">Enviar</button>

  <script>
    const log = document.getElementById('log');
    const ws = new WebSocket('ws://ENDERECO_IP_ESP32:81');

    ws.onopen = () => {
      log.value += "Conectado ao servidor WebSocket\n";
    };

    ws.onmessage = event => {
      log.value += "Recebido: " + event.data + "\n";
    };

    ws.onclose = () => {
      log.value += "Conexão fechada\n";
    };

    function sendMessage() {
      const input = document.getElementById('message');
      ws.send(input.value);
      log.value += "Enviado: " + input.value + "\n";
      input.value = '';
    }
  </script>
</body>
</html>
```

Exemplo WebSocket no Node-RED

O Node-RED oferece dois nós para WebSocket:

- WebSocket In (para receber mensagens)

- WebSocket Out (para enviar mensagens)

Eles podem operar como servidor (modo "Listen On") para aceitar conexões, ou
como cliente para se conectar a servidores externos. Criando um servidor
WebSocket que recebe e envia mensagens

- Adicione um nó WebSocket In configurado como "Listen On", com o caminho
  /ws/chat.

- Conecte a um nó Debug para visualizar as mensagens recebidas.

- Para enviar mensagens, use um nó Inject conectado a um nó WebSocket Out
  configurado com o mesmo caminho.

Fluxo básico de chat via WebSocket

```json
[ { "id": "websocket_in", "type": "websocket in", "z": "flow2", "name": "Escutar
/ws/chat", "server": "ws_server", "client": "", "x": 140, "y": 180, "wires":
[["debug_1"]] }, { "id": "debug_1", "type": "debug", "z": "flow2", "name":
"Mostrar mensagens", "active": true, "x": 330, "y": 180, "wires": [] }, { "id":
"inject_1", "type": "inject", "z": "flow2", "name": "Enviar 'Olá WebSocket'",
"props": [{"p": "payload"}], "payload": "Olá WebSocket", "payloadType": "str",
"repeat": "", "once": false, "x": 150, "y": 240, "wires": [["websocket_out"]] },
{ "id": "websocket_out", "type": "websocket out", "z": "flow2", "name": "Enviar
para /ws/chat", "server": "ws_server", "client": "", "x": 340, "y": 240,
"wires": [] }, { "id": "ws_server", "type": "websocket-listener", "path":
"/ws/chat", "wholemsg": "payload" } ]
```

Assim, clientes WebSocket podem se conectar ao endpoint
ws://<endereço-node-red>:1880/ws/chat, enviar mensagens que aparecerão no debug,
e receber mensagens enviadas pelo nó Inject.

