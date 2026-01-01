# File System & Partition

## 개요

- 본 문서는 Linux 환경에서 디스크 추가 이후 파티션 생성, 파일 시스템 생성, 마운트 및 언마운트 과정을 정리한 실습 문서입니다.
- NVMe 디스크를 추가하여 실제 디바이스 인식부터 파일 접근까지의 전체 흐름을 확인합니다.

---

## 디스크 및 파티션 상태 확인

- 필자는 이번 실습을 위해 NVMe 디스크를 추가하였습니다.
- 현재 연결된 디스크, 파티션, 마운트 구조를 한 번에 확인하기 위해 다음 명령어를 사용합니다.
```text
lsblk
```
![01](/Linux/File%20System%20&%20Partition/imgs/01.png)

- 추가된 NVMe 디스크 `nvme0n1`이 출력되지만 `MOUNTPOINTS` 항목이 비어 있으므로 아직 마운트되지 않은 상태임을 알 수 있습니다.

---

## 디바이스 파일 확인

- 추가된 NVMe 디스크가 디바이스 파일 디렉토리에서 어떤 이름으로 인식되는지 확인합니다.
```text
ls /dev/
```
![02](/Linux/File%20System%20&%20Partition/imgs/02.png)

- 출력 결과 중 `nvme0n1`이라는 디바이스 파일을 확인할 수 있습니다.
- `nvme0`은 NVMe 컨트롤러를 의미하며 `nvme0n1`은 해당 컨트롤러에 연결된 실제 디스크를 의미합니다.

---

## 마운트 상태 확인

- 현재 시스템에 마운트된 파일 시스템 목록을 확인합니다.
```text
df
```
![03](/Linux/File%20System%20&%20Partition/imgs/03.png)

- 여러 파일 시스템이 보이지만 추가한 `NVMe` 디스크는 아직 마운트되지 않은 상태임을 확인할 수 있습니다.

- 마운트란 디스크(or 파티션)에 존재하는 파일 시스템을 특정 디렉터리에 연결하여 파일처럼 접근할 수 있도록 만드는 과정입니다.

---

## 파티션 개념 정리

- 파티션이란 하나의 물리적 디스크를 여러 개의 영역으로 나누어 각각 독립적으로 관리할 수 있도록 하는 구조입니다.

### Primary Partition
- 물리적인 디스크에 독립적으로 존재합니다.
- 파일 시스템을 생성할 수 있으며 운영체제가 직접 사용 가능합니다.
- 하나의 디스크당 최대 `4개`까지 생성할 수 있습니다.

### Extended Partition
- 하나의 디스크당 `1`개만 생성할 수 있습니다.
- 직접 사용할 수 없으며 내부에 `Logical Partition`을 생성하기 위한 영역입니다.

### Logical Partition
- `Extended Partition` 내부에 생성됩니다.
- 사용 방식은 `Primary Partition`과 동일합니다.

- `Primary Partition`과 `Extended Partition`을 합쳐 최대 `4개`까지 생성 가능합니다.

---

## 파티션 생성 계획

- 필자의 목표는 20GB NVMe 디스크에 다음과 같은 구조를 만드는 것입니다.
  - `Primary Partition 10GB`
  - `Logical Partition1 2.5GB`
  - `Logical Partition2 2.5GB`
  - `Logical Partition 5GB`
- 이후 `Primary Partition`을 마운트합니다.

---

## 파티션 생성 (fdisk)

- 다음 명령어로 디스크 파티션 설정을 시작합니다.
```text
fdisk /dev/nvme0n1
```
![04](/Linux/File%20System%20&%20Partition/imgs/04.png)

- `n`을 입력하여 새 파티션을 생성합니다.
- `Primary Partition`을 먼저 생성하기 위해 `p`를 입력합니다.
- 파티션 번호는 기본값인 `1번`을 사용합니다.
- 첫 번째 섹터는 기본값인 `2048`을 사용합니다.
  - 이는 디스크 정렬과 호환성을 보장하는 가장 안전한 시작 위치입니다.
- 마지막 섹터는 `10GB`로 설정합니다.

---

- `p`를 입력하여 현재 파티션 테이블을 확인합니다.

![05](/Linux/File%20System%20&%20Partition/imgs/05.png)

