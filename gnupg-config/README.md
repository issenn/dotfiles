# gnupg-config

- `密码`: 用来保护私钥的.这样子, 即使你的 `~/.gnupg` 目录被泄漏了, 还有一层密码保护
- `主密钥` : 一般是你调用 `gpg --full-gen-key` 命令生成的那个密钥.
- `子密钥` : `subkey`, 跟主密钥几乎没有差别, 除了它是隶属于 `主密钥` 的. 注意, 子密钥是可以单独用来加解密的. 只是它要隶属于主密钥来管理.
- 密钥的能力
  - `[S]` : 表示用于签名
  - `[E]` : 表示用于加密
  - `[A]` : 表示用于身份认证
  - `[C]` : 用于认证其他的密钥. 带有这个的, 一般是 `主密钥` 这个密钥的数字, 就是你的身份.



### 生成密钥

#### 生成主密钥对
```sh
gpg --full-generate-key
```

生成一张撤销证书，用于密钥作废，请求外部公钥服务器撤销你的公钥
```sh
gpg --generate-revocation [key-id]

# 二进制证书 revocation-gmail.cert； 但会提示 “已强行使用 ASCII 封装过的输出”
gpg --output revocation-gmail.cert --gen-revoke MASTERKEYID

# -a (--armor)输出文件为 revocation-gmail-cert.txt 文本文件. armor参数可以将其转换为ASCII码显示。
gpg --armor --output revocation-gmail-cert.txt --gen-revoke MASTERKEYID
```
将吊销证书导入即可吊销本地

吊销证书默认存储位置 `${GNUPGHOME}/openpgp-revocs.d/`

#### 生成子密钥对
```sh
gpg --edit-key MASTERKEYID
gpg> addkey
```

### 密钥管理

列出公钥
```sh
gpg --list-keys
gpg -k 
```

下面那两个 `ssb` 就是 subkey. 第一次创建时, 默认也会创建一个 subkey.

删除公钥

```sh
gpg --delete-keys [key-id]
```

列出私钥
```sh
gpg --list-secret-keys
gpg -K
```
```sh
# 使用此命令列出我的key id，顾名思义LONG 这种形式的id 比一般的id要长
gpg --list-secret-keys --keyid-format LONG
```

删除私钥
```sh
gpg --delete-secret-keys [key-id]
```

#### 将二进制的公钥(私钥)导出为ASSCII码

导出主公密钥
```sh
gpg --armor --output master-public-key.txt --export MASTERKEYID
```
```sh
# Prints the GPG key ID, in ASCII armor format
gpg --armor --export [key-id]
```

导出主私密钥
```sh
gpg --armor --output master-secret-key.txt --export-secret-keys MASTERKEYID
```
导出子私密钥
```sh
# 这会导出所有的 subkey 的私密钥
gpg --armor --output secret-all-subkey-gmail.txt --export-secret-subkeys MASTERKEYID

# 导出特定的子私密钥. 注意后面的 ! 号.
gpg --armor --output secret-subkey-gmail.txt --export-secret-subkeys [key-id]!
```

吊销 subkey
```sh
gpg --edit-key MASTERKEYID
选定哪个 key. N 是序号. 如果正确选中的话, 某个 key 会出现 `*` 符号. 默认是选中所有.
gpg>key N
gpg>revkey
gpg>save
```

删除 subkey
```sh
gpg --edit-key MASTERKEYID
选定哪个 key. N 是序号. 如果正确选中的话, 某个 key 会出现 `*` 符号. 默认是选中所有.
gpg>key N
gpg>delkey
gpg>save
```

信任 subkey
```sh
gpg --edit-key MASTERKEYID
选定哪个 key. N 是序号. 如果正确选中的话, 某个 key 会出现 `*` 符号. 默认是选中所有.
gpg>key N
gpg>trust
gpg>save
```

#### 上传公钥

上传公钥到公钥服务器
```sh
gpg --send-keys [用户ID] --keyserver hkp://subkeys.pgp.net
```

生成用于公布的公钥指纹（用于他人校验）
```sh
gpg --fingerprint [用户ID]
```

#### 导入密钥
```sh
gpg --import [密钥文件]
gpg --keyserver hkp://subkeys.pgp.net --search-keys [用户ID]
```

### 加密和解密

加密
```sh
gpg --recipient [用户ID] --output demo.en.txt --encrypt demo.txt
```

解密
```sh
gpg --decrypt demo.en.txt --output demo.de.txt
```

### 签名

对文件签名
```sh
# 采用二进制储存.gpg文件
gpg --sign demo.txt

# 生成ASCII码的.asc签名文件
gpg --clearsign demo.txt

# 生成单独的.sig签名文件
gpg --detach-sign demo.txt

# 采用ASCII码形式
gpg --armor --detach-sign demo.txt
```

签名+加密
```sh
gpg --local-user [发信者ID] --recipient [接收者ID] --armor --sign --encrypt demo.txt
```

验证签名
```sh
gpg --verify demo.txt.asc demo.txt
```

编辑模式
```sh
gpg --edit-key MASTERKEYID
gpg --edit-key [key-id]
gpg --expert --edit-key MASTERKEYID
```
```
Constant           Character      Explanation
─────────────────────────────────────────────────────
PUBKEY_USAGE_SIG      S       key is good for signing
PUBKEY_USAGE_CERT     C       key is good for certifying other signatures
PUBKEY_USAGE_ENC      E       key is good for encryption
PUBKEY_USAGE_AUTH     A       key is good for authentication
```
```
主密钥显示：SC（“sign”&“certify”，代表可以签名和认证其它密钥）

第一个副密钥显示：E（“encrypt”，加密）

第二个副密钥显示：S（“sign”，签名
```

停止gpg-agent

```sh
gpgconf --kill gpg-agent
```
重新加载gpg-agent
```sh
gpg-connect-agent reloadagent /bye
```

```sh
git config --global user.signingkey B28FACA42EBC87DF
```

名词简写
```
sec => 'SECret key'
ssb => 'Secret SuBkey'
pub => 'PUBlic key'
sub => 'public SUBkey'
```

```sh
chown -R $(whoami) "${GNUPGHOME}"
find "${GNUPGHOME}" -type f -exec chmod 600 {} \;
find "${GNUPGHOME}" -type d -exec chmod 700 {} \;
```

> [GPG 的正确使用姿势](https://mogeko.me/2019/068/)
