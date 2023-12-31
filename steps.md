# NestJS with React

## Initial setup

### 001: Building the App infra

```
nvm use 18
npm install -D turbo
```

We will need the nest CLI:

```
npm install -g @nestjs/cli
```

### 002: Adding workspaces and creating a NEST API

Then we edit the package.json to add workspaces, and create a NestJS project:

```
cd apps ; nest new api
```

We do this within the apps folder which is part of the workspaces setup.
When asked I chose "npm" and it created the NEST app skeleton.
The skeleton comes with some "git" stuff I deleted/moved to the parent repo.

### 003: Creating a React client with Vite

```
# still @ apps folder
npm create vite@latest client
```

I choose "React" & "Typescript", and get a project's scaffolding for the FE.
We now need to do the "npm install", but because we use workspaces... We should do it on the base directory!
	(workspaces will look subdirs to get inner package.json files)

```
# root folder (with Turbo installed node_modules)
cd .. ; npm install
```

### 004: Making all work together with Turbo

We create a turbo config file (turbo.json) and change package.json scripts to match dev.
Turbo will run all at the same time for us.
And now we can run:

```
npm run dev
```

Both running at the same time: [Vite:5173](http://localhost:5173/) & [Nest:3000](http://localhost:3000/)

What we have now is **WebPack** waiting for changes in the *Nest App* & **Vite** waiting for changes in the *React App*.
This is for the development application, but for production we want to have a different setup, with all running together.
But for the development setup we need to "proxy" the requests of the ReactApp to NestApp.

### 005: Setting up Vite to proxy requests from React to Nest

We add a couple of configs:

- `apps/client/vite.config.ts`: We tell Vite to proxy requests to the Nest server.
- `apps/api/src/main.ts`: We make the default path `/api`.

After this the Nest server URL changes to [/api](http://localhost:3000/api/).

### 006: Making React request data from Nest

We modify `apps/client/src/App.tsx` and now what we get in React is the Nest data: [localhost:5173](http://localhost:5173/).
This proxy only will happen in your dev setup, for production there will be no proxying.

### 007: Creating the production setup

We already have the `build` for both Nest & React. So we create the *Turbo* config for build and we can now do:

```
npm run build
```

Plus some organization with the gitignore.

### 008: Having a single server in production

We now need to change the application into a "single page application" and for that we'll use [Nest Serve Static](https://docs.nestjs.com/recipes/serve-static) module:

```
npm install --workspace api @nestjs/serve-static
```

Change the Nest App to use `serve-static` by `modifying apps/api/src/app.module.ts` and do some additional tweaking so we can run again:

```
npm run build
npm run start
```

So now our app is running entirely together at [localhost:3000](http://localhost:3000/).
Both React and NEST running within a single server which is powered by NEST.

### 009: Making production port configurable

This is important to be able to change it for production.


## Summary

We now have the application running in DEV mode "decoupled":

```
npm run dev
```

And for production mode we "bind" it together so we have a single server:

```
npm run build
npm run start
```

Original work from [Marius Espejo YTvideo: Can we combine a NestJS app with React?](https://www.youtube.com/watch?v=nY0R7pslbCI).

Besides NestJS & React, this projects uses the following technologies:
- [workspaces](https://docs.npmjs.com/cli/v7/using-npm/workspaces): Helps with the mono-repo setup.
- [Turbo](https://github.com/vercel/turbo): Toolchain for frontend development. By Vercel & written in Rust.
- [Vite](https://vitejs.dev/guide/): To scaffold the React App.
- [Nest Serve Static](https://docs.nestjs.com/recipes/serve-static): To have a single server in production.
