#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>

#define RELAY_PIN 5   // D1 = GPIO5

const char* ssid = "TU-INTERNET";
const char* password = "TU-CONTRASEÑA";

#define BOT_TOKEN "TOKEN-BOT"
#define CHAT_ID   "TU-ID"

WiFiClientSecure client;
UniversalTelegramBot bot(BOT_TOKEN, client);

unsigned long lastTime = 0;

void setup() {
  Serial.begin(115200);

  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH);   // relé apagado y estable

  WiFi.begin(ssid, password);
  client.setInsecure();
  client.setTimeout(20000);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nWiFi conectado");
  bot.sendMessage(CHAT_ID, "ESP listo 💻", "");
}

void handleNewMessages(int n) {
  for (int i = 0; i < n; i++) {
    String chat_id = bot.messages[i].chat_id;
    String text = bot.messages[i].text;

    Serial.print("Texto: ");
    Serial.println(text);

    if (chat_id != CHAT_ID) return;

    if (text.startsWith("/on")) {
      Serial.println("PULSO RELÉ");
      digitalWrite(RELAY_PIN, LOW);
      delay(2000);                 // ⬅️ pulso largo
      digitalWrite(RELAY_PIN, HIGH);
      bot.sendMessage(CHAT_ID, "PC prendida 🟢", "");
    }
  }
}

void loop() {
  int n = bot.getUpdates(bot.last_message_received + 1);
  if (n) handleNewMessages(n);
  delay(1000);
}
