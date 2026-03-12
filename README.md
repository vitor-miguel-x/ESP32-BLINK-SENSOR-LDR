# Projeto: Monitor de Luminosidade com LDR e ESP32 

Este repositório contém o projeto de um sistema de controle de iluminação automática desenvolvido para a plataforma **ESP32**. O sistema utiliza um sensor **LDR (Light Dependent Resistor)** para medir a luz ambiente e aciona um LED automaticamente quando detecta que o ambiente está escuro.

VIDEO: [SENSOR LDR](https://youtu.be/e2Rd60zRVaQ)
---

## 📝 Descrição do Projeto
O objetivo desta atividade é utilizar o **ADC (Analog-to-Digital Converter)** do ESP32 para interpretar variações de resistência baseadas na luz.
- O sensor LDR altera sua resistência conforme a intensidade luminosa.
- O sistema realiza a leitura analógica (0 a 4095).
- Se a luminosidade cair abaixo do **limiar de 400**, o sistema identifica que o ambiente está escuro e acende o LED de iluminação de emergência/conforto.
- Os dados são monitorados em tempo real via **Monitor Serial**.

## 🛠️ Hardware Necessário
| Componente | Quantidade |
| :--- | :---: |
| Placa ESP32 (DevKit V1) | 1 |
| Sensor LDR (Módulo ou componente) | 1 |
| LED Vermelho | 1 |
| Resistor (220Ω para o LED) | 1 |
| Protoboard | 1 |
| Jumpers (Macho-Fêmea e Macho-Macho) | 7 |

## 🔌 Esquema de Ligação
As conexões foram realizadas conforme a imagem da montagem física:

1. **Sensor LDR (Módulo):**
   - **VCC:** Cabo **Vermelho**, conectado ao pino **VIN** ou **3.3V**.
   - **GND:** Cabo **Preto**, conectado ao **GND**.
   - **DO (Saída Digital):** Não utilizado neste código.
   - **AO (Saída Analógica):** Cabo **Roxo**, conectado à **GPIO 34**.

2. **LED de Alerta:**
   - **Anodo (perna longa):** Cabo **Laranja**, conectado à **GPIO 32**.
   - **Catodo (perna curta):** Conectado ao resistor, que liga ao cabo **Verde** (GND).

3. **Outras Conexões de Apoio:**
   - Cabos **Cinza** e **Branco** utilizados para extensão de barramento de energia/GND na protoboard.

## 💻 Configuração do Ambiente
1. Instale a **Arduino IDE**.
2. No menu `Preferências`, adicione a URL: 
   `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
3. Instale o pacote **esp32** no `Gerenciador de Placas`.

## 📜 Código Fonte
```cpp
/*
 * Projeto: Monitor de Luminosidade (LDR)
 * Autores: Vitor Miguel, Kauan, Davi
 */

// Configuração dos Pinos
const int PINO_LDR = 34;       // Entrada analógica (Cabo Roxo)
const int PINO_LED_ESCURO = 32; // Saída digital (Cabo Laranja)

// Limiar de decisão: quanto menos luz, menor o valor lido
const int LIMIAR_ESCURO = 400;

void setup() {
  Serial.begin(115200);
  pinMode(PINO_LED_ESCURO, OUTPUT);
}

void loop() {
  // Leitura do valor de luminosidade (0 - 4095)
  int valor_luminosidade = analogRead(PINO_LDR);

  Serial.print("Valor de luminosidade: ");
  Serial.println(valor_luminosidade);

  // Lógica de decisão: Invertida conforme a necessidade do ambiente
  if (valor_luminosidade < LIMIAR_ESCURO) {
    // Ambiente escuro
    Serial.println("Ambiente escuro! Acendendo o LED.");
    digitalWrite(PINO_LED_ESCURO, HIGH); 
  }
  else {
    // Ambiente claro
    Serial.println("Ambiente claro. LED apagado.");
    digitalWrite(PINO_LED_ESCURO, LOW);
  }

  delay(1000); // Intervalo de 1 segundo
}
