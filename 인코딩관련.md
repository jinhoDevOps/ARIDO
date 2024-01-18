---
tags:
  - python
  - encoding
  - utf-8
  - "#error"
---
## Python의 csv 모듈은 기본적으로 utf-8 인코딩을 사용하지만, 몇몇 시스템에서는 이를 제대로 처리하지 못할 수 있습니다.
한글이 깨져서 파일이 생성되는 문제는 일반적으로 인코딩 문제로 발생합니다. Python의 `csv` 모듈은 기본적으로 `utf-8` 인코딩을 사용하지만, 몇몇 시스템에서는 이를 제대로 처리하지 못할 수 있습니다.

### 해결 방안:

1. **인코딩 명시**: 파일을 열 때 인코딩을 명시적으로 지정해보세요.
    ```python
    with open(local_file_path, 'w', newline='', encoding='utf-8-sig') as f:
        writer = csv.writer(f)
        writer.writerows(csv_data)
    ```

2. **BOM(Byte Order Mark) 추가**: 윈도우에서는 UTF-8 파일을 열 때 BOM(Byte Order Mark)이 없으면 한글이 깨질 수 있습니다. `utf-8-sig` 인코딩은 이 BOM을 추가해줍니다.

3. **CSV 라이브러리 대신 Pandas 사용**: Pandas 라이브러리를 사용하면 인코딩 문제를 더 쉽게 해결할 수 있습니다.
    ```python
    import pandas as pd

    df = pd.DataFrame(csv_data)
    df.to_csv(local_file_path, index=False, encoding='utf-8-sig')
    ```

4. **파일을 열 때 인코딩 확인**: 파일을 열 때도 인코딩을 `utf-8-sig`로 설정해보세요.

---

`utf-8`과 `utf-8-sig`의 차이점은 Byte Order Mark (BOM)의 유무입니다.

### utf-8

- `utf-8`은 BOM(Byte Order Mark)을 추가하지 않습니다.
- 대부분의 시스템과 언어에서 널리 지원됩니다.
- 리눅스나 macOS 같은 유닉스 기반 시스템에서 주로 사용됩니다.

### utf-8-sig

- `utf-8-sig`는 파일의 시작 부분에 BOM을 추가합니다.
- BOM은 파일이 UTF-8로 인코딩되었다는 것을 명시적으로 알려주는 역할을 합니다.
- 주로 Windows 환경에서 사용되며, Windows의 메모장 등 일부 텍스트 에디터에서는 BOM 없이 저장된 UTF-8 텍스트를 제대로 인식하지 못할 수 있습니다.

BOM이 있는 경우, 일부 텍스트 에디터나 시스템에서는 파일의 시작 부분에 특수한 문자가 보일 수 있습니다. 따라서 사용 환경에 따라 적절한 인코딩 방식을 선택하는 것이 중요합니다.