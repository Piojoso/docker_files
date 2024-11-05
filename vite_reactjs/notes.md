1. In some cases (in my case, all of them) we need to add a config to the package.json of the project.

  ```JSON
  {
    "name": "project-example",
    "private": true,
    "version": "0.0.0",
    "type": "module",
    "scripts": {
      "dev": "vite --host 0.0.0.0",   // The --host 0.0.0.0 is the config that we need to add.
      "build": "vite build",
      "lint": "eslint .",
      "preview": "vite preview"
    },
    // ...
  }
  ```
