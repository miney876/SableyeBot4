# GitHub Actions Workflows

## CI Workflow (`ci.yml`)

This workflow runs automated checks on every push to the `release` branch and on pull requests targeting `release`.

### What it does:

1. **Checks out the code** - Gets the latest version of the repository
2. **Sets up Node.js 24.x** - Installs the required Node.js version with npm caching
3. **Installs dependencies** - Runs `npm ci` in the root, server, and website directories
4. **Verifies installation** - Confirms Node.js and npm are properly set up
5. **Syntax checks** - Validates JavaScript syntax for main entry point files:
   - `server/index.js`
   - `server/updateCommands.js`
   - `server/deleteCommands.js`
6. **Builds website** - Compiles the documentation website using Eleventy

### Triggered by:

- Push to `release` branch
- Pull requests to `release` branch

### Requirements:

- Node.js 24.x (as specified in package.json engines field)
- All package-lock.json files must be committed
- Code must pass syntax validation

### Local Testing:

You can test these steps locally before pushing:

```bash
# Install dependencies
npm ci
cd server && npm ci && cd ..
cd website && npm ci && cd ..

# Check syntax
node -c server/index.js
node -c server/updateCommands.js
node -c server/deleteCommands.js

# Build website
cd website && npm run build-docs
```

### Caching:

The workflow caches npm dependencies across runs for faster builds. The cache is based on:
- `package-lock.json`
- `server/package-lock.json`
- `website/package-lock.json`
