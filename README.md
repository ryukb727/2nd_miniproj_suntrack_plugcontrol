
| [Korean 🇰🇷](#korean) | [Japanese 🇯🇵](#japanese) |
| :---: | :---: |

</div>

---

<div id="korean">

### 🇰🇷 Korean Version

# 🌞 빛 추적 스마트 전력 제어 시스템  
**Smart Solar Tracking & Power Control System**
<img width="1840" height="777" alt="Image" src="https://github.com/user-attachments/assets/3232f946-62a0-4f54-866c-43c3ea1adb5a" />
<img width="2308" height="685" alt="Image" src="https://github.com/user-attachments/assets/335bddba-67d5-42a7-bbe8-9c78f891ad86" />
---

## 💡 1. 프로젝트 개요
빛 추적 스마트 전력 제어 시스템은 빛 추적 태양패널로 발전 효율을 높이고 스마트 콘센트로 전력을 제어하는 IoT 기반 에너지 관리 프로젝트입니다.

기존 고정형 태양광 패널은 태양의 이동이나 주변 장애물에 따른 조도 변화에 대응하지 못해 발전 효율이 떨어집니다.  
이를 해결하기 위해 **CDS 센서 기반 태양광 추적** 기능을 구현하는 한편, 효과적으로 에너지를 관리할 수 있도록 패널 관련 정보 **실시간 서버 저장**, **스마트 콘센트 제어**, **LCD 시각화 UI** 기능을 포함하는 통합 에너지 관리 시스템을 구축했습니다.


### 📍 전체 시스템 구성
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/43fa549c-5ee2-49ef-9705-b3aec7b91486" />

- #### **STM32 Nucleo-F411RE**
  - 8개의 CDS 센서 입력
  - 태양광 패널 자동 회전(스텝 모터)
  - 태양광 패널 전압 측정
  - Wi-Fi(ESP 모듈)로 Raspberry Pi 서버에 데이터 송신

- #### **Raspberry Pi 5 (서버)**
  - 소켓 서버(iot_server.c)
  - MariaDB 데이터 저장
  - Bluetooth로 Arduino와 데이터 송수신

- #### **Arduino UNO**
  - 패널 방향 / 전압 / 콘센트 상태 LCD 출력
  - 사용자 콘센트 ON/OFF 제어(릴레이)
  - Bluetooth로 명령 수신


### 🔗 통신 구조
- STM32 ↔ **Wi-Fi** ↔ Raspberry Pi  
- Arduino ↔ **Bluetooth** ↔ Raspberry Pi

---

## 🛠 2. 기술 스택

### Hardware  
![STM32](https://img.shields.io/badge/MCU-STM32F411RE-03234B?style=for-the-badge&logo=stmicroelectronics&logoColor=white)
![Arduino](https://img.shields.io/badge/Board-Arduino%20UNO-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![RaspberryPi](https://img.shields.io/badge/SBC-Raspberry%20Pi%205-C51A4A?style=for-the-badge&logo=raspberrypi&logoColor=white)
![WiFi](https://img.shields.io/badge/Module-ESP%20WiFi-1E90FF?style=for-the-badge)
![Bluetooth](https://img.shields.io/badge/Wireless-Bluetooth-3A75C4?style=for-the-badge&logo=bluetooth&logoColor=white)
![LCD](https://img.shields.io/badge/Display-I2C%20LCD-1E90FF?style=for-the-badge)
![Relay](https://img.shields.io/badge/Output-Relay%20Module-FFB400?style=for-the-badge)

### Software / Languages  
![C](https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c&logoColor=white)
![MariaDB](https://img.shields.io/badge/DB-MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Linux](https://img.shields.io/badge/Server-Linux%20Socket%20Programming-333333?style=for-the-badge)
![Bluetooth](https://img.shields.io/badge/Protocol-Bluetooth-3A75C4?style=for-the-badge)
![I2C](https://img.shields.io/badge/Bus-I2C-1E90FF?style=for-the-badge)
![UART](https://img.shields.io/badge/Bus-UART-FF5722?style=for-the-badge)
![ADC](https://img.shields.io/badge/Input-ADC%20Sensors-A2C93A?style=for-the-badge)

---

## ⭐ 3. 주요 기능

### 1) STM32 — 태양광 패널 자동 추적 및 센서 데이터 송신
- 8개 CDS 센서로 빛의 강도 측정
- 가장 밝은 방향으로 패널 자동 회전
- 태양광 패널 전압 측정
- Wi-Fi(ESP 모듈)로 Raspberry Pi에 실시간 정보 송신
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/78693961-5826-4d20-9175-7829c63f153f" />

### 2) Raspberry Pi 5 — IoT 서버 + 데이터베이스 저장
- **iot_server.c**
  - 멀티 클라이언트 소켓 연결
  - ID/PW 기반 인증 처리
  - 메시지 라우팅 (ALLMSG, 특정 ID 전송)

- **sql_client.c**
  - “[LT_STM_SQL]SENSOR@…” 패킷 파싱
  - MariaDB(sensor 테이블)에 실시간 INSERT

- **bt_client.c**
  - Bluetooth HC-06 모듈 사용
  - Arduino에 명령 전달
  - Arduino 상태 재수신하여 서버로 전달
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/df6e9a35-0a65-4247-82fc-e0819f64ae56" />
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/64098ebe-fb36-40ef-9fb6-705e0fad388c" />

### 3) Arduino UNO — LCD UI + 스마트 콘센트 제어
- Raspberry Pi → Bluetooth로 데이터 수신
- LCD에 패널 방향, 전압, 콘센트 상태 출력
- 사용자 버튼 입력으로 콘센트 ON/OFF
- 릴레이 제어 + 상태를 Raspberry Pi로 다시 송신
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/0c44df98-3c01-47f0-9a35-7bc1c6404dd3" />

---

## 👨‍💻 4. 담당 역할

### 1) Raspberry Pi **클라이언트 코드(iot_client 계열)** 개발
- 서버와 통신하는 iot_client 클라이언트 프로그램 구현
- 메시지 송·수신 스레드 기반 구조(send_msg / recv_msg) 분석 및 개선
- 사용자 입력 기반 제어 명령 송신 기능 개발
- Bluetooth → Arduino로 전달되는 메시지 흐름 검증
- 패킷 포맷( [ID]MSG ) 구조 이해 및 통신 테스트 수행

### 2) Arduino UNO — **UI 출력 & 사용자 조작 로직 전체 구현**

#### ① LCD 출력 시스템 구현
- 패널 방향 / 전압 / 콘센트 상태 실시간 표시
- **Bluetooth (HC-06)** 메시지 기반 UI 갱신
- 데이터 갱신 시 LCD 잔상 제거 및 부분 업데이트 처리

#### ② Bluetooth (HC-06) 메시지 처리
- Raspberry Pi에서 전송되는 문자열 명령 파싱
- 상태 변화 반영 후 즉시 LCD 업데이트
- 연결 오류, 데이터 깨짐 대응 로직 구성

#### ③ 릴레이 기반 스마트 콘센트 제어
- 사용자 버튼 입력 처리
- 콘센트 ON/OFF 동작 제어
- 상태를 다시 Raspberry Pi에 전송해 DB 갱신 흐름 유지

#### ④ 전체 제어 파이프라인 구축
- "수신 → 처리 → LCD 표시 → 릴레이 제어 → 상태 재전송" 전체 흐름 개발  
- Arduino를 사용자 인터페이스 핵심 장치로 완성

---

## 🎯 5. 트러블슈팅
### 1) **데이터 송신 중 스텝 모터 끊김 현상**  
- 원인: Wi-Fi 송신(ESP) 루틴이 블로킹으로 동작  
- 해결: **TIM4 기반 논블로킹(non-blocking)방식 스텝 모터 제어 구조 구현**  
- 결과: 모터 회전 안정성 확보  

### 2) **스텝 모터 회전 시 전압 강하 → CDS 값 일시적 하락**  
- 원인: 모터 구동 시 순간적인 전류 피크 → ADC 레일 변동  
- 해결: **모터 전원과 MCU/센서 전원을 완전 분리**  
- 결과: CDS 값 일시적 하락 현상 해결  

### 3) **LCD 잔상(ghosting) 발생** 
- 원인: 문자열 길이 변화로 이전 텍스트 일부가 남음  
- 해결:  
  - `lcd.clear()` 반복 호출 지양  
  - **partial update 구조 도입 (변경된 라인만 업데이트)**  
  - 공백 패딩으로 잔상 제거  
- 결과: LCD 화면이 깨끗하게 유지되고 출력 속도 향상  

---

## 📚 6. 배운 점  
- Arduino UI 설계 및 장치 제어(릴레이) 전체 구현 경험  
- 서버 → 클라이언트 → MCU로 이어지는 IoT 데이터 흐름의 구조적 이해  
- 이기종 디바이스 간 통신 흐름을 직접 구축하며 IoT 시스템 아키텍처 감각 향상  
- 센서, 서버, UI를 통합하는 과정에서 실제 하드웨어 디버깅 능력 강화  

---

<div align="center">
<a href="#japanese">⬇️ 日本語バージョンへ移動 (Go to Japanese Version) ⬇️</a>
</div>

</div>

---

<div id="japanese">

### 🇯🇵 Japanese Version

# 🌞 太陽光追跡スマート電力制御システム
Smart Solar Tracking & Power Control System

<img width="1840" height="777" alt="Image" src="https://github.com/user-attachments/assets/3232f946-62a0-4f54-866c-43c3ea1adb5a" />
<img width="2308" height="685" alt="Image" src="https://github.com/user-attachments/assets/335bddba-67d5-42a7-bbe8-9c78f891ad86" />

## 💡 1. プロジェクト概要

本プロジェクトは、太陽光発電の効率向上と電力制御機能を組み合わせたIoTベースのエネルギー管理システムです。

従来の固定型太陽光パネルは、太陽の移動や周辺の障害物による照度変化に対応できず、発電効率が低下するという課題がありました。  
これを解決するため、**CDSセンサーに基づく太陽光追跡機能**を実装するとともに、エネルギーを効率的に管理するための機能として、パネル関連情報の**リアルタイムサーバー保存、スマートプラグ制御、LCDによる可視化UI**を含む統合エネルギー管理システムを構築しました。

### 📍 システム全体構成
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/43fa549c-5ee2-49ef-9705-b3aec7b91486" />

- **STM32 Nucleo-F411RE**
  - 8つのCDSセンサー入力
  - 太陽光パネルの自動回転（ステップモーター）
  - 太陽光パネルから電圧測定
  - Wi-Fi（ESPモジュール）経由でのRaspberry Piサーバーへのデータ送信

- **Raspberry Pi 5 (サーバー)**
  - ソケットサーバー（iot_server.c）
  - MariaDBへのデータ保存
  - Bluetooth経由でのArduinoとのデータ送受信

- **Arduino UNO**
  - パネル方向 / 電圧 / プラグ状態のLCD表示
  - ユーザーによるプラグON/OFF制御（リレー）
  - Bluetooth経由でのコマンド受信

### 🔗 通信構造 (Communication Structure)
- STM32 ↔ **Wi-Fi** ↔ Raspberry Pi
- Arduino ↔ **Bluetooth** ↔ Raspberry Pi

---

## 🛠 2. 技術スタック

### Hardware  
![STM32](https://img.shields.io/badge/MCU-STM32F411RE-03234B?style=for-the-badge&logo=stmicroelectronics&logoColor=white)
![Arduino](https://img.shields.io/badge/Board-Arduino%20UNO-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![RaspberryPi](https://img.shields.io/badge/SBC-Raspberry%20Pi%205-C51A4A?style=for-the-badge&logo=raspberrypi&logoColor=white)
![WiFi](https://img.shields.io/badge/Module-ESP%20WiFi-1E90FF?style=for-the-badge)
![Bluetooth](https://img.shields.io/badge/Wireless-Bluetooth-3A75C4?style=for-the-badge&logo=bluetooth&logoColor=white)
![LCD](https://img.shields.io/badge/Display-I2C%20LCD-1E90FF?style=for-the-badge)
![Relay](https://img.shields.io/badge/Output-Relay%20Module-FFB400?style=for-the-badge)

### Software / Languages  
![C](https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c&logoColor=white)
![MariaDB](https://img.shields.io/badge/DB-MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Linux](https://img.shields.io/badge/Server-Linux%20Socket%20Programming-333333?style=for-the-badge)
![Bluetooth](https://img.shields.io/badge/Protocol-Bluetooth-3A75C4?style=for-the-badge)
![I2C](https://img.shields.io/badge/Bus-I2C-1E90FF?style=for-the-badge)
![UART](https://img.shields.io/badge/Bus-UART-FF5722?style=for-the-badge)
![ADC](https://img.shields.io/badge/Input-ADC%20Sensors-A2C93A?style=for-the-badge)

---

## ⭐ 3. 主要機能

### 1) STM32 — 太陽光パネルの自動追跡およびセンサーデータ送信
- 8つのCDSセンサーによる光の強度測定
- 最も明るい方向へのパネル自動回転
- 太陽光パネルから電圧測定
- Wi-Fi（ESPモジュール）を経由したRaspberry Piへのリアルタイム送信

<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/78693961-5826-4d20-9175-7829c63f153f" />

### 2) Raspberry Pi 5 — IoTサーバー + データベース保存
- **iot_server.c**
  - マルチクライアントソケット接続
  - ID/PWベースの認証処理
  - メッセージルーティング (ALLMSG、特定IDへの送信)

- **sql_client.c**
  - 「[LT_STM_SQL]SENSOR@…」パケットのパース
  - MariaDB（sensorテーブル）へのリアルタイムINSERT

- **bt_client.c**
  - Bluetooth HC-06モジュールの使用
  - Arduinoへのコマンド転送
  - Arduinoの状態を再受信しサーバーへ転送

<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/df6e9a35-0a65-4247-82fc-e0819f64ae56" />
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/64098ebe-fb36-40ef-9fb6-705e0fad388c" />

### 3) Arduino UNO — LCD UI + スマートプラグ制御
- Raspberry Pi → Bluetoothからデータを受信
- LCDにパネル方向、電圧、プラグ状態を表示
- ユーザーボタン入力によるプラグのON/OFF
- リレー制御 + 状態をRaspberry Piに返信しDB更新フローを維持

<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/0c44df98-3c01-47f0-9a35-7bc1c6404dd3" />

---

## 👨‍💻 4. 担当役割と貢献

### 1) Raspberry Pi クライアントコード(iot_client系) 開発
- サーバーと通信するiot_clientクライアントプログラムの実装
- メッセージ送受信のスレッドベース構造（send_msg / recv_msg）の分析と改善
- ユーザー入力に基づく制御コマンド送信機能の開発
- Bluetooth → Arduinoへ転送されるメッセージフローの検証
- パケットフォーマット（ [ID]MSG ）構造の理解および通信テストの実施

### 2) Arduino UNO — UI表示およびユーザー操作ロジック全体の実現
#### ① LCD表示システムの実装
- パネル方向 / 電圧 / プラグ状態のリアルタイム表示
- Bluetooth (HC-06) メッセージに基づくUI更新
- データ更新時のLCD残像除去および部分的な更新処理

#### ② Bluetooth (HC-06) メッセージ処理
- Raspberry Piから送信される文字列コマンドのパース（解析）
- 状態変化反映後、即時のLCD更新
- 接続エラー、データ破損への対応ロジック構成

#### ③ リレーに基づくスマートプラグ制御
- ユーザーボタン入力処理
- プラグのON/OFF動作制御
- 状態をRaspberry Piへ再送信し、DB更新フローを維持

#### ④ 全体制御パイプラインの構築
- 「受信 → 処理 → LCD表示 → リレー制御 → 状態再送信」という全体のフローを開発
- Arduinoをユーザーインターフェースの中核デバイスとして完成

---

## 🎯 5. トラブルシューティング

### 1) データ送信中に発生したステップモーター停止現象
- 原因: Wi-Fi送信（ESP）ルーチンがブロッキングで動作していたため
- 解決策: TIM4に基づくノンブロッキングステップモーター制御構造の実装
- 結果: モーター回転の安定性を確保

### 2) ステップモーター回転時の電圧降下 → CDS値の一時的な低下
- 原因: モーター駆動時の瞬間的な電流ピーク → ADCレールの変動
- 解決策: モーター電源とMCU/センサー電源を完全に分離
- 結果: CDS値の一時的な低下現象を解決

### 3) LCD残像（ゴースト）の発生
- 原因: 文字列の長さ変化により、以前のテキストの一部が残存
- 解決策:
  - lcd.clear()の繰り返し呼び出しを避ける
  - 部分更新構造の導入（変更されたラインのみを更新）
  - 空白パディングによる残像除去
- 結果: LCD画面がクリアに保たれ、表示速度が向上

---

## 📚 6. 学んだこと

- Arduino UI設計およびデバイス制御（リレー）の全体的な実装経験
- サーバー → クライアント → MCUへとつながるIoTデータフローの構造的な理解
- 異機種デバイス間の通信フローを自ら構築することで、IoTシステムアーキテクチャの感覚が向上
- センサー、サーバー、UIを統合する過程で、実際のハードウェアデバッグ能力が強化

---

<div align="center">
<a href="#korean">⬆️ 한국어 버전으로 돌아가기 (Go back to Korean Version) ⬆️</a>
</div>

</div>

---
