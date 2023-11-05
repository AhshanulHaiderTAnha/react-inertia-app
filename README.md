<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

# Laravel React Inertia App

This is a basic setup for a Laravel application with React using the Inertia.js stack. It allows you to build dynamic web applications with the power of React on the frontend and Laravel on the backend.

## Getting Started

### Installation

To get started, follow these steps:

1. Clone this repository or create a new Laravel application using the Laravel CLI:

   ```bash
   $ composer create-project laravel/laravel react-inertia-app
   # Or you can just use Laravel CLI
   $ laravel new react-inertia-app
   $ cd react-inertia-app

2. Install the Inertia.js composer dependency:

  $ composer require inertiajs/inertia-laravel

3. Rename the file resources/app/welcome.blade.php to app.blade.php and paste the following code inside:
    
    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <!-- ... (other meta tags) -->
            @viteReactRefresh 
            @vite(['resources/css/app.css', 'resources/js/app.jsx'])
            <!-- As you can see, we will use vite with jsx syntax for React-->
            @inertiaHead
        </head>
        <body>
            @inertia
        </body>
    </html>

4. Generate a middleware for Inertia using the following command:

    $ php artisan inertia:middleware

5. Open the app/Http/Kernel.php file, go to the web middleware group, and add the generated middleware to the group:
    
    ```php
    // ...
    'web' => [
        // ...
        \App\Http\Middleware\HandleInertiaRequests::class,
    ],
    // ...

## Client-side Setup

1. Install all dependencies needed for the client-side:

    $ npm install @inertiajs/inertia-react @inertiajs/react @vitejs/plugin-react react react-dom

2. Configure Vite in the vite.config.js file:

    ```javascript
    import { defineConfig } from 'vite';
    import laravel from 'laravel-vite-plugin';
    import react from '@vitejs/plugin-react';

    export default defineConfig({
        plugins: [
            react(), // React plugin that we installed for vite.js
            laravel({
                input: ['resources/css/app.css', 'resources/js/app.js'],
                refresh: true,
            }),
        ],
    });

3.  Rename the app.js file to app.jsx and configure it:

    ```javascript
    import React from 'react'
    import {createRoot} from 'react-dom/client'
    import {createInertiaApp } from '@inertiajs/inertia-react'
    import {resolvePageComponent} from 'laravel-vite-plugin/inertia-helpers'

    createInertiaApp({
        // Below you can see that we are going to get all React components from resources/js/Pages folder
        resolve: (name) => resolvePageComponent(`./Pages/${name}.jsx`,import.meta.glob('./Pages/**/*.jsx')),
        setup({ el, App, props }) {
            createRoot(el).render(<App {...props} />)
        },
    })

4. Create a Pages folder inside the js folder, and inside this folder, create a test component named Test.jsx:

    ```javascript
    import React, { useState } from 'react';
    const Test = () => {
        return (
            <h1>This is test component</h1>
        )
    }

    export default Test

## Render Component

    Open routes/web.php and try to render Test.jsx on the homepage:

    ```php
    <?php

    use Illuminate\Support\Facades\Route;
    use Inertia\Inertia; // We are going to use this class to render React components

    Route::get('/', function () {
        return Inertia::render('Test'); // This will get component Test.jsx from the resources/js/Pages/Test.jsx
    });

## Running the Application

    Now, you're ready to run the local server and Vite in the terminal:

    $ php artisan serve
    $ npm run dev


## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
