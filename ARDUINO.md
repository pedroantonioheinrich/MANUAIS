# 📚 MANUAL COMPLETO DE ARDUINO
## Do Básico às Aplicações Avançadas

---

## **PARTE 1: FUNDAMENTOS DO ARDUINO**

### 1.1 O que é Arduino e Por que Usá-lo?

**Arduino** é uma plataforma de prototipagem eletrônica de código aberto. Imagine um pequeno cérebro que você pode programar para controlar coisas no mundo físico: acender luzes, ler sensores, mover motores.

**Por que existe?** Antes do Arduino, trabalhar com microcontroladores era complexo e caro. O Arduino democratizou a eletrônica, permitindo que qualquer pessoa crie projetos interativos.

### 1.2 Anatomia de uma Placa Arduino (Uno)

```
┌─────────────────────────────────────┐
│  ○ USB   ○○ POWER                    │
│  [RESET] ○○○○○ DIGITAL (PWM~)        │
│  ○ 3.3V  ┌─────────────────────┐    │
│  ○ 5V    │   MICROCONTROLADOR  │    │
│  ○ GND   │   ATMEGA328P        │    │
│  ○ Vin   └─────────────────────┘    │
│  ○○○○○○ ANALOG IN                    │
└─────────────────────────────────────┘
```

**Cada parte tem uma função específica:**

- **USB**: Comunicação com computador e alimentação
- **Pinos Digitais (0-13)**: Entrada/Saída de sinais binários (0 ou 5V)
- **Pinos Analógicos (A0-A5)**: Leitura de valores variáveis (0-1023)
- **PWM (~)**: Simulação de tensão variável
- **Power**: Alimentação externa (5V, 3.3V, GND)

---

## **PARTE 2: ESTRUTURA DE UM PROGRAMA ARDUINO**

### 2.1 O Esqueleto Básico

```cpp
void setup() {
  // Executa UMA VEZ ao ligar
}

void loop() {
  // Executa PARA SEMPRE em ciclo
}
```

**Por que essa estrutura?** 
- **setup()**: Inicializa configurações que não precisam repetir (como definir se um pino é entrada ou saída)
- **loop()**: O microcontrolador fica em espera constante, executando este bloco repetidamente. É como se você perguntasse "o que fazer agora?" milhões de vezes por segundo.

### 2.2 Analogia do Restaurante

Imagine que o Arduino é um chef de cozinha:
- **setup()** = preparar os ingredientes, aquecer o forno (configuração inicial)
- **loop()** = cozinhar continuamente os pedidos que chegam

---

## **PARTE 3: APLICAÇÕES PRÁTICAS COMENTADAS**

### **PROJETO 1: Pisca-LED (O "Hello World" da Eletrônica)**

```cpp
// Definição de constantes - facilita manutenção
const int LED_PIN = 13;  // Pino onde o LED está conectado

void setup() {
  // Configura o pino como SAÍDA
  // Por quê? Precisamos ENVIAR tensão para o LED, não receber
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);  // Liga LED (5V no pino)
  // HIGH = 5V, LOW = 0V
  
  delay(1000);  // Espera 1000 milissegundos (1 segundo)
  // Por quê? Para vermos o LED ligado antes de desligar
  
  digitalWrite(LED_PIN, LOW);   // Desliga LED (0V no pino)
  delay(1000);  // Espera 1 segundo com LED desligado
}
```

**Aplicação real**: Semáforos, indicadores luminosos, pisca-alerta.

### **PROJETO 2: Leitura de Sensor (Botão)**

```cpp
const int BUTTON_PIN = 2;  // Pino do botão
const int LED_PIN = 13;    // Pino do LED

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // Botão como entrada com resistor interno
  pinMode(LED_PIN, OUTPUT);
  
  Serial.begin(9600);  // Inicia comunicação serial
  // 9600 é a velocidade (baud rate) - bits por segundo
  // Por quê? Para enviar dados ao computador
}

void loop() {
  int buttonState = digitalRead(BUTTON_PIN);  // Lê o botão
  // digitalRead retorna HIGH ou LOW
  
  // Botão com INPUT_PULLUP: HIGH = solto, LOW = pressionado
  // Por quê? O resistor pull-up mantém o pino em 5V quando aberto
  
  if (buttonState == LOW) {  // Botão pressionado
    digitalWrite(LED_PIN, HIGH);
    Serial.println("Botão pressionado!");  // Envia mensagem ao PC
  } else {
    digitalWrite(LED_PIN, LOW);
  }
  
  delay(50);  // Pequeno delay para estabilidade
  // Evita leituras instáveis devido a ruídos elétricos
}
```

