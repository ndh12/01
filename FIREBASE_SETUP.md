# Firebase Configuration Setup Guide

## 🔥 Firebase 설정 방법

현재 `src/firebase.js` 파일에 Firebase API 키가 필요합니다.

### 1단계: Firebase Console에서 API 키 가져오기

1. [Firebase Console](https://console.firebase.google.com/)에 접속
2. 프로젝트 `product-2778e` 선택
3. 프로젝트 설정 (⚙️ 아이콘) 클릭
4. "일반" 탭에서 "내 앱" 섹션으로 스크롤
5. 웹 앱이 없다면 "앱 추가" 클릭 → 웹(</>) 선택
6. 앱 닉네임 입력 (예: "PC-ERP Pro Web")
7. Firebase SDK 구성 정보 복사

### 2단계: firebase.js 파일 업데이트

`src/firebase.js` 파일을 열고 다음 값들을 업데이트하세요:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDHwDueHtTGnuoedNTVaPhERZ5eT10sFjU",
  authDomain: "product-2778e.firebaseapp.com",
  projectId: "product-2778e",
  storageBucket: "product-2778e.firebasestorage.app",
  messagingSenderId: "212439259428",
  appId: "1:212439259428:web:72f17df9937f73aa1fbbb1",
  measurementId: "G-6BG4WM57P5"
};
```

### 3단계: Firebase Authentication 활성화

1. Firebase Console에서 "Authentication" 메뉴 클릭
2. "시작하기" 버튼 클릭
3. "Sign-in method" 탭 선택
4. "이메일/비밀번호" 활성화
5. 저장

### 4단계: Firestore Database 생성

1. Firebase Console에서 "Firestore Database" 메뉴 클릭
2. "데이터베이스 만들기" 클릭
3. "테스트 모드로 시작" 선택 (개발 중)
4. 지역 선택 (예: asia-northeast3 - 서울)
5. "사용 설정" 클릭

### 5단계: Firestore 보안 규칙 설정

Firestore Database → 규칙 탭에서 다음 규칙을 설정하세요:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 인증된 사용자만 자신의 데이터에 접근 가능
    match /inventory/{document} {
      allow read, write: if request.auth != null && 
                         resource.data.userId == request.auth.uid;
      allow create: if request.auth != null;
    }
    
    match /partners/{document} {
      allow read, write: if request.auth != null && 
                         resource.data.userId == request.auth.uid;
      allow create: if request.auth != null;
    }
    
    match /transactions/{document} {
      allow read, write: if request.auth != null && 
                         resource.data.userId == request.auth.uid;
      allow create: if request.auth != null;
    }
    
    match /serials/{document} {
      allow read, write: if request.auth != null && 
                         resource.data.userId == request.auth.uid;
      allow create: if request.auth != null;
    }
  }
}
```

### 6단계: 애플리케이션 실행

```bash
npm run dev
```

브라우저에서 http://localhost:3000 접속

---

## ✅ 완료 확인

1. 랜딩 페이지에서 "시작하기" 버튼 클릭
2. 회원가입으로 새 계정 생성
3. 로그인 후 대시보드 접근 확인
4. 재고 등록 테스트
5. 거래처 등록 테스트
6. 입출고 거래 등록 테스트

---

## 🔧 문제 해결

### "Firebase: Error (auth/configuration-not-found)"
→ Firebase Authentication이 활성화되지 않았습니다. 3단계를 다시 확인하세요.

### "Missing or insufficient permissions"
→ Firestore 보안 규칙이 올바르지 않습니다. 5단계를 다시 확인하세요.

### "Firebase: Error (auth/invalid-api-key)"
→ API 키가 올바르지 않습니다. 2단계를 다시 확인하세요.
