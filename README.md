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

## In production

Used in production by the [uSpeedo email product](https://console.uspeedo.com/email?source_code=HI3880).

A large collection of email templates is available at: https://console.uspeedo.com/email/template?source_code=HI3880

<img width="1513" height="941" alt="image" src="https://github.com/user-attachments/assets/a0861627-2894-40aa-a464-7624aaa59c07" />


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
