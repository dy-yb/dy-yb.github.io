---
title: Github repository 연결(+ push error)
date: 2022-02-11 14:50:00
tags: github mac git study
---

(macOS 기준 입니다.)

## 로컬 저장소에 있는 프로젝트를 Git 원격 저장소로 연결하는 법

1. 로컬 저장소에 프로젝트를 생성후 해당 경로로 진입
2. git 초기화
    
    ```bash
    git init
    ```
    
3. 원격 저장소에 새로운 repo 생성 (아무것도 없는 empty repository 상태로 만들 것)
    
    ![스크린샷 2022-02-11 14.31.11.png](https://user-images.githubusercontent.com/40792935/153543793-8e15f6c1-8a62-4e6c-b796-6ca26331aac2.png)
    
4. 새로 만든 repository 주소를 로컬에 연결하기
    
    ```bash
    git remote add origin https://github.com/username/repository.git
    ```
    
    - repository 주소는 아래 두 곳에서 복사하면 된다. (HTTPS)
        
        <img width="1245" alt="스크린샷_2022-02-11_14 45 36" src="https://user-images.githubusercontent.com/40792935/153543836-79fcbb26-0f80-4b55-a4e6-333d9f6ef45b.png">

        
        <img width="401" alt="스크린샷_2022-02-11_14 44 54" src="https://user-images.githubusercontent.com/40792935/153543834-b3bd0b96-922e-4a4f-9f1a-efaa39544617.png">
        
5. git push
    
    ```bash
    git push -u origin main
    ```
    

+) 이 때 나는 private repository로 생성해서 그런지(아닐지도..?) 계속해서 다음과 같은 에러가 발생했다.

```bash
error: src refspec main does not match any
error: failed to push some refs to 'https://github.com/dy-yb/json_Example.git'
```

![스크린샷_2022-02-11_14 33 01](https://user-images.githubusercontent.com/40792935/153543832-a96264e3-5ecd-4f64-8034-72bb4b091001.png)

해당 에러에 관하여 검색해봤더니 github가 pull을 진행하지 않고 push를 할 경우에 원격 저장소에 저장된 내용이 삭제 될 수 도 있어 에러를 발생시키는 것이라고 한다. 해결 방법은 만능 해결사인 “처음부터 다시 시작!” 하는 것을 추천한다고 한다! ^^; 

내가 해결한 방법은 아래와 같다.

### src refspec main does not match any 에러 해결 방법

1. 초기화를 위해 기존 .git file 삭제
2. git 초기화
3. 현재까지 상태 commit
    
    ```bash
    git add .
    git commit -m "initial commit"
    ```
    
- 참고한 글에서는 이 단계까지 수행하면 git cofig를 통해 git name과 이메일을 입력하라고 메세지가 뜬다고하는데 나는 아무런 말도 없었다.. 하지만 직접 하면되니 걱정말자..!
1. git config
    - github username과 github에 연결 된 이메일을 입력하면 된다.
    
    ```bash
    git config user.name "username"
    git config user.email "user@email"
    ```
    
2. git 연결 & push
    
    ```bash
    git remote add origin "repourl"
    git push -u origin main
    ```
    
3. 끝~~

![스크린샷_2022-02-11_14 47 45](https://user-images.githubusercontent.com/40792935/153543828-9f20f746-0693-43c9-a988-442d8a1a7479.png)

### Refrence

[깃허브에 push 하는 방법과 error : src refspec master does not match any 에러 해결 방법](https://junheejang.tistory.com/221)
