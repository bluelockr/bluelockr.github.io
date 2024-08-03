## ⭐Windows 기준

### 💧1. Scoop 설치
Powershell에 다음 문구를 붙여넣고 실행

```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

[https://scoop.sh/](https://scoop.sh/)


### 💧2. Git, Hugo-extended 설치

```bash
scoop install git
scoop install hugo-extended
```
[https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98)

[https://gohugo.io/installation/windows/](https://gohugo.io/installation/windows/)

### 💧3. VSCODE 설치

```bash
scoop bucket add extras
scoop install vscode
```

[https://dalinaum.github.io/windows/2021/03/14/scoop-visual-studio-code.html#google_vignette](https://dalinaum.github.io/windows/2021/03/14/scoop-visual-studio-code.html#google_vignette)

### 💧4. VSCODE를 열고 Clone from Git

### 💧5. Github 유저 로그인

Terminal-New Terminal 또는 Ctrl+Shift+`

```bash
git config --global user.name "유저명"
git config --global user.email "유저 이메일"
```

### 💧6. 작업 후 커밋

```bash
git add .
git commit -m "커밋 문구"
git push origin main
```

## ⭐Workflow 추가 혹은 수정

참고 : 
<br>
[https://gohugo.io/hosting-and-deployment/hosting-on-github/](https://gohugo.io/hosting-and-deployment/hosting-on-github/)