# Vinícola IoT

Sistema IoT desenvolvido para o **Projeto Integrador — Inteligência de Dados no Vale do São Francisco**.

O projeto utiliza um **ESP32 e sensores** para coletar dados climáticos, como temperatura e umidade, e enviar essas informações para o **ThingSpeak** por meio de Wi-Fi e MQTT.

---

## 1. Instalação do Arduino IDE

Baixe e instale o Arduino IDE pelo site oficial:

[https://www.arduino.cc/en/software/](https://www.arduino.cc/en/software/)

Após instalar, abra o **Arduino IDE**.

---

## 2. Configuração do ESP32

No Arduino IDE, abra:

**Arquivo → Preferências**

No campo **URLs adicionais para gerenciadores de placas**, adicione:

```text
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

Clique em **OK**.

Depois acesse:

**Ferramentas → Placa → Gerenciador de Placas**

Pesquise por:

```text
ESP32
```

Instale:

```text
esp32 by Espressif Systems
```

Após a instalação, selecione a placa ESP32 utilizada no projeto em:

**Ferramentas → Placa → esp32**

---

## 3. Instalação das bibliotecas

No Arduino IDE, acesse:

**Ferramentas → Gerenciar Bibliotecas**

Pesquise e instale as seguintes bibliotecas:

### DHT sensor library

Procure por:

```text
DHT sensor library
```

Instale a biblioteca da **Adafruit**.

### Adafruit Unified Sensor

Procure por:

```text
Adafruit Unified Sensor
```

Instale a biblioteca da **Adafruit**.

### PubSubClient

Procure por:

```text
PubSubClient
```

Essa biblioteca será utilizada para a comunicação **MQTT**.

> A biblioteca `WiFi.h` já está disponível através do pacote de placas ESP32 instalado anteriormente.

---

## 4. Configuração das credenciais

O projeto possui o arquivo:

```text
arduino_secrets.h.example
```

Esse arquivo serve como modelo para configurar as informações necessárias para conexão.

Faça uma cópia dele e renomeie a cópia para:

```text
arduino_secrets.h
```

Depois, abra o arquivo e preencha as informações solicitadas, como:

* Nome da rede Wi-Fi;
* Senha do Wi-Fi;
* Credenciais necessárias para comunicação com o ThingSpeak.

**Importante:** não envie o arquivo `arduino_secrets.h` para o GitHub, pois ele pode conter informações privadas.

Utilize apenas o arquivo:

```text
arduino_secrets.h.example
```

no repositório.

---

## 5. ThingSpeak

Os dados coletados pelo ESP32 são enviados para o canal do projeto no **ThingSpeak**.

### Canal do projeto

**[PI - ThingSpeak IoT](https://thingspeak.mathworks.com/channels/3500405/)**

Nesse canal é possível acompanhar os dados enviados pelo dispositivo IoT.

---

## 6. Enviando o código para o ESP32

1. Conecte o ESP32 ao computador utilizando um cabo USB.
2. Abra o arquivo:

```text
Vinicola-IoT.ino
```

3. No Arduino IDE, selecione a placa ESP32 utilizada no projeto.
4. Acesse **Ferramentas → Porta** e selecione a porta correspondente ao ESP32.
5. Clique em **Verificar** para compilar o código.
6. Clique em **Upload** para enviar o código para o ESP32.

Após o envio, o ESP32 iniciará o programa e realizará a coleta e o envio dos dados.

---

## Estrutura do projeto

```text
Vinicola-IoT/
│
├── README.md
├── Vinicola-IoT.ino
└── arduino_secrets.h.example
```

---

## Projeto Integrador

**Inteligência de Dados no Vale do São Francisco**

Este repositório corresponde à parte de **IoT e coleta de dados** do projeto, utilizando ESP32 e sensores para obtenção de informações climáticas.

