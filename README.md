# 🌞 빛 추적 스마트 전력 제어 시스템  
**Smart Solar Tracking & Power Control System**
![Image](https://github.com/user-attachments/assets/80a95308-9922-466e-92a1-ce06e72ec58f)
![Image](https://github.com/user-attachments/assets/df89fdf7-1ff2-40e6-a1a3-9b0afc427ea3)
<img width="1455" height="805" alt="Image" src="https://github.com/user-attachments/assets/b53b6157-e69a-470a-b9e5-282e7c4cb7c7" />
<img width="720" height="425" alt="Image" src="https://github.com/user-attachments/assets/ae98a460-ed6f-43a9-b95d-f49705d6f7d4" />
---

## 💡 1. 프로젝트 개요
빛 추적 스마트 전력 제어 시스템은 태양광 발전 효율을 높이고 전력 제어 기능을 결합한 IoT 기반 에너지 관리 프로젝트입니다.

기존 고정형 태양광 패널은 태양의 이동이나 주변 장애물에 따라 조도 변화에 대응하지 못해 발전 효율이 떨어집니다.  
이를 해결하기 위해 **CDS 센서 기반 태양광 추적**, **실시간 서버 저장**, **스마트 플러그 제어**, **LCD 시각화 UI** 기능을 하나의 시스템으로 구축했습니다.


### 📍 전체 시스템 구성
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/43fa549c-5ee2-49ef-9705-b3aec7b91486" />

#### **STM32 Nucleo-F411RE**
- 8개의 CDS 센서 입력
- 태양광 패널 자동 회전(스텝모터)
- Solar 센서 기반 발전량 측정
- Wi-Fi(ESP 모듈)로 Raspberry Pi 서버에 데이터 송신

#### **Raspberry Pi 5 (서버)**
- 소켓 서버(iot_server.c)
- MariaDB 데이터 저장
- **Bluetooth**로 Arduino와 데이터 송수신

#### **Arduino UNO**
- 패널 방향 / 발전량 / 플러그 상태 LCD 출력
- 사용자 플러그 ON/OFF 제어(릴레이)
- Bluetooth로로 명령 수신


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
- Solar 센서로 발전량(mV) 측정
- Wi-Fi(ESP 모듈)로 Raspberry Pi에 실시간 송신
- 센서 요청 명령에 따라 주기적 데이터 업데이트
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/78693961-5826-4d20-9175-7829c63f153f" />

### 2) Raspberry Pi 5 — IoT 서버 + 데이터베이스 저장
**iot_server.c**
- 멀티 클라이언트 소켓 연결
- ID/PW 기반 인증 처리
- 메시지 라우팅 (ALLMSG, 특정 ID 전송)

**sql_client.c**
- “[LT_STM_SQL]SENSOR@…” 패킷 파싱
- MariaDB(sensor 테이블)에 실시간 INSERT

**bt_client.c**
- Bluetooth HC-06 모듈 사용
- Arduino에 명령 전달
- Arduino 상태 재수신하여 서버로 전달
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/df6e9a35-0a65-4247-82fc-e0819f64ae56" />
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/64098ebe-fb36-40ef-9fb6-705e0fad388c" />

### 3) Arduino UNO — LCD UI + 스마트 플러그 제어
- Raspberry Pi → Bluetooth Classic으로 데이터 수신
- LCD에 패널 방향, 발전량, 플러그 상태 출력
- 사용자 버튼 입력으로 플러그 ON/OFF
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
- 패널 방향 / 발전량 / 플러그 상태 실시간 표시
- **Bluetooth (HC-06)** 메시지 기반 UI 갱신
- 데이터 갱신 시 LCD 잔상 제거 및 부분 업데이트 처리

#### ② Bluetooth (HC-06) 메시지 처리
- Raspberry Pi에서 전송되는 문자열 명령 파싱
- 상태 변화 반영 후 즉시 LCD 업데이트
- 연결 오류, 데이터 깨짐 대응 로직 구성

#### ③ 릴레이 기반 스마트 플러그 제어
- 사용자 버튼 입력 처리
- 플러그 ON/OFF 동작 제어
- 상태를 다시 Raspberry Pi에 전송해 DB 갱신 흐름 유지

#### ④ 전체 제어 파이프라인 구축
"수신 → 처리 → LCD 표시 → 릴레이 제어 → 상태 재전송" 전체 흐름 개발  
Arduino를 사용자 인터페이스 핵심 장치로 완성

---

## 🎯 5. 트러블슈팅
### 1) **데이터 송신 중 스텝모터 끊김 현상**  
- 원인: Wi-Fi 송신(ESP) 루틴이 블로킹으로 동작  
- 해결: **TIM4 기반 논블로킹 스텝모터 제어 구조 구현**  
- 결과: 모터 회전 안정성 확보  

### 2) **스텝모터 회전 시 전압 강하 → CDS 값 일시적 하락**  
- 원인: 모터 구동 시 순간적인 전류 피크 → ADC 레일 변동  
- 해결: **모터 전원과 MCU/센서 전원을 완전 분리**  
- 결과: CDS 값 드랍 현상 해결  

### 3) **LCD 잔상(ghosting) 발생** 
- 원인: 문자열 길이 변화로 이전 텍스트 일부가 남음  
- 해결:  
  - `lcd.clear()` 반복 호출 지  
  - **partial update 구조 도입 (변경된 라인만 업데이트)**  
  - 공백 패딩으로 잔상 제거  
- 결과: LCD 화면이 깨끗하게 유지되고 출력 속도 향상  

---

## 📚 6. 배운 점  
- Arduino UI 설계 및 장치 제어(릴레이) 전체 구현 경험 확보  
- 서버 → 클라이언트 → MCU로 이어지는 IoT 데이터 흐름의 구조적 이해  
- 이기종 디바이스 간 통신 흐름을 직접 구축하며 IoT 시스템 아키텍처 감각 향상  
- 센서, 서버, UI를 통합하는 과정에서 실제 하드웨어 디버깅 능력 강화  

---
