/**
 * NUXT CONFIGURATION FILE - Dashboard Template
 * ============================================
 * 
 * This is the main Nuxt.js configuration file for a dashboard template project.
 * It defines the core setup, modules, and build configurations for the application.
 * 
 * PROJECT CONTEXT:
 * - Template: Nuxt UI Pro Dashboard Template
 * - Framework: Nuxt 4.0.0 (Latest stable release)
 * - Vue Version: Vue 3 with Composition API
 * - UI Library: Nuxt UI Pro v3.2.0 (Premium component library)
 * - TypeScript: v5.8.3 with full type safety
 * - Package Manager: PNPM v10.13.1
 * - Purpose: Modern dashboard/admin panel interface
 * 
 * NUXT VERSION DETAILS:
 * - Nuxt: ^4.0.0 (Major release with performance improvements)
 * - Compatibility Date: 2024-07-11 (Ensures stable feature set)
 * - Module Type: ESM (ES Modules)
 * - Node.js: Compatible with modern Node.js versions
 * - Build System: Vite-powered with hot reload
 * 
 * KEY CONFIGURATION SECTIONS:
 * 
 * 1. MODULES ECOSYSTEM:
 *    - @nuxt/eslint v1.5.2: Advanced code linting and formatting
 *    - @nuxt/ui-pro v3.2.0: Premium UI components with Tailwind CSS
 *    - @vueuse/nuxt v13.5.0: Vue composition utilities and helpers
 * 
 * 2. DEVELOPMENT EXPERIENCE:
 *    - DevTools enabled for enhanced development workflow
 *    - Hot reloading and instant feedback
 *    - TypeScript support with vue-tsc v3.0.1
 *    - ESLint integration for code quality
 * 
 * 3. STYLING ARCHITECTURE:
 *    - Main CSS: ~/assets/css/main.css
 *    - Tailwind CSS integration (via Nuxt UI Pro)
 *    - Icon libraries: Lucide Icons v1.2.57, Simple Icons v1.2.43
 *    - Component-based styling system
 * 
 * 4. ROUTING & API CONFIGURATION:
 *    - CORS enabled for /api/** routes
 *    - Server-side API endpoints support
 *    - Route-level configurations and middleware
 * 
 * 5. CODE QUALITY & STANDARDS:
 *    - ESLint v9.31.0 with stylistic rules
 *    - Comma dangle: never (consistent code style)
 *    - Brace style: 1tbs (one true brace style)
 *    - TypeScript strict mode enabled
 * 
 * 6. ADDITIONAL DEPENDENCIES:
 *    - @unovis/ts & @unovis/vue v1.5.2: Data visualization components
 *    - date-fns v4.1.0: Modern date utility library
 *    - zod v3.25.76: TypeScript-first schema validation
 * 
 * PERFORMANCE OPTIMIZATIONS:
 * - Nuxt 4.0 includes improved tree-shaking
 * - Optimized bundle splitting
 * - Enhanced SSR/SSG capabilities
 * - Better hydration strategies
 * 
 * USAGE INSTRUCTIONS:
 * This file is automatically loaded by Nuxt during build and development.
 * Modify this file to add new modules, configure build options, or adjust
 * application behavior. Changes require server restart in development mode.
 * 
 * COMPATIBILITY NOTES:
 * - Compatible with Nuxt 4.x ecosystem
 * - Uses modern Vue 3 Composition API
 * - Requires Node.js 18+ for optimal performance
 * - PNPM workspace compatible
 * 
 * MIGRATION NOTES:
 * - Migrated from Nuxt 3.x to 4.0 for improved performance
 * - Uses latest Nuxt UI Pro components
 * - Updated dependency resolutions for stability
 */

// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  modules: [
    '@nuxt/eslint',
    '@nuxt/ui-pro',
    '@vueuse/nuxt'
  ],

  devtools: {
    enabled: true
  },

  css: ['~/assets/css/main.css'],

  routeRules: {
    '/api/**': {
      cors: true
    }
  },

  compatibilityDate: '2024-07-11',

  eslint: {
    config: {
      stylistic: {
        commaDangle: 'never',
        braceStyle: '1tbs'
      }
    }
  }
})
