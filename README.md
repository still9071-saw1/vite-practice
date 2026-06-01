# vite-practice

Vite 学習用プロジェクト。

## 学習内容

- Vite 初期構築
- npm create vite@latest
- npm run dev
- .gitignore の確認
- node_modules の理解
- package.json の確認

## トラブル

- Node.js の古いバージョンが残っていた
- VS Code 側で古い Node を参照
- PATH 確認で解決

## 起動方法

```bash
npm install
npm run dev

---

# ESLint / Prettier 導入まとめ（今回の流れ）

---

# 1. ESLint導入

## 実行コマンド

```
npm init @eslint/config@latest
```

---

# 2. ESLint設定質問

今回選択した内容。

| 質問 | 選択 |
| --- | --- |
| What do you want to lint? | JavaScript |
| How would you like to use ESLint? | To check syntax and find problems |
| What type of modules? | JavaScript modules (import/export) |
| Which framework? | None |
| TypeScript? | No |
| Where does code run? | Browser |
| Install now? | Yes |
| Package manager? | npm |

---

# 3. 作成されたファイル

```
eslint.config.js
```

---

# 4. 追加したESLintルール

```
rules: {
"no-undef":"error",
"no-unused-vars":"error",
eqeqeq:"error",
"prefer-const":"error",
"no-extra-semi":"error",
},
```

---

# 5. 各ルールの意味

| ルール | 内容 |
| --- | --- |
| no-undef | 未定義変数禁止 |
| no-unused-vars | 未使用変数禁止 |
| eqeqeq | == 禁止、=== 強制 |
| prefer-const | 変更しない変数は const |
| no-extra-semi | 余分なセミコロン禁止 |

---

# 6. Prettier導入

インストール：

## [Prettier VSCode Extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode&utm_source=chatgpt.com)

---

# 7. 発生した問題

## 保存時自動整形が動かない

原因：

```
Prettier拡張状態不整合
```

---

# 解決方法

```
Prettier無効化
↓
再有効化
```

で解消。

---

# 8. eslint-config-prettier 導入

インストール：

```
npm install-D eslint-config-prettier
```

---

# 役割

```
ESLint と Prettier の競合防止
```

---

# 9. eslint.config.js 最終形

```
importjsfrom"@eslint/js";
importglobalsfrom"globals";
import {defineConfig }from"eslint/config";
importeslintConfigPrettierfrom"eslint-config-prettier";

exportdefaultdefineConfig([
  {
    files: ["**/*.{js,mjs,cjs}"],
    plugins: { js },
    extends: ["js/recommended"],

    rules: {
"no-undef":"error",
"no-unused-vars":"error",
      eqeqeq:"error",
"prefer-const":"error",
"no-extra-semi":"error",
    },

    languageOptions: {
      globals:globals.browser,
    },
  },

eslintConfigPrettier,
]);
```

---

# 10. settings.json 最終形

```
{
  "[javascript]": {
    "editor.defaultFormatter":"esbenp.prettier-vscode"
  },

  "[typescript]": {
    "editor.defaultFormatter":"esbenp.prettier-vscode"
  },

  "[json]": {
    "editor.defaultFormatter":"esbenp.prettier-vscode"
  },

  "files.autoSave":"afterDelay",

  "editor.formatOnSave":true,

  "editor.codeActionsOnSave": {
    "source.fixAll.eslint":"explicit"
  }
}
```

---

# 11. 各設定の役割

| 設定 | 内容 |
| --- | --- |
| defaultFormatter | Prettierを整形担当に |
| autoSave | 自動保存 |
| formatOnSave | 保存時整形 |
| fixAll.eslint | 保存時ESLint修正 |

---

# 12. 今回重要だった学習

---

# コードだけが原因ではない

今回実際に起きた：

- 拡張状態問題
- 古い設定
- VSCode型変更
- バージョン差異
- 設定残骸

---

# 重要な理解

```
設定が正しい
≠
正常動作
```

---

# 実際の開発で重要

今後：

- React
- Firebase
- TypeScript
- Docker

などでも、

```
環境問題
```

は頻出。

---

# 今回得た重要な視点

```
まず環境を疑う
```

---

# 特に重要だった切り分け

```
Format Document は動く
↓
Prettier本体は正常
↓
VSCode拡張状態問題
```

この流れ。

---

# 13. 現在の環境

現在は：

```
AutoSave
↓
Prettier整形
↓
ESLint修正
```

が動く、

かなり現代的なJS開発環境になっています。

---
## フローチャート

```
① VSCode settings.json
   ↓
「保存時に何を動かすか」を決める

② 保存する / AutoSaveされる
   ↓
VSCodeが処理開始

③ Prettier
   ↓
コードの見た目を整形

④ ESLint
   ↓
コードの問題を検査・自動修正

⑤ eslint.config.js
   ↓
ESLintがどのルールで見るか参照

⑥ eslint-config-prettier
   ↓
Prettierと競合するESLintルールを無効化
```

全体フローです。

```
ファイル編集
  ↓
AutoSave発動
  ↓
settings.json を参照
  ↓
editor.formatOnSave: true
  ↓
Prettier実行
  ↓
[javascript] の defaultFormatter を参照
  ↓
esbenp.prettier-vscode で整形
  ↓
editor.codeActionsOnSave を参照
  ↓
source.fixAll.eslint 実行
  ↓
ESLintが eslint.config.js を読む
  ↓
rules を確認
  ↓
no-undef / no-unused-vars / eqeqeq などを適用
  ↓
eslint-config-prettier を最後に適用
  ↓
Prettierと競合するルールをOFF
  ↓
保存完了
```

役割で見るとこうです。

```
settings.json
= VSCodeに「保存時に何をするか」を指示

.prettierrc
= Prettierに「どう整形するか」を指示

eslint.config.js
= ESLintに「何をエラーにするか」を指示

eslint-config-prettier
= ESLintとPrettierの衝突を防ぐ
```

つまり一言で言うと、

```
settings.json が実行指示
Prettier が整形
ESLint が検査
eslint.config.js が検査ルール
eslint-config-prettier が衝突回避
```
