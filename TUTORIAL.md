# EVENTO APP

## ABOUT EVENTO
This is a nuxtJS app for learning basics of nuxtJS.

## Installation

```bash
    pnpm create nuxt@latest
```
Go with defaults. Don't add any modules. We will add them manually.

Once you are done with installation, navigate to the app directory. Then open the directory in your favorite editor.

Open a browser and go to https://nuxt.com. The go to the Modules link. Find NuxtUI and select it. Look for installation instructions.

You will find a command to install it as per your package manager. On my side, I am using pnpm, so:

```bash
 pnpm add @nuxt/ui tailwindcss
 ```

Once you are done with installation, you need to add the module in the `nuxt.config.ts` file.

```typescript
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  compatibilityDate: '2025-07-15',
  devtools: { enabled: true },
  modules: ['@nuxt/ui'],
})
```

Then we need to import tailwind and nuxtui in your css. In the app directory, create assets/css/main.css file. Inside it, import TailWindCSS and and nuxtui.

```css
@import "tailwindcss";
@import "@nuxt/ui";
```

Add css in your nuxt.config.ts file.

```typescript
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  compatibilityDate: '2025-07-15',
  devtools: { enabled: true },
  modules: ['@nuxt/ui'],
  css: [`~/assets/css/main.css`],
})
```

Next, let us add fonts. Navigate to fonts.google.com. I selected roboto and anton. You can choose whatever you want. Then add them into the main.css as embedded code.

```css
@import url('https://fonts.googleapis.com/css2?family=Anton&family=Roboto+Condensed:ital,wght@0,100..900;1,100..900&display=swap');
@import "tailwindcss";
@import "@nuxt/ui";

:root {
    --font-sans: 'Roboto Condensed', sans-serif;
}

.font-antony {
    font-family: 'Anton', sans-serif;
}
```

## Install nuxt-auth-utils module
Install nuxt-auth-utils module using pnpm:

```bash
npx nuxi@latest module add auth-utils
```
Add .env file and add a session password which should be 32 characters long.

To create 32-character session password, do the following if you wish:

```bash
openssl rand -base64 32 | tr -d '\n'
```

or you can use whatever 32-character string you wish.

## Installing Pinia
Next we install Pinia. Again go back to modules and search for Pinia. Once you get it, follow the instructions to install it.

```bash
npx nuxi@latest module add pinia
```

Add other libraries that we will need.

```bash
pnpm add yup@latest mongoose@latest bcryptjs@latest
```

## AppHeader
- Create a layout directory
- Create default layout (default.vue)
- Add NuxtLayout component in App.vue
- Create components directory in the app directory
- Inside componentsdirectory, add app-header.vue