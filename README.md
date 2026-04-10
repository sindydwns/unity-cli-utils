# Unity Tools

Unity 프로젝트에서 에셋의 GUID를 추적하고 참조 관계를 분석하기 위한 셸 스크립트 모음입니다.

## 요구 사항

- **bash** (macOS/Linux 기본 포함)
- **[ripgrep](https://github.com/BurntSushi/ripgrep)** (`rg`) — 고속 텍스트 검색에 사용

### ripgrep 설치

```bash
# macOS (Homebrew)
brew install ripgrep

# Ubuntu / Debian
sudo apt install ripgrep

# Windows (Scoop)
scoop install ripgrep
```

설치 확인:

```bash
rg --version
```

## 설치

스크립트가 있는 디렉토리를 `PATH`에 추가합니다.

```bash
export PATH="/path/to/unity-tools:$PATH"
```

셸 설정 파일(`~/.zshrc`, `~/.bashrc` 등)에 위 줄을 추가하면 영구적으로 적용됩니다.

## 명령어

모든 명령어는 **Unity 프로젝트 루트**에서 실행하세요.
두 번째 인자 `search_root`를 생략하면 현재 디렉토리(`.`)를 기준으로 검색합니다.

### find-guid — GUID로 에셋 찾기

`.meta` 파일에서 GUID를 검색하여 해당 에셋의 경로를 출력합니다.

```bash
find-guid <guid> [search_root]
```

```bash
find-guid a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4
find-guid a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4 Assets/Prefabs
```

### find-guid-refs — GUID를 참조하는 에셋 찾기

특정 GUID를 참조하고 있는 에셋 파일들을 찾습니다.
`.meta` 파일과 바이너리 에셋은 검색에서 자동 제외됩니다.

```bash
find-guid-refs <guid> [search_root]
```

```bash
find-guid-refs a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4
find-guid-refs a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4 Assets/Scenes
```

### find-asset-refs — 파일 경로로 참조 에셋 찾기

파일 경로를 입력하면 `.meta`에서 GUID를 자동 추출하고, 해당 에셋을 참조하는 모든 파일을 찾습니다.

```bash
find-asset-refs <asset_path> [search_root]
```

```bash
find-asset-refs Assets/Scripts/PlayerController.cs
find-asset-refs Assets/Materials/Default.mat Assets/Scenes
```

## 검색 제외 대상

검색 성능과 정확도를 위해 다음은 자동으로 제외됩니다.

| 구분 | 제외 항목 |
|------|-----------|
| 디렉토리 | `Library/` `Temp/` `Logs/` `obj/` `.git/` |
| 바이너리 에셋 | 이미지(`png`, `jpg`, `psd` 등), 오디오(`wav`, `mp3` 등), 3D 모델(`fbx`, `blend` 등), 동영상, 폰트, 네이티브 라이브러리, 압축 파일 |
