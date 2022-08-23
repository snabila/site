---
title: "Adding Tailwind CSS to a Hugo Theme"
date: 2022-06-12T13:07:09+07:00
draft: false
tags: ["ssg", "hugo", "jamstack"]
---

## Steps

I'm going to assume that you already have Hugo and NodeJS installed on your machine. If you don't, here are some links that might be useful:

- [Install Hugo](https://gohugo.io/getting-started/installing). **\*Make sure you're installing the extended version.**
- [Install Node with NVM](https://github.com/nvm-sh/nvm#install--update-script).
- [Install Node with Node Installer](https://nodejs.org/en/).

### Creating a New Site

- If you don't have an existing Hugo project, you can make one by using the following command in your terminal. For example `hugo new site new-hugo-site` will create a new Hugo project in the `new-hugo-site` directory.

  ```bash
  hugo new site site-name
  ```

- After it's done, go inside your project's directory (eg; `cd new-hugo-site`), and create a new theme with the following command. This will create a new directory for your new theme in `themes`.

  ```bash
  hugo new theme theme-name
  ```

- Link the new theme by editing your config file. It's usually in the project root and by default uses the `.toml` format, but you can change it to `.json` or `.yaml` if you want.

  ```toml
  baseURL = ''
  languageCode = 'en-us'
  title = 'My New Hugo Site'
  theme = 'theme-name'
  ```

### Adding Tailwind

- Initialise `package.json` by running the following command. This will generate the file with all the default values.

  ```bash
  npm init -y
  ```

- Install Tailwind CSS as a dev dependency.

  ```bash
  npm i tailwindcss --save-dev
  ```

- Make a tailwind config file by running the command.

  ```bash
  npx tailwindcss init
  ```

  And the following code:

  ```js
  module.exports = {
    content: ["./layouts/**/*.html"],
    theme: {
      extend: {},
    },
    plugins: [],
  };
  ```

- Create a new `assets` directory in your theme folder, create a file called `main.css` and add the code below.

  ```css
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
  ```

- Link your css by adding the following lines in `/layouts/index.html`

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      {{ $styles := resources.Get "css/style.css" }}
      <link rel="stylesheet" href="{{ $styles.RelPermalink }}" />
    </head>
    <body>
      <h1 class="text-3xl font-bold underline text-red-600">Hello world!</h1>
    </body>
  </html>
  ```

- Add a new script in the `package.json` file

  ```json
  "watch": "npx tailwindcss -i ./assets/main.css -o ./static/css/style.css --watch"
  ```

- Test out your new Hugo site by running the following commands

  ```bash
  # Run Hugo
  hugo server -D

  # Watch any file changes (run in a new terminal window)
  npm run watch
  ```

And that's it, you can open `http://localhost:1313` in your browser to view the changes.
