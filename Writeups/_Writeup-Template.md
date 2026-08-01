# Boot2Root - MachineName  
（HackTheBox / VulnHub / TryHackMe）

この Writeup は 1 台のマシンを Recon → Initial Access → PrivEsc → Root の流れで攻略する  
Boot2Root 形式の攻略ログです。

---

# 🧭 0. Target Info
- IP: `10.10.x.x`
- OS: Unknown（後で判明）
- Difficulty: Easy / Medium / Hard
- Tags: Web / SMB / FTP / SSH / PrivEsc / Linux / Windows

---

# 🔍 1. Recon（定石コマンド＋よくある罠）

## 1-1. ポートスキャン（鉄板）
Boot2Root の 90% は **nmap の初動で決まる**。

### 🔥 最強の初動（高速・広範囲・サービス判定）
```
nmap -p- -T4 -A -v 10.10.x.x
```

### ポートだけ先に知りたい（高速）
```
nmap -p- --min-rate 5000 -v 10.10.x.x
```

### よく使う追加オプション
```
-sC   → default scripts
-sV   → version detection
-O    → OS detection
--script vuln → 脆弱性スキャン
```

### よくある罠
- **80/443 だけ見て満足する → 実は 8080 / 8000 / 5000 が本命**
- **UDP を見ない → DNS / SNMP / NTP が突破口**
- **-sC -sV を付け忘れて情報不足になる**

---

## 1-2. Web 調査（深掘り）

### 🔥 まずは HTTP の基本情報
```
curl -I http://10.10.x.x
```