**Aplicação real**: Interruptores, sensores de porta, teclados.

### **PROJETO 3: Leitura Analógica (Potenciômetro)**

```cpp
const int POT_PIN = A0;     // Pino analógico do potenciômetro
const int LED_PIN = 9;      // Pino PWM para controle de brilho

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  // Lê valor analógico (0 a 1023)
  int sensorValue = analogRead(POT_PIN);
  // Por quê 0-1023? O conversor analógico-digital tem 10 bits (2^10 = 1024)
  
  // Mapeia de 0-1023 para 0-255 (PWM)
  int brightness = map(sensorValue, 0, 1023, 0, 255);
  // map() é uma função que escala valores entre faixas diferentes
  
  analogWrite(LED_PIN, brightness);  // Controla brilho do LED
  // analogWrite usa PWM para simular tensão variável
  
  // Mostra valores no Serial Monitor
  Serial.print("Sensor: ");
  Serial.print(sensorValue);
  Serial.print(" - Brilho: ");
  Serial.println(brightness);
  
  delay(100);
}
```

**Por que PWM?** O Arduino não pode gerar tensões intermediárias (só 0V ou 5V). PWM liga e desliga tão rápido que o olho humano percebe como brilho intermediário.

**Aplicação real**: Controle de volume, dimmers de luz, sensores de posição.

---

## **PARTE 4: CONCEITOS AVANÇADOS E BOAS PRÁTICAS**

### 4.1 Debounce de Botões (Eliminando Ruídos)

```cpp
const int BUTTON_PIN = 2;
int lastState = HIGH;
unsigned long lastDebounceTime = 0;
const unsigned long debounceDelay = 50;  // 50ms

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  Serial.begin(9600);
}

void loop() {
  int reading = digitalRead(BUTTON_PIN);
  
  // Se o estado mudou, reinicia o timer de debounce
  if (reading != lastState) {
    lastDebounceTime = millis();  // millis() conta milissegundos desde o início
  }
  
  // Se passou tempo suficiente desde a última mudança
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // Agora sim, podemos considerar a leitura estável
    if (reading == LOW) {
      Serial.println("Botão pressionado (confirmado)");
    }
  }
  
  lastState = reading;
}
```

**Por que isso é necessário?** Botões mecânicos "quicam" fisicamente, gerando múltiplas leituras falsas em milissegundos.

### 4.2 Interrupções (Atenção Imediata)

```cpp
const int INTERRUPT_PIN = 2;  // Pino 2 suporta interrupção (INT0)
volatile bool buttonPressed = false;
// volatile: avisa ao compilador que a variável pode mudar a qualquer momento

void setup() {
  pinMode(INTERRUPT_PIN, INPUT_PULLUP);
  Serial.begin(9600);
  
  // Configura interrupção no pino 2
  attachInterrupt(digitalPinToInterrupt(INTERRUPT_PIN), 
                  buttonISR, 
                  FALLING);
  // FALLING = quando o sinal cai de HIGH para LOW
  // ISR = Interrupt Service Routine
}

void loop() {
  if (buttonPressed) {
    Serial.println("Interrupção detectada!");
    buttonPressed = false;  // Reseta a flag
  }
  // O loop continua fazendo outras coisas...
}

void buttonISR() {
  buttonPressed = true;  // Seta a flag
  // NÃO use Serial, delay ou outras funções demoradas dentro da ISR!
}
```

**Por que usar interrupções?** Permite que o Arduino responda instantaneamente a eventos, mesmo se estiver ocupado com outras tarefas.

---

## **PARTE 5: PROJETOS INTEGRADOS COMPLETOS**

### **PROJETO 4: Estação Meteorológica Simplificada**

