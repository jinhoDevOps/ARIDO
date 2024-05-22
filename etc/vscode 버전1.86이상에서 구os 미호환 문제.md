

두줄요약
1. 1.85이하로 다운그레이드 해야함
2. 다운로드 링크 Downloads: Windows: [x64](https://update.code.visualstudio.com/1.85.2/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.85.2/win32-arm64-user/stable) | Mac: [Universal](https://update.code.visualstudio.com/1.85.2/darwin-universal/stable) [Intel](https://update.code.visualstudio.com/1.85.2/darwin/stable) [silicon](https://update.code.visualstudio.com/1.85.2/darwin-arm64/stable) | Linux: [deb](https://update.code.visualstudio.com/1.85.2/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.85.2/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.85.2/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.85.2/linux-snap-x64/stable)
---

구버전으로 다운그레이드 해야된다는 공식 문서
### [Can I run VS Code Server on older Linux distributions?](https://code.visualstudio.com/docs/remote/faq#_can-i-run-vs-code-server-on-older-linux-distributions)

Starting with VS Code release 1.86, the minimum requirements for the build toolchain of the remote server were raised. The prebuilt servers distributed by VS Code are compatible with Linux distributions based on glibc 2.28 or later, for example, Debian 10, RHEL 8, Ubuntu 20.04.

If your setup does not meet these requirements and you are unable to upgrade the Linux distribution, you can downgrade the VS Code client to version 1.85 to continue using Remote Development. You can downgrade the VS Code client on both desktop and web:

- On desktop, you can download the VS Code release 1.85 from [here](https://code.visualstudio.com/updates/v1_85). Depending on your platform, make sure to disable updates to stay on that version. A good recommendation is to have release 1.85 as a separate installation, set up with [Portable Mode](https://code.visualstudio.com/docs/editor/portable). That way, you won't affect your main desktop VS Code version.
- On web, you can add the following query argument [`?vscode-version=0ee08df0cf4527e40edc9aa28f4b5bd38bbff2b2`](https://vscode.dev/?vscode-version=0ee08df0cf4527e40edc9aa28f4b5bd38bbff2b2) to use VS Code release 1.85.

### [Can I install individual extensions instead of the extension pack?](https://code.visualstudio.com/docs/remote/faq#_can-i-install-individual-extensions-instead-of-the-extension-pack)

Yes. The [Remote Development extension pack](https://aka.ms/vscode-remote/download/extension) provides a convenient way for you to access all of the latest remote capabilities as they are released. However, you can always install the individual extensions from the Marketplace or VS Code Extensions view.

- [Remote - SSH](https://aka.ms/vscode-remote/download/ssh)
- [Dev Containers](https://aka.ms/vscode-remote/download/containers)
- [WSL](https://aka.ms/vscode-remote/download/wsl)
---

# 1.85 릴리즈노트
# November 2023 (version 1.85)

**Update 1.85.1**: The update addresses these [issues](https://github.com/microsoft/vscode/issues?q=is%3Aissue+milestone%3A%22November+2023+Recovery+1%22+is%3Aclosed).

**Update 1.85.2**: The update addresses these [issues](https://github.com/microsoft/vscode/issues?q=is%3Aissue+milestone%3A%22November+2023+Recovery+2%22+is%3Aclosed).

Downloads: Windows: [x64](https://update.code.visualstudio.com/1.85.2/win32-x64-user/stable) [Arm64](https://update.code.visualstudio.com/1.85.2/win32-arm64-user/stable) | Mac: [Universal](https://update.code.visualstudio.com/1.85.2/darwin-universal/stable) [Intel](https://update.code.visualstudio.com/1.85.2/darwin/stable) [silicon](https://update.code.visualstudio.com/1.85.2/darwin-arm64/stable) | Linux: [deb](https://update.code.visualstudio.com/1.85.2/linux-deb-x64/stable) [rpm](https://update.code.visualstudio.com/1.85.2/linux-rpm-x64/stable) [tarball](https://update.code.visualstudio.com/1.85.2/linux-x64/stable) [Arm](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions) [snap](https://update.code.visualstudio.com/1.85.2/linux-snap-x64/stable)

---

Welcome to the November 2023 release of Visual Studio Code. There are many updates in this version that we hope you'll like, some of the key highlights include:

- **[Floating editor windows](https://code.visualstudio.com/updates/v1_85#_floating-editor-windows)** - Drag and drop editors onto your desktop.
- **[Accessible View workflow](https://code.visualstudio.com/updates/v1_85#_accessibility)** - Smoother transitions to and from the Accessible View.
- **[Finer extension update control](https://code.visualstudio.com/updates/v1_85#_extension-auto-update-control)** - Choose which extensions to auto update.
- **[Source Control incoming and outgoing view](https://code.visualstudio.com/updates/v1_85#_source-control)** - Easily review pending repository changes.
- **[JavaScript heap snapshots](https://code.visualstudio.com/updates/v1_85#_javascript-debugger)** - Visualize heap snapshots including memory object graphs.
- **[TypeScript Go to Definition from inlay hints](https://code.visualstudio.com/updates/v1_85#_jump-to-definition-for-inlay-hints)** - Jump to definition from inlay hint hovers.
- **[Python type hierarchy display](https://code.visualstudio.com/updates/v1_85#_python)** - Quickly review and navigate complex type relationships.
- **[GitHub Copilot updates](https://code.visualstudio.com/updates/v1_85#_github-copilot)** - Inline chat improvements, Rust code explanation.
- **[Preview: expanded Sticky Scroll support](https://code.visualstudio.com/updates/v1_85#_preview-features)** - Sticky Scroll in tree views and the terminal.

> If you'd like to read these release notes online, go to [Updates](https://code.visualstudio.com/updates) on [code.visualstudio.com](https://code.visualstudio.com/).

**Insiders:** Want to try new features as soon as possible? You can download the nightly [Insiders](https://code.visualstudio.com/insiders) build and try the latest updates as soon as they are available.