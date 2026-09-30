# このファイルについて
- ESPでのWiFiを用いたOTAをする
# 注意事項！
- 僕がやった感じ、学校のWi-Fiではできなかったので、スマホなどからインターネット共有してやるかしよう
# やりかた
- USBでESP３２を接続し、書き込みする。
```code
#include <WiFi.h>
#include <ArduinoOTA.h>

// ====================================================
// 【設定】ご自宅やスマホテザリングのWi-Fi情報を入力してください
// ====================================================
const char* ssid = "YOUR_WIFI_SSID";          // Wi-FiのSSID（ネットワーク名）
const char* password = "YOUR_WIFI_PASSWORD";  // Wi-Fiのパスワード

void setup() {
  Serial.begin(115200);
  delay(10);

  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  // Wi-Fi子機（STA）モードに設定して接続開始
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  // Wi-Fiに接続できるまで待機
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  // ----------------------------------------------------
  // ArduinoOTA（無線書き込み）の設定
  // ----------------------------------------------------
  ArduinoOTA.setHostname("esp32-home-device"); // ネットワーク上の表示名

  // OTA更新開始時の処理
  ArduinoOTA.onStart([]() {
    String type;
    if (ArduinoOTA.getCommand() == U_FLASH) {
      type = "sketch";
    } else {
      type = "filesystem";
    }
    Serial.println("Start updating " + type);
  });

  // OTA更新完了時の処理
  ArduinoOTA.onEnd([]() {
    Serial.println("\nEnd update");
  });

  // OTA更新中の進捗表示（%）
  ArduinoOTA.onProgress([](unsigned int progress, unsigned int total) {
    Serial.printf("Progress: %u%%\r", (progress / (total / 100)));
  });

  // エラー発生時の処理
  ArduinoOTA.onError([](ota_error_t error) {
    Serial.printf("Error[%u]: ", error);
  });

  // OTA機能を開始
  ArduinoOTA.begin();
  Serial.println("OTA Ready");
}

void loop() {
  // OTAの更新リクエストを監視（★この1行は必ず残す）
  ArduinoOTA.handle();

  // ----------------------------------------------------
  // ここに実行したい処理（LED点滅やセンサー取得など）を書く
  // ----------------------------------------------------
}
```
成功すると、シリアルモニター(115200)で、
```code
WiFi connected!

IP Address: xxx.xxx.xxx.xxx

OTA Ready
```
のように出ればOK。

- IDE側の設定をする
IDEの上のタブから、ツール＞Port　を確認し、Networkポートが出ていることを確認し、それをクリック。
- ボードとポートのセレクト画面に、WiFiマークの出たマイコン名と、IPv4が出ているやつをクリック。
- OTAで書き込む

```code
#include <WiFi.h>
#include <ArduinoOTA.h>

const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";

// ----------------------------------------------------
// 【追加】自分で定義した変数やピン設定など
// ----------------------------------------------------
const int LED_PIN = 2; // 例: 内蔵LEDピン

void setup() {
  Serial.begin(115200);

  // ① ピン設定などの一瞬で終わる初期化
  pinMode(LED_PIN, OUTPUT);

  // ② Wi-Fi接続（最初に繋ぐ。変更しない）
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nWiFi connected!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  // ③ OTAの開始
  ArduinoOTA.setHostname("esp32-home-device");
  ArduinoOTA.begin();
  Serial.println("OTA Ready");

  // ④ 時間のかかる初期化処理や for ループはここで行う
}

void loop() {
  // ★これ（OTA待機処理）は絶対に消さない！
  ArduinoOTA.handle();

  // ----------------------------------------------------
  // 【追加】ここにメインで動かしたい処理を書く
  // ----------------------------------------------------
  digitalWrite(LED_PIN, HIGH);
  delay(1000);
  digitalWrite(LED_PIN, LOW);
  delay(1000);
}
```
これでOK
