# ng Angular Dockerize App

Application developed with Stack of Node.JS, Angular.

## Setup Docker

- Step 1: Create the file .dockerignore and add below code

```dockerignore
**/.dockerignore
**/.env
**/.git
**/.gitignore
**/.vscode
**/Dockerfile*
**/node_modules
**/npm-debug.log
README.md
```

- Step 2: Create the file Dockerfile.Dev and add below code

```dockerfile
#  Create a new image from the base nodejs 12.7 image.
FROM node:12.14-alpine
# Create the target directory in the image
RUN mkdir -p /usr/src/app
# Set the created directory as the working directory
WORKDIR /usr/src/app
# Copy the package.json inside the working directory
COPY ./package.json /usr/src/app
# Install required dependencies
RUN  npm install;

# Copy the client application source files. You can use .dockerignore to exlcude files. Works just as .gitignore does.
COPY ./ /usr/src/app
# Open port 3000. This is the port that our development server uses
EXPOSE 4200
# Start the application. This is the same as running ng serve.
CMD ["npm", "start"]
```

- Step 3: Configure Host on package.json

We need to modify your package.json to set the Host for ng serve command as below,

```json
{
    ...
    Scripts: {
        "start": "ng serve --host 0.0.0.0"
        ...
    }
}
```

- Step 3: Create Docker Image & Run Container

```powershell
docker build -t ngangulardockerizeapp:dev -f Dockerfile.Dev .
docker run -it --rm -p 9001:4200 -v ${PWD}/src:/usr/app/src --name ngangulardockerizeapp ngangulardockerizeapp:dev

```