```cpp
#include <LiquidCrystal.h>  // Biblioteca para display LCD

// Componentes
const int LM35_PIN = A0;     // Sensor de temperatura
const int LDR_PIN = A1;       // Sensor de luz (fotoresistor)
const int LED_PIN = 9;        // LED indicador

// Inicializa LCD (pinos: RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Variáveis para médias móveis (filtro)
float tempReadings[10];
int readIndex = 0;
float tempTotal = 0;
float tempAverage = 0;

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  
  // Configura LCD (16x2)
  lcd.begin(16, 2);
  lcd.print("Estacao Meteo");
  lcd.setCursor(0, 1);
  lcd.print("Iniciando...");
  delay(2000);
  
  // Inicializa array de temperaturas
  for (int i = 0; i < 10; i++) {
    tempReadings[i] = 0;
  }
}

void loop() {
  // === LEITURA DA TEMPERATURA (LM35) ===
  int tempRaw = analogRead(LM35_PIN);
  // LM35: 10mV por °C, referência 5V = 5000mV
  // Cada unidade analógica = 5000mV/1024 = 4.88mV
  // Temperatura = (tensão em mV) / 10
  float temperature = (tempRaw * 5000.0 / 1024.0) / 10.0;
  
  // === FILTRO DE MÉDIA MÓVEL ===
  // Remove ruídos e estabiliza a leitura
  tempTotal = tempTotal - tempReadings[readIndex];
  tempReadings[readIndex] = temperature;
  tempTotal = tempTotal + tempReadings[readIndex];
  readIndex = (readIndex + 1) % 10;
  tempAverage = tempTotal / 10;
  
  // === LEITURA DE LUMINOSIDADE ===
  int lightLevel = analogRead(LDR_PIN);
  // Quanto maior o valor, mais escuro (resistor pull-down)
  int brightness = map(lightLevel, 0, 1023, 255, 0);
  analogWrite(LED_PIN, brightness);  // LED acende no escuro
  
  // === EXIBIÇÃO NO LCD ===
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(tempAverage, 1);  // 1 casa decimal
  lcd.print(" C");
  
  lcd.setCursor(0, 1);
  lcd.print("Luz: ");
  if (lightLevel < 100) lcd.print("Muito Clara");
  else if (lightLevel < 300) lcd.print("Clara");
  else if (lightLevel < 600) lcd.print("Media");
  else if (lightLevel < 800) lcd.print("Escura");
  else lcd.print("Muito Escura");
  
  // === LOG NO SERIAL ===
  Serial.print("Temperatura: ");
  Serial.print(tempAverage);
  Serial.print(" C, Luz: ");
  Serial.println(lightLevel);
  
  delay(1000);  // Atualiza a cada segundo
}
```

**Explicação do código:**
- **Média móvel**: Suaviza leituras evitando variações bruscas
- **Mapeamento**: Converte a leitura do LDR em controle de brilho
- **LCD**: Mostra informações em tempo real

---

## **PARTE 6: COMUNICAÇÃO E INTEGRAÇÃO**

### 6.1 Comunicação Serial com Python

**Arduino (código):**
```cpp
void setup() {
  Serial.begin(9600);
}

void loop() {
  if (Serial.available() > 0) {
    char command = Serial.read();
    
    switch(command) {
      case 'L':
        digitalWrite(13, HIGH);
        Serial.println("LED LIGADO");
        break;
      case 'D':
        digitalWrite(13, LOW);
        Serial.println("LED DESLIGADO");
        break;
      default:
        Serial.println("Comando invalido");
    }
  }
}
```

**Python (controle):**
```python
import serial
import time

# Conecta ao Arduino (ajuste a porta)
arduino = serial.Serial('COM3', 9600, timeout=1)
time.sleep(2)  # Aguarda inicialização

def send_command(cmd):
    arduino.write(cmd.encode())
    response = arduino.readline().decode().strip()
    return response

# Exemplo de uso
print(send_command('L'))  # Liga LED
time.sleep(2)
print(send_command('D'))  # Desliga LED

arduino.close()
```

**Por que isso é útil?** Permite criar interfaces gráficas, integrar com internet, processar dados no computador.

---

## **PARTE 7: SOLUÇÃO DE PROBLEMAS E DEPURAÇÃO**

### 7.1 Técnicas de Debug

