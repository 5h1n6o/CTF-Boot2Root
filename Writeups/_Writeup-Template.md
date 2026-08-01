# 🏆 Boot2Root - MachineName  
（HackTheBox / VulnHub / TryHackMe）

Boot2Root は「実戦ログ」、  
Pentest-Playbook は「詳細技術体系」という役割分担で記録します。

Boot2Root では **結果と証拠**を記録し、  
Pentest-Playbook では **技術的背景・体系化された知識**を参照します。

---

## 0. Target Information

- Target IP:  `10.10.x.x`
- OS: Unknown（後で判明）
- Difficulty: Easy / Medium / Hard
- Tags: Web / SMB / FTP / SSH / PrivEsc / Pivot / Tunneling / Linux / Windows
- Notes:  

---

## 1. OSINT 
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/OSINT/README.md


### 実施内容（Boot2Root は結果のみ）
- 収集したサブドメイン  
- メールアドレス  
- 公開情報  
- 漏洩情報（あれば）  

### 証拠
（スクショ）

---

## 2. Reconnaissance  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Reconnaissance/README.md


### 実施内容
- ポートスキャン結果  
- バナー情報  
- 外部サービス一覧  

### 証拠
（スクショ）

---

## 3. Enumeration  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Enumeration/README.md

### 実施内容
- Web / SMB / FTP / SSH の詳細調査  
- ディレクトリ探索  
- バージョン情報  
- 脆弱性候補  

### 証拠
（スクショ）

---

## 4. Initial Access  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Initial-Access/README.md

### 実施内容
- SQLi / RCE / File Upload / LFI / SSRF  
- SMB / FTP / SSH / DB からの侵入  
- WebShell の設置  

### 証拠
（スクショ）

---

## 5. Local Enumeration  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Local-Enumeration/README.md

### 実施内容
- whoami / id / hostname  
- OS / カーネル情報  
- プロセス / サービス  
- 認証情報探索  
- 内部サービスの発見  

### 証拠
（スクショ）

---

## 6. Privilege Escalation  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Privilege-Escalation/README.md

### 実施内容
- sudo -l  
- SUID / Capability  
- Cron  
- Kernel Exploit  
- 設定ミスの悪用  

### 証拠
（スクショ）

---

## 7. Credential Access  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Credential-Access/README.md

### 実施内容
- パスワード探索  
- 設定ファイル  
- SSH鍵  
- DB 認証情報  
- Cookie / Token  

### 証拠
（スクショ）

---

## 8. Internal Enumeration  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Internal-Enumeration/README.md

### 実施内容
- 内部ネットワークの構造  
- 内部サービス  
- 内部ホスト  
- 内部 Web / API  
- 内部 DB / Redis  

### 証拠
（スクショ）

---

## 9. Pivot  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Pivot/README.md

### 実施内容
- SSH Local / Remote / Dynamic  
- Chisel  
- Ligolo-ng  
- ProxyChains  
- 内部サービスへのアクセス  

### 証拠
（スクショ）

---

## 10. Lateral Movement  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Lateral-Movement/README.md

### 実施内容
- SSH 横展開  
- SMB / RDP / VNC  
- 内部 Web / API  
- 内部 DB  
- 内部管理ツール  

### 証拠
（スクショ）

---

## 11. Reporting  
👉 詳細: https://github.com/5h1n6o/Pentest-Playbook/blob/main/Reporting/README.md

👉 このマシンのレポートはこちら  
- ここにレポートへのリンクを貼りつける。


## 🎯 このテンプレートの目的

- Boot2Root の全章が Pentest-Playbook と 1:1 で連携  
- Boot2Root は「結果」、Pentest-Playbook は「技術体系」  
- OSCP レポート品質の Writeup を自動的に構築  
- 将来の章追加にも強い構造  
