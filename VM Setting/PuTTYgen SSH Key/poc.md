# PuTTYgen SSH Key

## 개요

- PuTTYgen은 PuTTY로 SSH 원격 접속을 할 때 공개키와 개인키를 생성하여 비밀번호 없이 인증서 방식으로 로그인할 수 있도록 해주는 도구입니다.
- 비밀번호 인증 방식보다 보안성이 높으며 무차별 대입 공격에 매우 강합니다.
- 공개키는 서버에 저장되며 유출되어도 직접적인 위험은 없습니다.
- 개인키는 클라이언트에 저장되며 유출 시 즉시 폐기 및 재발급이 필요합니다.
- 본 문서는 PuTTYgen을 이용한 키 생성부터 서버 설정, PuTTY 접속까지의 전체 과정을 정리합니다.

---

## PuTTYgen 실행 및 키 생성

- PuTTY Key Generator(PuTTYgen)를 실행하면 다음과 같은 키 생성 화면이 나타납니다.
- Parameters 영역에서 RSA, DSA, ECDSA, EdDSA, SSH-1(RSA) 중 알고리즘을 선택할 수 있습니다.
- 본 실습에서는 호환성이 가장 좋은 RSA 알고리즘을 사용합니다.
- `Key size`의 기본값은 `2048 bits`로 설정되며 충분히 안전한 길이입니다.

![01](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/01.png)

- `Generate` 버튼을 클릭하여 키 생성을 시작합니다.

![02](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/02.png)

- 흥미로운 점은 키 생성 중 마우스를 이리저리 움직이면 엔트로피가 증가하여 생성 속도가 빨라집니다.

---

## 공개키 및 개인키 생성 완료

- 키 생성이 완료되면 공개키 문자열과 함께 개인키 저장 옵션이 활성화됩니다.

![03](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/03.png)

- 우측 하단의 `Save private key` 버튼을 클릭하여 개인키를 저장합니다.
- 실습 목적이므로 개인키 암호는 설정하지 않습니다.

![04](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/04.png)

- 개인키는 다음과 같이 개인 디렉토리에 저장합니다.

![05](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/05.png)

---

## 서버 측 공개키 저장 준비

- 키 기반 인증을 사용하려면 공개키를 서버의 다음 경로에 저장해야 합니다.
```text
.ssh/authorized_keys
```
- PuTTY로 서버에 접속 후 디렉토리 존재 여부를 확인합니다.

![06](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/06.png)

- 기본 상태에서는 `.ssh` 디렉토리가 존재하지 않습니다.

---

## 공개키 저장 경로 생성

- 다음 명령어로 디렉토리와 파일을 생성합니다.
```text
mkdir .ssh  
vi .ssh/authorized_keys
```
- PuTTYgen에서 생성된 공개키 문자열 전체를 `authorized_keys` 파일에 붙여넣습니다.

![07](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/07.png)

![08](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/08.png)

---

## 공개키 저장 확인

- 다음 명령어로 공개키가 정상적으로 저장되었는지 확인합니다.
```text
cat .ssh/authorized_keys
```
![09](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/09.png)

---

## 권한 설정

- SSH는 보안상 권한이 올바르게 설정되지 않으면 키 인증을 거부합니다.
- 다음 명령어로 권한을 설정합니다.
```text
chmod -R 700 .ssh/authorized_keys
``` 
- 설정 후 권한을 확인합니다.
```text
ls -laR .ssh
```
![10](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/10.png)

---

## PuTTY 설정 - 로그인 계정

- `PuTTY Configuration`에서 `Connection` - `Data`로 이동합니다.
- `Auto`-`login username` 항목에 로그인 계정을 입력합니다.
- 본 실습에서는 기존에 만들어둔 `te1` 계정을 사용합니다.

![11](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/11.png)

---

## PuTTY 설정 - 개인키 연결

- `Connection` - `SSH` - `Auth` - `Credentials`로 이동합니다.
- `Private key file for authentication` 항목에서 저장한 개인키 파일을 선택합니다.

![12](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/12.png)
![13](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/13.png)

---

## 접속 확인

- 설정 완료 후 PuTTY로 서버에 접속합니다.
- 비밀번호 입력 없이 공개키와 개인키 기반으로 로그인이 성공합니다.

![14](/VM%20Setting/PuTTYgen%20SSH%20Key/imgs/14.png)

---

## 정리

- PuTTYgen을 이용한 키 기반 인증은 실무에서 필수적으로 사용되는 보안 방식입니다.
- 개인키는 반드시 안전하게 보관해야 하며 유출 시 즉시 폐기해야 합니다.
