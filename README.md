# 📍 QR Geo-Route Finder (프로젝트 이름)

QR 코드를 스캔하여 인식된 GPS 좌표를 기반으로 최적의 목적지 경로를 설정하고, 사용자의 위치 정보를 유연하게 관리하는 프로그램입니다.

<br>

## ✨ 주요 기능 (Key Features)

* **QR 기반 위치 동기화:** QR 코드를 찍은 위치의 GPS 좌표를 추출하여 사용자의 '현 위치(출발지)'로 즉시 설정하고 목적지까지의 경로를 탐색합니다.
* **사진 GPS 연동 보정:** 카메라로 직접 촬영한 사진의 메타데이터(Exif)에서 GPS 좌표를 읽어와, 현재 위치 정보를 정밀하게 대체하고 보정하는 기능을 제공합니다.
* **안전한 데이터 관리:** 트랜잭션 제어(`COMMIT`, `ROLLBACK`)를 적용하여, 사용자의 위치 정보나 상태를 변경(`UPDATE`)할 때 데이터의 무결성을 엄격하게 보장합니다.
* **유연한 시스템 구조:** 자바(Java)의 인터페이스(Interface)와 다형성을 활용하여, 다양한 지도 API(Google Maps, Kakao 등)나 위치 계산 로직을 손쉽게 교체하고 확장할 수 있도록 설계되었습니다.

<br>

## 🛠 기술 스택 (Tech Stack)

### Backend & Database
<img src="https://img.shields.io/badge/Java-007396?style=flat-square&logo=Java&logoColor=white"/> <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=MySQL&logoColor=white"/> 
*(사용하시는 다른 도구가 있다면 여기에 추가해 주세요)*

<br>

## 💻 핵심 코드 구조 (Code Preview)

위치 정보를 업데이트하고 경로를 재설정하는 핵심 비즈니스 로직입니다.

<details>
<summary>코드 보기 (클릭)</summary>

```java
// 위치 설정 인터페이스
public interface LocationService {
    void updateCurrentLocation(GPSCoordinate coord);
    Route calculateRoute(GPSCoordinate start, GPSCoordinate destination);
}

// 오버라이딩을 통한 QR 기반 위치 설정 구현체
@Override
public void updateCurrentLocation(GPSCoordinate qrCoord) {
    try {
        // SQL: UPDATE currentLocation SET ... 
        // 1. QR 위치를 현 위치로 설정하는 로직 실행
        databaseConnection.commit();
    } catch (SQLException e) {
        // 오류 발생 시 이전 위치로 롤백
        databaseConnection.rollback();
    }
}
