# Docusaurus Workshop Guide

This guide covers:

- Setting up Docusaurus
- Adding a new React component
- Enabling localization
- Deploying the site on GitHub pages

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

2. **Generate translation files**:

```shell
   yarn run write-translations --locale ja
```

3. **Translate content**:

   - Edit the files in `i18n/ja` and provide Japanese translations.

For example `i18n/ja/new-page.json`:

```json
   {
     "New Component": "新しいコンポーネント",
     "Welcome to the new component. This section covers advanced topics.": "新しいコンポーネントへようこそ。このセクションでは高度なトピックをカバーします。"
   }
```


4. **Test localization**:

   - Start the development server with the Japanese locale:

```shell
   yarn start --locale ja
```

5. **Build the site with all locales**:

```shell
   yarn build
```

6. **Serve the built site**:

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

#### 🚧Note: If you are using `npm`, replace `yarn` with `npm` in the workflow file. For example, `yarn install` becomes `npm install` and `yarn build` becomes `npm run build`.

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
