# Installing Tailwind into Laravel


## Laravel

First, download the Laravel installer using Composer:

    composer global require laravel/installer

Make sure to place Composer's system-wide vendor bin directory in your `$PATH` so the laravel executable can be located by your system:

    setx /M path "%path%;%appdata%\Composer\vendor\bin"

Once installed, the `laravel new` command will create a fresh Laravel installation in the directory you specify. For instance, `laravel new blog` will create a directory named `blog` containing a fresh Laravel installation with all of Laravel's dependencies already installed:

    laravel new my-project

Install Laravel dependencies:

    npm install


## Tailwind CSS & PurgeCSS

Install Tailwind CSS via npm:

    npm install tailwindcss --save-dev

Install PurgeCSS via npm:

    npm install laravel-mix-purgecss --save-dev

Create the Tailwind config file:

    ./node_modules/.bin/tailwind init

In the `resources/sass` folder, create a new file named `_tailwind.scss`
Put this code inside the newly created file:

    /* purgecss start ignore */
    @tailwind base;
    /* purgecss end ignore */
    @tailwind components;
    @tailwind utilities;

Open `app.scss` and import the previously created `_tailwind.scss`:

    @import 'tailwind';

Open the `webpack.mix.js` file and replace the contents with the following code:

    const mix = require('laravel-mix');
    const tailwindcss = require('tailwindcss');
    require('laravel-mix-purgecss');
    
    mix.js('resources/js/app.js', 'public/js')
        .sass('resources/sass/app.scss', 'public/css')
        .options({
            processCssUrls: false,
            postCss: [ tailwindcss('./tailwind.config.js') ],
        })
        .purgeCss({
            enabled: true,
            content: [
                './resources/views/**/*.htm',
                './resources/views/**/*.html',
                './resources/views/**/*.js',
                './resources/views/**/*.jsx',
                './resources/views/**/*.php',
                './resources/views/**/*.vue',
            ],
            extensions: ['htm', 'html', 'js', 'php', 'vue'],
        });

Lastly, to handle different environments, these scripts make use of `cross-env`:

    npm install cross-env --save-dev

Run the following command to test if everything is working:

    npm run dev
