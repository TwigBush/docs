# TwigBush Docs

## 🚀 Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

The site will be running at `http://localhost:3000`



## 📁 Project Structure

```
my-docs/
├── content/              # Your markdown content
│   ├── en/              # English content
│   │   ├── index.md     # English homepage
│   │   └── docs/        # English documentation
│   └── fr/              # French content
│       ├── index.md     # French homepage
│       └── docs/        # French documentation
├── public/              # Static assets
├── nuxt.config.ts       # Nuxt configuration with i18n setup
└── package.json         # Dependencies and scripts
```

### Content Structure

The content is organized by language, making it easy to manage translations:

```
content/
├── en/                   # English content
│   ├── index.md
│   ├── 1.getting-started/
│   │   ├── installation.md
│   │   └── configuration.md
│   └── 2.essentials/
│       ├── markdown.md
│       └── components.md
└── fr/                   # French content
    ├── index.md
    ├── 1.getting-started/
    │   ├── installation.md
    │   └── configuration.md
    └── 2.essentials/
        ├── markdown.md
        └── components.md
```

## 🔗 URL Structure

The i18n starter generates URLs with language prefixes:

- English: `/en/getting-started/installation`
- Default locale fallback: `/getting-started/installation` (redirects to English)


## Deployment

Create build

```bash
npm run build
```

The built files will be in the `.output` directory, ready for deployment to any hosting provider that supports Node.js.

## 📄 License

[MIT License](https://opensource.org/licenses/MIT) 