---

## Extended Partition 생성

- `Logical Partition`을 생성하기 위해 `Extended Partition`을 먼저 생성합니다.

![06](/Linux/File%20System%20&%20Partition/imgs/06.png)

- `n`을 입력한 후 `e`를 선택하여 `Extended Partition`을 생성합니다.
- 파티션 번호는 자동으로 `2번`이 할당됩니다.
- 시작 섹터는 `Primary Partition` 이후로 자동 설정됩니다.
- `Extended Partition` 크기는 남은 10GB중 `5GB`를 사용합니다.

- `p`를 입력하여 파티션이 정상적으로 생성되었는지 확인합니다.

---

## Logical Partition 생성

- 동일한 방식으로 `Logical Partition` 2.5GB를 생성합니다.

![07](/Linux/File%20System%20&%20Partition/imgs/07.png)

---

- `Extended Partition` 총 5GB중 2.5GB를 사용하고 나머지 2.5GB `Logical Partition`을 추가하고자 합니다.

![08](/Linux/File%20System%20&%20Partition/imgs/08.png)

- 5GB에서 2.5GB를 사용하고 남은 용량 2.5GB를 추가하려 했으나 생성되지 않는 현상이 발생합니다.
- 그 이유는 Extended Partition 내부에는 `EBR(Logical Partition 관리용 메타데이터)` 공간이 필요하기 때문입니다.
- 숫자상 10GB라도 Logical Partition으로 전부 채울 수 없습니다.
- 조금 여유롭게 2GB를 사용하여 Logical Partition을 생성합니다.

---

## 파일 시스템 생성

- 디스크를 사용하기 위해 파일 시스템을 생성합니다.
- 파일 시스템 생성에는 `mkfs` 명령어를 사용합니다.
- 필자는 `Primary Partition`에 `XFS` 파일 시스템을 생성합니다.
```text
mkfs.xfs -f /dev/nvme0n1p1
```
![09](/Linux/File%20System%20&%20Partition/imgs/09.png)

- `mkfs -t xfs` 명령어는 `mkfs.xfs`와 동일한 의미입니다.
- 기존 파일 시스템을 덮어쓸 경우 `-f` 옵션이 필요할 수 있습니다.

---

## 마운트 디렉터리 생성 및 마운트

- 마운트 전에 파일 시스템을 연결할 빈 디렉터리를 생성합니다.
```text
mkdir /home1
```
- 다음 명령어로 파티션을 마운트합니다.
```text
mount /dev/nvme0n1p1 /home1
```
- df 명령어로 마운트 상태를 확인합니다.

![10](/Linux/File%20System%20&%20Partition/imgs/10.png)

- Primary Partition `nvme0n1p1`이 정상적으로 마운트된 것을 확인할 수 있습니다.

---

## 파일 생성 테스트

- 마운트된 디렉터리에 파일을 생성합니다.
```text
vi /home1/a.txt
```
- 파일이 정상적으로 작성되었는지 확인합니다.
```text
cat /home1/a.txt
```
![11](/Linux/File%20System%20&%20Partition/imgs/11.png)

---

## 언마운트 수행

- 언마운트는 파일 시스템을 디렉터리에서 분리하여 안전하게 제거하는 과정입니다.
- 먼저 현재 마운트 상태를 확인합니다.
```text
df
```
- 다음 명령어로 언마운트를 수행합니다.
```text
umount /dev/nvme0n1p1
```
![12](/Linux/File%20System%20&%20Partition/imgs/12.png)

- `df` 출력 결과에서 해당 파티션이 제거된 것을 확인합니다.

---

## 언마운트 결과 확인

- 기존에 파일이 존재하던 디렉터리로 이동하여 파일이 보이지 않는 것을 확인합니다.
- 이는 파일 시스템이 정상적으로 언마운트되었음을 의미합니다.

![13](/Linux/File%20System%20&%20Partition/imgs/13.png)


---

## 정리

- 디스크 추가 후에는 디바이스 인식, 파티션 생성, 파일 시스템 생성, 마운트 과정을 순차적으로 수행해야 합니다.
- 파티션 구조와 파일 시스템은 데이터 관리와 안정성에 직접적인 영향을 미칩니다.
