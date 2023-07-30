# VK-IO Streaming
<a href="https://www.npmjs.com/package/@vk-io/streaming"><img src="https://img.shields.io/npm/v/@vk-io/streaming.svg?style=flat-square" alt="NPM version"></a>
<a href="https://github.com/negezor/vk-io/actions/workflows/tests.yml"><img src="https://img.shields.io/github/actions/workflow/status/negezor/vk-io/tests.yml?style=flat-square" alt="Build Status"></a>
<a href="https://www.npmjs.com/package/@vk-io/streaming"><img src="https://img.shields.io/npm/dt/@vk-io/streaming.svg?style=flat-square" alt="NPM downloads"></a>

> VK-IO Streaming API - Separated module for receiving data with Streaming API ⚙️

## 📦 Installation

> **[Node.js](https://nodejs.org/) 12.20.0 or newer is required**

- **Using `npm`** (recommended)
  ```shell
  npm i @vk-io/streaming
  ```
- **Using `Yarn`**
  ```shell
  yarn add @vk-io/streaming
  ```
- **Using `pnpm`**
  ```shell
  pnpm add @vk-io/streaming
  ```

## Example usage
```javascript
import { VK } from 'vk-io';

import { StreamingAPI } from '@vk-io/streaming';

const vk = new VK({
    token: process.env.TOKEN
});

const streaming = new StreamingAPI({
    api: vk.api,
    updates: vk.updates
});

vk.updates.on('publication', (context) => {
    console.log('Streaming context', context);
});

async function run() {
    await streaming.startWebSocket();

    await streaming.addRule({
        tag: 'halloween',
        value: 'тыква'
    });
}

run().catch(console.error);
```
