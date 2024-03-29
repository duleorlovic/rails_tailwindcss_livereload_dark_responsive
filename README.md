# Rails Tailwind Livereload

You can start designing with https://tailwindcss.com/ with livereload just like
on their landing page.

## Install

Simplest is to use gem tailwindcss-rails and its generator
https://tailwindcss.com/docs/guides/ruby-on-rails
```
bundle add tailwindcss-rails
rails tailwindcss:install
```

but here we will enable manually using yarn postcss.
You can use propshaft instead of webpack.

`rails new` command can accept `-j` and `--css` options.
For js bundler we can choose esbuild, rollup or webpack
https://github.com/rails/jsbundling-rails
For css we can choose tailwind, bootstrap, bulma, postcss, sass
https://github.com/rails/cssbundling-rails

```
rails new rails_tailwind_livereload -j esbuild --css tailwind --skip-active-storage -a propshaft --skip-jbuilder
```
while this commands output a lot of lines you can notice two messages, that we
need to add build scripts to package.json
```
# package.json
  "scripts": {
    "build": "esbuild app/javascript/*.* --bundle --sourcemap --outdir=app/assets/builds --public-path=assets",
    "build:css": "tailwindcss -i ./app/assets/stylesheets/application.tailwind.css -o ./app/assets/builds/application.css --minify"
  }
```

Than create sample page
```
rails g controller pages index --no-helper --no-test

# config/routes.rb
  root 'pages#index'
```

Install livereload

```
# Gemfile
group :development do
  gem "hotwire-livereload"
end

# this will enable some options
rails livereload:install

# config/environments/development.rb
config.hotwire_livereload.listen_paths << Rails.root.join("app/assets/builds")
```
You do not need any extension or add-on for firefox chrome safari.

Run the app using
```
bin/dev
```

Open browser on <http://localhost:3000/> and edit to add red and blue text color
`md:text-blue-500 text-red-500` to `app/views/pages/index.html.erb`

Please use this class order to see how autosorting using prettier works. In vim
it can be installed using Coc https://github.com/iamcco/coc-tailwindcss
```
:CocInstall coc-tailwindcss
```
When you save the file, class should be `text-red-500 md:text-blue-500`

For the error
```
The asset "application.css" is not present in the asset pipeline.
```
it is probably that you run the app using `rails s` command

You can also run with
```
docker-compose up
docker-compose down
```

# Core concepts

https://tailwindcss.com/docs/responsive-design/
Responsive design uses 5 breakpoints
* `@media (min-width: 640px)` small `sm`
* `@media (min-width: 768px)` medium `md`
* `@media (min-width: 1024px)` large `lg`
* `@media (min-width: 1280px)` extra large `xl`
* `@media (min-width: 1536px)` extra large `2xl`

Use prefix like `md:` to apply on that screensize and bigger.
Default is mobile size so do not use prefix `sm:`.

Use arbitraty values https://tailwindcss.com/docs/adding-custom-styles#using-arbitrary-values
```
<div class="top-[117px] lg:top-[344px]">
```
Use prefix to apply only in that state https://tailwindcss.com/docs/hover-focus-and-other-states
```
<button class="bg-sky-600 hover:bg-sky-700 ...">

<li class="flex py-4 first:pt-0 last:pb-0">

<tr class="odd:bg-white even:bg-slate-100">
```
also form elements based on `required`, `invalid` and `disabled`
and also based on parent state using `group-`, for example cards
https://tailwindcss.com/docs/hover-focus-and-other-states#styling-based-on-parent-state
```
<div class='max-w-lg p-6 bg-white group ring hover:bg-sky-500'>
  <h3 class='text-sm font-semibold text-slate-900 group-hover:text-white'>+ New project</h3>
</div>
```

Dark mode https://tailwindcss.com/docs/dark-mode is enabled using `dark` class
on parent element
```
# tailwind.config.js
  darkMode: 'class'
```
or using system settings.

Add custom style
TODO https://tailwindcss.com/docs/adding-custom-styles

## Whats new in v3.0

https://www.youtube.com/watch?v=mSC6GwizOag

TODO rebuild ios https://www.youtube.com/watch?v=eSzNNYk7nVU

# Todo

Add holder for resizing iframe, like on
https://tailwindcss.com/docs/responsive-design

# Components

Use free component library https://daisyui.com
