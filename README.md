# AI Homelab

Kubernetes 기반 AI 워크로드의 리소스 관리, 스케줄링, 실행 생명주기 및 모델 서빙을 연구하고 구현하는 개인 프로젝트입니다.

기술 설치 자체보다 **문제 정의 → 원인 분석 → 설계·구현 → 실험 → 정량적 검증 → 문서화**에 집중합니다. AI Infrastructure / Platform Engineering 역량을 재현 가능한 코드와 검증 결과로 보여주는 것을 목표로 합니다.

## 저장소 안내

이 저장소는 전체 프로젝트를 소개하는 인덱스입니다. 기술 상세 문서와 검증 결과는 담당 저장소에서 관리하며, 별도 위키는 두지 않습니다.

| 저장소 | 책임 | 상태 |
| --- | --- | --- |
| [ai-homelab](https://github.com/twotwobread/ai-homelab) | 전체 소개, 로드맵, 저장소·성과 안내 | 초기 구성 |
| [homelab-infra](https://github.com/twotwobread/homelab-infra) | K3s 운영 설정, 배포·점검·복구 절차 | 초기 구성 |
| `scheduling-lab` | Kubernetes / Volcano 스케줄링 실험 | 생성 예정 |
| `ai-serving-operator` | CRD·Controller 및 모델 서빙 자동화 | 생성 예정 |

기존 `llm-serving-lab`은 별도 프로젝트로 보존하며, 기존 위키를 이 프로젝트의 근거로 자동 승계하지 않습니다.

## 환경과 제약

사용자가 제공한 기존 확인 사항 기준이며, 이 저장소 초기화 시 실시간 검증한 상태는 아닙니다.

- 운영: Intel N100 / RAM 16GB Mini PC의 Ubuntu + 단일 서버 K3s. HA 구성은 아닙니다.
- 개발: Windows + WSL. 개발용 kind는 필요할 때 구성합니다.
- GPU: Intel 내장 GPU의 호스트 OpenVINO 인식·연산 실험 경험이 있습니다. Kubernetes GPU 할당은 미검증입니다.
- 확장 후보: Raspberry Pi 4 ARM Worker. 참여 전 OS·메모리·네트워크 확인이 필요합니다.
- 비용: 보유 CPU·Intel GPU를 우선 활용합니다. CUDA가 필요하면 승인된 범위에서 시간제 클라우드 GPU를 검토합니다.

## 목표 아키텍처

아래는 구현 완료 상태가 아닌 단계적 확장 방향입니다.

```text
Git / CI / 배포 자동화
        ↓
Kubernetes AI Platform
        ├── AIWorkload API / Controller
        └── ModelService API / Controller
        ↓
Kubernetes 기본 스케줄러 / Volcano
        ↓
CPU / Intel GPU / 필요시 클라우드 GPU
        ↓
Batch 실행 / 모델 서빙 / 관측·성능 검증
```

기존 Kubernetes·Volcano 기능을 우선 활용합니다. Custom Controller는 확인된 플랫폼 요구사항에 한해 개발하며, 추론 엔진과 스케줄러 알고리즘을 불필요하게 재구현하지 않습니다.

## 로드맵

1. **운영 기반 확인** — WSL에서 K3s API 접근, 노드·시스템 구성요소·네트워크·스토리지 확인 및 구성 코드화
2. **기본 스케줄링 실험** — 개발용 kind에서 Requests / Limits, Affinity, Priority / Preemption 검증
3. **Volcano 실험** — CPU 자원으로 Gang Scheduling, Queue, 자원 경합 비교
4. **문제 분석과 기여** — 재현 가능한 upstream 문제를 찾으면 수정·테스트·PR 검토
5. **플랫폼 개발** — 요구사항에 근거한 AIWorkload / ModelService API와 Controller 구현
6. **서빙·최적화** — CPU / OpenVINO부터 시작하여 필요시 vLLM / SGLang 확장

관측과 검증은 마지막 단계에만 수행하지 않고 각 단계에 포함합니다. Operator 및 클라우드 연동의 상세 설계는 아직 확정하지 않았습니다.

## 현재 상태와 다음 작업

현재는 저장소 골격을 준비하는 단계입니다. 다음 작업은 `homelab-infra`에서 원격 관리와 K3s 기본 상태를 확인하는 것입니다. 이 저장소 생성으로 클러스터 설정이 변경되지는 않습니다.

아직 이 프로젝트에서 새로 검증한 실험 성과는 없습니다. 성과를 소개할 때에는 담당 저장소의 재현 절차, 실행 환경, 결과 및 한계에 연결하고 측정하지 않은 개선 수치는 작성하지 않습니다.
