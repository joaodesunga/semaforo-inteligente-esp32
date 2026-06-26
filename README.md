# semaforo-inteligente-esp32
Projeto de Sistemas Embarcados, 5 Periodo. (2026)
# Semáforo Inteligente com ESP32

Este projeto consiste em um **semáforo inteligente** desenvolvido com **ESP32**, que simula um sistema de travessia de pedestres com botão de acionamento e sinal sonoro (buzzer).

- Funcionalidades

* Estado inicial com sinal **verde ativo**
* Botão para solicitação de travessia
* Transição automática:

  * Verde → Amarelo → Vermelho
* Sinal sonoro para acessibilidade
* LED vermelho piscando durante a travessia
* Retorno automático ao estado inicial

---

- Componentes Utilizados

* ESP32
* 3 LEDs (Verde, Amarelo, Vermelho)
* 1 Botão
* 1 Buzzer
* Resistores
* Protoboard e jumpers

---

-  Pinagem

| Componente   | Pino |
| ------------ | ---- |
| LED Verde    | 21   |
| LED Amarelo  | 22   |
| LED Vermelho | 23   |
| Botão        | 19   |
| Buzzer       | 18   |

---

- ⚙️ Funcionamento

1. O sistema inicia com o LED **verde ligado**
2. Ao pressionar o botão:

   * Um **bip sonoro** é emitido
   * O sinal muda para **amarelo** por 4 segundos
   * Em seguida, o **vermelho pisca por 10 segundos**
   * Durante o vermelho, o buzzer emite som intermitente
3. Após o ciclo, o sistema retorna ao estado inicial (**verde ligado**)

---

- 💻 Código

```cpp
// Pinos
const int verde = 21;
const int amarelo = 22;
const int vermelho = 23;
const int botao = 19;
const int buzzer = 18;

void setup() {
  pinMode(verde, OUTPUT);
  pinMode(amarelo, OUTPUT);
  pinMode(vermelho, OUTPUT);

  pinMode(botao, INPUT_PULLUP);
  pinMode(buzzer, OUTPUT);

  // Estado inicial
  digitalWrite(verde, HIGH);
  digitalWrite(amarelo, LOW);
  digitalWrite(vermelho, LOW);
}

void loop() {

  // Aguarda o botão ser pressionado
  if (digitalRead(botao) == LOW) {

    // Bip de aviso
    tone(buzzer, 1000);
    delay(500);
    noTone(buzzer);

    // Evita múltiplos acionamentos
    delay(300);

    // Verde -> Amarelo
    digitalWrite(verde, LOW);
    digitalWrite(amarelo, HIGH);

    delay(4000); // 4 segundos

    // Amarelo -> Vermelho
    digitalWrite(amarelo, LOW);

    // Vermelho piscando por 10 segundos
    for (int i = 0; i < 20; i++) {

      digitalWrite(vermelho, HIGH);
      tone(buzzer, 2000);
      delay(250);

      digitalWrite(vermelho, LOW);
      noTone(buzzer);
      delay(250);
    }

    // Retorna ao verde
    digitalWrite(vermelho, LOW);
    digitalWrite(verde, HIGH);

    // Aguarda soltar o botão
    while (digitalRead(botao) == LOW) {
      delay(10);
    }
  }
}
```

---

-  Possíveis Melhorias

* Adicionar display OLED para status do sistema
* Integração com IoT (monitoramento remoto)
* Sensor de presença ao invés de botão
* Ajuste dinâmico de tempo (horário de pico)
* Uso de FreeRTOS para melhor gerenciamento de tarefas

---


Desenvolvido por @joaodesunga

---
