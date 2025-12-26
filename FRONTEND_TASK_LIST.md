# SafeBell 프론트엔드 수정 작업 리스트

> **작성일**: 2024년  
> **목적**: 다른 AI가 한눈에 파악하고 작업할 수 있도록 체계적으로 정리

---

## 📋 작업 우선순위

### 🔴 **우선순위 1: 기존 레이아웃 디테일 수정**
### 🟡 **우선순위 2: 긴급 신고 문자 전송 구현**
### 🟢 **우선순위 3: 수정/추가 레이아웃 구현**

---

## 🔴 우선순위 1: 기존 레이아웃 디테일 수정

### 1.1 랜딩 페이지 버튼 교체

**파일 경로**: `feature/auth/src/main/java/com/example/auth/ui/LandingScreen.kt`

**수정 내용**:
- 현재: "회원가입" 버튼이 상단, "로그인하기" 텍스트가 하단
- 변경: **"로그인" 버튼을 상단으로**, "회원가입하기" 텍스트를 하단으로 교체

**코드 위치**: 
- Line 71-87: 회원가입 버튼
- Line 89-112: 로그인 텍스트

**작업**: 두 영역의 위치와 텍스트 교체

---

### 1.2 회원가입 - 아이디 입력 추가

**파일 경로**: 
- `feature/auth/src/main/java/com/example/auth/ui/PhoneNumberScreen.kt`
- `feature/auth/src/main/java/com/example/auth/ui/PasswordScreen.kt` (확인 필요)

**수정 내용**:
- 전화번호 입력 후 **아이디 입력 화면 추가**
- 회원가입 플로우: 전화번호 → **아이디** → 비밀번호 → 프로필

**필요 작업**:
1. `PhoneNumberScreen.kt` 다음에 `UserIdScreen.kt` 새로 생성
2. `AppRoute.kt`에 `USER_ID_ROUTE` 추가
3. `AppNavHost.kt`에 라우팅 추가
4. `AuthViewModel`에 아이디 저장 로직 추가

**API 필요**: ❌ (프론트만)

---

### 1.3 연락처 등록 로직 수정 (자동 친구 → 요청/수락)

**파일 경로**: 
- `feature/auth/src/main/java/com/example/auth/ui/ContactRegistrationScreen.kt`
- `feature/auth/src/main/java/com/example/auth/ui/ContactRegistrationViewModel.kt`

**현재 상태**: 
- Line 88: `viewModel.sendContactRequest(phoneNumber)` - 이미 요청 로직 존재
- Line 191-201: `checkUserExists` 후 자동으로 선택/초대 처리

**수정 내용**:
- ❌ **자동 친구 처리 제거**
- ✅ **모든 연락처는 "요청" 상태로만 전송**
- ✅ **상대가 수락해야만 친구 관계 성립**

**필요 작업**:
1. `ContactRegistrationScreen.kt` Line 191-201 로직 수정
2. 요청 상태 UI 추가 (요청됨, 수락됨, 대기중)
3. 친구 관리 화면에서 수락/거절 처리

**API 필요**: ✅
- 친구 요청 API (이미 존재할 수 있음)
- 친구 수락/거절 API
- 친구 상태 조회 API

---

### 1.4 히스토리 필터링 (최근 기간)

**파일 경로**: 
- `feature/home/src/main/java/com/selfbell/home/ui/HistoryScreen.kt`
- `feature/home/src/main/java/com/selfbell/home/ui/HistoryViewModel.kt`
- `feature/home/src/main/java/com/selfbell/home/ui/HistoryFilterComposables.kt`

**현재 상태**: 
- Line 61-77: `HistoryDateFilterDropdown` 존재
- `HistoryDateFilter` enum에 기간 필터 있음

**수정 내용**:
- 필터 옵션 확인 및 필요시 추가 (최근 1일, 1주, 1개월, 전체 등)

**확인 필요**: `HistoryDateFilter` enum 값 확인

---

### 1.5 로그아웃 기능 구현

