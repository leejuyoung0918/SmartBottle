# SmartBottle

물병의 수위를 측정하고 BLE로 Android 앱에 전달하는 KAIST CS305 팀 프로젝트입니다. 이 저장소는 하드웨어·앱·서버·설계 문서의 특정 commit을 함께 고정하는 통합 저장소입니다.

## 문제와 기여

기기와 앱이 각각 동작해도 화면에 올바른 측정값이 도착하는 것은 별도의 문제였습니다. [기존 포트폴리오](https://leejuyoung0918.github.io/smart-bottle.html)에 기록된 이주영의 담당은 하드웨어 제어, 앱 BLE 연동, 통합 시험입니다.

Arduino의 초음파 센서로 수면까지의 거리를 측정하고, HM-10을 거쳐 Android 앱에 값을 전달했습니다. 수신값을 화면에 표시해 송신 문자열과 대조하고, 앱 요청과 서버 응답을 차례로 확인할 수 있도록 했습니다.

- [Android BLE 연동 변경](https://github.com/CS30500/FrontEnd/commit/6819891ba4da3e7fe3cb00e8ea1581651d0f5403)
- [수신값 표시 화면 변경](https://github.com/CS30500/FrontEnd/commit/5ad1bdae50933f01ed63827a72c0e3551aeb9081)

## 결과와 배운 점

센서 → BLE → 앱 → 서버의 경계를 따라 측정값이 어디까지 도착했는지 확인하는 통합 시연을 구성했습니다. 수신값을 노출하는 작은 진단 기능이 점검할 구간을 좁히는 데 쓰였습니다. 수치로 측정한 디버깅 시간 단축 효과는 기록되어 있지 않습니다.

물병에 남은 양과 사용자가 마신 양은 다른 값입니다. 인터페이스를 설계할 때 데이터 형식뿐 아니라 단위·의미·검증 위치까지 합의해야 한다는 학습을 남겼습니다.

## 구조와 재현

| Submodule | 역할 |
|---|---|
| [Hardware](https://github.com/CS30500/Hardware) | 센서 측정과 기기 제어 |
| [FrontEnd](https://github.com/CS30500/FrontEnd) | Kotlin·Jetpack Compose Android 앱과 BLE |
| [BackEnd](https://github.com/CS30500/BackEnd) | FastAPI·MongoDB 서버 |
| [Diagrams](https://github.com/CS30500/Diagrams) | 요구사항과 설계 자료 |

```bash
git clone --recurse-submodules https://github.com/leejuyoung0918/SmartBottle.git
cd SmartBottle
git submodule status
```

이미 clone했다면 `git submodule update --init --recursive`로 고정된 버전을 가져옵니다. 각 구성요소의 설치·실행 안내는 해당 submodule에서 확인합니다. `--remote` 업데이트는 고정된 통합 조합을 바꾸므로 재현 절차에 포함하지 않습니다.

## 검증 범위

이 상위 저장소 자체에는 자동 CI나 테스트 코드가 없습니다. 기존 통합 시험 경험과 자동화된 회귀 검증을 구분합니다. 실제 기기·BLE 연결을 포함한 재시연은 하드웨어 환경이 필요합니다.
