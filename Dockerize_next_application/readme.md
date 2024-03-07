# Dockerize a next frontend is slightly different from dockerize a server application here

## 1 Start from writting a .dockerignore, but what to ignore?

### generated files or dir like .next, out, jest report

### development realted files like .eslintrc prettier, and more

### the black hole => node_modules

!!!!!!!!!!!!!! if you don't ignore node_modules, there will be strange errors during docker build.
!!!!!!!!!!!!!! Something like docker internal error, and you could not even find the detailed error from the docker desktop.

### the md files like readme.md

### But make sure you include the license.md

Refer to the dockerignore files in this repo

## 2. Start writing the docker file.

It is better to separate the whole process into 3 stages. (3 stages it the way I learned, and of course it is not the only way)

### Dependencies

setting up the env variable.

Install dependencies and make them ready for the next step.

### Build

Copy the everything from the previous stage. (so the node modules will be bringing over)

copy the source code from your local into the container.

then run the build command.

### Host

If you are still doing the npm start at the end or yarn start, you are a noob then. Because you should use apache or nginx server to serve the site as if you are working in the production environment with a legit team.

first of all, next.js, you will need to set this up in your local project repo.


```js

// https://nextjs.org/docs/pages/building-your-application/deploying/static-exports
/*
* @type {import('next').NextConfig} 
*/
const nextConfig = {
    output:'export',

}

module.exports = nextConfig


```

So when you run build, the out dir will be created

Then from the build stage, you use the nginx layer with alpine( because this is the smallest linux distribution )

COPY the out dir onto the /usr/share/nginx/html

expose the port

Then have a little health check for container management system.

### Test it on local, if everything good, the dockerize next application is done.