**파일 경로**: 
- `feature/settings/src/main/java/com/selfbell/settings/ui/SettingsScreen.kt` (Line 130-136)
- `feature/settings/src/main/java/com/selfbell/settings/ui/SettingsViewModel.kt` (Line 39-42)

**현재 상태**: 
- Line 133: `// TODO: 로그아웃 로직` 주석만 있음

**수정 내용**:
1. `SettingsViewModel.logout()` 구현
2. 토큰 삭제 (DataStore)
3. 로그인 화면으로 네비게이션

**API 필요**: ❌ (로컬 데이터 삭제만)

---

### 1.6 출발지 입력 추가 (안심귀가)

**파일 경로**: 
- `feature/escort/src/main/java/com/selfbell/escort/ui/EscortScreen.kt` (Line 140)
- `feature/escort/src/main/java/com/selfbell/escort/ui/EscortViewModel.kt`

**현재 상태**: 
- Line 140: `startLocationName = "현재 위치"` - 하드코딩

**수정 내용**:
1. 출발지 선택 UI 추가 (현재 위치 / 즐겨찾기 / 직접 입력)
2. `EscortViewModel`에 출발지 선택 로직 추가
3. `EscortSetupSheet`에 출발지 입력 필드 추가

**API 필요**: ❌ (로컬 로직)

---

### 1.7 즐겨찾기 주소 로직 수정

**파일 경로**: 
- `feature/escort/src/main/java/com/selfbell/escort/ui/EscortViewModel.kt`

**현재 상태**: 
- 메인 주소로만 등록됨

**수정 내용**:
- 즐겨찾기 주소를 별도로 저장/관리
- 메인 주소와 분리

**API 필요**: ✅
- 즐겨찾기 주소 추가/삭제/조회 API

---

### 1.8 예상 시간 레이아웃 수정 (타이머와 동일하게)

**파일 경로**: 
- `feature/escort/src/main/java/com/selfbell/escort/ui/EscortScreen.kt`
- `EscortSetupSheet` 컴포저블 (확인 필요)

**수정 내용**:
- 예상 시간 입력 UI를 타이머 입력 UI와 동일한 스타일로 통일

**확인 필요**: `EscortSetupSheet` 파일 위치

---

### 1.9 홈화면 레이아웃 수정

**파일 경로**: 
- `feature/home/src/main/java/com/selfbell/home/ui/HomeScreen.kt`
- `feature/home/src/main/java/com/selfbell/home/ui/HomeViewModel.kt`

**수정 항목**:

#### 1.9.1 안심벨까지 추천 경로 추가
- 안심벨 클릭 시 경로 안내 기능 추가
- 네이버 지도 경로 API 연동

#### 1.9.2 지도 디폴트 화면 스케일 확대
- `ReusableNaverMap` 초기 줌 레벨 조정
- `HomeViewModel`에서 카메라 초기 위치 설정

#### 1.9.3 "내 주변 탐색 모달" 텍스트 변경
- "내 주변 탐색 모달" → **"주변 안심벨 검색"** (주소지 입력)
- `HomeScreen.kt` Line 225-226 주소 검색 UI

#### 1.9.4 새로고침 버튼 추가
- 현재 위치 기준으로 주변 안심벨 검색
- `HomeViewModel`에 `refreshNearbyBells()` 메서드 추가

#### 1.9.5 필터 버튼 변경
- 현재: 안심벨/성범죄자 토글 (Line 201-209)
- 변경: **안심벨, 성범죄자, 경찰서** 3개 필터 버튼

#### 1.9.6 키보드 입력 시 SafeArea 추가
- `imePadding()` 추가하여 키보드 올라올 때 UI 조정
- `HomeScreen.kt`에 `Modifier.imePadding()` 적용

#### 1.9.7 "내 주변 탐색 모달" 최소 크기 추가
- 지도 화면 시야 확보를 위한 모달 최소 높이 설정
- `ModalBottomSheet` 높이 제한

