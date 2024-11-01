# Docusaurus Workshop Guide

This guide covers:

- Setting up Docusaurus
- Adding a new React component
- Enabling localization

## Prerequisites

- [Node.js](https://nodejs.org/en/download/) installed on your machine.
- Basic knowledge of [Git](https://git-scm.com/) and GitHub Pages.

### Note: `npm` can be used instead of `yarn` for package management

## Setting Up Docusaurus

1. **Install Docusaurus CLI**:

```shell
   yarn create docusaurus my-website classic
```

2. **Navigate to the project directory**:
```shell
   cd my-website
```
3. **Start the development server**:

```shell
   yarn start
```

   - Open `http://localhost:3000` in your browser to see the site.

## Adding a New React Component

1. **Create a new React component**:

   - Create a file named `NewComponent.js` in the `src/components` directory.

2. **Add content to the new component**:

```js
   import React from 'react';

   const NewComponent = () => {
     return (
       <div>
         <h1>New Component</h1>
         <p>Welcome to the new component. This section covers advanced topics.</p>
       </div>
     );
   };

   export default NewComponent;
```

3. **Use the new component in a page**:

   - Open an existing page or create a new one in the 

pages

 directory, for example, `src/pages/new-page.js`.

   - Import and use the new component:

```js
   import React from 'react';
   import NewComponent from '../components/NewComponent';

   const NewPage = () => {
     return (
       <div>
         <NewComponent />
       </div>
     );
   };

   export default NewPage;
```

4. **Update the sidebar configuration** (if necessary):

   - Modify `sidebars.js` to include the new page:

```js
   module.exports = {
     tutorialSidebar: [
       'new-page',
     ],
   };
```

## Enabling Localization

1. **Configure localization settings**:

   - Update `docusaurus.config.js`:

   module.exports = {
     i18n: {
       defaultLocale: 'en',
       locales: ['en', 'ja'],
     },
   };

  - Add locale dropdown to the header in `docusaurus.config.js` (optional):

```js
   module.exports = {
     themeConfig: {
       navbar: {
         title: 'My Website',
         items: [
           {
             type: 'localeDropdown',
             position: 'right',
           },
         ],
       },
     },
   };
```


2. **Generate translation files**:

```shell
   yarn run write-translations --locale ja
```

3. **Translate content**:

   - Navigate to the newly created folder `docusaurus-plugin-content-docs` in `i18n/ja` 
   - Create a new folder called `current` and add translation files for the pages you want to localize.
      - This folder should contain the translated Markdown files for the pages.
      - For example, `i18n/ja/docusaurus-plugin-content-docs/current/new-page.md`:


4. **Add translation files for the components**:

For React components, we need import `Translate` and add a `<translate>` tag in the JSX code. For example, in `NewComponent.js`:

  ```js
     import React from 'react';
     import Translate from '@docusaurus/Translate';

     const NewComponent = () => {
       return (
         <div>
           <h1><Translate>New Component</Translate></h1>
           <p><Translate>Welcome to the new component. This section covers advanced topics.</Translate></p>
         </div>
       );
     };

     export default NewComponent;
   ```
- Then we need to run 
```shell
yarn run write-translations --locale ja
```
This generate the translation files for the components by creating a JSON file for each component in the `i18n` directory.
- Then we can add the translations for the components in the JSON files. For example, in `i18n/ja/docusaurus-plugin-content-docs/current.json`:

```json
   {
     "New Component": "新しいコンポーネント",
     "Welcome to the new component. This section covers advanced topics.": "新しいコンポーネントへようこそ。このセクションでは高度なトピックをカバーします。"
   }
```

1. **Test localization**:

   - Start the development server with the Japanese locale:

```shell
   yarn start --locale ja
```

6. **Build the site with all locales**:

```shell
   yarn build
```

7. **Serve the built site**:

```shell
   yarn serve
```

   - The site will now be available in both English and Japanese.

## Deploying Documentation Using GitHub Actions

1. **Create a workflow**: Create a new file named `deploy.yml` in the `.github/workflows` directory.
2. **Configure the workflow**: Add the following content to the `deploy.yml` file:

```yaml
name: Deploy Documentation

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy-documentation:
    runs-on: ubuntu-latest
    name: Deploy Documentation to GitHub Pages
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: yarn
          cache-dependency-path: "./package.json"
      - name: Install dependencies
        run: yarn install
      - name: Build website
        run: yarn build
      - name: Deploy Documentation to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GH_ACTIONS_TOKEN }}
          publish_dir: ./build
          publish_branch: gh-pages
          user_name: github-actions[bot]
          user_email: 41898282+github-actions[bot]@users.noreply.github.com
```

#### 🚧Note
If you are using `npm`, replace `yarn` with `npm` in the workflow file. For example, `yarn install` becomes `npm install` and `yarn build` becomes `npm run build`.

3. **Commit and push the changes**: Commit the changes and push them to the repository.
4. **View the deployment**: View the deployment on GitHub Pages.

### Setting Up GitHub Tokens

1. **Create a Personal Access Token**:
   - Go to GitHub and navigate to **Settings**.
   - Select **Developer settings** from the sidebar.
   - Click on **Personal access tokens**.
   - Click on **Generate new token**.
   - Give your token a descriptive name.
   - Select the scopes `repo` and `workflow`.
   - Click **Generate token**.
   - Copy the token and save it securely. You will not be able to see it again.

2. **Add the Token to Your Repository**:
   - Go to your GitHub repository.
   - Click on **Settings**.
   - Select **Secrets and variables** from the sidebar, then click on **Actions**.
   - Click on **New repository secret**.
   - Name the secret `GH_ACTIONS_TOKEN`.
   - Paste the personal access token you copied earlier.
   - Click **Add secret**.

### Workflow Details

1. **Trigger the Workflow**: The workflow is triggered automatically on a push to the `main` branch or can be manually triggered.
2. **Checkout the Repository**: The workflow uses the `actions/checkout@v3` action to checkout the repository.
3. **Setup Node.js**: The workflow sets up Node.js version 18 using the `actions/setup-node@v3` action and caches dependencies using Yarn.
4. **Install Dependencies**: The workflow installs the project dependencies using the `yarn install` command.
5. **Build the Website**: The workflow builds the website using the `yarn build` command.
6. **Deploy to GitHub Pages**: The workflow deploys the built documentation to GitHub Pages using the `peaceiris/actions-gh-pages@v3` action. It uses the GitHub token stored in the `GH_ACTIONS_TOKEN` secret to authenticate the deployment. The built files are published from the `./build` directory to the `gh-pages` branch.

## Conclusion

You have now set up a Docusaurus site, added a new React component, and enabled localization for English and Japanese.

# ドキュサウルス ワークショップ ガイド

このガイドでは、次の内容について説明します。

- ドキュサウルスのセットアップ
- 新しい React コンポーネントの追加
- ローカライゼーションの有効化
- GitHub Actions を使用したドキュメントのデプロイ
- GitHub トークンの設定
- ワークフローの詳細
- 結論

## 前提条件

- マシンに [Node.js](https://nodejs.org/en/download/) がインストールされていること。
- [Git](https://git-scm.com/) と GitHub Pages の基本的な知識。
- パッケージ管理に `yarn` の代わりに `npm` を使用できます。

## ドキュサウルスのセットアップ

1. **Docusaurus CLI のインストール**:

```shell
   yarn create docusaurus my-website classic
```

2. **プロジェクトディレクトリに移動**:
```shell
   cd my-website
```

3. **開発サーバーを起動**:

```shell
   yarn start
```

   - サイトを表示するには、ブラウザで `http://localhost:3000` を開きます。
 - このページはテスト用です。

## 新しい React コンポーネントの追加

1. **新しい React コンポーネントを作成**:

   - `src/components` ディレクトリに `NewComponent.js` という名前のファイルを作成します。
  

2. **新しいコンポーネントにコンテンツを追加**:

```js
   import React from 'react';

   const NewComponent = () => {
     return (
       <div>
         <h1>New Component</h1>
         <p>Welcome to the new component. This section covers advanced topics.</p>
       </div>
     );
   };

   export default NewComponent;
```

3. **新しいコンポーネントをページで使用**:

   - 既存のページを開いて、または `src/pages/new-page.js` のような新しいページを作成します。

   - 新しいコンポーネントをインポートして使用します:

```js
   import React from 'react';
   import NewComponent from '../components/NewComponent';

   const NewPage = () => {
     return (
       <div>
         <NewComponent />
       </div>
     );
   };

   export default NewPage;
```

4. **サイドバーの構成を更新** (必要に応じて):

   - 新しいページを含めるように `sidebars.js` を変更します:

```js
   module.exports = {
     tutorialSidebar: [
       'new-page',
     ],
   };
```

## ローカライゼーションの有効化

1. **ローカライゼーション設定を構成**:

   - `docusaurus.config.js` を更新します:

   module.exports = {
     i18n: {
       defaultLocale: 'en',
       locales: ['en', 'ja'],
     },
   };

  - ヘッダーにロケールドロップダウンを追加します (オプション):

```js
   module.exports = {
     themeConfig: {
       navbar: {
         title: 'My Website',
         items: [
           {
             type: 'localeDropdown',
             position: 'right',
           },
         ],
       },
     },
   };
```

2. **翻訳ファイルを生成**:

```shell
   yarn run write-translations --locale ja
```

3. **コンテンツを翻訳**:

   - `i18n/ja` の `docusaurus-plugin-content-docs` ディレクトリに移動します。
   - `current` という新しいフォルダを作成し、ローカライズしたいページの翻訳ファイルを追加します。
      - このフォルダには、ページの翻訳済み Markdown ファイルが含まれている必要があります。
      - たとえば、`i18n/ja/docusaurus-plugin-content-docs/current/new-page.md`:

4. **コンポーネントの翻訳ファイルを追加**:

React コンポーネントの場合、`Translate` をインポートし、JSX コードに `<translate>` タグを追加する必要があります。たとえば、`NewComponent.js` では次のようになります:

  ```js
     import React from 'react';
     import Translate from '@docusaurus/Translate';

     const NewComponent = () => {
       return (
         <div>
           <h1><Translate>New Component</Translate></h1>
           <p><Translate>Welcome to the new component. This section covers advanced topics.</Translate></p>
         </div>
       );
     };

     export default NewComponent;
   ```

- 次に、次のコマンドを実行します
```shell
yarn run write-translations --locale ja
```
これにより、`i18n` ディレクトリにコンポーネントの翻訳ファイルが生成され、各コンポーネントのために JSON ファイルが作成されます。

- 次に、JSON ファイルにコンポーネントの翻訳を追加できます。たとえば、`i18n/ja/docusaurus-plugin-content-docs/current.json`:

```json
   {
     "New Component": "新しいコンポーネント",
     "Welcome to the new component. This section covers advanced topics.": "新しいコンポーネントへようこそ。このセクションでは高度なトピックをカバーします。"
   }
```

1. **ローカライゼーションをテスト**:

   - 日本語ロケールで開発サーバーを起動します:

```shell
   yarn start --locale ja
```

6. **すべてのロケールでサイトをビルド**:

```shell
   yarn build
```

7. **ビルドされたサイトを公開**:

```shell
   yarn serve
```

   - サイトは英語と日本語の両方で利用できるようになります。

## GitHub Actions を使用したドキュメントのデプロイ

1. **ワークフローを作成**: `.github/workflows` ディレクトリに `deploy.yml` という新しいファイルを作成します。
2. **ワークフローを構成**: `deploy.yml` ファイルに次の内容を追加します:

```yaml
name: Deploy Documentation

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy-documentation:
    runs-on: ubuntu-latest
    name: Deploy Documentation to GitHub Pages
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: yarn
          cache-dependency-path: "./package.json"
      - name: Install dependencies
        run: yarn install
      - name: Build website
        run: yarn build
      - name: Deploy Documentation to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GH_ACTIONS_TOKEN }}
          publish_dir: ./build
          publish_branch: gh-pages
          user_name: github-actions[bot]
          user_email: 41898282+github-actions[bot]@users.noreply.github.com
```

#### 🚧注意
`npm` を使用している場合は、ワークフローファイルで `yarn` を `npm` に置き換えてください。たとえば、`yarn install` は `npm install` になり、`yarn build` は `npm run build` になります。

3. **変更をコミットしてプッシュ**: 変更をコミットしてリポジトリにプッシュします。
4. **デプロイを表示**: GitHub Pages でデプロイを表示します。
5. **GitHub トークンの設定**
6. **パーソナルアクセストークンを作成**:
   - GitHub に移動し、**Settings** に移動します。
   - サイドバーから **Developer settings** を選択します。
   - **Personal access tokens** をクリックします。
   - **Generate new token** をクリックします。
   - トークンに説明的な名前を付けます。
   - スコープに `repo` と `workflow` を選択します。
   - **Generate token** をクリックします。
   - トークンをコピーして安全に保存します。もう一度表示することはできません。
7. **トークンをリポジトリに追加**:
   - GitHub リポジトリに移動します。
   - **Settings** をクリックします。
   - サイドバーから **Secrets and variables** を選択し、**Actions** をクリックします。
   - **New repository secret** をクリックします。
   - シークレットの名前を `GH_ACTIONS_TOKEN` とします。
   - 以前にコピーしたパーソナルアクセストークンを貼り付けます。
   - **Add secret** をクリックします。
### ワークフローの詳細

1. **ワークフローのトリガー**: ワークフローは `main` ブランチへのプッシュで自動的にトリガーされるか、手動でトリガーできます。
2. **リポジトリをチェックアウト**: ワークフローはリポジトリをチェックアウトするために `actions/checkout@v3` アクションを使用します。
3. **Node.js をセットアップ**: ワークフローは `actions/setup-node@v3` アクションを使用して Node.js バージョン 18 をセットアップし、Yarn を使用して依存関係をキャッシュします。
4. **依存関係をインストール**: ワークフローは `yarn install` コマンドを使用してプロジェクトの依存関係をインストールします。
5. **ウェブサイトをビルド**: ワークフローは `yarn build` コマンドを使用してウェブサイトをビルドします。
6. **GitHub Pages にデプロイ**: ワークフローは `peaceiris/actions-gh-pages@v3` アクションを使用してビルドされたドキュメントを GitHub Pages にデプロイします。デプロイメントを認証するために `GH_ACTIONS_TOKEN` シークレットに保存されている GitHub トークンを使用します。ビルドされたファイルは `./build` ディレクトリから `gh-pages` ブランチに公開されます。

## 結論

Docusaurus サイトをセットアップし、新しい React コンポーネントを追加し、英語と日本語のローカライゼーションを有効にしました。GitHub Actions を使用してドキュメントをデプロイしました。
