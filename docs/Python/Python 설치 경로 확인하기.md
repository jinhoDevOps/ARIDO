### 방법 1: 파일 탐색기를 사용하여 수동으로 찾기

1. 파일 탐색기를 엽니다.
2. 주소 표시줄에 `%localappdata%\Programs\Python`을 입력하고 Enter 키를 누릅니다.
3. Python 폴더 안에 설치된 Python 버전의 폴더를 확인할 수 있습니다.

이 방법은 Python이 기본 경로에 설치된 경우 유효합니다.

### 방법 2: CMD를 사용하여 찾기

1. 검색을 통해 'cmd'를 찾아 명령 프롬프트를 엽니다.
    
2. 명령 프롬프트에 `python`을 입력하고 Enter 키를 누릅니다.
    
3. Python 인터프리터가 시작되면, 다음 코드를 입력합니다:
```python
    import sys 
    print(sys.executable)
```

> https://allhpy35.tistory.com/23