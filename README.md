# Vinícola — IoT

Sistema de Internet das Coisas (IoT) desenvolvido para o **Projeto Integrador — Inteligência de Dados no Vale do São Francisco**.

O projeto utiliza um **ESP32** conectado a um sensor **DHT11** para coletar dados de temperatura e umidade. As leituras são enviadas por Wi-Fi utilizando o protocolo MQTT para o **ThingSpeak**.

## Arquitetura

```text
DHT11
  │
  │ Temperatura / Umidade
  ▼
ESP32
  │
  │ Wi-Fi
  │
  │ MQTT
  ▼
ThingSpeak
  │
  │ API REST
  ▼
Backend FastAPI
  │
  ▼
Supabase PostgreSQL
  │
  ▼
Dashboard Web
```

## Hardware

| Componente | Função |
|---|---|
| ESP32 | Processamento e comunicação Wi-Fi |
| DHT11 | Medição de temperatura e umidade |
| Cabo USB | Alimentação e gravação do firmware |

### Pino utilizado

O sensor DHT11 está configurado no código para utilizar o pino:

```text
GPIO 4
```

Tipo do sensor:

```text
DHT11
```

## Tecnologias

- ESP32
- Arduino IDE
- C/C++ para Arduino
- DHT Sensor Library
- Adafruit Unified Sensor
- PubSubClient
- Wi-Fi
- MQTT
- ThingSpeak

## ThingSpeak

O dispositivo publica as leituras no canal utilizado pelo projeto:

```text
Channel ID: 3500405
```

Canal:

**PI - ThingSpeak IoT**

https://thingspeak.mathworks.com/channels/3500405/

Os campos utilizados são:

| Campo | Dado |
|---|---|
| `field1` | Temperatura |
| `field2` | Umidade |

O payload publicado possui o formato:

```text
field1=24.50&field2=60.00
```

## Comunicação MQTT

O ESP32 utiliza o broker MQTT do ThingSpeak:

```text
Broker: mqtt3.thingspeak.com
Porta: 1883
```

O tópico utilizado para publicação é construído a partir do ID do canal:

```text
channels/<CHANNEL_ID>/publish
```

As credenciais MQTT são mantidas no arquivo local `arduino_secrets.h`.

> O arquivo `arduino_secrets.h` não deve ser enviado para o GitHub. Utilize somente o arquivo `arduino_secrets.h.example` no repositório.

## Instalação do Arduino IDE

Baixe o Arduino IDE pelo site oficial:

https://www.arduino.cc/en/software/

Após a instalação, abra o Arduino IDE.

## Configuração da placa ESP32

No Arduino IDE, acesse:

**Arquivo → Preferências**

Em **URLs adicionais para gerenciadores de placas**, adicione:

```text
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

Depois acesse:

**Ferramentas → Placa → Gerenciador de Placas**

Pesquise por:

```text
ESP32
```

Instale o pacote:

```text
esp32 by Espressif Systems
```

Selecione posteriormente a placa ESP32 utilizada no projeto.

## Bibliotecas

No Arduino IDE, acesse:

**Ferramentas → Gerenciar Bibliotecas**

Instale:

### DHT sensor library

Biblioteca da Adafruit:

```text
DHT sensor library
```

### Adafruit Unified Sensor

```text
Adafruit Unified Sensor
```

### PubSubClient

```text
PubSubClient
```

A biblioteca `WiFi.h` já é disponibilizada pelo pacote ESP32 instalado no Arduino IDE.

## Configuração das credenciais

O projeto contém:

```text
arduino_secrets.h.example
```

Crie uma cópia chamada:

```text
arduino_secrets.h
```

Preencha os dados necessários para conexão:

- SSID da rede Wi-Fi;
- senha do Wi-Fi;
- ID do canal ThingSpeak;
- Client ID MQTT;
- usuário MQTT;
- senha MQTT.

Exemplo conceitual:

```cpp
#define SECRET_SSID "SUA_REDE"
#define SECRET_PASS "SUA_SENHA"
#define SECRET_MQTT_CLIENT_ID "SEU_CLIENT_ID"
#define SECRET_MQTT_USERNAME "SEU_USUARIO"
#define SECRET_MQTT_PASSWORD "SUA_SENHA"
#define SECRET_CHANNEL_ID 3500405
```

> Nunca publique credenciais reais no código ou no GitHub.

## Upload para o ESP32

1. Conecte o ESP32 ao computador por USB.
2. Abra `Vinicola-IoT.ino`.
3. Selecione a placa ESP32 correta.
4. Selecione a porta correspondente ao dispositivo.
5. Clique em **Verificar** para compilar.
6. Clique em **Upload** para gravar o firmware.
7. Abra o Monitor Serial em `115200 baud` para acompanhar a conexão.

## Funcionamento do firmware

O fluxo executado pelo ESP32 é:

1. Inicialização do sensor DHT11.
2. Conexão à rede Wi-Fi.
3. Configuração do servidor MQTT.
4. Conexão ao broker do ThingSpeak.
5. Leitura da temperatura.
6. Leitura da umidade.
7. Validação básica das leituras.
8. Montagem do payload MQTT.
9. Publicação no canal ThingSpeak.
10. Repetição do processo.

O intervalo entre as publicações é de aproximadamente **16 segundos**, respeitando o intervalo mínimo necessário para utilização do plano gratuito do ThingSpeak.

## Reconexão automática

O firmware verifica continuamente o estado da conexão.

Caso o Wi-Fi seja desconectado, o ESP32 tenta estabelecer a conexão novamente.

Caso a sessão MQTT seja perdida, o dispositivo tenta se reconectar ao broker.

## Estrutura do projeto

```text
Vinicola-IoT/
├── README.md
├── Vinicola-IoT.ino
└── arduino_secrets.h.example
```

## Segurança

As credenciais utilizadas pelo dispositivo devem permanecer fora do código versionado.

O arquivo abaixo é apenas um modelo:

```text
arduino_secrets.h.example
```

O arquivo real deve permanecer local:

```text
arduino_secrets.h
```

Além disso, recomenda-se utilizar credenciais específicas para o dispositivo e realizar a rotação dessas credenciais quando necessário.

## Status atual

- [x] ESP32 configurado
- [x] Sensor DHT11
- [x] Leitura de temperatura
- [x] Leitura de umidade
- [x] Conexão Wi-Fi
- [x] Comunicação MQTT
- [x] Publicação no ThingSpeak
- [x] Reconexão automática de Wi-Fi
- [x] Reconexão automática de MQTT
- [x] Separação das credenciais em arquivo de secrets
- [ ] Comunicação MQTT com TLS
- [ ] Validação criptográfica da origem das mensagens
- [ ] Persistência local de leituras em caso de perda temporária da conexão

## Repositórios relacionados

- **Backend:** Vinicola-Back — API FastAPI hospedada no Render.
- **Frontend:** Vinicola-Front — dashboard web.
- **Banco:** Supabase PostgreSQL — banco utilizado pelo backend em produção.
