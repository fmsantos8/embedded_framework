# Embedded Framework

Este repositório centraliza os links para os projetos desenvolvidos para um
framework modular em C voltado ao compartilhamento de código de firmware entre
plataformas de microcontroladores. Reúne a aplicação, os componentes, os contratos
públicos da camada de abstração de hardware (HAL), a arena de memória, os portes
de plataforma e as implementações de referência.

A prova de conceito realiza aquisição de temperatura e umidade com o sensor
SHT31-D, apresenta os dados em um display SSD1306 e oferece interação por botão,
indicação por LED e telemetria pela UART. As implementações abrangem ESP32,
nRF52840, STM32F407 e STM32F411.

## Aplicação, interfaces da HAL e memória

A aplicação coordena o comportamento dos dispositivos por meio das interfaces
dos componentes. Os componentes utilizam os contratos públicos da HAL, enquanto
os portes implementam esses contratos com os SDKs de cada plataforma.

| Repositório | Responsabilidade |
| --- | --- |
| [framework_app](https://github.com/fmsantos8/framework_app) | Aplicação comum: aquisição, telas da interface gráfica, botão, indicação por LED e políticas de telemetria. |
| [hal_interface](https://github.com/fmsantos8/hal_interface) | Contratos públicos de GPIO, I2C, UART, temporizador e sistema, sem expor tipos dos SDKs. |
| [memory](https://github.com/fmsantos8/memory) | Arena linear compartilhada para armazenamento persistente dos objetos dos portes da HAL. |

## Componentes

Cada componente encapsula o acesso a um dispositivo ou serviço e recebe os
recursos necessários por configuração. A aplicação define quando e com qual
finalidade utiliza suas operações.

| Repositório | Responsabilidade |
| --- | --- |
| [button](https://github.com/fmsantos8/button) | Leitura de botão, tratamento de polaridade e estabilização do estado. |
| [led](https://github.com/fmsantos8/led) | Controle de LED com conversão do estado lógico conforme a polaridade configurada. |
| [sht31d](https://github.com/fmsantos8/sht31d) | Comunicação com o sensor SHT31-D, validação de CRC e conversão das leituras de temperatura e umidade. |
| [ssd1306](https://github.com/fmsantos8/ssd1306) | Desenho em memória e transferência da imagem para o display SSD1306 por I2C. |
| [telemetry](https://github.com/fmsantos8/telemetry) | Formatação e envio de registros de medições e erros pela UART. |

## Portes da HAL

Os portes traduzem os contratos públicos da HAL para os recursos de cada SDK.
O porte STM32F4 é compartilhado pelas implementações com STM32F407 e STM32F411.

| Repositório | Plataforma | SDK utilizado |
| --- | --- | --- |
| [esp32-wroom-32](https://github.com/fmsantos8/esp32-wroom-32) | ESP32 | ESP-IDF |
| [nrf52840](https://github.com/fmsantos8/nrf52840) | nRF52840 | nRF Connect SDK / Zephyr e nrfx |
| [stm32f4](https://github.com/fmsantos8/stm32f4) | STM32F407 e STM32F411 | STM32CubeF4 HAL e CMSIS |

## Implementações por plataforma

Os projetos com framework reúnem a configuração de placa (BSP), a composição
dos componentes e a integração com o SDK. A aplicação e os módulos compartilhados
são incorporados como submódulos Git.

As versões sem framework são implementações convencionais da mesma prova de
conceito, com acesso direto aos SDKs dos fabricantes, utilizadas como referência
na avaliação comparativa.

| Plataforma | Com framework | Sem framework (referência) |
| --- | --- | --- |
| ESP32 | [esp32_framework](https://github.com/fmsantos8/esp32_framework) | [esp32_sem_framework](https://github.com/fmsantos8/esp32_sem_framework) |
| nRF52840 | [nrf52840_framework](https://github.com/fmsantos8/nrf52840_framework) | [nrf52840_sem_framework](https://github.com/fmsantos8/nrf52840_sem_framework) |
| STM32F407 | [stm32f407_framework](https://github.com/fmsantos8/stm32f407_framework) | [stm32f407_sem_framework](https://github.com/fmsantos8/stm32f407_sem_framework) |
| STM32F411 | [stm32f411_framework](https://github.com/fmsantos8/stm32f411_framework) | [stm32f411_sem_framework](https://github.com/fmsantos8/stm32f411_sem_framework) |

Para utilizar uma implementação com framework, obtenha o projeto da plataforma
desejada incluindo seus submódulos. Por exemplo, para ESP32:

```sh
git clone --recurse-submodules https://github.com/fmsantos8/esp32_framework.git
```