```cpp
void debug(String message, int value) {
  Serial.print(" [DEBUG] ");
  Serial.print(message);
  Serial.print(": ");
  Serial.println(value);
}

void setup() {
  Serial.begin(9600);
  debug("Inicio do programa", 0);
}

void loop() {
  static int counter = 0;  // Variável estática mantém valor entre chamadas
  counter++;
  
  debug("Contador", counter);
  
  if (counter > 100) {
    Serial.println("=== REINICIANDO ===");
    counter = 0;
  }
  
  delay(100);
}
```

### 7.2 Problemas Comuns e Soluções

| Problema | Possível Causa | Solução |
|----------|---------------|---------|
| LED não acende | Pino errado | Verificar pinMode e conexões |
| Leituras instáveis | Ruído elétrico | Adicionar capacitor, usar filtro |
| Arduino reinicia | Falta de energia | Usar fonte externa |
| Programa não carrega | Porta errada | Verificar porta COM no IDE |

---

## **PARTE 8: OTIMIZAÇÕES E EFICIÊNCIA**

### 8.1 Economia de Energia (Modo Sleep)

```cpp
#include <avr/sleep.h>
#include <avr/power.h>

void setup() {
  pinMode(2, INPUT_PULLUP);  // Pino para acordar
  Serial.begin(9600);
}

void loop() {
  Serial.println("Indo dormir...");
  delay(100);
  
  enterSleep();  // Entra em modo de baixo consumo
  
  Serial.println("Acordei!");
  delay(1000);  // Faz algo antes de dormir de novo
}

void enterSleep() {
  set_sleep_mode(SLEEP_MODE_PWR_DOWN);
  sleep_enable();
  
  // Desliga módulos não usados
  power_adc_disable();
  power_spi_disable();
  power_twi_disable();
  
  // Configura interrupção para acordar
  attachInterrupt(digitalPinToInterrupt(2), wakeUp, LOW);
  
  sleep_mode();  // Dorme até a interrupção
  
  // Ao acordar, continua daqui
  sleep_disable();
  power_all_enable();
  detachInterrupt(digitalPinToInterrupt(2));
}

void wakeUp() {
  // Só precisa acordar, não fazer nada
}
```

**Por que isso importa?** Para projetos com bateria, cada microampère conta.

---

## **PARTE 9: MAPEAMENTO DE PINOS E LIMITAÇÕES**

### Tabela de Funções Especiais (Arduino Uno)

| Pino | Função Especial | Observação |
|------|----------------|------------|
| 0 | RX | Serial - não usar se usar Serial |
| 1 | TX | Serial - não usar se usar Serial |
| 2,3 | Interrupções | Resposta rápida a eventos |
| 3,5,6,9,10,11 | PWM | Controle analógico |
| 10,11,12,13 | SPI | Comunicação rápida |
| A4,A5 | I2C | Comunicação com sensores |

---

## **PARTE 10: PROJETO FINAL - AUTOMAÇÃO RESIDENCIAL**

