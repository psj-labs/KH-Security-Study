# Linux Basic Command

---

## 리눅스 명령어 기본 형식

| 구분 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 형식 | `command [옵션] [대상]` | 리눅스 명령 기본 구조 | `ls -l /etc` |
| 단일 옵션 | `-옵션` | 한 글자 옵션 | `ls -a` |
| 다중 옵션 | `--옵션` | 여러 글자 옵션 | `ls --all` |
| 옵션 축약 | `-abc` | 단일 옵션 결합 | `ls -alh` |

---

## 경로 표현 방식

| 표현 | 의미 | 예시 |
|---|---|---|
| `/` | 최상위 루트 디렉토리 | `/etc/passwd` |
| `./` | 현재 디렉토리 | `./test.txt` |
| `../` | 상위 디렉토리 | `../log` |
| `~/` | 사용자 홈 디렉토리 | `~/Desktop` |
| 절대 경로 | `/` 기준 | `/var/log/messages` |
| 상대 경로 | 현재 위치 기준 | `./a/b` |

---

## 디렉토리 구조

| 디렉토리 | 역할 | 예시 |
|---|---|---|
| `/etc` | 설정 파일 | `/etc/passwd` |
| `/var` | 로그/가변 데이터 | `/var/log` |
| `/home` | 사용자 홈 | `/home/user` |
| `/usr` | 사용자 프로그램 | `/usr/bin` |
| `/bin` | 기본 명령어 | `/bin/ls` |
| `/sbin` | 시스템 관리 | `/sbin/reboot` |

---

## 디렉토리 관련 명령어

| 명령어 | 형식 | 설명 | 예시 |
|---|---|---|---|
| `cd` | `cd [디렉토리]` | 디렉토리 이동 | `cd /etc` |
| `pwd` | `pwd` | 현재 경로 출력 | `pwd` |
| `mkdir` | `mkdir [옵션] 디렉토리` | 디렉토리 생성 | `mkdir a` |
| `mkdir -p` | `mkdir -p 경로` | 상위 포함 생성 | `mkdir -p a/b` |
| `rmdir` | `rmdir 디렉토리` | 빈 디렉토리 삭제 | `rmdir test` |
- `rm -rf`를 실무적으로 더 많이 사용함

---

## 디렉토리 구조 출력

| 명령어 | 형식 | 설명 | 예시 |
|---|---|---|---|
| `tree` | `tree [경로]` | 구조 출력 | `tree /etc` |
| `tree -d` | `tree -d [경로]` | 디렉토리만 출력 | `tree -d /etc` |

---

## 파일 목록 (ls)

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `ls` | 목록 출력 | `ls` |
| `-a` | `ls -a` | 숨김 포함 | `ls -a` |
| `-l` | `ls -l` | 상세 출력 | `ls -l` |
| `-R` | `ls -R` | 하위 포함 | `ls -R` |
| `-h` | `ls -lh` | 가독성 크기 | `ls -lh` |
| `-i` | `ls -i` | inode 출력 | `ls -i` |

---

## 리다이렉션

| 기호 | 형식 | 설명 | 예시 |
|---|---|---|---|
| `>` | `command > file` | 출력 저장 | `ls > a.txt` |
| `>>` | `command >> file` | 출력 추가 | `echo hi >> a.txt` |
| `<` | `command < file` | 입력 사용 | `cat < a.txt` |

---

## cat

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `cat file` | 파일 출력 | `cat a.txt` |
| `-n` | `cat -n file` | 줄 번호 | `cat -n a.txt` |
| `-b` | `cat -b file` | 공백 제외 번호 | `cat -b a.txt` |
| here-doc | `cat <<EOF` | 다중 입력 | `cat <<EOF` |

---

### echo / env 