### ディレクトリ探索（ffuf）
```
ffuf -u http://10.10.x.x/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

### バーチャルホスト探索（定石）
```
ffuf -u http://10.10.x.x -H "Host: FUZZ" -w subdomains.txt
```

### JS / hidden API 調査（鉄板）
```
curl -s http://10.10.x.x | grep -E "api|js|admin|login"
```

### CMS 判定（よくある突破口）
```
whatweb http://10.10.x.x
```

### Web の突破口の定石
- 古い CMS → RCE / File Upload  
- admin パネル → 弱パスワード  
- JS 内に API Key  
- robots.txt → 隠しディレクトリ  
- backup.zip → ソースコード流出  


### よくある罠
- **拡張子を探索しない → .php / .bak / .old が見逃される**
  ```
  ffuf -u http://10.10.x.x/FUZZ -w common.txt -e php,txt,bak,old
  ```
- **JS を見ない → API Key や hidden endpoint が隠れている**
- **robots.txt を見ない → admin パネルが隠れている**

---

## 1-3. SMB 調査（深掘り）

### SMB の鉄板スキャン
```
nmap --script smb-enum-shares -p 445 10.10.x.x
nmap --script smb-enum-users -p 445 10.10.x.x
```

### smbclient（匿名アクセス）
```
smbclient -N -L //10.10.x.x/
```

### 共有フォルダにアクセス
```
smbclient //10.10.x.x/share
```

### SMB の突破口の定石
- anonymous share → パスワードや config が落ちている  
- .txt / .conf → 認証情報  
- .ps1 → Windows の PrivEsc のヒント  
- バックアップファイル → Web の初期侵入につながる  

---

### よくある罠
- **anonymous share を見逃す → 認証情報が落ちている**
- **.txt / .conf を軽視 → パスワードがそのまま書かれている**
- **SMB のファイルをローカルに保存しない → 後で解析できない**

---
## 1-4. FTP 調査（深掘り）

### 匿名ログイン
```
ftp 10.10.x.x
Name: anonymous
Password: anonymous
```

### ファイル一覧
```
ls -la
```

### FTP の突破口の定石
- anonymous → Web のソースコードが落ちている  
- .php / .html → Web の脆弱性のヒント  
- .txt → 認証情報  
- 書き込み可能 → WebShell アップロード  


### よくある罠
- **anonymous login を試さない**
- **書き込み可能か確認しない → WebShell アップロードの突破口**
- **FTP 内の .php / .html を見逃す → Web の脆弱性のヒント**

---

## 1-5. SSH 調査（深掘り）

### バナー情報
```
ssh -v user@10.10.x.x
```

### よくある突破口
- 弱パスワード  
- 公開鍵が SMB/FTP に落ちている  
- Web のユーザー名と同じ  
- config ファイルにヒント  

### よくある罠
- **Web のユーザー名と SSH のユーザー名が一致することに気づかない**
- **SMB/FTP に落ちている鍵ファイルを使わない**
- **authorized_keys を見逃す → 横展開のヒント**

---

# 🚪 2. Initial Access（侵入の糸口）

## 2-1. Web Exploit（定石）
### SQL Injection
```
' OR 1=1-- -
```

### よくある罠
- **POST パラメータをテストしない → GET だけ見て終わる**
- **Cookie をテストしない → Cookie Injection が突破口**

---

### SSRF
```
http://127.0.0.1:80
http://169.254.169.254/latest/meta-data/
```

### よくある罠
- **URL エンコードを試さない → フィルタ bypass できない**
- **DNS rebinding を知らない → 内部サービスに届かない**

---

### Command Injection
```
; cat /etc/passwd
```

### よくある罠
- **スペースが使えない → ${IFS} を使う必要がある**
- **フィルタが厳しい → base64 経由で bypass**

---

### File Upload Bypass
```
shell.php.jpg
shell.phtml
```

### よくある罠
- **Content-Type を変更しない → バイパスできない**
- **二重拡張子を試さない → .php.jpg が突破口**

---

## 2-2. 弱パスワード / 認証突破
```
hydra -l admin -P rockyou.txt http://10.10.x.x/login
```

### よくある罠
- **rate limit を考慮しない → ロックされる**
- **パスワードリストが弱い → rockyou.txt を使わない**

---

## 2-3. Public Exploit（SearchSploit）
```
searchsploit apache 2.4
searchsploit openssl
```

### よくある罠
- **バージョンが微妙に違う → exploit が動かない**
- **PoC をそのまま使う → カスタム修正が必要**

---

# 🧗 3. Privilege Escalation（よくある罠）

## 3-1. Linux PrivEsc（鉄板）

### SUID
```
find / -type f -perm -04000 -ls 2>/dev/null
```

### Capabilities
```
getcap -r / 2>/dev/null
```

### 権限確認
```
sudo -l
```

### PATH 攻撃
```
echo "/bin/bash" > /tmp/ls
export PATH=/tmp:$PATH
```

### 定石ツール
```
linpeas.sh
linux-exploit-suggester.sh
```

### よくある罠
- **sudo -l を見逃す → ほぼ毎回突破口**
- **SUID の “意外なコマンド” を見逃す → find / vim / less / awk**
- **Capabilities を知らない → python3 cap_setuid が突破口**
- **PATH 攻撃を知らない → root 取れない**

---

## 3-2. Windows PrivEsc（鉄板）
```
whoami /priv
sc qc <service>
winPEAS.exe
```

### よくある罠
- **サービスの権限を見ない → unquoted service path が突破口**
- **DLL hijacking を知らない**
- **AlwaysInstallElevated を見逃す**

---

# 🔁 4. Lateral Movement（よくある罠）
```
ssh -L 8080:internal:80 user@10.10.x.x
ssh user@10.10.x.x
```

### よくある罠
- **内部ポートを見逃す → pivot しないと見えない**
- **同じパスワードを試さない → credential reuse が突破口**

---

# 👑 5. Root（最終攻略）
```
uname -a
searchsploit linux kernel
cat /root/root.txt
```

### よくある罠
- **Kernel exploit を試さない**
- **cron を見逃す → root 権限で実行されている**
- **/etc/sudoers を見逃す → ALL=(ALL) NOPASSWD が突破口**

---

# 📝 6. Notes（気づき・学び）
- このマシンのポイント  
- 初心者がハマりやすい点  
- 次に活かせるテクニック

---

# 🎯 7. Summary（まとめ）
- 初動：nmap → ffuf → SMB/FTP  
- 侵入：SQLi / RCE / 弱パスワード  
- PrivEsc：SUID / Capabilities / sudo  
- Root：Kernel exploit  