#### 1.9.8 현재 위치 버튼 가시화
- 현재 바텀바에 가려져 안 보임
- 위치 버튼을 상단으로 이동 또는 z-index 조정

#### 1.9.9 안심벨 클릭 시 UI 변경
- "상세 정보 바텀 모달" → **"말풍선"** (피그마 참고)
- "내 주변 탐색 모달" → **"경로 안내 모달"** (피그마 참고)
- `MapInfoBalloon` 컴포저블 수정 (Line 212)

#### 1.9.10 사용자 닉네임 표시 및 클릭
- 상단에 사용자 닉네임 표시 (현재 "SafeBell 사용자" 하드코딩)
- 닉네임 클릭 시 마이페이지로 이동
- `HomeViewModel`에서 사용자 정보 조회

**API 필요**: 
- ✅ 사용자 정보 조회 API
- ✅ 경찰서 위치 API (필터용)
- ✅ 경로 안내 API (네이버 지도)

---

### 1.10 닉네임 설정 레이아웃 수정

**파일 경로**: 
- `feature/auth/src/main/java/com/example/auth/ui/ProfileRegisterScreen.kt`
- `feature/settings/src/main/java/com/selfbell/settings/ui/SettingsScreen.kt`

**수정 내용**:
- 프로필 등록 화면 레이아웃 개선
- 마이페이지에서 닉네임 수정 UI 추가

**API 필요**: ✅
- 닉네임 수정 API

---

### 1.11 긴급 알림 받기 삭제

**파일 경로**: 
- `feature/settings/src/main/java/com/selfbell/settings/ui/SettingsScreen.kt` (Line 112-117)

**수정 내용**:
- Line 112-117: `SettingsSwitchItem` "긴급 알림 받기" 삭제

---

### 1.12 권한 설정 로직 수정

**파일 경로**: 
- `feature/auth/src/main/java/com/example/auth/ui/PermissionScreen.kt`

**수정 내용**:
- 권한 요청 순서 및 로직 개선
- 권한 설정 화면 레이아웃 추가 (마이페이지에서 접근)

**확인 필요**: 현재 권한 요청 로직 (Line 221-234)

---

### 1.13 현재 위치 기준 새로고침 → 주변 안심벨 업데이트

**파일 경로**: 
- `feature/home/src/main/java/com/selfbell/home/ui/HomeViewModel.kt`

**수정 내용**:
- 새로고침 버튼 클릭 시 현재 위치 기준으로 안심벨 재조회
- `refreshNearbyBells()` 메서드 구현

**API 필요**: ✅
- 현재 위치 기반 안심벨 조회 API (이미 존재할 수 있음)

---

### 1.14 예상시간 FCM 알림

**파일 경로**: 
- `app/src/main/java/com/selfbell/app/fcm/MyFirebaseMessagingService.kt`
- `feature/escort/src/main/java/com/selfbell/escort/ui/EscortViewModel.kt`

**수정 내용**:
- 예상 도착 시간이 지나면 보호자에게 FCM 알림 전송
- 백그라운드에서 시간 체크 로직 필요

**API 필요**: ✅
- FCM 알림 전송 API (서버 측)

---

## 🟡 우선순위 2: 긴급 신고 문자 전송 구현

### 2.1 긴급 신고 로직 분리 (보호자/피보호자)

**파일 경로**: 
- `feature/home/src/main/java/com/selfbell/home/ui/MessageReportFlow.kt`
- `feature/home/src/main/java/com/selfbell/home/ui/HomeViewModel.kt` (Line 358-397)

**현재 상태**: 
- 안심귀가와 긴급신고 로직이 혼재되어 있음

**수정 내용**:
- ❌ **안심귀가 로직과 완전히 분리**
- ✅ **긴급신고 전용 플로우 구현**

**작업 흐름**:
1. 회원가입 시 긴급신고 전송 대상 번호 설정
2. 긴급 신고 버튼 클릭
3. 사전 설정한 번호 목록 표시
4. 템플릿 선택
5. 문자 전송

