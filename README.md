# 🔥 CTF-Boot2Root  

**Practical Offensive Security Training — Hands-on, Structured, Repeatable**

CTF-Boot2Root は、1台のマシンを完全攻略する「Boot2Root」形式の演習を  
**体系化された学習フロー**としてまとめたリポジトリです。

単なる Writeup 集ではなく、  
**Pentest-Playbook と連携した “実戦ログ × 技術体系” の二段構造**で  
OSCP レベルの攻撃手順を再現性高く学べるように設計されています。

---

# 🎯 目的（Why Boot2Root?）

- 実戦的な攻撃手順を体系化して学ぶ  
- OSCP / Offensive Security 系の試験対策  
- 再現性のある Writeup と Reporting の作成  
- 内部ネットワーク侵入（Pivot / Lateral Movement）まで含む総合演習  
- Pentest-Playbook と連携した「攻撃手法の辞典化」

---

# 🧭 学習フロー（Pentest-Playbook 連携）

Boot2Root の Writeup は Pentest-Playbook の各章と 1:1 で対応しています。

```
OSINT
↓
Reconnaissance
↓
Enumeration
↓
Initial Access
↓
Local Enumeration
↓
Privilege Escalation
↓
Credential Access
↓
Internal Enumeration
↓
Pivot
↓
Lateral Movement
↓
Reporting
```

Boot2Root → 実戦ログ  
Pentest-Playbook → 技術体系  
Reporting → 最終成果物（OSCP品質）

---

# 📁 リポジトリ構造

```
CTF-Boot2Root/
 ├ Writeups/
 │   ├ Vuln-MachineA/
 │   │   ├ README.md
 │   │   └ images/
 │   │       ├ 01-nmap.png
 │   │       ├ 02-web-login.png
 │   │       ├ 03-shell.png
 │   │       └ 04-root.png
 │   ├ HTB-MachineA/
 │   │   ├ README.md
 │   │   └ images/
 │   └ _Writeup-Template.md
 │
 ├ Reports/
 │   ├ VulnHub-potato-Report.md
 │   ├ HTB-MachineA-Report.md
 │   └ _Report-Template.md
 │
 └ README.md
```

---

# 🛠 自動化ツール（Automation）

Boot2Root は単なる Writeup 集ではなく、  
**Writeup → Reporting → OSCPレポート** を自動生成できる仕組みを備えています。

### ✔ Reporting 自動生成  
```
python Scripts/generate_reporting.py Writeups/MachineA.md
```

### ✔ OSCPレポート自動生成  
```
python Scripts/oscp_report.py Reports/MachineA-Report.md Reports/MachineA-OSCP.md
pandoc Reports/MachineA-OSCP.md -o MachineA-OSCP-Report.pdf
```

---

# 🔗 Pentest-Playbook との連携

Boot2Root の各章は Pentest-Playbook の技術体系と完全連携しています。

- OSINT  
- Reconnaissance  
- Enumeration  
- Initial Access  
- Local Enumeration  
- PrivEsc  
- Credential Access  
- Internal Enumeration  
- Pivot  
- Lateral Movement  
- Reporting  

👉 **Pentest-Playbook:**  
https://github.com/5h1n6o/Pentest-Playbook

---

# 🏅 Vision（このプロジェクトが目指すもの）

Boot2Root は「攻略ログ」では終わらない。  
Pentest-Playbook と連携することで、  
**攻撃手法を体系化し、再現性のある学習資産として蓄積する**ことを目的としています。

- ただの Writeup → **技術書として使える資産へ**  
- ただの攻略 → **OSCPレポート品質へ**  
- ただの演習 → **内部ネットワーク侵入まで含む総合演習へ**

---
 
