# 🦈 레이다 데이터를 이용한 실시간 낙상 감지 시스템 설계

> **2026 소프트웨어캡스톤디자인 - T03 죠습니다**
> 레이다 센서와 온디바이스 AI를 활용한 비접촉·비침해 방식의 노인 안전 사고(낙상) 감지 솔루션

## 01. 프로젝트 개요 (Overview)
<img width="1920" height="1080" alt="캡디_연구계획발표" src="https://github.com/user-attachments/assets/14993980-83d2-4f3a-b6ba-a526901ac76c" />


대한민국이 초고령사회로 진입함에 따라, 노인 안전사고 중 63.5%를 차지하는 '낙상'은 매우 심각한 사회적 이슈입니다. 
그러나 기존의 카메라 기반 모니터링은 프라이버시 침해 우려가 크고, 웨어러블 기기는 착용의 번거로움과 낮은 사용률(7.1%)이라는 한계가 있습니다.

본 프로젝트는 **FMCW 레이다 센서**를 활용하여 사용자가 별도의 기기를 착용하지 않아도, 일상생활 공간 내에서 프라이버시를 보호하며 실시간으로 낙상을 감지하고 보호자에게 알림을 제공하는 시스템을 구축하는 것을 목표로 합니다.

### 🎯 핵심 목표

- **비접촉·비침해**: 카메라 없이 레이다 신호만을 이용해 사생활 보호 및 거부감 해소
- **상시 모니터링**: 조도에 영향을 받지 않고 24시간 감지 가능

## 02. 베이스 프로젝트 및 참조 (Base Project & Reference)

본 프로젝트는 Texas Instruments에서 제공하는 표준 산업용 레이다 솔루션을 기반으로 설계 및 커스터마이징되었습니다.

- **Base Project**: Radar Toolbox → Example Projects → Industrial and Personal Electronics → **Pose and Fall Detection**
- **Reference URL**: [TI Resource Explorer](https://dev.ti.com/tirex/explore/node?isTheia=false&node=A__AHUFv8dsEwrdjGFvhy0b2g__radar_toolbox__1AslXXD__LATEST)
- **Key Implementation**: TI에서 제공하는 최적화된 임베디드 알고리즘 및 AI 레이어 모델을 프로젝트 환경에 맞춰 통합 및 확장

## 03. 주요 특징 (Key Features)

- **5가지 행동 분류**: 실시간 데이터를 분석하여 행동을 5개 클래스로 분류합니다.
    - `FALLING`, `LYING`, `SITTING`, `STANDING`, `WALKING`
- **온디바이스 AI 아키텍처**: 저전력 MCU 환경에서 실시간 추론이 가능하도록 모델 경량화 및 최적화를 진행합니다.
- **통합 알림 시스템**: 낙상 발생 시 AWS 클라우드와 FCM을 연동하여 보호자 안드로이드 앱으로 즉각적인 푸시 알림을 전송합니다.


## 04. 프로젝트 아키텍처 (Project Architecture)
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/1e0e0316-64f3-4877-89b9-72aa47febff9" />

## 05. 기술 스택 (Tech Stack)

### Hardware

- **Sensor**: TI IWRL6432BOOST (FMCW Radar)
- **Gateway**: Raspberry Pi (데이터 수집 및 클라우드 전송)

### AI & Data

- **Model**: Simple Linear 4-Layer Model
- **Framework**: PyTorch, ONNX, TVM (Neural Network Compiler)
- **Tools**: TI Edge AI Studio, Code Composer Studio (CCS), Uniflash

### Backend & Cloud

- **Cloud**: AWS IoT Core, Amazon EC2
- **Framework**: FastAPI (Python)
- **Database**: PostgreSQL (Cloud), Room DB (Mobile)
- **Notification**: Firebase Cloud Messaging (FCM)

### Mobile

- **Platform**: Android (Kotlin/Java)
- **Database**: Room DB (Mobile)

## 06. 프로젝트 진행 및 데이터 구조

### 데이터 추출 및 전처리

- **Point Cloud**: 공간상 위치(X, Y, Z), 도플러 속도, SNR 추출
- **Track Data**: 객체 ID(TID), 위치, 속도, 가속도 데이터 분석

### 모델 최적화 (Model Optimization)

- 저전력 기반의 MCU(+NPU) 환경에서 실시간 추론 속도를 확보하기 위해 **Simple Linear 4-Layer Model** 채택
- PyTorch 학습 모델(.onnx) → TVM 컴파일러 → `.a` 라이브러리 변환 후 임베디드 프로젝트 통합

### 데이터셋 강화

- 클래스당 150~200개, 총 800개 이상의 샘플 확보 예정
- 거리(1m/2m/3m), 4방향 낙상, 엣지 케이스(비틀거림, 빠르게 주저앉기 등) 포함

## 06. 역할 분담 (Team Members)

| **이름** | **역할** | **담당 업무** |
| --- | --- | --- |
| **조영찬** | 팀장 / AI / HW | 데이터 수집 환경 구축, 인공지능 모델 설계 및 MCU 이식 |
| **장수민** | Cloud / DevOps | AWS 인프라 구축, IoT 통신 및 백엔드 API 개발 |
| **손정연** | Mobile / UI | 안드로이드 앱 개발, 실시간 모니터링 UI 및 알림 구현 |

© 2026 Team T03 죠습니다. All rights reserved.
