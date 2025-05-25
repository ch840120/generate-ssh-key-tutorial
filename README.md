🔐 建立 GitHub SSH 金鑰教學（適用於 macOS / Linux / WSL）<br>
使用 SSH 金鑰，可以更安全、方便地與 GitHub 建立連線，不必每次輸入帳號密碼！<br>
✅ 備註：下列指令請在 Git Bash 中執行

## 🗂️ 目錄
- [📌 步驟 1：檢查本機是否已有 SSH 金鑰](#-步驟-1檢查本機是否已有-ssh-金鑰)
- [🛠️ 步驟 2：產生新的 SSH 金鑰](#️-步驟-2產生新的-ssh-金鑰)
- [📂 步驟 3：選擇金鑰儲存路徑](#-步驟-3選擇金鑰儲存路徑)
- [🔒 步驟 4：設定密碼（可選）](#-步驟-4設定密碼可選)
- [🚀 步驟 5：將 SSH 金鑰加入 ssh-agent](#-步驟-5將-ssh-金鑰加入-ssh-agent)
  - [1️⃣ 啟動 ssh-agent](#1️⃣-啟動-ssh-agent)
  - [2️⃣ 加入 SSH 金鑰到 ssh-agent](#2️⃣-加入-ssh-金鑰到-ssh-agent)
- [📋 步驟 6：複製公鑰內容](#-步驟-6複製公鑰內容)
- [🌐 步驟 7：將 SSH 公鑰加入 GitHub](#-步驟-7將-ssh-公鑰加入-github)
- [🧪 步驟 8：測試連線是否成功](#-步驟-8測試連線是否成功)


## 📌 步驟 1：檢查本機是否已有 SSH 金鑰

```bash
ls -al ~/.ssh
```
🔍 查看是否已有 id_ed25519 或 id_rsa 等金鑰檔案
（若已有金鑰，可選擇直接使用或備份後重建）

## 🛠️ 步驟 2：產生新的 SSH 金鑰

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
📖 選項說明：
| 參數                | 意義                            |
| ----------------- | ----------------------------- |
| `-t ed25519`      | 指定金鑰類型為 Ed25519（建議，較安全快速）     |
| `-C "your_email"` | 加註一個備註（通常使用 GitHub 註冊的 Email） |

## 📂 步驟 3：選擇金鑰儲存路徑
```text
Enter a file in which to save the key (/home/you/.ssh/id_ed25519):
```
- 建議：按 Enter 使用預設位置
- 也可自訂，如：~/.ssh/id_ed25519_github

## 🔒 步驟 4：設定密碼（可選）
``` text
Enter passphrase (empty for no passphrase):
```
🔐 輸入密碼可提升安全性，也可直接按 Enter 略過

## 🚀 步驟 5：將 SSH 金鑰加入 ssh-agent

### 1️⃣ 啟動 ssh-agent

```bash
eval "$(ssh-agent -s)"
```
🔧 這個指令會啟動一個 ssh-agent 背景程序，用來暫時儲存你的私鑰，以便你在使用 Git 操作（如 push/pull）時不需要每次都輸入密碼。

執行後會出現類似：
```nginx
Agent pid 12345
```
### 2️⃣ 加入 SSH 金鑰到 ssh-agent
```bash
ssh-add ~/.ssh/id_ed25519
```
🔐 這行指令的作用是什麼？
<br>
這是將你產生的私鑰 (id_ed25519) 加入到 ssh-agent 中：
- ✅ 這樣你就不用每次操作 Git 都輸入私鑰密碼（若你有設定）
- ✅ 金鑰會在系統記憶體中暫存，只要當前 session 還在就有效
- ⚠️ 若你使用自訂路徑建立金鑰，例如 ~/.ssh/id_ed25519_github，這邊也要對應修改！

## 📋 步驟 6：複製公鑰內容
```bash
cat ~/.ssh/id_ed25519.pub
```

## 🌐 步驟 7：將 SSH 公鑰加入 GitHub

1. 登入 [GitHub](https://github.com)
2. 點選右上角頭像 → 選擇 **Settings**
3. 在左側選單中，點選 **SSH and GPG keys**
4. 點擊右上角的 **New SSH key** 按鈕
5. 在表單中：
   - 📝 **Title** 欄位：輸入此金鑰的名稱（例如：`My Laptop`）
   - 🔑 **Key** 欄位：貼上你剛剛複製的 SSH 公鑰內容
6. 點選 **Add SSH key** 儲存金鑰

## 🧪 步驟 8：測試連線是否成功

```bash
ssh -T git@github.com
```
🔍 這個指令會測試你是否能成功透過 SSH 與 GitHub 建立連線。

✅ 成功畫面如下：
```vbnet
Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.
```
🎉 表示你已經成功完成 SSH 設定，可以開始使用 GitHub 進行 clone、push、pull 等操作！
