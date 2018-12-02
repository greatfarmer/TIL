# Git

## 깃을 위한 flight rules
- https://github.com/k88hudson/git-flight-rules/blob/master/README_kr.md

## 자주 사용하는 깃 명령어 모음
- https://github.com/jeonghwan-kim/git-usage

## 좋은 커밋 메시지 작성하기
- https://item4.github.io/2016-11-01/How-to-Write-a-Git-Commit-Message/
- https://sujinlee.me/professional-github/
- https://djkeh.github.io/articles/How-to-write-a-git-commit-message-kor/

## 히스토리 단장하기
- https://git-scm.com/book/ko/v1/Git-도구-히스토리-단장하기
- https://git-scm.com/docs/git-filter-branch
- https://blog.outsider.ne.kr/1249

## pull request
- https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/

## rebase
- http://dogfeet.github.io/articles/2012/git-merge-rebase.html

## gitignore
- [gitignore](Git-gitignore.md)
- [gitignore.io](https://www.gitignore.io/) | Create useful .gitignore files for your project

## GitHub language colors
- [GiHub-Colors](Git-GitHub-Colors.md)

## Trouble Shooting 바로가기
- [Sourcetree에서 User information 내용을 변경해도 새 Repository에서 적용이 되지 않을 경우](#sourcetree에서-user-information-내용을-변경해도-새-repository에서-적용이-되지-않을-경우)
- [리모트 저장소에 푸시한 커밋을 리베이스했을 경우](#리모트-저장소에-푸시한-커밋을-리베이스했을-경우)
- [git remote repository url 확인 및 변경](#git-remote-repository-url-확인-및-변경)
- [git push시 '매번 github 인증정보 묻지 않기' 설정](#git-push시-매번-github-인증정보-묻지-않기-설정)
- ['fatal: HttpRequestException encountered.' 오류 해결](#fatal-HttpRequestException-encountered-오류-해결)


## Trouble Shooting
### Sourcetree에서 User information 내용을 변경해도 새 Repository에서 적용이 되지 않을 경우
![git-usage-sourcetree-repository-settings](images/git-usage-sourcetree-repository-settings.png)

- .gitconfig 파일에서 내용을 변경
  - `C:\Users\<username>\.gitconfig`
  ![git-usage-gitconfig](images/git-usage-gitconfig.png)
  - https://community.atlassian.com/t5/Sourcetree-questions/Change-the-author-and-committer-name-and-e-mail-of-multiple/qaq-p/807008
  - (같이보기) https://git-scm.com/book/ko/v1/시작하기-Git-최초-설정

### 리모트 저장소에 푸시한 커밋을 리베이스했을 경우
- **(경고)** 기본적으로 **리모트 저장소에 푸시한 커밋은 리베이스하지 말 것**
  - https://djkeh.github.io/articles/Do-not-rebase-commits-that-you-have-pushed-to-a-public-repository-kor/
- 하지만, 이미 실수를 해버렸을 경우에 대한 내용이다
- 상황
  - 리모트 저장소 `master` 브랜치의 커밋 중 하나를 삭제하고 싶었다
  - `master` 브랜치에서 Interactive Rebase `git rebase -i <커밋 해시값>``를 통해 삭제하고 싶은 커밋을 drop 했다 **(핵심적인 실수)**
    - [Git rebase를 이용한 커밋 수정 (Interactive Rebase)](https://wckhg89.github.io/archivers/rebase)
  -  하지만, **기존 커밋이 중복**되는 결과가 나왔다
  ![git-rebase-problem](images/git-rebase-problem/git-rebase-problem.png)
- 해결
  - **새 브랜치를 만들어 다시 Interactive Rebase 실행**
  - `dev`라는 브랜치를 만들고 이 브랜치로 checkout함
  - Sourcetree - Terminal (Git Bash)에서 `git rebase -i 7c36480`(7c36480은 Initial commit의 커밋번호) 실행
    - 해당 작업의 [Git Bash Log](data/git-rebase-log.docx)
  - `nothing to commit, working tree clean` 메시지
    - `git rebase --skip`
    ![git-rebase-skip](images/git-rebase-problem/git-rebase-skip.png)
  - `CONFLICT (rename/add): Rename Spring/Spring-MVC->Spring/Spring-MVC.md in 4e63237... ` 메시지
    - conflict 해결 후
    - `git rebase --continue`
    ![git-rebase-continue](images/git-rebase-problem/git-rebase-continue.png)
  - rebase 완료
    - 커밋 중복 제거된 상태
    ![git-rebase-done](images/git-rebase-problem/git-rebase-done.png)
  - 고려해야 할 문제
    - rebase가 완료되면 기존 커밋날짜가 rebase가 완료된 날짜로 변경된다
    - 즉, GitHub의 Remote repository 경우 기존 Contributions(잔디심은 것)이 날아간다
  - 위의 문제가 상관없다면
    - 기존 `master`브랜치를 제거하고 `dev`브랜치의 이름을 `master`로 변경
  - 느낀점
    - **절대! `master` 브랜치에서 rebase작업을 하지말자**
      - rebase 작업은 별도의 브랜치를 만들어 진행하자
      - remote repository에 push한 커밋이라면 더 주의하자
        - 공동작업의 경우 더 심각한 상황을 초래한다
      - Best: 이런 상황 자체를 만들지 말자
        - 하지만, 세상은 생각대로만 흘러가지 않는다

### git remote repository url 확인 및 변경
- 확인
  - `git config --get remote.origin.url`
- 변경
  - `git remote set-url origin "repository 주소"`
    - 예) `git remote set-url origin "https://github.com/greatfarmer/vue-tutorial-hello.git"`

### git push시 '매번 github 인증정보 묻지 않기' 설정
> https://git-scm.com/book/ko/v2/Git-도구-Credential-저장소

- `git config --global credential.helper <옵션>`
  - `<옵션>`
    - 설정하지 않음
      - 어떤 암호도 저장하지 않음
      - 인증이 필요할 때 매번 인증정보(사용자이름과 암호)를 입력해야함
    - `cache`
      - 일정 시간 동안 메모리에 인증정보를 기억
      - 이 정보를 Disk에 저장하지 않음
      - 메모리에서 15분까지만 유지
    - `store`
      - 인증정보를 Disk의 텍스트 파일로 저장하며 계속 유지
      - 리모트의 인증정보를 변경하지 않는 한 다시 인증정보를 입력하지 않아도 접근 가능
        - 매번 사용자이름과 암호를 입력하지 않아도 됨
      - 주의사항
        - 인증정보가 사용자 홈 디렉토리 아래의 일반 텍스트 파일로 저장됨
          - (Windows의 경우) `C:\Users\<Windows 사용자이름>\.git-credentials`
            - `https://<github 사용자이름>:<비밀번호>@github.com`
            - 암호화 되지 않고 인증정보가 그대로 저장됨
        - 암호화하려면 Github 로그인 후 아래의 경로에서 토큰을 생성 (**권장**)
          - `Settings` - `Developer settings` - `Personal access tokens` - `Generate new token`
            - `Token description` 입력
            - `Select scopes` - `repo` 전체선택
              - 이렇게 하면 repository 관리 권한만 따로 주게 됨
              - 즉, github사이트에서 로그인 시 비밀번호에 이 토큰을 적용해도 로그인되지 않음
            - `Generate token`
          - 최초 발급된 token값을 최초 push에서 github로그인 시 비밀번호란에 사용하면 됨
            - token값을 잃어버렸다면?
              - 해당 토큰에서 `Regenerate token`을 하거나
              - 새로운 토큰을 생성
    - `osxkeychain`
      - Mac에서 제공하는 Keychain시스템 사용
      - 자세한 내용은 레퍼런스 사이트 참고

### 'fatal: HttpRequestException encountered.' 오류 해결
- 원인
  - GitHub이 TLS 1.2를 사용하도록 전환함
  - 현재 프로그램이 TLS 1.0을 사용하여 GitHub에 계속 연결하려고 할 때 이 오류가 발생
- 해결 참고 사이트
  - http://recoveryman.tistory.com/418
  - https://codeshare.co.uk/blog/how-to-solve-the-github-error-fatal-httprequestexception-encountered/
