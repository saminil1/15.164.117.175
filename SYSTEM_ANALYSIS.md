# 시스템 분석 보고서

**분석 일시:** 2025-10-08 01:07 KST
**분석 대상:** 15.164.117.175

---

## 시스템 환경

- **OS:** Debian Linux 6.1.0-40-cloud-amd64
- **커널:** #1 SMP PREEMPT_DYNAMIC Debian 6.1.153-1
- **아키텍처:** x86_64
- **호스트명:** ip-172-31-36-216
- **작업 디렉토리:** /opt/bitnami

---

## 리소스 현황

### 디스크 사용량
```
Filesystem       Size  Used Avail Use% Mounted on
/dev/nvme0n1p1    40G   19G   19G  51% /
```
- **총 용량:** 40GB
- **사용량:** 19GB (51%)
- **여유 공간:** 19GB

### 메모리 사용량
```
               total        used        free      shared  buff/cache   available
Mem:           1.9Gi       1.2Gi       247Mi        28Mi       675Mi       711Mi
Swap:             0B          0B          0B
```
- **총 메모리:** 1.9GB
- **사용 중:** 1.2GB
- **여유:** 247MB
- **가용:** 711MB
- **스왑:** 미설정 (0B)

---

## 실행 중인 프로세스 (메모리 사용 Top 10)

| 순위 | 프로세스 | PID | %MEM | 메모리(MB) | 설명 |
|------|----------|-----|------|------------|------|
| 1 | mariadbd | 949 | 13.6% | 267MB | MariaDB 데이터베이스 서버 |
| 2 | Cursor Server (ExtensionHost) | 1598 | 13.2% | 258MB | VS Code 확장 호스트 |
| 3 | Claude Code | 1758 | 10.3% | 202MB | Claude Code 에이전트 |
| 4 | Cursor Server (Main) | 1573 | 5.5% | 108MB | Cursor 메인 서버 |
| 5 | PHP Cron | 1977 | 3.1% | 62MB | Moodle Cron 작업 |
| 6 | Cursor Server (PtyHost) | 1640 | 3.0% | 60MB | Cursor 터미널 호스트 |
| 7 | PHP Cron | 2023 | 2.8% | 56MB | Moodle Cron 작업 |
| 8 | Cursor Server (FileWatcher) | 1619 | 2.8% | 56MB | Cursor 파일 감시 |
| 9 | PHP Cron | 2057 | 2.6% | 51MB | Moodle Cron 작업 |

---

## Claude 관련 프로세스

### 1. Claude Code (PID: 1758)
- **메모리 사용:** 206MB (10.8%)
- **기능:** 현재 실행 중인 Claude Code 에이전트
- **상태:** 실행 중

### 2. Moodle-SuperClaude Bridge (PID: 440)
- **프로세스:** `/home/bitnami/superclaude-env/bin/python /home/bitnami/moodle-superclaude-bridge/bridge_system.py`
- **메모리 사용:** 36MB (1.8%)
- **포트:** 5000
- **기능:**
  - Moodle(PHP)와 SuperClaude(Python) 연동 Flask API 서버
  - 코스 분석 API 제공
  - 사용자 학습 패턴 분석
  - DB 쿼리 최적화
  - 교육 콘텐츠 생성
  - SuperClaude 명령 실행 인터페이스

#### 제공 API 엔드포인트:
- `GET /api/health` - 상태 확인
- `GET /api/analyze/course/<id>` - 코스 분석
- `GET /api/analyze/user/<id>` - 사용자 학습 패턴 분석
- `POST /api/optimize/query` - DB 쿼리 최적화
- `POST /api/generate/content` - 교육 콘텐츠 생성
- `POST /api/superclaude/command` - SuperClaude 명령 실행

#### 삭제 시 영향:
- Moodle의 AI 기반 분석/생성 기능 중단
- Moodle 기본 LMS 기능은 정상 작동 (부가 기능만 중단)

#### 보안 이슈:
- ⚠️ DB 비밀번호가 평문으로 하드코딩됨 (bridge_system.py:29)
- 권장사항: 환경변수로 이동 필요

---

## 설치된 구성요소

| 구성요소 | 경로 | 소유자 | 설명 |
|----------|------|--------|------|
| Apache | /opt/bitnami/apache | root | 웹 서버 |
| MariaDB | /opt/bitnami/mariadb | root | 데이터베이스 |
| PHP | /opt/bitnami/php | root | PHP 런타임 |
| Moodle | /bitnami/moodle (symlink) | - | LMS 플랫폼 |
| phpMyAdmin | /opt/bitnami/phpmyadmin | bitnami | DB 관리 도구 |
| Nami | /opt/bitnami/nami | root | Bitnami 패키지 관리자 |
| Gonit | /opt/bitnami/gonit | root | 프로세스 모니터링 |

---

## 주요 이슈 및 권장사항

### 🔴 긴급
1. **메모리 부족 위험**
   - 현재 가용 메모리: 247MB
   - 스왑 미설정으로 메모리 부족 시 시스템 불안정 가능
   - 권장: 스왑 파일 생성 (최소 2GB)

### 🟡 주의
2. **여러 PHP Cron 프로세스 동시 실행**
   - 3개의 Moodle Cron 프로세스 실행 중
   - 메모리 중복 사용 (총 169MB)
   - 확인 필요: Cron 설정 중복 여부

3. **보안: DB 인증정보 노출**
   - bridge_system.py에 DB 비밀번호 평문 저장
   - 권장: 환경변수 또는 설정 파일 사용

### ℹ️ 정보
4. **디스크 사용량**
   - 현재 51% 사용 (양호)
   - 모니터링 권장

---

## 시스템 설정 파일

### Bitnami 구성 정보
- **파일:** `/opt/bitnami/properties.ini`
- **컴포넌트 정보:** `/opt/bitnami/.bitnami_components.json`
- **제어 스크립트:** `/opt/bitnami/ctlscript.sh`

### Moodle 설정
- **설정 파일:** `/opt/bitnami/moodle/config.php`
- **데이터 디렉토리:** `/bitnami/moodledata`
- **WWW Root:** http://15.164.117.175

---

## 결론

전반적으로 시스템은 안정적으로 작동 중이나, **메모리 관리**에 주의가 필요합니다. 특히 스왑 미설정 상태에서 메모리 부족 시 시스템이 불안정해질 수 있으므로 스왑 파일 생성을 권장합니다.

Moodle-SuperClaude Bridge는 선택적 부가 기능이므로, 메모리 부족 시 중단을 고려할 수 있습니다.
