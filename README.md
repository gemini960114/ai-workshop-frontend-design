# AI 工作坊教材範例

## 單元 0：先備知識

### 什麼是 Skill？
Skill 是寫給 agent 的工作說明書：告訴它某一類任務該怎麼做、要遵守哪些規範、可以用哪些工具或腳本。有了 skill，agent 就不必每次從頭摸索，而是照你（或別人）整理好的經驗做事。

Skill 是一個資料夾，內含 `SKILL.md`（以及可選的腳本、資料、參考文件）。Agent 會依據 `SKILL.md` 開頭的 `description` 判斷什麼時候該自動載入它；載入後就照裡面的流程與規範做事。

你通常不需要點名 skill，只要描述任務，符合條件的 skill 就會被觸發。若想強制使用，也可以直接寫 `/skill名稱` 或「請使用 xxx skill」。

### Skill 的安裝位置

| 位置 | 範圍 |
| :--- | :--- |
| **專案目錄**（例如 `.claude/skills/`、`.agents/skills/`） | 只在這個專案生效，本課程的 skill 都裝在這裡 |
| **使用者目錄**（例如 `~/.claude/skills/`） | 全域 skill，所有專案都會生效 |

> [!NOTE]
> 如果你的電腦上另外裝了全域 skill，它們也會在本課程的專案裡出現，可能和課程的 skill 搶著被觸發。

---

### 安裝指令說明

> [!WARNING]
> **本課程的 12 個 skill 已經裝好，課堂上不要執行本教材裡的安裝、更新、移除指令。**  
> 重新安裝可能換成不同版本，讓你的結果和教材不同。這些指令是給你離開本課程、在自己的專案安裝 skill 時參考。唯一例外是標示「選做」、要另外安裝 skill 的段落：課後可以依該段的指示安裝，做完照指示移除。

#### 安裝語法
```bash
npx skills add <GitHub擁有者>/<repo>[@skill名稱] -a antigravity -a claude-code --copy -y
```

| 參數 | 意義 |
| :--- | :--- |
| `owner/repo` | 安裝該 repo 內所有 skills |
| `owner/repo@name` | 只安裝 repo 中的某一個 skill |
| `-a antigravity -a claude-code` | 同時安裝給兩個 agent（寫入 `.agents/skills/` 與 `.claude/skills/`） |
| `--copy` | 複製檔案而非建立連結（Windows 上較穩定） |
| `-y` | 略過確認提示 |

#### 常用管理指令
```bash
npx skills find <關鍵字>   # 搜尋 skill
npx skills update          # 更新所有已安裝 skill
```

> [!TIP]
> 安裝完成後，專案根目錄會出現 `skills-lock.json` 記錄版本。**重新啟動 agent 工作階段後 skill 才會生效**。

---

## 單元 1：設計品質 — frontend-design

```bash
npx skills add anthropics/skills@frontend-design -a antigravity -a claude-code --copy -y
```

- **用途**：Anthropic 官方 skill。讓 AI 做前端時像「設計工作室的設計總監」，從主題本身找出配色、字體與版面方向，避免千篇一律的「紫色漸層 + 大數字 + 卡片」AI 模板感。

### 重點觀念
1. **先釐清主題、受眾、頁面的主要任務**，再決定美學方向。
2. **Hero 區要放「這個主題最有代表性的東西」**。
3. **字體刻意挑選，建立明確的字級系統**；字體本身就是設計的一部分。

---

### 範例 1：從零做一個有個性的產品頁

#### 任務 Prompt
```text
在目錄 example/case01/ 中，幫我做一個台灣手工茶葉品牌「山嵐茶研所」的單頁官網（HTML + CSS），主頁面存成 example/case01/index.html。
受眾是 30–50 歲重視產地與職人精神的消費者。
需要：品牌故事、三款茶品介紹、產地地圖區、訂購按鈕。
不要用常見的 SaaS 模板風格，請先提出 2 個美學方向讓我選。
```

- **預期行為**：Agent 先提出例如「宣紙水墨 + 襯線字」與「高山晨霧 + 大留白」兩種方向，確認後再實作，配色與字體都會呼應茶與山的主題。
- **驗證觸發**：完成後追問下面這句，確認 agent 確實載入了 skill：
  ```text
  在這個 Case 你使用了哪些 Skills
  ```

