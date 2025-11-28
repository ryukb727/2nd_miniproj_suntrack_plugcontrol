
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
빛 추적 스마트 전력 제어 시스템은 태양광 발전 효율을 높이고 전력 제어 기능을 결합한 IoT 기반 에너지 관리 프로젝트입니다.

기존 고정형 태양광 패널은 태양의 이동이나 주변 장애물에 따라 조도 변화에 대응하지 못해 발전 효율이 떨어집니다.  
이를 해결하기 위해 **CDS 센서 기반 태양광 추적** 기능을 구현하는 한편, 효과적으로 에너지를 관리할 수 있도록 패널 관련 정보 **실시간 서버 저장**, **스마트 콘센트 제어**, **LCD 시각화 UI** 기능을 포함하는 통합 에너지 관리 시스템을 구축했습니다.


### 📍 전체 시스템 구성
<img width="960" height="540" alt="Image" src="https://github.com/user-attachments/assets/43fa549c-5ee2-49ef-9705-b3aec7b91486" />

- #### **STM32 Nucleo-F411RE**
  - 8개의 CDS 센서 입력
  - 태양광 패널 자동 회전(스텝모터)
  - Solar 센서 기반 발전량 측정
  - Wi-Fi(ESP 모듈)로 Raspberry Pi 서버에 데이터 송신

- #### **Raspberry Pi 5 (서버)**
  - 소켓 서버(iot_server.c)
  - MariaDB 데이터 저장
  - **Bluetooth**로 Arduino와 데이터 송수신

- #### **Arduino UNO**
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
![WiFi](h트 ON/OFF 동작 제어
- 상태를 다시 Raspberry Pi에 전송해 DB 갱신 흐름 유지

#### ④ 전체 제어 파이프라인 구축
- "수신 → 처리 → LCD 표시 → 릴레이 제어 → 상태 재전송" 전체 흐름 개발  
- Arduino를 사용자 인터페이스 핵심 장치로 완성

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
  - `lcd.clear()` 반복 호출 지양  
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

<div align="center">
<a href="#japanese">⬇️ 日本語バージョンへ移動 (Go to Japanese Version) ⬇️</a>
</div>

</div>

---

<div id="japanese">

### 🇯🇵 Japanese Version
