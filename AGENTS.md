# Evento App - Agent Guide

## Project Overview

This is a Nuxt 4 event management application with:
- **Frontend**: Vue 3 + Nuxt 4 with Nuxt UI components
- **Backend**: Nuxt server with MongoDB (Mongoose)
- **Styling**: Tailwind CSS with Nuxt UI
- **State Management**: Pinia
- **Authentication**: nuxt-auth-utils
- **Validation**: yup
- **Password Hashing**: bcryptjs

## Essential Commands

```bash
# Development
npm run dev              # Start dev server at http://localhost:3000

# Production
npm run build            # Build for production
npm run preview          # Preview production build locally
npm run generate         # Generate static site (if applicable)

# Installation
npm install              # Install dependencies
```

## Environment Setup

Create a `.env` file in the root directory with:

```bash
MONGODB_URI=mongodb://localhost:27017/evento
NUXT_SESSION_PASSWORD=<32-character-string>
```

To generate a 32-character session password:
```bash
openssl rand -base64 32 | tr -d '\n'
```

## Project Structure

```
evento/
├── app/                          # Frontend application code
│   ├── app.vue                  # Root Vue component
│   ├── pages/                   # File-based routing
│   │   ├── index.vue           # Home page
│   │   ├── auth/
│   │   │   └── sign-up.vue     # Sign up/in page
│   │   └── events/
│   │       └── create-event.vue
│   ├── components/              # Vue components
│   │   └── app-header.vue      # Application header
│   ├── layouts/                # Layout templates
│   │   ├── default.vue         # Default layout with header
│   │   └── auth.vue            # Auth-only layout
│   └── assets/
│       └── css/
│           └── main.css        # Global styles, Tailwind imports
├── server/                     # Backend code
│   ├── api/                   # API routes (currently empty)
│   ├── utils/
│   │   ├── db.js             # MongoDB connection with caching
│   │   └── models/           # Mongoose models
│   │       └── user.js       # User model (currently empty)
│   └── middleware/           # Server middleware (currently empty)
├── nuxt.config.ts             # Nuxt configuration
├── package.json               # Dependencies and scripts
└── tsconfig.json              # TypeScript configuration
```

## Code Patterns & Conventions

### Vue Components (Composition API)

All Vue components use the Composition API with `<script setup>`:

```vue
<template>
  <UForm :schema="schema" :state="formData" @submit="onSubmit">
    <UFormField label="Email" name="email">
      <UInput v-model="formData.email" type="email" />
    </UFormField>
  </UForm>
</template>

<script setup>
import * as yup from 'yup';

// Schema for form validation
const schema = yup.object({
  email: yup.string().email().required(),
  password: yup.string().min(8).required(),
});

// Reactive state
const type = ref('sign-up');
const loading = ref(false);
const formData = reactive({
  email: '',
  password: '',
});

// Methods
async function onSubmit(event) {
  event.preventDefault();
  // Handle submit
}

// Page meta
definePageMeta({
  layout: 'auth'
});
</script>
```

### Nuxt UI Components

All UI components use the Nuxt UI prefix `U`:
- `UApp` - App wrapper
- `UForm`, `UFormField`, `UInput` - Form components
- `UButton` - Button with variants (text, ghost, primary, etc.)
- `UHeader`, `UNavigationMenu` - Navigation components
- `UContainer` - Container for layout
- `USeparator` - Visual separator
- `UMain` - Main content wrapper

Icons use Iconify syntax with prefixes:
- `i-lucide-arrow-right` for Lucide icons
- `mdi:account-alert-outline` for Material Design Icons

### Backend: MongoDB Connection

Use the cached connection pattern from `server/utils/db.js`:

```javascript
import mongoose from 'mongoose';
import { dbConnect } from '~/server/utils/db.js';

// In API routes or server utilities
await dbConnect();
const User = mongoose.model('User', userSchema);
```

The connection uses global caching to avoid multiple connections in serverless environments.

### Routing

- **File-based routing**: Pages are automatically routes based on file path in `app/pages/`
- **Dynamic routes**: Use square brackets for dynamic segments
- **Navigation**: Use `navigateTo('/')` for programmatic navigation
- **Layouts**: Set layout per page with `definePageMeta({ layout: 'auth' })`

### Styling

- **Tailwind CSS**: Use utility classes for all styling
- **Custom fonts**: Imported in `main.css` from Google Fonts
  - `font-anton` - Display font
  - Font family: 'Roboto Condensed' for body text (set via `--font-sans` CSS variable)
- **Note**: There's a typo in CSS - `.font-antony` should be `.font-anton`

## Testing

No test suite is currently configured. Tests should be added when implementing:
- Component testing (Vitest or Vitest + Vue Test Utils)
- API route testing
- Database model testing

## Important Gotchas

1. **TypeScript Configuration Warning**: `tsconfig.json` contains a comment on line 2 which is invalid JSON. This doesn't break the build but should be removed for clean configuration.

2. **Empty Backend Files**:
   - `server/utils/models/user.js` is empty - this needs to be populated with the User schema
   - `server/api/` directory is empty - API routes need to be created
   - `server/middleware/` directory is empty - middleware may be needed for auth

3. **MongoDB Required**: The app requires MongoDB to be running and configured via `MONGODB_URI`. The app will crash on startup if this is not set.

4. **Form Submission Not Implemented**: The sign-up form's `onSubmit` function only logs to console - API integration is pending.

5. **Authentication Not Connected**: The form exists but nuxt-auth-utils is not yet wired up to actual authentication logic.

6. **CSS Typo**: Custom font class is defined as `.font-antony` but likely should be `.font-anton` to match the font name.

## Development Workflow

1. **Start MongoDB**: Ensure MongoDB is running locally or update MONGODB_URI for remote instance
2. **Set Environment**: Create `.env` with MONGODB_URI and NUXT_SESSION_PASSWORD
3. **Install Dependencies**: `npm install`
4. **Start Dev Server**: `npm run dev`
5. **Access App**: Navigate to http://localhost:3000

## Key Dependencies

- **@nuxt/ui**: UI component library - uses Nuxt UI components exclusively
- **@pinia/nuxt**: State management - for global state (stores not yet created)
- **nuxt-auth-utils**: Authentication utilities - to be implemented
- **mongoose**: MongoDB ODM - for database operations
- **bcryptjs**: Password hashing - for secure password storage
- **yup**: Schema validation - for form validation
- **tailwindcss**: Utility-first CSS framework

## Current Implementation Status

**Implemented**:
- ✅ Project scaffolding with Nuxt 4
- ✅ Nuxt UI integration
- ✅ Tailwind CSS setup
- ✅ Layouts (default, auth)
- ✅ Sign Up/Sign In form UI
- ✅ Form validation with yup
- ✅ MongoDB connection utility
- ✅ Pinia module configuration
- ✅ Navigation header component

**Not Yet Implemented**:
- ❌ User model schema (`server/utils/models/user.js` is empty)
- ❌ API routes for auth (register, login)
- ❌ Authentication logic with nuxt-auth-utils
- ❌ Event creation page implementation
- ❌ Event management features
- ❌ Protected routes/middleware
- ❌ Database seeding or initialization
