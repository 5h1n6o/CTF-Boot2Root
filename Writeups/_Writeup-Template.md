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

この章では、Boot2Root の攻略結果を  
**Pentest-Playbook の Reporting 体系に沿ってまとめます。**

Boot2Root は「実戦ログ」、  
Pentest-Playbook は「詳細技術体系」という役割分担です。

---

### 1. Executive Summary（概要）

ターゲットの概要、最終的な侵入結果、取得した権限を簡潔に記述します。

例：
- 192.168.1.10 に対して OSINT → Recon → Enumeration → Exploitation → PrivEsc → Pivot → Lateral Movement を実施  
- 最終的に root 権限を取得  
- 内部ネットワークの 192.168.1.50 に横展開成功  

---

### 2. Scope（対象範囲）

- 対象ホスト  
- 実施期間  
- 使用した手法（Blackbox / OSINT / Recon / Enumeration / Exploitation / PrivEsc / Pivot / Lateral Movement）

---

### 3. Methodology（手法）

Pentest-Playbook の各章に対応する形で記述します。

例：

- OSINT → [Pentest-Playbook / OSINT](../../Pentest-Playbook/OSINT/README.md)  
- Recon → [Pentest-Playbook / Reconnaissance](../../Pentest-Playbook/Reconnaissance/README.md)  
- Enumeration → [Pentest-Playbook / Enumeration](../../Pentest-Playbook/Enumeration/README.md)  
- Initial Access → [Pentest-Playbook / Initial-Access](../../Pentest-Playbook/Initial-Access/README.md)  
- Local Enumeration → [Pentest-Playbook / Local-Enumeration](../../Pentest-Playbook/Local-Enumeration/README.md)  
- PrivEsc → [Pentest-Playbook / Privilege-Escalation](../../Pentest-Playbook/Privilege-Escalation/README.md)  
- Credential Access → [Pentest-Playbook / Credential-Access](../../Pentest-Playbook/Credential-Access/README.md)  
- Internal Enumeration → [Pentest-Playbook / Internal-Enumeration](../../Pentest-Playbook/Internal-Enumeration/README.md)  
- Pivot → [Pentest-Playbook / Pivot](../../Pentest-Playbook/Pivot/README.md)  
- Lateral Movement → [Pentest-Playbook / Lateral-Movement](../../Pentest-Playbook/Lateral-Movement/README.md)  

---

### 4. Findings（脆弱性一覧）

Boot2Root の攻略結果を一覧化します。

| ID | 脆弱性 | 重要度 | 影響範囲 |
|----|--------|--------|-----------|
| F-01 | SQL Injection | High | 認証バイパス |
| F-02 | Weak SSH Password | High | 横展開可能 |
| F-03 | Misconfigured Cron | Medium | PrivEsc 可能 |

---

### 5. Detailed Findings（脆弱性詳細）

Pentest-Playbook の Reporting 章の構造に合わせて記述します。

#### F-01: SQL Injection（High）

##### ✔ 再現手順
```
' OR 1=1 --
```

##### ✔ 証拠（Screenshots）
（スクショを貼る）

##### ✔ 影響範囲
- 認証バイパス  
- DB ダンプ  

##### ✔ 改善策
- Prepared Statement  
- WAF  

---

### 6. Attack Path（攻撃経路）

Boot2Root の実戦ログを Pentest-Playbook の Kill Chain に沿って整理します。

例：

```
OSINT
↓
Recon（80, 22）
↓
Enumeration（SQLi 発見）
↓
Initial Access（WebShell）
↓
Local Enumeration（内部 Web 発見）
↓
Pivot（8080 → Local）
↓
Lateral Movement（SSH鍵取得）
↓
PrivEsc（SUID）
↓
Root 取得
```

---

### 7. Evidence（証拠）

- コマンド結果  
- WebShell の画面  
- PrivEsc の証拠  
- 内部ネットワークの構造  
- 横展開の証拠  

※ Boot2Root のスクリーンショットをそのまま貼る。

---

### 8. Recommendations（改善策）

Pentest-Playbook の Reporting 章の改善策を参照しながら記述。

##### ✔ システム側
- パッチ適用  
- 不要サービス停止  

##### ✔ ネットワーク側
- 内部ネットワークのセグメント化  
- 管理系サービスの外部非公開化  

##### ✔ 運用側
- パスワードポリシー  
- ログ監視  

---

### 9. Appendix（補足）

- 使用ツール一覧  
- 参考リンク  
- Pentest-Playbook の該当章一覧  

---

### 🎯 このテンプレートの目的

- Boot2Root の全章が Pentest-Playbook と 1:1 で連携  
- Boot2Root は「結果」、Pentest-Playbook は「技術体系」  
- OSCP レポート品質の Writeup を自動的に構築  
- 将来の章追加にも強い構造  