**API 필요**: ✅
- 긴급신고 대상 번호 설정 API
- 긴급신고 템플릿 조회 API
- 문자 전송 API

---

### 2.2 긴급신고 템플릿 바텀모달 축소

**파일 경로**: 
- `feature/home/src/main/java/com/selfbell/home/ui/MessageReportFlow.kt`
- `core/src/main/java/com/selfbell/core/ui/composables/MessageReportBottomSheet.kt`

**수정 내용**:
- 현재 아래로 스크롤 가능하나 화면 위에 붙어서 잘 안 보임
- 모달 높이 조정 및 스크롤 영역 개선

---

### 2.3 긴급신고 전송 대상 번호 관리

**파일 경로**: 
- `feature/settings/src/main/java/com/selfbell/settings/ui/SettingsScreen.kt`

**수정 내용**:
- 마이페이지에 "긴급 신고 번호 관리" 메뉴 추가
- 번호 추가/삭제/수정 기능

**API 필요**: ✅
- 긴급신고 번호 CRUD API

---

## 🟢 우선순위 3: 수정/추가 레이아웃 구현

### 3.1 안심귀가 실시간 귀가중 화면 추가

**파일 경로**: 
- `feature/escort/src/main/java/com/selfbell/escort/ui/EscortScreen.kt` (Line 181-218)
- 새 파일: `SafeWalkInProgressScreen.kt` (생성 필요)

**접근 경로**:
1. 안심귀가 알림 클릭 시
2. 히스토리 화면에서 피보호자 귀가중일 때
3. 이탈 알림 (타원 형태 구현)
4. 예상 시간 넘었을 시 보호자에게 알림

**필요 작업**:
1. 실시간 귀가중 화면 UI 구현
2. 이탈 감지 로직 (경로 이탈 시 알림)
3. 예상 시간 초과 알림 (FCM)

**API 필요**: ✅
- 실시간 위치 조회 API
- 이탈 감지 API
- FCM 알림 API

---

### 3.2 마이페이지 추가 기능

**파일 경로**: 
- `feature/settings/src/main/java/com/selfbell/settings/ui/SettingsScreen.kt`

**추가 항목**:

#### 3.2.1 프로필 닉네임 수정 레이아웃
- Line 77-83: "프로필 관리" 클릭 시 닉네임 수정 화면
- `ProfileRegisterScreen.kt` 재사용 또는 새 화면 생성

#### 3.2.2 프로필 닉네임 수정 API 연동
- `AuthViewModel` 또는 `SettingsViewModel`에 닉네임 수정 메서드 추가

#### 3.2.3 메인 주소 수정 레이아웃
- Line 104-109: "메인 주소 설정" 클릭 시 주소 수정 화면
- `AddressRegisterScreen.kt` 재사용

#### 3.2.4 긴급 알림 받기 버튼 삭제
- Line 112-117 삭제

#### 3.2.5 로그아웃 기능
- Line 130-136 구현 (1.5 참고)

#### 3.2.6 권한 설정 레이아웃
- Line 119-125: "권한 설정" 클릭 시 권한 설정 화면
- `PermissionScreen.kt` 재사용

#### 3.2.7 문의사항 버튼 추가
- 새 메뉴 아이템 추가
- 문의사항 화면 생성 필요

#### 3.2.8 긴급 신고 번호 관리
- 새 메뉴 아이템 추가
- 번호 추가/삭제/수정 화면 생성

#### 3.2.9 친구 관리 (3개 페이지)
- Line 86-91: "친구 관리" 클릭
- 친구 목록, 요청 목록, 초대 목록 3개 탭/화면
- `ContactListScreen.kt` 수정 필요

**API 필요**: ✅
- 닉네임 수정 API
- 메인 주소 수정 API
- 문의사항 API
- 긴급신고 번호 관리 API
- 친구 관리 API (이미 존재할 수 있음)

---

### 3.3 알림 페이지 구현

**파일 경로**: 
- 새 파일: `feature/alerts/src/main/java/com/selfbell/alerts/ui/NotificationScreen.kt` (생성 필요)

