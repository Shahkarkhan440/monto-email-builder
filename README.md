# Email Builder - Email Template Editor Component Library

> node >= 18+ + pnpm 10

A full-featured email template editor React component that can be used in other React projects.

## Project structure

```
packages/editor-sample/
├── src/              # Library source code (bundled and published to npm)
│   ├── EmailBuilder/    # Main component
│   ├── App/             # Internal components
│   ├── documents/       # Core logic
│   ├── getConfiguration/ # Configuration management
│   ├── i18n/            # Internationalization
│   ├── theme.ts         # Theme configuration
│   └── index.ts         # Library entry point
├── docs/             # Development preview project (not bundled)
│   ├── main.tsx         # Preview entry point
│   ├── index.html       # Preview HTML
│   └── favicon/         # Site icons
├── dist/              # Library build output (published to npm)
└── docs-dist/         # Preview build output (not published)
```

- **Library code**: the `src/` folder contains all library code and is bundled for npm
- **Development preview**: the `docs/` folder contains code for local development and preview, and is not bundled
- **Build commands**:
  - `npm run dev` - start the development preview server (uses the docs folder)
  - `npm run build:lib` - build the library (outputs to the dist folder)
  - `npm run build` - build the preview site (outputs to the docs-dist folder)

## Installation

```bash
npm install monto-email-builder
# or
yarn add monto-email-builder
# or
pnpm add monto-email-builder
```

### Install peerDependencies

Because this is a library, you need to install the following peerDependencies:

```bash
# Required dependencies
npm install react react-dom
npm install @mui/material @mui/icons-material
npm install @emotion/react @emotion/styled
npm install zustand zod react-colorful

# monto-email packages
npm install monto-email-block-button \
  monto-email-block-columns-container monto-email-block-container \
  monto-email-block-divider monto-email-block-heading \
  monto-email-block-html monto-email-block-image \
  monto-email-block-spacer monto-email-block-text \
  monto-email-document-core monto-email-core \
  monto-email-block-video monto-email-block-socials

# Optional dependency (for syntax highlighting in the HTML/JSON output preview)
# Install react-syntax-highlighter if you use syntax highlighting
npm install react-syntax-highlighter
# Note: code formatting is implemented in plain JavaScript and needs no extra dependencies
```

Or with yarn/pnpm:

```bash
# yarn
yarn add react react-dom @mui/material @mui/icons-material @emotion/react @emotion/styled zustand zod react-colorful monto-email-block-button monto-email-block-columns-container monto-email-block-container monto-email-block-divider monto-email-block-heading monto-email-block-html monto-email-block-image monto-email-block-spacer monto-email-block-text monto-email-document-core monto-email-core monto-email-block-video monto-email-block-socials

# pnpm
pnpm add react react-dom @mui/material @mui/icons-material @emotion/react @emotion/styled zustand zod react-colorful monto-email-block-button monto-email-block-columns-container monto-email-block-container monto-email-block-divider monto-email-block-heading monto-email-block-html monto-email-block-image monto-email-block-spacer monto-email-block-text monto-email-document-core monto-email-core monto-email-block-video monto-email-block-socials
```

## Basic usage

```tsx
import { EmailBuilder } from 'monto-email-builder';

function MyApp() {
  return <EmailBuilder />;
}
```

### Embedding in a container

The component can be embedded in any container; just give the container a fixed height:

```tsx
import { EmailBuilder } from 'monto-email-builder';

function MyApp() {
  return (
    <div style={{ width: '100%', height: '800px' }}>
      <EmailBuilder />
    </div>
  );
}
```

Or with CSS:

```tsx
import { EmailBuilder } from 'monto-email-builder';

function MyApp() {
  return (
    <div className="email-builder-container">
      <EmailBuilder />
    </div>
  );
}
```

```css
.email-builder-container {
  width: 100%;
  height: 800px; /* or any other height */
}
```

## Full example

```tsx
import { EmailBuilder, TEditorConfiguration } from 'monto-email-builder';

function MyApp() {
  // Initial document configuration (optional)
  const initialDocument: TEditorConfiguration = {
    root: {
      type: 'EmailLayout',
      data: {
        backdropColor: '#F5F5F5',
        canvasColor: '#FFFFFF',
        textColor: '#262626',
        fontFamily: 'MODERN_SANS',
        childrenIds: [],
      },
    },
  };

  // Called when the document changes
  const handleChange = (document: TEditorConfiguration) => {
    console.log('Document changed:', document);
    // You can save the document to your server
    // saveToServer(document);
  };

  // Image upload handler
  const handleImageUpload = async (file: File): Promise<string> => {
    const formData = new FormData();
    formData.append('image', file);
    
    const response = await fetch('/api/upload', {
      method: 'POST',
      body: formData,
    });
    
    const data = await response.json();
    return data.url; // Return the image URL
  };

  return (
    <EmailBuilder
      initialDocument={initialDocument}
      initialLanguage="zh"
      imageUploadHandler={handleImageUpload}
      onChange={handleChange}
    />
  );
}
```

## API

### EmailBuilder Props

