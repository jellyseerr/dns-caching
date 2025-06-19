# dns-caching

A small package to cache DNS lookups in Node.js, with support for custom DNS servers and TTL (time-to-live) settings.

[![NPM](https://img.shields.io/npm/v/dns-caching.svg)](https://www.npmjs.com/package/dns-caching) ![npm bundle size](https://img.shields.io/bundlephobia/min/dns-caching) ![npm](https://img.shields.io/npm/dt/dns-caching) ![GitHub](https://img.shields.io/github/license/jellyseerr/dns-caching)

## Install

| Package Manager | Command |
|---|---|
| NPM | `npm i dns-caching` |
| Yarn | `yarn add dns-caching` |
| PNPM | `pnpm add dns-caching` |

## Usage

### Basic Setup

Import the DnsCacheManager and initialize it with optional configurations:

```javascript
import DnsCacheManager from 'dns-caching';

const dnsCacheManager = new DnsCacheManager({
  cacheMaxEntries: 1000, // Optional: Maximum number of entries in the cache
  hardTtlMs: 600000,     // Optional: Hard TTL in milliseconds
  maxRetries: 5,         // Optional: Maximum number of retries for DNS lookup
  logger: console,       // Optional: Custom logger
});
```

### Initialization

Initialize the DNS cache manager to override the default dns.lookup function:

```javascript
dnsCacheManager.initialize();
```

This will intercept all DNS lookups made using the `dns` module and cache the results.

### Performing DNS Lookups

Use the lookup method to perform DNS lookups with caching:

```javascript
async function performLookup(hostname) {
  try {
    const result = await dnsCacheManager.lookup(hostname);
    console.log(`DNS Lookup Result for ${hostname}:`, result);
  } catch (error) {
    console.error(`DNS Lookup Error for ${hostname}:`, error);
  }
}

performLookup('example.com');
```

### Getting Cache Statistics

Retrieve statistics about the cache performance:

```javascript
const stats = dnsCacheManager.getStats();
console.log('Cache Statistics:', stats);
```

### Clearing the Cache

Clear the DNS cache for a specific hostname or for all entries:

```javascript
// Clear cache for a specific hostname
dnsCacheManager.clearHostname('example.com');

// Clear the entire cache
dnsCacheManager.clear();
```

### Handling Network Errors

Report network errors to the DNS cache manager to handle address switching:

```javascript
dnsCacheManager.reportNetworkError('example.com');
```

### Advanced Configuration

For advanced use cases, you can customize the behavior of the DNS cache manager by providing a custom logger or adjusting the retry logic:

```javascript
const customLogger = {
  warn: (message, meta) => console.warn(message, meta),
  debug: (message, meta) => console.debug(message, meta),
  error: (message, meta) => console.error(message, meta),
  info: (message, meta) => console.info(message, meta),
};

const customDnsCacheManager = new DnsCacheManager({
  logger: customLogger,
  maxRetries: 3,
});
```

This setup will help you efficiently manage DNS lookups with caching, improving the performance and reliability of your application.

## License

MIT © [jellyseerr](https://github.com/jellyseerr)

Created by [fallenbagel](https://github.com/fallenbagel) and [gauthier-th](https://github.com/gauthier-thc)
