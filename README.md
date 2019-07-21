# docker_intro for Digbio lab 
by Chunhui Xu 

contact: cx9p9@mail.missouri.edu

docker intro for digbio
Before you start to build Docker:
Make sure your website works well on your local properly.


Register a user at Docker Hub:
https://hub.docker.com/

Download Docker:
https://hub.docker.com/editions/community/docker-ce-desktop-windows

Start Docker service if needed:
```
systemctl start docker
```

After your installation completely, you may open a CMD window, and type:
```
docker --version
```


Under your web project folder, you need to have a `package.json` file, which describe all dependencies of your project,
in general, this file should be generated automatically when you build your web project.

Example of `package.json` file:

```
{
  "name": "fatplant",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "nodemon app.js --exec babel-node --presets es2015,stage-2",
    "style": "./node_modules/.bin/gulp"
  },
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "cookie-parser": "~1.4.3",
    "cytoscape": "^3.7.0",
    "cytoscape-avsdf": "^1.0.0",
    "cytoscape-cola": "^2.3.0",
    "debug": "~2.6.9",
    "dotenv": "^7.0.0",
    "ejs": "~2.5.7",
    "express": "^4.16.4",
    "express-fileupload": "^1.1.4",
    "http-errors": "~1.6.2",
    "mongodb": "^3.2.6",
    "mongoose": "^5.4.18",
    "morgan": "~1.9.0",
    "multer": "^1.4.1",
    "node-sass-middleware": "^0.11.0",
    "serve-index": "^1.9.1",
    "tar": "^4.4.8"
  },
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-stage-2": "^6.24.1",
    "csvtojson": "^2.0.8",
    "gulp": "^4.0.0",
    "nodemon": "^1.18.10",
    "rimraf": "^2.6.3"
  }
} 
```

After you have the `package.json` file, create a `package-lock.json` by:
``
npm install
``
it will generate the `package-lock.json` and Docker into the image later.



Create a new Dockerfile,
Dockerfile is considered as a script to run the command **line by line** :
```
FROM node:11-alpine                     #This line MUST to be the first line in your Docker file to indicate where to download all depedencies.
ADD . /fatplant                         #Create the work directory for your projecct
WORKDIR /fatplant                       #change work folder
COPY package*.json ./                   #copy package.json and package-lock.josn firstly to your work folder.
RUN npm install                         #install depedencies 
RUN npm install pm2 -g
RUN npm install babel-cli -g
RUN npm rebuild node-sass
EXPOSE 3000                             #expose the port for your site (ask Juexin or change it depends on your web project

CMD ["pm2-runtime", "--interpreter", "babel-node", "/fatplant/app.js"]  #run the app.js, it' same as command: ~/fatplant/pm2-runtime --interpreter
                                                                                                              ~/fatplant/babel-node /fatplant/app.js
```
