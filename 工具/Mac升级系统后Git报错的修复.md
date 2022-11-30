升级系统后git有如下报错

[Git is not working after macOS Update (xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools)](https://stackoverflow.com/questions/52522565/git-is-not-working-after-macos-update-xcrun-error-invalid-active-developer-pa)

执行如下命令即可

```
sudo xcode-select --install
```