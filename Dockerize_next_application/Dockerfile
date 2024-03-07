# stage 0 install dependencies 

FROM node:20.11.1-bullseye@sha256:c92af5751f3d9c79622658593269d7f6de0a871f252f7bac51ba2c7b666a056e AS DEPENDENCIES

ENV NODE_ENV=production

WORKDIR /site

COPY ./fragement-ui/package-lock.json ./fragement-ui/package.json ./fragement-ui/

WORKDIR /site/fragement-ui

RUN npm install

# Stage 1 building the site

FROM node:20.11.1-bullseye@sha256:c92af5751f3d9c79622658593269d7f6de0a871f252f7bac51ba2c7b666a056e AS BUILD

WORKDIR /site

# Copy the dependencies from the stage 0
COPY --from=DEPENDENCIES /site /site

# Copy the rest of the application code
COPY . .

# go inside the dir
WORKDIR /site/fragement-ui

# Build the application
RUN npm run build

FROM nginx:1.25.4-alpine@sha256:6a2f8b28e45c4adea04ec207a251fd4a2df03ddc930f782af51e315ebc76e9a9

COPY --from=BUILD /site/fragement-ui/out /usr/share/nginx/html

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
            CMD curl --fail localhost || exit 1
            