| 구분 | 표현 | 형식 | 설명 | 예시 |
|---|---|---|---|---|
| echo | 문자열 출력 | `echo 문자열` | 입력한 문자열을 그대로 출력 | `echo hello` |
| echo | 변수 치환 | `echo $VAR` | 변수에 저장된 값을 출력 | `echo $PATH` |
| echo | 명령 치환 | `echo $(command)` | 명령 실행 결과를 출력 | `echo $(pwd)` |
| env | 환경 변수 출력 | `env` | 현재 세션의 모든 환경 변수 출력 | `env` |
| env + grep | 특정 변수 확인 | `env \| grep VAR` | 특정 환경 변수만 필터링 | `env \| grep HOME` |


---

## 변수 / 명령 치환

| 표현 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 변수 | `$VAR` | 변수 참조 | `$PATH` |
| 명령 | `$(command)` | 실행 결과 | `$(pwd)` |
| 큰따옴표 | `" "` | 해석 | `"$(pwd)"` |
| 작은따옴표 | `' '` | 문자 그대로 | `'$(pwd)'` |

---

## 파일 복사 (cp)

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `cp src dst` | 파일 복사 | `cp a b` |
| `-r` | `cp -r src dst` | 디렉토리 | `cp -r a b` |
| `-a` | `cp -a src dst` | 속성 유지 | `cp -a a b` |
| `-u` | `cp -u src dst` | 덮어쓰기 방지 | `cp -u a b` |

---

## 파일 이동 (mv)

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `mv src dst` | 파일 또는 디렉토리를 이동(이름 변경 포함) | `mv a b` |
| `-i` | `mv -i src dst` | 덮어쓰기 전 사용자에게 확인 요청 | `mv -i a b` |
| `-b` | `mv -b src dst` | 덮어쓰기 발생 시 백업 파일 생성 | `mv -b a b` |
| `-f` | `mv -f src dst` | 확인 없이 강제 이동 | `mv -f a b` |
| `-v` | `mv -v src dst` | 이동 과정을 출력 | `mv -v a b` |

---

## 파일 삭제 (rm)

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `rm file` | 파일 삭제 | `rm a.txt` |
| `-r` | `rm -r dir` | 디렉토리 삭제 | `rm -r a` |
| `-f` | `rm -rf dir` | 강제 삭제 | `rm -rf a` |

---

## 링크(ln)

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 하드 | `ln src link` | 하드 링크 | `ln a b` |
| 소프트 | `ln -s src link` | 심볼릭 링크 | `ln -s a b` |

---

## 파이프 / 페이지 출력

| 명령어 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 파이프 (`\|`) | `cmd1 \| cmd2` | 앞 명령의 출력을 뒤 명령의 입력으로 전달 | `ls -al \| more` |
| more | `more file` | 한 화면씩 출력 | `more a.txt` |
| less | `less file` | 스크롤 가능한 출력 | `less a.txt` |

---

## grep

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `grep str file` | 파일에서 문자열이 포함된 행만 출력 | `grep root /etc/passwd` |
| 파이프 | `cmd \| grep str` | 앞 명령 출력 결과에서 필터링 | `ls -al \| grep conf` |
| `-i` | `grep -i str file` | 대소문자 구분 없이 검색 | `grep -i root /etc/passwd` |
| `-n` | `grep -n str file` | 일치하는 행 번호와 함께 출력 | `grep -n root /etc/passwd` |
| `-w` | `grep -w str file` | 단어 단위로 정확히 일치 | `grep -w root /etc/passwd` |
| `-v` | `grep -v str file` | 문자열이 **없는** 행만 출력 | `grep -v nologin /etc/passwd` |
| `-l` | `grep -l str files` | 문자열이 포함된 파일명만 출력 | `grep -l root *.conf` |
| `-r` | `grep -r str dir` | 디렉토리 하위까지 재귀 검색 | `grep -r password /etc` |
| `-c` | `grep -c str file` | 일치하는 행 개수 출력 | `grep -c root /etc/passwd` |

---

## head

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| (기본) | `head file` | 파일 **앞 10줄** 출력 | `head a.txt` |
| `-n N` | `head -n N file` | 파일 **앞 N줄** 출력 | `head -n 5 a.txt` |
| `-c N` | `head -c N file` | 파일 **앞 N바이트** 출력 | `head -c 20 a.txt` |

