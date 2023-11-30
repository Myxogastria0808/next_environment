# Next.js Setup

## 参考サイト URL

[https://zenn.dev/brachio\_takumi/articles/a8fecd8b1b2742](https://zenn.dev/brachio\_takumi/articles/a8fecd8b1b2742)

```bash
npx create-next-app@latest --ts

yarn
wsl rm -rf package-lock.json
yarn add --dev @typescript-eslint/eslint-plugin @typescript-eslint/parser
yarn add --dev eslint-plugin-unused-imports
yarn add --dev prettier eslint-config-prettier
yarn add --dev husky lint-staged
npx husky install
yarn install
npx husky set .husky/pre-commit "npx lint-staged"
wsl touch .eslintrc.js
wsl rm -rf .eslintrc.json
wsl touch .eslintignore
wsl touch .prettierrc
wsl touch .prettierignore

```

## `.eslintrc.js`

```javascript
module.exports = {
  root: true,
  extends: [
    "plugin:@typescript-eslint/recommended", //Typescriptのエラーを無視させないためのpackageの反映
    "next/core-web-vitals", //デフォルトの設定
    "plugin:tailwindcss/recommended", //TailwindにESLintをかけるpackageの反映
    "prettier", //Prettierの反映
  ],
  plugins: ["unused-imports"],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    sourceType: "module", //moduleかscriptを指定 moduleにすることで、import文 export文 が利用できる。
    project: "./tsconfig.json", // TypeScriptのLint時に参照するconfigファイルを指定　(tsconfigRootDirからの相対パス)
    tsconfigRootDir: __dirname, //tsconfigRootDirはプロジェクトルートの絶対パスを指定する
  },
  rules: {
    //以下の設定で、Typescriptのルールを増やしている
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/no-unsafe-call": "error",
    "@typescript-eslint/no-unsafe-member-access": "error",
    "@typescript-eslint/no-unsafe-return": "error",
    "unused-imports/no-unused-imports-ts": "warn",
    //以下の設定で、import分の順番にルールを持してみやすくする
    "import/order": [
      "error",
      {
        groups: [
          "builtin",
          "external",
          "internal",
          "parent",
          "sibling",
          "index",
          "object",
          "type",
        ],
        pathGroups: [
          {
            pattern: "{react,react-dom/**,react-router-dom}",
            group: "builtin",
            position: "before",
          },
        ],
        pathGroupsExcludedImportTypes: ["builtin"],
        alphabetize: {
          order: "asc",
        },
      },
    ],
  },
};

```

## `.eslintignore`

```
node_modules
.next
out
public
.prettierrc.js
.eslintrc.js
tailwind.config.js
next.config.js
postcss.config.js

```

## `.prettierrc`

```json
{
    "printWidth": 120,
    "tabWidth": 4,
    "singleQuote": true,
    "trailingComma": "es5",
    "semi": true
}
```

## `.prettierignore`

```
node_modules
.next
dist
out
public/

```

## `package.json`

```json
    "scripts": {
        "prepare": "husky install",
        "dev": "next dev",
        "build": "next build",
        "start": "next start",
        "format": "prettier --write \"./**/*.{js, ts, tsx, jsx}\"",
        "lint-fix": "prettier --write \"./**/*.{js, ts, tsx, jsx}\" && eslint --fix --ext .jsx,.js,.tsx,.ts .",
        "lint": "prettier --check \"./**/*.{js, ts, tsx, jsx}\" && eslint --ext .jsx,.js,.tsx,.ts ."
    },
    "lint-staged": {
        "*.{js, ts, jsx, tsx}": [
            "yarn prettier --write \"./**/*.{js, ts, tsx, jsx}\"",
            "yarn prettier --check \"./**/*.{js, ts, tsx, jsx}\"",
            "yarn eslint --fix --ext .jsx,.js,.tsx,.ts ."
        ]
    },
```

***

## Next.jsのプロジェクト作成

```bash
npx create-next-app@latest --ts
```

```bash
√ What is your project named? ... next-setup
√ Would you like to use ESLint? ... No / Yes #Yes
√ Would you like to use Tailwind CSS? ... No / Yes #Yes
√ Would you like to use `src/` directory? ... No / Yes #Yes
√ Would you like to use App Router? (recommended) ... No / Yes #Yes
√ Would you like to customize the default import alias (@/*)? ... No / Yes #No
```

## yarnでpackage管理するための操作

```bash
yarn
wsl rm -rf package-lock.json
```

## ESLintについて

デフォルトでは、`next/core-web-vitals` を使用することになっている。

ESLint プラグインの推奨ルールセットはすべて eslint-config-next で使用される。

### Core Web Vitalsとは

Google の Core Web Vitals は、ウェブページのパフォーマンスを測定するための主要な指標のセット

### TypeScriptのエラーを通さないためのESLintの拡張package

```bash
yarn add --dev @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

### 未使用のimport文を削除するESLintの拡張package

```bash
yarn add --dev eslint-plugin-unused-imports
```

### ESLint + Tailwind関連のpackage

```bash
yarn add --dev eslint-plugin-tailwindcss
```

### Prettier関連のpackage

```bash
yarn add --dev prettier eslint-config-prettier
```

## `.eslintrc.js`

```javascript
module.exports = {
  root: true,
  extends: [
    "plugin:@typescript-eslint/recommended", //Typescriptのエラーを無視させないためのpackageの反映
    "next/core-web-vitals", //デフォルトの設定
    "plugin:tailwindcss/recommended", //TailwindにESLintをかけるpackageの反映
    "prettier", //Prettierの反映
  ],
  plugins: ["unused-imports"],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    sourceType: "module", //moduleかscriptを指定 moduleにすることで、import文 export文 が利用できる。
    project: "./tsconfig.json", // TypeScriptのLint時に参照するconfigファイルを指定　(tsconfigRootDirからの相対パス)
    tsconfigRootDir: __dirname, //tsconfigRootDirはプロジェクトルートの絶対パスを指定する
  },
  rules: {
    //以下の設定で、Typescriptのルールを増やしている
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/no-unsafe-call": "error",
    "@typescript-eslint/no-unsafe-member-access": "error",
    "@typescript-eslint/no-unsafe-return": "error",
    "unused-imports/no-unused-imports-ts": "warn",
    //以下の設定で、import分の順番にルールを持してみやすくする
    "import/order": [
      "error",
      {
        groups: [
          "builtin",
          "external",
          "internal",
          "parent",
          "sibling",
          "index",
          "object",
          "type",
        ],
        pathGroups: [
          {
            pattern: "{react,react-dom/**,react-router-dom}",
            group: "builtin",
            position: "before",
          },
        ],
        pathGroupsExcludedImportTypes: ["builtin"],
        alphabetize: {
          order: "asc",
        },
      },
    ],
  },
};

