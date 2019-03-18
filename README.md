# N-App2Deb
App2Deb 原为 的一个自动打包 ipa 文件为 deb，并且能将打包后的 APP 安装至 `/Applications` 下，以 `root` 权限运行的 shell 脚本。但是该作者的脚本过于简单，并未针对 Apple 的重签名政策进行设置。例如 `KeyChain` 等。因此，Nactro 将该脚本进行扩充，目前增加了 `KeyChain` 访问，并将该脚本改名为：N-App2Deb

##### 适用范围
* 你的 APP 内部涉及了与 `KeyChain` 相关的操作，例如 RSA 加密便用到了 `SecKeyRef` ，而 `SecKeyRef` 则涉及了 `KeyChain`

##### 如何使用

##### 首先我们要明白，以 `KeyChain` 为例子，每一个涉及了 `KeyChain` 的 APP，都会在 IPA 目录内生产一个 `embedded.mobileprovision` 该文件中包含了各种 APP 相关的头信息，例如 `keychain-access-groups` 等等。

##### 如果你想要使用 N-App2Deb 脚本，请将该脚本 fork 至 你的仓库，修改 Resources 目录下的 `entitlements.xml` 并添加 `embedded.mobileprovision` 中相关头文件信息。

##### 代码示例
1. 首先用 ATOM 等任意代码编辑器打开 `embedded.mobileprovision` 复制该文件下 KeyChain 相关的头信息

```xml
	<dict>
	<key>application-identifier</key>
	<string>2AMH2MYA4V.devs.nactro.achelper</string>
	<key>keychain-access-groups</key>
	<array>
	<string>2AMH2MYA4V.*</string>
	</array>
	<key>get-task-allow</key>
	<false/>
	<key>com.apple.developer.team-identifier</key>
	<string>2AMH2MYA4V</string>
	</dict>
```

2. 将该头信息复制到 `entitlements.xml` 下，完整的 `entitlements.xml` 应该如下所示

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.private.security.no-container</key>
    <true/>
    <key>com.apple.private.skip-library-validation</key>
    <true/>
    <key>get-task-allow</key>
    <true/>
    <key>platform-application</key>
    <true/>
    <key>application-identifier</key>
    <string>2AMH2MYA4V.devs.nactro.achelper</string>
    <key>keychain-access-groups</key>
    <array>
    <string>2AMH2MYA4V.*</string>
    </array>
    <key>com.apple.developer.team-identifier</key>
    <string>2AMH2MYA4V</string>
</dict>
</plist>
```
##### 至此，你可以使用 N-App2Deb 打包 IPA 文件为 DEB，并安装到你的 iPhone

```bash
dpkg -i [Your Package Name]
```

##### 使用方法与 App2Deb 一致
```bash
App2Deb [Your IPA Path]
```
### 值得注意的是，不要忘记替换 `Install` 与 `App2deb` 文件中相关的下载链接为你自己仓库的地址。