| Prop | Type | Default | Description |
|------|------|--------|------|
| `initialDocument` | `TEditorConfiguration \| undefined` | `undefined` | Initial email template configuration JSON |
| `initialLanguage` | `'zh' \| 'en'` | `'en'` | Initial language |
| `imageUploadHandler` | `(file: File) => Promise<string> \| undefined` | `undefined` | Image upload callback; receives a File and returns a Promise<string> (the image URL) |
| `onChange` | `(document: TEditorConfiguration) => void \| undefined` | `undefined` | Called when the document changes |
| `theme` | `Theme \| undefined` | `undefined` | Custom Material-UI theme |

### Type exports

```tsx
import type {
  EmailBuilderProps,
  TEditorConfiguration,
  TEditorBlock,
  Language,
} from 'monto-email-builder';
```

### Hook exports

```tsx
import { useDocument, useLanguage } from 'monto-email-builder';

function MyComponent() {
  const document = useDocument(); // Get the current document
  const language = useLanguage(); // Get the current language
}
```

## Features

- ✅ Visual email template editor
- ✅ Many block types (text, image, button, container and more)
- ✅ Live preview
- ✅ Export to HTML and JSON
- ✅ Internationalization (Chinese/English)
- ✅ Image upload support
- ✅ Fully customizable
- ✅ Locked header and footer with predefined templates
- ✅ Template galleries with live previews and categories
- ✅ 31 ready-to-use starter email templates

## Header, footer and templates

### Locked header and footer

Every document always has one **header** and one **footer**, kept at the top and bottom of the email. They can't be deleted, moved or duplicated, so every email you send has a consistent header and footer.

- **Choosing one:** an empty header or footer shows a **+ Select header** / **+ Select footer** box on the canvas. Click it to pick from the predefined templates.
- **View-only in the email:** in the normal view, the header and footer can't be edited block by block. Hover over one to **Change** it (pick another template) or **Edit** it.
- **Editing mode:** **Edit** shows only that header or footer, where you can change, add or remove its blocks. Click **Done** to return to the full email.
- **Resetting:** click the header or footer and use **Delete** in its block menu to clear it. The **Select** box then appears again.

There are 8 header and 8 footer templates, including logo + navigation, brand banner, announcement bar, newsletter masthead, social links, dark footer, two-column footer and app download. Every footer includes an unsubscribe link using the `{%unsubscribe_link%}` system variable.

### Template galleries

- **Header / Footer Templates** (left panel): the first 3 headers and 3 footers are listed, and **More** opens a gallery with live previews of all of them.
- **Built-in templates** (left panel): the first 5 are listed, and **Show more** opens a gallery of all templates, organised into category tabs: All, Featured, Layouts, Ecommerce, Marketing, Transactional, Onboarding and Notifications.

### Starter templates

31 starter templates are included, each with a predefined header and footer:

- **Featured:** branded, photo-based emails (coffee shop menu, fashion lookbook, Black Friday sale, travel deals, restaurant specials, gym membership, real estate listings, skincare launch, SaaS product update, festival tickets)
- **Layouts:** one-column, two-column zig-zag, 2×2 grid, 3×2 product grid, 4-column stats, split hero, magazine, webinar agenda, testimonials and holiday greeting
- **Essentials:** welcome, newsletter, promotion, order confirmation, shipping update, password reset, verification code, event invitation, abandoned cart, feedback request and product announcement

> Starter templates use placeholder images from `placehold.co` and free photos from Unsplash (`images.unsplash.com`). For production, replace them with your own hosted images.

### Adding your own templates

| What | Where |
|------|-------|
| Header and footer templates | `src/documents/editor/headerFooterTemplates.ts` |
| Starter email templates | `src/getConfiguration/starterTemplates.ts` |
| Built-in template list, categories and sidebar order | `src/getConfiguration/builtInTemplates.ts` |
| Template loaders | `src/getConfiguration/index.tsx` |
| Labels (English / Chinese) | `src/i18n/locales/en.json`, `src/i18n/locales/zh.json` |

To add a starter template:

1. Add it to `STARTERS` in `starterTemplates.ts`, choosing a `header` and `footer` template id and the body blocks.
2. Register a loader in `getConfiguration/index.tsx`:
   ```ts
   'starter-my-template': () => import('./starterTemplates').then(m => m.buildStarterTemplate('my-template')),
   ```
3. Add it to `BUILT_IN_TEMPLATES` in `builtInTemplates.ts` with one or more categories:
   ```ts
   { sampleName: 'starter-my-template', labelKey: 'starterTemplates.my-template', categories: ['marketing'] },
   ```
4. Add its name under `starterTemplates` in both locale files.

To add a category, add its id to `TEMPLATE_CATEGORIES` in `builtInTemplates.ts` and its label under `templateCategories` in both locale files. Categories with no templates are hidden automatically. The order of `BUILT_IN_TEMPLATES` decides which 5 templates appear in the sidebar.

## Development

```bash
# Install dependencies
npm install

# Start the development server
npm run dev

# Build
npm run build
```

## License

MIT