## tail

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| (기본) | `tail file` | 파일 **뒤 10줄** 출력 | `tail a.txt` |
| `-n N` | `tail -n N file` | 파일 **뒤 N줄** 출력 | `tail -n 5 a.txt` |
| `-c N` | `tail -c N file` | 파일 **뒤 N바이트** 출력 | `tail -c 20 a.txt` |
| `-f` | `tail -f file` | 파일에 **새로 추가되는 내용 실시간 출력(로그 추적)** | `tail -f /var/log/messages` |
| `-f -n N` | `tail -f -n N file` | **마지막 N줄부터 보여주고**, 이후는 실시간 추적 | `tail -f -n 50 /var/log/messages` |


---

## truncate

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `truncate -s N file` | 파일 크기를 **N 바이트로 설정** | `truncate -s 0 a.txt` |
| 증가 | `-s +SIZE` | 현재 크기에서 **SIZE만큼 증가** | `truncate -s +1M a.txt` |
| 감소 | `-s -SIZE` | 현재 크기에서 **SIZE만큼 감소** | `truncate -s -1K a.txt` |
| 단위 | `K, M, G` | KB, MB, GB 단위 사용 가능 | `-s 10M` |
| 조건 | `-c` | 파일이 존재할 때만 실행 | `truncate -c -s 0 a.txt` |
| 기준 | `-r file` | 다른 파일 크기를 기준으로 설정 | `truncate -r b.txt a.txt` |

---

## find

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 이름 | `find path -name name` | 이름 검색 | `find / -name '*.conf'` |
| 타입 | `-type f/d` | 파일/디렉토리 | `find . -type f` |
| 시간 | `-mtime` | 변경 | `find . -mtime -1` |
| 빈파일 | `-empty` | 크기 0 | `find . -empty` |

---

## tar (압축 / 백업)

| 구분 | 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|---|
| 기본 구조 |  | `tar [옵션] 파일 대상` | tar 명령 기본 형태 | `tar cvf a.tar a` |
| 생성 | `-c` | `tar -cvf` | 아카이브 생성 | `tar cvf a.tar a` |
| 해제 | `-x` | `tar -xvf` | 아카이브 해제 | `tar xvf a.tar` |
| 파일 지정 | `-f` | `-f 파일명` | 아카이브 파일 지정 | `tar cvf a.tar a` |
| 진행 표시 | `-v` | `-v` | 작업 과정 출력 | `tar cvvf a.tar a` |
| gzip 압축 | `-z` | `-cvfz` | gzip으로 압축 | `tar cvfz a.tgz a` |
| bzip2 압축 | `-j` | `-cvfj` | bzip2 압축 | `tar cvfj a.tar.bz2 a` |
| 퍼미션 유지 | `-p` | `-p` | 권한 유지 | `tar cvfp a.tar a` |
| 증분 백업 | `-g` | `-g snapshot` | 증분 백업 | `tar cvfz inc.tar.gz -g snap a` |

---

## 압축 도구

| 명령어 | 형식 | 설명 | 예시 |
|---|---|---|---|
| gzip | `gzip file` | 압축 | `gzip a.txt` |
| gzip -d | `gzip -d file.gz` | 해제 | `gzip -d a.gz` |
| bzip2 | `bzip2 file` | 압축 | `bzip2 a.txt` |

---

## 파일 시간 정보

| 항목 | 의미 | 예시 |
|---|---|---|
| atime | 접근 시간 | `cat a.txt` |
| mtime | 내용 변경 | `vi a.txt` |
| ctime | 속성 변경 | `chmod 755 a.txt` |

---

## touch

| 옵션 | 형식 | 설명 | 예시 |
|---|---|---|---|
| 기본 | `touch file` | 파일 생성 | `touch a.txt` |
| 시간 | `-t YYYYMMDDhhmm.ss` | 시간 지정 | `touch -t 202001010000 a` |
| 접근 | `-a` | atime | `touch -a a.txt` |
| 수정 | `-m` | mtime | `touch -m a.txt` |
