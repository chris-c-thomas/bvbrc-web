# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BV-BRC (Bacterial and Viral Bioinformatics Resource Center) is a web-based bioinformatics platform for infectious disease research. It combines an Express.js backend with a Dojo AMD frontend containing 260+ widgets for genomic data visualization and analysis.

## Common Commands

```bash
# Install dependencies and initialize submodules
npm install
git submodule update --init

# Start development server (runs on http://localhost:3000)
npm start

# Run linting
./node_modules/.bin/eslint public/js/p3

# Run linting on specific file
./node_modules/.bin/eslint public/js/p3/widget/SomeWidget.js

# Auto-fix linting issues
./node_modules/.bin/eslint public/js/p3/widget/SomeWidget.js --fix

# Run tests
npm test
```

## Architecture

### Backend (Node.js/Express)
- **Entry point**: `bin/p3-web` - Creates HTTP/HTTPS server on port 3000
- **Main app**: `app.js` - Express configuration, middleware, route mounting
- **Routes**: `routes/` - Express route handlers (workspace, jobs, search, viewers, apps)
- **Config**: `config.js` - nconf-based configuration; requires `p3-web.conf` file (copy from `p3-web.conf.sample`)
- **Templates**: `views/` - EJS templates for server-side rendering
- **Security**: `lib/securityUtils.js` - Input sanitization utilities

### Frontend (Dojo/AMD)
- **Application**: `public/js/p3/app/` - Main application entry points
  - `app.js` - Core application setup
  - `p3app.js` - P3-specific configuration and panel management
- **Widgets**: `public/js/p3/widget/` - 260+ Dojo widgets for UI components
- **Data stores**: `public/js/p3/store/` - Memory/REST stores for genomic data
- **Managers**: `public/js/p3/` - WorkspaceManager.js, JobManager.js, UploadManager.js

### Submodules
Frontend libraries are managed as git submodules in `public/js/`:
- Dojo framework: `dojo/`, `dijit/`, `dojox/`
- Visualization: `d3/`, `cytoscape/`, `JBrowse/`, `phyloview/`
- Data grid: `dgrid/`, `rql/`

## Code Conventions

### JavaScript Style
- Uses AMD (Asynchronous Module Definition) pattern via Dojo
- ESLint config extends `airbnb-base/legacy` with many rules relaxed for legacy code compatibility
- Global variables available: `dojo`, `d3`, `cytoscape`, `msa`, `BVBRCClient`, `Hotmap`
- `no-unused-vars` is enforced as an error
- `no-undef` is a warning

### Module Pattern Example
```javascript
define([
  'dojo/_base/declare',
  'dijit/_WidgetBase',
  './SomeOtherWidget'
], function (
  declare,
  WidgetBase,
  SomeOtherWidget
) {
  return declare([WidgetBase], {
    // widget implementation
  });
});
```

## Configuration

The application requires a `p3-web.conf` file for service URLs and settings:
```bash
cp p3-web.conf.sample p3-web.conf
# Edit p3-web.conf with appropriate service URLs
```

Configuration changes require restarting `./bin/p3-web`.

## Pre-commit Hook (Optional)

See `docs/pre-commit-linting.md` for setup instructions to run ESLint automatically on staged files before commits.
