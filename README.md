# Flip Clock

## 실행 방법 (Windows 11)

### 1) PowerShell에서 실행
```powershell
cd <프로젝트_폴더>
python -m http.server 8000
```
브라우저에서 `http://127.0.0.1:8000/`로 접속합니다.

### 2) 시작 스크립트 사용
- `start-server.ps1` 또는 `start-server.cmd`를 실행하면 현재 폴더 기준으로 서버가 열립니다.
- 실행 후 `http://127.0.0.1:8000/`로 접속하세요.

### 3) 파일 직접 열기
`index.html`을 더블클릭해도 동작합니다.

## 미리보기/접속 문제 해결
- **404 오류가 나는 경우**: 서버를 실행한 폴더가 프로젝트 폴더인지 확인하세요. `start-server.ps1`/`start-server.cmd`로 실행하면 자동으로 올바른 위치에서 시작됩니다.
- **포트가 사용 중인 경우**: `python -m http.server 8001`처럼 다른 포트를 사용한 뒤 `http://127.0.0.1:8001/`로 접속하세요.
- **python 명령이 없는 경우**: `py -m http.server 8000` 또는 `python3 -m http.server 8000`을 시도하세요.

## exe 파일 만들기 (Windows 11)
웹 페이지를 데스크톱 앱으로 묶어 exe로 배포하려면 **Electron** 방식이 가장 간단합니다.

### 1) 준비
1. Node.js LTS 설치: https://nodejs.org/
2. 아래 명령을 프로젝트 폴더에서 실행합니다.

```powershell
npm init -y
npm install --save-dev electron electron-builder
```

### 2) 앱 엔트리 추가
프로젝트 루트에 `main.js`를 만들고 아래 내용을 넣습니다.

```js
const { app, BrowserWindow } = require("electron");
const path = require("path");

const createWindow = () => {
  const win = new BrowserWindow({
    width: 1000,
    height: 700,
    backgroundColor: "#0f172a",
  });

  win.loadFile(path.join(__dirname, "index.html"));
};

app.whenReady().then(createWindow);
```

### 3) package.json에 빌드 스크립트 추가
`package.json`에 아래 항목을 추가합니다.

```json
{
  "main": "main.js",
  "scripts": {
    "start": "electron .",
    "build": "electron-builder"
  },
  "build": {
    "appId": "com.example.flipclock",
    "productName": "FlipClock",
    "files": [
      "index.html",
      "main.js"
    ],
    "win": {
      "target": "nsis"
    }
  }
}
```

### 4) 실행/빌드
```powershell
npm run start
npm run build
```
빌드가 끝나면 `dist/` 폴더 안에 `FlipClock Setup.exe`가 생성됩니다.