**수정 내용**:
- 알림 목록 화면 구현
- FCM 알림 히스토리 표시

**API 필요**: ✅
- 알림 목록 조회 API

---

### 3.4 긴급신고 템플릿 화면

**파일 경로**: 
- `feature/home/src/main/java/com/selfbell/home/ui/MessageReportFlow.kt`

**수정 내용**:
- 템플릿 선택 UI 개선
- 템플릿 커스터마이징 기능 (선택사항)

**API 필요**: ✅
- 템플릿 조회 API
- 템플릿 저장 API (선택사항)

---

## 📝 추가 확인 사항

### 친구 관리 로직
**파일 경로**: 
- `feature/settings/src/main/java/com/selfbell/settings/ui/ContactListScreen.kt`
- `feature/settings/src/main/java/com/selfbell/settings/ui/ContactsViewModel.kt`

**현재 상태**: 
- "이게 왜 되지?" 상태
- 로직 이해 필요

**작업**: 
- 친구 관리 로직 전체 재검토 및 수정

---

## 🔗 관련 파일 목록

### 핵심 파일
- `app/src/main/java/com/selfbell/app/navigation/AppNaveHost.kt` - 네비게이션
- `core/src/main/java/com/selfbell/core/navigation/AppRoute.kt` - 라우트 정의
- `feature/auth/src/main/java/com/example/auth/ui/` - 인증 관련
- `feature/home/src/main/java/com/selfbell/home/ui/` - 홈 화면
- `feature/escort/src/main/java/com/selfbell/escort/ui/` - 안심귀가
- `feature/settings/src/main/java/com/selfbell/settings/ui/` - 설정/마이페이지
- `feature/alerts/src/main/java/com/selfbell/alerts/ui/` - 알림

### ViewModel 파일
- `feature/auth/src/main/java/com/example/auth/ui/ContactRegistrationViewModel.kt`
- `feature/home/src/main/java/com/selfbell/home/ui/HomeViewModel.kt`
- `feature/escort/src/main/java/com/selfbell/escort/ui/EscortViewModel.kt`
- `feature/settings/src/main/java/com/selfbell/settings/ui/SettingsViewModel.kt`

---

## ✅ 체크리스트

### 우선순위 1
- [ ] 1.1 랜딩 페이지 버튼 교체
- [ ] 1.2 회원가입 아이디 입력 추가
- [ ] 1.3 연락처 등록 로직 수정
- [ ] 1.4 히스토리 필터링
- [ ] 1.5 로그아웃 기능
- [ ] 1.6 출발지 입력 추가
- [ ] 1.7 즐겨찾기 주소 로직 수정
- [ ] 1.8 예상 시간 레이아웃 수정
- [ ] 1.9 홈화면 레이아웃 수정 (10개 항목)
- [ ] 1.10 닉네임 설정 레이아웃 수정
- [ ] 1.11 긴급 알림 받기 삭제
- [ ] 1.12 권한 설정 로직 수정
- [ ] 1.13 현재 위치 기준 새로고침
- [ ] 1.14 예상시간 FCM 알림

### 우선순위 2
- [ ] 2.1 긴급 신고 로직 분리
- [ ] 2.2 긴급신고 템플릿 바텀모달 축소
- [ ] 2.3 긴급신고 전송 대상 번호 관리

### 우선순위 3
- [ ] 3.1 안심귀가 실시간 귀가중 화면
- [ ] 3.2 마이페이지 추가 기능 (9개 항목)
- [ ] 3.3 알림 페이지 구현
- [ ] 3.4 긴급신고 템플릿 화면

---

## 📌 참고사항

1. **피그마 디자인 참고**: 안심벨 말풍선, 경로 안내 모달 등
2. **API 연동**: 백엔드와 협의하여 필요한 API 확인
3. **테스트**: 각 기능 구현 후 충분한 테스트 필요
4. **에러 처리**: 네트워크 오류, 권한 거부 등 예외 상황 처리

---

**작성 완료** ✅

