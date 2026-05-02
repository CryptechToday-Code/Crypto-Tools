# Deployment Guide — CrypTechToday Umbrella Repo

This guide details the deployment of the entire CrypTechToday Crypto Tools Suite.

## Cloudflare Routing Configuration

To route all standalone tool repositories seamlessly under the single custom domain `https://tools.cryptechtoday.com/*`, configure the following edge routing setup:

### 1. Unified Cloudflare Worker Routing

Deploy a Cloudflare Worker that dynamically routes matching subpaths to the corresponding Cloudflare Pages project deployment:

```javascript
export default {
  async fetch(request, env) {
    const url = new URL(request.url);
    const path = url.pathname;

    if (path.startsWith('/password-generator')) {
      return fetch(`https://crypto-password-generator.pages.dev${path}`, request);
    }
    if (path.startsWith('/satoshi-converter')) {
      return fetch(`https://satoshi-to-usd-converter.pages.dev${path}`, request);
    }
    if (path.startsWith('/profit-loss-calculator')) {
      return fetch(`https://crypto-profit-loss-calculator.pages.dev${path}`, request);
    }
    if (path.startsWith('/token-roi-calculator')) {
      return fetch(`https://token-roi-calculator.pages.dev${path}`, request);
    }
    if (path.startsWith('/eth-gas-fee-estimator')) {
      return fetch(`https://eth-gas-fee-estimator.pages.dev${path}`, request);
    }
    if (path.startsWith('/mining-calculator')) {
      return fetch(`https://bitcoin-mining-calculator.pages.dev${path}`, request);
    }
    if (path.startsWith('/crypto-transfer-optimizer')) {
      return fetch(`https://crypto-transfer-optimizer.pages.dev${path}`, request);
    }

    // Default Fallback
    return fetch(`https://crypto-tools-hub.pages.dev${path}`, request);
  }
}
```

### 2. Manual Custom Domains

Alternatively, you can manually point multiple subdomains or Cloudflare Pages domains directly using CNAME DNS routing or standard Vercel rewrites.
