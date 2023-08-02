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

