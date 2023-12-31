---
tags: git
---
# `git filter-branch` 사용 가이드라인

## !!주의 사항!!
- 이 작업은 Git 리포지터리의 히스토리를 변경합니다. 다른 사람과 공유 중인 리포지터리에서 사용할 경우, 반드시 팀 멤버들과 협의해야 합니다.
- 작업을 수행하기 전에 리포지터리의 백업을 권장합니다.

## 목적
- 이미 커밋된 파일을 리포지터리 히스토리에서 제거하기 위해 사용합니다.
  
## 예시: `settings.py` 파일 제거
1. 로컬 리포지터리로 이동
    ```bash
    cd path/to/your/repo
    ```

2. `filter-branch` 명령 실행
    ```bash
    git filter-branch --force --index-filter "git rm --cached --ignore-unmatch path/to/settings.py" --prune-empty --tag-name-filter cat -- --all
    ```
    ```bash
    # 줄바꿈 
    git filter-branch --force --index-filter \
      "git rm --cached --ignore-unmatch path/to/settings.py" \
      --prune-empty --tag-name-filter cat -- --all
    ```
    - `--force`: 이미 적용된 `filter-branch`를 다시 적용합니다.
    - `--index-filter`: 각 커밋에 적용할 명령입니다.
    - `--ignore-unmatch`: 파일이 없을 경우 무시합니다.
    - `--prune-empty`: 빈 커밋을 제거합니다.
    - `--tag-name-filter cat`: 태그 이름은 변경하지 않습니다.
    - `-- --all`: 모든 브랜치와 태그에 적용합니다.

3. 원격 저장소에 반영
    ```bash
    git push origin --force --all
    ```
    **주의**: 이 명령은 위험하며, 모든 브랜치와 태그를 강제로 업데이트합니다. 공유 리포지터리에서는 신중하게 사용해야 합니다.

4. 다른 참여자에게 리포지터리를 새로 복제하거나 업데이트하도록 지시합니다.
   
이 가이드라인은 `git filter-branch`를 사용하여 `settings.py` 파일을 모든 커밋에서 제거하는 방법을 설명한 것입니다. 다른 사용 사례에도 유사한 방법을 적용할 수 있습니다.
> Generated by GPT-4 model