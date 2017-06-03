---
layout: post
title: Docker Node
---

# Table of Contents
  * [Chapter 1 - Workflows](#chapter-1)
  * [Chapter 2 - Yarn Commands](#chapter-2)
  * [Chapter 3 - Docker Commands](#chapter-3)
  * [Chapter 4 - Open Source Development](#chapter-4)

## Chapter 1 - Workflows<a id="chapter-1"></a>

### Create Note App

* Reference:
    * [1] Docker + node guide
        * https://medium.com/@tribou/react-and-flux-a-docker-development-workflow-469957f3bbf0
    * [2] Webpack + yarn guide https://scotch.io/tutorials/setup-a-react-environment-using-webpack-and-babel
    * [3] Docker Machine to manage virtual machines

* Download and Install Docker CE https://store.docker.com/editions/community/docker-ce-desktop-mac?tab=description
* Run Docker `man open; open -a /Applications/Docker.app`
* Install Node Version Manager (NVM). Switch to latest NPM version
    ```
    nvm list; nvm install 7.9.0; nvm use v7.9.0
    ```
* Download and Install Yarn `brew install yarn`
* Create new Yarn project `yarn init`
* Add Webpack 2 dependencies `yarn add webpack webpack-dev-server path`
* Create Webpack config file `touch webpack.config.js` with contents defined in [2]
* Setup and add Babel dependencies `yarn add babel-loader babel-core babel-preset-es2015 babel-preset-react --dev`
* Configure Babel `touch .babelrc`. Add contents defined in [2]
* Help `docker --help`
* Create Dockerfile #1 `touch Dockerfile` (using port 8080)
    * Add contents defined in [1]
* Create Dockerfile #2 `touch Dockerfile-dev` (to skip reinstalling dependencies subsequent builds)
    * Add contents defined in [1]
* Create .dockerignore file. Add these contents:
    ```
    .git
    .gitignore
    .editorconfig
    node_modules
    *.log
    *.md
    ```
* Create .gitignore
    ```
    .DS_Store
    ```
* Add Node Image to `FROM` field https://hub.docker.com/_/node/
* Create "start" script `mkdir client; touch client/index.js`.
    * Add contents below, or React App component as shown in [2]:
        ```
        function MyClass(myArg) {
            this.name = myArg;
        }
        module.exports = MyClass;

        var myClass = new MyClass("luke");
        console.log(myClass.name);
        ```
* Create HTML file `touch client/index.html`. Add contents defined in [2]
* Install `html-webpack-plugin` to configure script insertions into index.html (instead of manually adding script tag to index.html)
    ```
    yarn add html-webpack-plugin
    ```
* Append "start" Script in package.json to immediately invoke
    ```
    cat package.json
    npm install -g json
    json --in-place -f package.json -e 'this.scripts={"start": "webpack-dev-server"}'
    cat package.json
    ```
    * Note `"start": "webpack-dev-server"` instead of `"start": "node client/index.js"}'`
* Add React Components `yarn add react react-dom`
* Create React Components: `mkdir client/components && touch client/components/App.js`
* Modify contents of App.js as shown in [2]

* Use Docker Machine (uses Boot2Docker ISO) to manage local machines using virtualbox driver https://docs.docker.com/machine/get-started/#use-machine-to-run-docker-containers
    * Show available local machines `docker-machine ls`
    * Check Docker Machine `docker-machine create --driver virtualbox node-app-machine`
    * Show running machines `docker-machine ls`.
    * Start a machine `docker-machine start node-app-machine`
        * Note: Running just `docker-machine ip` without specific machine name checks the ip of the `default` local machine
    * Check IP Address of a running Docker Machine `docker-machine ip node-app-machine`: 192.168.99.100
    * Show Env of the Docker Machine `docker-machine env node-app-machine`
        ```
        export DOCKER_TLS_VERIFY="1"
        export DOCKER_HOST="tcp://192.168.99.100:2376"
        export DOCKER_CERT_PATH="/Users/Ls/.docker/machine/machines/node-app-machine"
        export DOCKER_MACHINE_NAME="node-app-machine"
        # Run this command to configure your shell:
        # eval $(docker-machine env node-app-machine)
        ```
    * Note: Docker Machine's `docker` binary connects to Docker daemon on locally
    running VM where it builds Docker Image.
    * SSH into specific Docker Machine
        ```
        docker-machine ssh node-app-machine
        ifconfig
        netstat -tpla
        ```

* Start MongoDB database Docker Container to run in background:
    ```
    # Start MongoDB db, check running, stop, list all previously stopped containers
    docker run -d --name db mongo
    docker ps
    docker stop db
    docker ps -a

    # Restart Docker db container
    docker start 9030fcd11c07
    ```
* Export Docker Machine IP obtained above `export DB_HOST=192.168.99.100`

* Build Docker Image #1 `docker build -t ltfschoen/node-app:v1 .`
    * https://github.com/strongloop/fsevents/issues/165
* Re-Build (faster) using Docker Image #2 (skips reinstalling dependencies if no changes)
    `docker build -t ltfschoen/node-app:v1 -f Dockerfile-dev .`
* Run Docker Image (linking db container, exposing port, and using image to make container)
    ```
    docker run -it --rm --name node-app-web --link db:db -e DB_HOST=db -p 8080:8080 ltfschoen/node-app:v1`

    docker run -it --rm --name node-app-web --link db:db -e DB_HOST=db -p 192.168.99.100:8080:8080 -d ltfschoen/node-app:v1`
      30b4d16c2616f26bb7c6a7ed85a0b3207c79f92d8023e50b3fb9a8d31a50f748

    docker logs 30b4d16c2616f26bb7c6a7ed85a0b3207c79f92d8023e50b3fb9a8d31a50f748

    docker ps -a

    docker inspect 30b4d16c2616f26bb7c6a7ed85a0b3207c79f92d8023e50b3fb9a8d31a50f748

    curl --verbose http://192.168.99.100:8080
    ```
    * Optionally with Bash prompt `... /bin/bash`
    * Stop container
        `docker
* ALTERNATIVE (instead of `docker run ...`) use Docker Composer to store command line args in YAML file
    * Create `touch docker-compose.yml`
    * Add contents:
        ```
        web:
          build: .
          links:
            - db
          environment:
            - DB_HOST=db
          ports:
            - '8080:8080'
          command: npm start
        db:
          image: mongo
        ```
    * Run with `docker-compose up -d` (detached mode)
    * Show running containers `docker-compose ps`
        ```
               Name                     Command             State           Ports
        ----------------------------------------------------------------------------------
        spaceapps2017_db_1    docker-entrypoint.sh mongod   Up      27017/tcp
        spaceapps2017_web_1   npm start                     Up      0.0.0.0:8080->8080/tcp
        ```
* Open in browser at Docker Machine's IP `http://192.168.99.100:8080`
    * `open "http://$(docker-machine ip node-app-machine):8080"`

* Show Docker Images `docker images -a`

* Show and Remove dangling images (those without a **tag**)
    ```
    docker images -f dangling=true
    docker rmi $(docker images -f dangling=true -q)
    ```
* Remove specific image `docker rmi [image_id]`

## Chapter 2 - Yarn<a id="chapter-2"></a>

### Setup and Commands

* Yarn
	* Install Yarn
	```
	brew update
	brew install yarn
	```

	* Add Yarn to PATH
	```
	export PATH="$PATH:`yarn global bin`"
	```

	* Check Yarn installed
	```
	yarn --version
	```

	* Use Yarn https://yarnpkg.com/en/docs/usage
		* Add create-react-app command globally
			```
			yarn global add create-react-app --prefix /usr/local
			which create-react-app
			create-react-app
			```
			* Create React app (without build configuration) https://github.com/facebookincubator/create-react-app
		* Create New Project (package.json)
			```
			yarn init
			```
		* Show current and latest dependencies
			```
			yarn outdated [package]
			```
		* Add, Upgrade, or Remove Dependency https://yarnpkg.com/en/docs/managing-dependencies
			```
			yarn add|upgrade|remove package@[version|tag]
			```
		* Show dependency info
			```
			yarn info [package]
			```
		* Install dependencies
			```
			yarn OR yarn install
			```
		* Distribution tags to label published versions (instead of using version numbers)
		but they should not appear like version numbers
			```
			yarn tag ls
			yarn add [package]@[version] [tag] (i.e. latest|stable|beta|canary|dev)
			```
		* Run test script
			```
			yarn test OR yarn run test
			```
		* Temporarily link/unlink packages between different projects using symlinks
			```
			cd project1
			yarn link
			cd ../project2
			yarn link project1
			yarn unlink project1
			```
		* Advanced
			* Yarn CLI https://yarnpkg.com/en/docs/cli/
				* Show and modify default Yarn config
					```
					yarn config list
					yarn config set [key] [value] --global
					yarn config delete [key]
					```
				* Run single instance on same server (avoid conflicts)
					```
					yarn --mutex network:31997|30330
					```
				* Cache packages locally
					```
					yarn cache ls|dir
					yarn cache clean
					```
				* Check versions in package.json match yarn.lock
					```
					yarn check --integrity
					```
				* Run specific scripts in package.json "scripts" or executable in node_modules/.bin/
					```
					yarn run -- [args]
					```
				* Publish to NPM https://yarnpkg.com/en/docs/cli/publish
					```
					yarn login|logout
					yarn publish
					```


## Chapter 3 - Docker<a id="chapter-3"></a>

### Commands

* Check installed `docker --version`
* Run Docker `man open; open -a /Applications/Docker.app`
* Help `docker --help`
* Create Docker Image `docker build ...`
* Show Docker Images created `docker images`
* Remove pattern of Docker Images
```
docker ps -a |  grep "pattern"
docker images | grep "pattern" | awk '{print $1}' | xargs docker rm
```
* Remove Docker Container upon exit `docker run --rm image_name`


    docker ps
    docker info
    docker version



## Chapter 4 - Open Source Development<a id="chapter-4"></a>

### Methodology

	* Twelve-Factor apps https://12factor.net/
        * Backing Services https://12factor.net/backing-services

