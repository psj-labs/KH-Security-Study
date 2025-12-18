# VM Clone (VMware Workstation)

## 개요

실무 환경에서 업무를 하다 보면 하나의 가상머신을 기반으로 여러 개의 리눅스 환경을 동시에 사용해야 하는 경우가 많습니다.  
하지만 그때마다 OS ISO 파일을 다운로드하고, 가상머신을 새로 생성해 설정하는 과정은 상당히 번거롭습니다.

이러한 불편함을 줄이기 위해 사용하는 기능이 **VM Clone**입니다.  
VM Clone을 사용하면 이미 설치 및 설정이 완료된 가상머신을 **현재 상태 그대로 복제**할 수 있습니다.

필자는 다음 환경을 기준으로 VM Clone을 진행합니다.

- Host OS: Windows
- Hypervisor: VMware Workstation
- Guest OS: Rocky Linux
- 목표: 기존 Rocky Linux 환경을 동일하게 복제

---

## 기존 가상머신 환경

VMware Workstation 화면에서 기존 가상머신 `Rlinux153`이 존재합니다.

- Rocky Linux 설치 완료 상태
- DHCP 미사용
- 수동 IP 설정: `192.168.10.153`

가상머신 이름은 Rocky linux에 할당한 IP 153번을 의미하는 `Rlinux153`으로 지정하였습니다.

![01](/VM%20Setting/VM%20clone/imgs/01.png)

---

## VM Clone 실행

복제를 진행할 원본 가상머신에서 다음 메뉴를 선택합니다.

- `우클릭 → Manage → Clone`

![02](/VM%20Setting/VM%20clone/imgs/02.png)

---

`Clone Virtual Machine Wizard` 창이 나타나면 **다음** 을 클릭합니다.

![03](/VM%20Setting/VM%20clone/imgs/03.png)

---

## 복제 기준 상태 선택

복제 기준이 될 가상머신의 상태를 선택합니다.

- **Current state of the virtual machine**  
  → 현재 실행 중인 가상머신 상태를 기준으로 복제
- **Snapshot**  
  → 미리 생성해 둔 스냅샷 상태를 기준으로 복제

필자는 **현재 상태(Current state)** 를 기준으로 복제를 진행합니다.

![04](/VM%20Setting/VM%20clone/imgs/04.png)

---

## Clone 방식 선택

다음은 가상머신을 어떤 방식으로 복제할지 선택하는 단계입니다.

- **Create a linked clone**  
  - 원본 가상머신을 참조하는 방식
  - 디스크 사용량이 적음
  - 원본 가상머신이 없으면 실행 불가

- **Create a full clone**  
  - 원본 가상머신을 그대로 복사
  - 완전히 독립적인 가상머신
  - 디스크 사용량이 큼

필자는 **Create a linked clone** 방식을 선택합니다.

![05](/VM%20Setting/VM%20clone/imgs/05.png)

---

## 가상머신 이름 지정

복제될 가상머신의 이름을 지정합니다.

> Clone 후에는 반드시 **MAC Address와 IP Address를 기존 가상머신과 다르게 변경해야 합니다.**

새 가상머신에 `192.168.10.154` IP를 할당할 예정이므로  
가상머신 이름을 `Rlinux154`로 지정합니다.

![06](/VM%20Setting/VM%20clone/imgs/06.png)

---

## Clone 완료

**마침(Finish)** 을 클릭하면 복제가 시작되며 약 1분 정도 소요됩니다.

가상머신이 필요할 때마다 새로 설치하는 것보다  
Clone 기능을 사용하는 것이 훨씬 효율적입니다.

![07](/VM%20Setting/VM%20clone/imgs/07.png)

---

Clone이 완료되면 다음과 같은 화면이 표시됩니다.

![08](/VM%20Setting/VM%20clone/imgs/08.png)

---

VMware Workstation을 다시 확인하면  
복제된 가상머신 `Rlinux154`가 생성된 것을 확인할 수 있습니다.

![09](/VM%20Setting/VM%20clone/imgs/09.png)

---

## MAC Address 변경

복제된 가상머신 `Rlinux154`에서 다음 메뉴로 이동합니다.

- `Edit virtual machine settings`
- `Network Adapter`
- 우측 하단 `Advanced`

`MAC Address` 항목에서 **Generate** 버튼을 눌러 MAC 주소를 변경합니다.

> MAC Address는 네트워크 장치마다 고유해야 하며,  
> 동일한 LAN 내에서 중복될 경우 충돌이 발생할 수 있습니다.

![10](/VM%20Setting/VM%20clone/imgs/10.png)

---

## IP Address 변경

가상머신을 실행한 뒤 다음 경로로 이동합니다.

- `설정 → 네트워크 → 톱니바퀴 아이콘`

현재 설정은 원본 가상머신의 정보가 그대로 복제된 상태입니다.

- IP Address: `192.168.10.153`
- MAC Address: `00:50:56:26:F8:59`

![11](/VM%20Setting/VM%20clone/imgs/11.png)

---

### IPv4 설정 변경

IPv4 메뉴에서 IP 주소를 다음과 같이 변경합니다.

- IP Address: `192.168.10.154`
- 나머지 설정은 변경하지 않음

![12](/VM%20Setting/VM%20clone/imgs/12.png)

---

## 네트워크 설정 확인

설정 후 터미널에 들어가 `ip a`를 입력해 다음과같이 `IP address: 192.168.10.154`와 `MAC address: 00:50:56:26:f8:69`로 잘 할당받은 것을 볼 수 있습니다.

![12](/VM%20Setting/VM%20clone/imgs/13.png)