```

## `.eslintignore`

ESLintにかけたくないファイルやディレクトリを記入

```
node_modules
.next
out
public
.prettierrc.js
.eslintrc.js
tailwind.config.js
next.config.js
postcss.config.js

```

## `.prettierrc`

```json
{
    "printWidth": 120,
    "tabWidth": 4,
    "singleQuote": true,
    "trailingComma": "es5",
    "semi": true
}
```

## `.prettierignore`

Prettierにかけたくないファイルやディレクトリを記入

```
node_modules
.next
dist
out
public/

```

## husky + lint-staged

### huskyとlint-stagedのinstallとセットアップ

```bash
yarn add --dev husky lint-staged
npx husky install
yarn install
```

### pre-commit時に実行するlint-stagedを呼び出す

```bash
npx husky set .husky/pre-commit "npx lint-staged"
```

## `package.json`

```json
    "scripts": {
        "prepare": "husky install",
        "dev": "next dev",
        "build": "next build",
        "start": "next start",
        "format": "prettier --write \"./**/*.{js, ts, tsx, jsx}\"",
        "lint-fix": "prettier --write \"./**/*.{js, ts, tsx, jsx}\" && eslint --fix --ext .jsx,.js,.tsx,.ts .",
        "lint": "prettier --check \"./**/*.{js, ts, tsx, jsx}\" && eslint --ext .jsx,.js,.tsx,.ts ."
    },
    "lint-staged": {
        "*.{js, ts, jsx, tsx}": [
            "yarn prettier --write \"./**/*.{js, ts, tsx, jsx}\"",
            "yarn prettier --check \"./**/*.{js, ts, tsx, jsx}\"",
            "yarn eslint --fix --ext .jsx,.js,.tsx,.ts ."
        ]
    },
```
