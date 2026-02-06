---
layout: "class"
course: "disruptive"
section:
    name: "Protocolos TCP/IP (Aplicação)"
    order: 5
class:
    title: "HTTP"
    order: 0
---

O protocolo HTTP (Hypertext Transfer Protocol) é um dos pilares da internet
moderna, sendo amplamente utilizado para a transferência de dados entre clientes
e servidores. Diferente do MQTT, que foi concebido para aplicações IoT e tem uma
abordagem leve e eficiente, o HTTP é mais robusto e universal, o que o torna
ideal para comunicação geral na web, incluindo na interação com dispositivos
conectados.

No modelo HTTP, a comunicação acontece de forma direta entre cliente e servidor,
seguindo a arquitetura cliente/servidor. O cliente, que pode ser um navegador,
aplicativo ou dispositivo IoT, envia requisições HTTP (requests) ao servidor,
que processa essas requisições e envia uma resposta (response), contendo o
recurso solicitado ou um código de status indicando o resultado da operação.

Cada requisição HTTP contém uma linha de método (GET, POST, PUT, DELETE, entre
outros), que indica a ação desejada, além de cabeçalhos que carregam informações
adicionais e, opcionalmente, um corpo com dados, especialmente em métodos como
POST e PUT. Essa estrutura simples, porém poderosa, permite a construção de APIs
RESTful, que são amplamente utilizadas para comunicação entre sistemas e
dispositivos.


# Servidor HTTP Simples com ESP32

Para experiências iniciais com HTTP no mundo IoT, o ESP32 pode ser programado
para atuar como um servidor web simples, possibilitando a interação via
navegador ou aplicações que façam requisições HTTP. Abaixo, um exemplo básico
que cria uma página web que exibe "Olá, Mundo!" e responde a requisições GET.

```cpp
#include <WiFi.h>
#include <WebServer.h>

// Defina suas credenciais Wi-Fi
const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";

WebServer server(80);

void handleRoot() {
  server.send(200, "text/plain", "Olá, Mundo!");
}

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  
  Serial.print("Conectando à rede Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("conectado!");
  Serial.print("Endereço IP: ");
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.begin();
  Serial.println("Servidor HTTP iniciado");
}

void loop() {
  server.handleClient();
}
```


# Cliente HTTP GET com ESP32

Além de responder requisições, o ESP32 também pode agir como cliente HTTP,
consumindo dados de servidores externos. O exemplo a seguir faz uma requisição
GET a um servidor web e imprime a resposta no monitor serial.

```cpp
#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);

  Serial.print("Conectando à rede Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("conectado!");
  
  HTTPClient http;
  http.begin("http://worldtimeapi.org/api/timezone/Etc/UTC"); // URL exemplo
  int httpCode = http.GET();
  
  if (httpCode > 0) {
    String payload = http.getString();
    Serial.println("Resposta do servidor:");
    Serial.println(payload);
  } else {
    Serial.printf("Erro na requisição: %d\n", httpCode);
  }
  http.end();
}

void loop() {
  // Não faz nada no loop
}
```

# Exemplo HTTP no Node-RED

No Node-RED, criar um endpoint HTTP é simples e permite que seus fluxos
respondam a requisições externas.

- Adicione um nó HTTP In, configure o método (GET, POST, etc.) e defina a URL do
  endpoint, por exemplo, /hello.

- Use um nó Template ou Function para preparar a resposta (exemplo: "Olá do
  Node-RED!").

- Conecte à saída a um nó HTTP Response para enviar a resposta ao cliente.

Fluxo básico de um endpoint GET /hello

```json
[ { "id": "http_in_1", "type": "http in", "z": "flow1", "name": "Receber GET
/hello", "url": "/hello", "method": "get", "x": 130, "y": 100, "wires":
[["function_1"]] }, { "id": "function_1", "type": "function", "z": "flow1",
"name": "Resposta simples", "func": "msg.payload = \"Olá do Node-RED!\";\nreturn
msg;", "x": 320, "y": 100, "wires": [["http_response_1"]] }, { "id":
"http_response_1", "type": "http response", "z": "flow1", "name": "", "x": 510,
"y": 100, "wires": [] } ]
```

Com este fluxo, ao acessar http://<endereço-node-red>:1880/hello em um
navegador, verá a mensagem "Olá do Node-RED!".