```cpp
// Sistema de Automação Residencial Completo
#include <Wire.h>
#include <LiquidCrystal.h>

// Definições de pinos
const int LDR_PIN = A0;
const int TEMP_PIN = A1;
const int PIR_PIN = 2;      // Sensor de movimento
const int LED_PIN = 9;
const int FAN_PIN = 10;      // Controle de ventilador (PWM)
const int BUZZER_PIN = 11;   // Alarme

// Limiares
const int TEMP_LIMIT = 30;   // Liga ventilador acima de 30°C
const int LIGHT_LIMIT = 500; // Acende luz no escuro

// Estados
enum SystemState {
  NORMAL,
  ALARM,
  ECONOMY
};

SystemState currentState = NORMAL;
unsigned long lastMotionTime = 0;
const unsigned long AUTO_OFF_DELAY = 300000; // 5 minutos

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  Serial.begin(9600);
  
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  pinMode(FAN_PIN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  
  lcd.begin(16, 2);
  lcd.print("Smart Home v1.0");
  delay(2000);
}

void loop() {
  // Leitura dos sensores
  int light = analogRead(LDR_PIN);
  float temp = readTemperature();
  int motion = digitalRead(PIR_PIN);
  
  // Lógica de estados
  switch(currentState) {
    case NORMAL:
      handleNormalMode(light, temp, motion);
      break;
    case ALARM:
      handleAlarmMode();
      break;
    case ECONOMY:
      handleEconomyMode();
      break;
  }
  
  // Display informações
  updateDisplay(temp, light, motion);
  
  // Comunicação serial para monitoramento
  sendDataToSerial(temp, light, motion);
  
  delay(500);
}

float readTemperature() {
  int raw = analogRead(TEMP_PIN);
  return (raw * 5000.0 / 1024.0) / 10.0;
}

void handleNormalMode(int light, float temp, int motion) {
  // Controle automático de luz
  if (light > LIGHT_LIMIT) {
    analogWrite(LED_PIN, 255);  // Luz máxima
  } else {
    analogWrite(LED_PIN, 50);    // Luz noturna
  }
  
  // Controle de temperatura
  if (temp > TEMP_LIMIT) {
    int fanSpeed = map(temp, TEMP_LIMIT, 50, 128, 255);
    fanSpeed = constrain(fanSpeed, 128, 255);
    analogWrite(FAN_PIN, fanSpeed);
  } else {
    analogWrite(FAN_PIN, 0);
  }
  
  // Detecção de movimento
  if (motion == HIGH) {
    lastMotionTime = millis();
    lcd.setCursor(0, 0);
    lcd.print("Movimento!      ");
  }
  
  // Verifica se precisa entrar em modo economia
  if (millis() - lastMotionTime > AUTO_OFF_DELAY) {
    currentState = ECONOMY;
  }
}

void handleAlarmMode() {
  // Modo alarme - pisca tudo e buzzer
  digitalWrite(BUZZER_PIN, HIGH);
  for(int i = 0; i < 3; i++) {
    digitalWrite(LED_PIN, HIGH);
    delay(200);
    digitalWrite(LED_PIN, LOW);
    delay(200);
  }
  digitalWrite(BUZZER_PIN, LOW);
}

void handleEconomyMode() {
  // Desliga tudo para economizar energia
  digitalWrite(LED_PIN, LOW);
  digitalWrite(FAN_PIN, LOW);
  
  // Verifica se alguém chegou
  if (digitalRead(PIR_PIN) == HIGH) {
    currentState = NORMAL;
    lastMotionTime = millis();
  }
}

void updateDisplay(float temp, int light, int motion) {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("T:");
  lcd.print(temp, 1);
  lcd.print("C L:");
  lcd.print(light);
  
  lcd.setCursor(0, 1);
  lcd.print("Modo:");
  switch(currentState) {
    case NORMAL:
      lcd.print("Normal");
      break;
    case ALARM:
      lcd.print("ALARME");
      break;
    case ECONOMY:
      lcd.print("Economia");
      break;
  }
}

void sendDataToSerial(float temp, int light, int motion) {
  Serial.print(temp);
  Serial.print(",");
  Serial.print(light);
  Serial.print(",");
  Serial.print(motion);
  Serial.print(",");
  Serial.println(currentState);
}
```

---

## **PARTE 11: GLOSSÁRIO E CONCEITOS IMPORTANTES**

### Termos Técnicos Explicados

| Termo | Significado | Analogia |
|-------|-------------|----------|
| **Pull-up** | Resistor que mantém pino em HIGH | Corda que mantém uma porta fechada |
| **PWM** | Modulação por largura de pulso | Piscar muito rápido para simular intensidade |
| **ADC** | Conversor analógico-digital | Tradutor que converte volts em números |
| **ISR** | Rotina de serviço de interrupção | Atendente que para tudo para atender uma emergência |
| **Baud Rate** | Velocidade de comunicação serial | Taxa de palavras por minuto |
| **Volátil** | Variável que pode mudar fora do programa | Placa de "em obras" que muda sem aviso |

---

## **PARTE 12: EXERCÍCIOS PRÁTICOS**

### Nível 1: Básico
1. Modifique o pisca-LED para piscar em padrão SOS (··· --- ···)
2. Crie um semáforo com 3 LEDs
3. Leia um potenciômetro e mostre o valor no Serial Monitor

### Nível 2: Intermediário
1. Controle a velocidade de um motor DC com potenciômetro
2. Crie um termostato que liga um LED quando a temperatura passa de um limite
3. Implemente um contador de pessoas usando sensor PIR

### Nível 3: Avançado
1. Sistema de irrigação automática com sensor de umidade
2. Fechadura eletrônica com teclado matricial
3. Estação meteorológica com armazenamento em cartão SD

