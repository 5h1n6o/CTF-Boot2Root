# 📝 Reporting — {machine_name}

Boot2Root の攻略結果を Pentest-Playbook の Reporting 体系に沿ってまとめたレポート。

---

## 1. Executive Summary（概要）

ターゲットの概要、最終的な侵入結果、取得した権限を簡潔に記述します。

例：
- 192.168.1.10 に対して OSINT → Recon → Enumeration → Exploitation → PrivEsc → Pivot → Lateral Movement を実施  
- 最終的に root 権限を取得  
- 内部ネットワークの 192.168.1.50 に横展開成功  

---

## 2. Scope（対象範囲）

- 対象ホスト  
- 実施期間  
- 使用した手法（Blackbox / OSINT / Recon / Enumeration / Exploitation / PrivEsc / Pivot / Lateral Movement）

---

## 3. Methodology（手法）

### 🔹記述サンプル（実際の攻略内容に沿って書く例）

本調査では Pentest-Playbook の各章に準拠し、以下の手法で攻撃を進めた。

1. **OSINT**  
   公開情報（サブドメイン・メール・漏洩情報）を収集し、攻撃対象の外部情報を整理した。  

2. **Reconnaissance**  
   Nmap により全ポートスキャンを実施し、80/tcp と 22/tcp を特定した。  

3. **Enumeration**  
   HTTP サービスのディレクトリ探索とバナー調査を行い、SQLi の可能性を確認した。  

4. **Initial Access**  
   ログインフォームの SQL Injection を利用して認証バイパスし、WebShell を設置した。  

5. **Local Enumeration**  
   侵入後のホストで OS / プロセス / 内部サービスを調査し、8080 の内部 Web を発見した。  

6. **Privilege Escalation**  
   SUID バイナリ（nmap）を悪用して root 権限を取得した。  

7. **Credential Access**  
   Web アプリの設定ファイルから DB 認証情報を取得した。  

8. **Internal Enumeration**  
   内部ネットワーク（192.168.1.0/24）を探索し、複数ホストを特定した。  

9. **Pivot**  
   SSH Local Port Forward を利用して内部 Web（8080）へアクセスした。  

10. **Lateral Movement**  
    内部 Web の管理画面から SSH鍵を取得し、192.168.1.50 へ横展開した。  

---

## 4. Findings（脆弱性一覧）

Boot2Root の攻略結果を一覧化します。

| ID | 脆弱性 | 重要度 | 影響範囲 |
|----|--------|--------|-----------|
| F-01 | SQL Injection | High | 認証バイパス |
| F-02 | Weak SSH Password | High | 横展開可能 |
| F-03 | Misconfigured Cron | Medium | PrivEsc 可能 |

---

## 5. Detailed Findings（脆弱性詳細）

Pentest-Playbook の Reporting 章の構造に合わせて記述します。

### F-01: SQL Injection（High）

#### ✔ 再現手順
```
' OR 1=1 --
```

#### ✔ 証拠（Screenshots）
（スクショを貼る）

#### ✔ 影響範囲
- 認証バイパス  
- DB ダンプ  

#### ✔ 改善策
- Prepared Statement  
- WAF  

---

## 6. Attack Path（攻撃経路）

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

## 7. Evidence（証拠）

- コマンド結果  
- WebShell の画面  
- PrivEsc の証拠  
- 内部ネットワークの構造  
- 横展開の証拠  

※ Boot2Root のスクリーンショットをそのまま貼る。

---

## 8. Recommendations（改善策）

Pentest-Playbook の Reporting 章の改善策を参照しながら記述。

#### ✔ システム側
- パッチ適用  
- 不要サービス停止  

#### ✔ ネットワーク側
- 内部ネットワークのセグメント化  
- 管理系サービスの外部非公開化  

#### ✔ 運用側
- パスワードポリシー  
- ログ監視  

---

## 9. Appendix（補足）

### 使用ツール一覧  

### 参考リンク

### Pentest-Playbook 連携

- OSINT  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/OSINT/README.md

- Reconnaissance  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Reconnaissance/README.md

- Enumeration  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Enumeration/README.md

- Initial Access  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Initial-Access/README.md

- Local Enumeration  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Local-Enumeration/README.md

- PrivEsc  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Privilege-Escalation/README.md

- Credential Access  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Credential-Access/README.md

- Internal Enumeration  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Internal-Enumeration/README.md

- Pivot  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Pivot/README.md

- Lateral Movement  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Lateral-Movement/README.md

- Reporting  
  https://github.com/5h1n6o/Pentest-Playbook/blob/main/Reporting/README.md

---
