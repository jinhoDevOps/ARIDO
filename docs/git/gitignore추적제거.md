---
tags: git
---
# `.gitignore`추적제거

`.gitignore` 파일을 수정했다면, 그 변경사항은 새로 추가되는 파일에만 적용됩니다. 이미 Git 추적 목록(`tracking list`)에 있는 파일은 `.gitignore`의 변경사항에 영향을 받지 않습니다.

따라서, 이미 추적 중인 파일을 `.gitignore`에 추가했다면 다음과 같은 절차를 따라야 파일이 Git에서 무시됩니다:

1. 먼저 `.gitignore` 파일을 수정해서 무시하고 싶은 파일이나 폴더를 추가합니다.
2. 그 다음에 Git 캐시를 지워 해당 파일을 추적 목록에서 제거합니다.

이를 위해 다음 명령어를 실행하세요:

```bash
# 특정 파일을 추적 목록에서 제거
git rm --cached [file_path]

# 폴더 전체를 추적 목록에서 제거
git rm -r --cached [folder_path]
```

3. 마지막으로 변경사항을 커밋합니다.

```bash
git commit -m "Remove files that are now in .gitignore"
```

4. 변경사항을 원격 저장소에 푸시합니다.

```bash
git push origin [your-branch-name]
```

이렇게 하면 `.gitignore`에 추가한 파일이나 폴더는 더 이상 Git에 의해 추적되지 않습니다.