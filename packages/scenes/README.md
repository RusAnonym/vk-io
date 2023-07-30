# VK-IO Scenes

<a href="https://www.npmjs.com/package/@vk-io/scenes"><img src="https://img.shields.io/npm/v/@vk-io/scenes.svg?style=flat-square" alt="NPM version"></a>
<a href="https://github.com/negezor/vk-io/actions/workflows/tests.yml"><img src="https://img.shields.io/github/actions/workflow/status/negezor/vk-io/tests.yml?style=flat-square" alt="Build Status"></a>
<a href="https://www.npmjs.com/package/@vk-io/scenes"><img src="https://img.shields.io/npm/dt/@vk-io/scenes.svg?style=flat-square" alt="NPM downloads"></a>

> VK-IO Scenes - Simple implementation of middleware-based scene management 🎬

## 📦 Installation

> **[Node.js](https://nodejs.org/) 12.20.0 or newer is required**

- **Using `Yarn`** (recommended)
  ```shell
  yarn add @vk-io/scenes
  ```
- **Using `npm`**
  ```shell
  npm i @vk-io/scenes
  ```
- **Using `pnpm`**
  ```shell
  pnpm add @vk-io/scenes
  ```

## Example usage

```javascript
import { VK } from 'vk-io';

// Session implementation can be any
import { SessionManager } from '@vk-io/session';
import { SceneManager, StepScene } from '@vk-io/scenes';

const vk = new VK({
    token: process.env.TOKEN
});

const sessionManager = new SessionManager();
const sceneManager = new SceneManager();

vk.updates.on('message_new', sessionManager.middleware);

vk.updates.on('message_new', sceneManager.middleware);
vk.updates.on('message_new', sceneManager.middlewareIntercept); // Default scene entry handler

vk.updates.on('message_new', (context, next) => {
    if (context.text === '/signup') {
        return context.scene.enter('signup');
    }

    return next();
});

sceneManager.addScenes([
    new StepScene('signup', [
        (context) => {
            if (context.scene.step.firstTime || !context.text) {
                return context.send('What\'s your name?');
            }

            context.scene.state.firstName = context.text;

            return context.scene.step.next();
        },
        (context) => {
            if (context.scene.step.firstTime || !context.text) {
                return context.send('How old are you?');
            }

            context.scene.state.age = Number(context.text);

            return context.scene.step.next();
        },
        async (context) => {
            const { firstName, age } = context.scene.state;

            await context.send(`👤 ${firstName} ${age} ages`);

            return context.scene.step.next(); // Automatic exit, since this is the last scene
        }
    ])
]);

vk.updates.start().catch(console.error);
```
