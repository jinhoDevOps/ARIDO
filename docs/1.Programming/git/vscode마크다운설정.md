## VScode 미세먼지팁- Markdown 미리보기 설정하기

Visual Studio Code에서 `.md` 파일을 미리보기 모드로 기본 열기를 설정하는 방법입니다.  
미리보기로 열린 상태에서 더블클릭하면 편집기모드로 바뀝니다.

### 1. 설정에 접근하기

- Visual Studio Code를 열고, `Ctrl` + `,` 단축키를 사용하여 설정에 진입합니다. (Mac 사용자의 경우 `Cmd` + `,`)

### 2. `settings.json` 열기

- 화면 오른쪽 상단에 위치한 분할 아이콘 옆의 `{}` 아이콘 (설정을 JSON으로 열기)을 클릭합니다.

### 3. `workbench.editorAssociations` 설정 추가하기

- `settings.json`에 아래 설정을 추가하세요.

```json
"workbench.editorAssociations": {
    "*.jsp": "default",
    "*.md": "vscode.markdown.preview.editor"
}
```

### 4. 변경사항 저장

- 변경 사항을 저장하고 Visual Studio Code를 재시작하세요.

이제 `.md` 파일을 열 때마다 기본적으로 미리보기 모드로 열리게 됩니다

---
 * [돌아가기](../README.md) 