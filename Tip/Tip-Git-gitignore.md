# [Git] .gitignore 적용하기
> http://seunggabi.com/entry/Git-gitignore-적용하기

## 작성하기
```
# .ext 확장자를 가진 파일을 모두 무시한다.
*.ext

# .ext 중 name.ext 파일만 무시하지 않는다.
!name.ext

# 루트 하위 디렉토리 dir 디렉토리 무시한다.
/dir

# 모든 경로의 dir 디렉토리 하위 무시한다.
dir/

# 모든 경로의 dir 디렉토리 바로 하위의 .ext 확장자를 가진 파일을 모두 무시한다.
dir/*.ext

# 모든 경로의 dir 디렉토리 하위의 .ext 확장자를 가진 파일을 모두 무시한다.
dir/**/*.ext
```

## 적용하기
```
git add .
git commit -m "add .gitignore"
git rm -r --cached .
git add .
git commit -m "reset tracking files"
```

## 언어와 프레임워크, OS 별로 원격저장소에서 제외시킬 파일 확장자를 관리해주는 API 사이트
> https://www.gitignore.io/
