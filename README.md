# 🪴 AI 표정 인식 기반 스마트 반려 화분

>**ESP32**와 **Raspberry Pi 5**를 연동하여 환경 데이터를 수집하고, 표정 인식 기반 노인 케어, 식물 성장 상태 분석, 환경 모니터링, 누적 조도 기반 화분 자동 회전 및 알림 기능을 제공하는 스마트 반려 화분 시스템이다.

<br>

## 📖 프로젝트 소개

> 본 프로젝트는 독거노인을 포함한 고령자가 일상 속에서 정서적 안정감을 느끼고, 보호자와의 정서적 연결을 유지할 수 있도록 지원하는 보호자 연계형 멀티모달 AI 기반 스마트 화분 시스템을 개발하는 것을 목표로 한다. 이를 위해 화분이라는 친숙한 오브젝트를 인터페이스로 활용하여 노인의 거부감을 최소화하고, 다음과 같은 정서, 생활 케어 기능을 통합한다.

- 표정 인식(CNN) 기반 정서 분석과 생성형 AI(Gemini API), TTS를 통한 음성 소통
- 객체 인식(YOLO) 기반 식물 성장 분석 및 성장일지 제공을 통한 성취감 강화
- 물 감지 및 환경 센서를 활용한 일상 활동, 건강 상태의 간접적 모니터링
- 일조량 기반 회전 화분 받침을 통한 식물 성장 안정화 및 생활 리듬 형성 지원

> 또한 정서 분석 결과와 성장일지는 Pushbullet API를 통해 보호자에게 전달되어, 보호자가 노인의 정서 상태를 원격으로 이해하고 지속적인 정서적 소통을 이어갈 수 있는 보호자 연계형 돌봄 구조를 구현한다.

---

## 📅 개발 기간

- 2025.03 ~ 2026.01

---

## 👨‍👩‍👧‍👦 팀 구성

| 이름 | 역할 |
|------|------|
| 이영은 | 팀장, HW 설계(ESP32, Raspberry Pi)/화분 받침 (모터)제어, 통신 구현 |
| 박기성 | 식물 인식 및 성장일지 작성 시스템(SW)/모터 제어, 통신 로직 구현|
| 남형우 | HW 설계(ESP32, Raspberry Pi)/3D 프린터, 화분 받침 (모터) 제어 |
| 신수진 | 표정 인식 모델 생성 및 성능 개선(SW)/카메라, 스피커 연동 작업 |

---

## ⚙️ 시스템 구성도

<p align="center">
<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/0cb9d8e6-4e59-4816-b241-52c8660e343e" />
</p>

**화분 받침 제어는 그림에서 생략되어 있습니다(순차적으로 기능하는 것이 아님)**

---

## 📂 프로젝트 구조

```text
Capstone_design_final
│
├── ESP32
│   └── sesorconnect              # 센서 데이터 수집 및 Raspberry Pi UART 전송
│
├── emotion_detect
│   ├── main.py                   # 표정 인식 메인 실행 파일
│   ├── config.py                 # 환경 설정 파일
│   └── module1
│       ├── camera_control.py     # 카메라 제어
│       ├── emotion.py            # 표정 인식 및 분석
│       ├── notifier.py           # 알림 전송
│       └── sensors.py            # 센서 데이터 처리
│
├── plant_detect
│   ├── run.py                    # 식물 인식 메인 실행 파일
│   ├── client.py                 # 통신 클라이언트
│   └── module2
│       ├── pushbullet_utils.py   # Pushbullet 알림 기능
│       └── rotating_pot.py       # 누적 조도 기반 화분 회전 제어
│
├── 5조 논문_박기성,신수진,이영은,남형우_최종본.hwp
└── README.md
```

## 📊 결과

**1. 화분 외형**

<p align="center">
<img width="887" height="308" alt="image" src="https://github.com/user-attachments/assets/ef69b5b7-cbdd-416f-a0f2-a4dae760c54a" />
</p>

**2. 표정 인식 모델**

<img width="281" height="221" alt="image" src="https://github.com/user-attachments/assets/225c3eb2-51f2-45b9-826e-dbbbc7fcfd30" />
<img width="493" height="187" alt="image" src="https://github.com/user-attachments/assets/de6456b9-af36-45f8-9613-5685f0f9d371" />


- 데이터셋: FER2013(베이스 모델) → RAF-DB 전이학습 적용
- ONNX 변환, FP16 양자화 → 모델 경량화. 
- 라즈베리파이5 환경: 평균 5~10 FPS
- 표정 인식 정확도 80.36% 달성
- Sad, Happy 감정: 높은 인식 신뢰도 확보

**3. 식물 인식 및 성장일지 작성 시스템**

<a href="https://github.com/kiseongpark/Capstone_Design-Growth_journal_with_raspberry-Pi5">제작 과정 및 결과

**4. 환경 모니터링 시스템 및 자동 회전 화분 받침 구현**

<img width="490" height="209" alt="image" src="https://github.com/user-attachments/assets/91d7502b-ca46-44ed-86ec-5593f587452a" />

이미지 제작 : 이영은

```text
- RaspberryPi 1 : 표정 인식용
- RaspberryPi 2 : 식물 인식용
- ESP32 : 센서 허브

- 통신 방법 :
  1) 표정 인식용 라즈베리파이 - ESP32 : UART 통신
  2) 라즈베리파이 2대 : TCP 소켓 통신
```

**화분 받침**


<img width="121" height="128" alt="image" src="https://github.com/user-attachments/assets/e6a753e0-d00d-4d42-80cd-53d345685cc2" />

특징 : 
- BH1750 조도 센서의 누적 조도값이 5000 lux 이상이 되면 화분이 180° 회전하도록 구현함.
- Half-step 구동 방식을 적용한 스텝모터를 제어하여 정해진 각도만큼 안정적으로 회전하도록 구현함.

---

## 🎥 실행 영상

<img width="400" height="225" alt="성과포럼 발표" src="https://github.com/user-attachments/assets/7abe3948-f6ae-451f-912c-331c7e8d5d6b" />

---

## 👏 Contributors

| 이름 | GitHub |
|------|---------|
| 이영은 | https://github.com/olorlo |
| 신수진 | https://github.com/s0415j |
| 남형우 | https://github.com/namhw0301 |

