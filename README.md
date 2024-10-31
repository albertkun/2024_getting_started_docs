# Docusaurus Workshop Guide

This guide covers:

- Setting up Docusaurus
- Adding a new React component
- Enabling localization

## Prerequisites

- [Node.js](https://nodejs.org/en/download/) installed on your machine.
- Basic knowledge of [Git](https://git-scm.com/) and [Yarn](https://yarnpkg.com/) or npm (https://www.npmjs.com/).

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

## Conclusion

You have now set up a Docusaurus site, added a new React component, and enabled localization for English and Japanese.
