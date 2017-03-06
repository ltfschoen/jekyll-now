### Docker Image

* `.dockerignore`: Define files and directories not to upload to Docker host when build image (i.e. `.git`) 

### Create Docker Image buy initiating Dockerfile build using Sample app

* Important: Check Docker Server is running, and Docker Daemon (Client) ready to communicate before build Docker Image

* Initiate building and tagging Docker Image based on Dockerfile using files in current directory
    - Downloads base image defined by `FROM`
    - Important: Use cache busting by appending `RUN` with `--no-cache` argument when build time is slow

* Download example app
{% highlight shell %}
git clone https://github.com/spkane/docker-node-hello.git
{% endhighlight %}


* Create minimal parent Docker Container for Ruby apps to inherit from
[Link](https://blog.codeship.com/build-minimal-docker-container-ruby-apps/)


* [Suggestion (try Phusion Passenger)](http://pacuna.io/2015/05/31/rails-environment-with-docker-and-vagrant/)
* [Suggestions (using Docker-Compose instead of Vagrant)](https://blog.abevoelker.com/why-i-dont-use-docker-much-anymore/)
* [Setup (using Docker-Compose instead of Vagrant)](https://blog.codeship.com/building-microservices-docker-rails-api-gem/)

{% highlight shell %}
mkdir docker_api && cd docker_api
touch Dockerfile docker-compose.yml .dockerignore Gemfile Gemfile.lock
{% endhighlight %}

* Configure Dockerfile, docker-compose.yml, and Gemfile

* Build image from Dockerfile and install Gems (also use to Rebuild)
{% highlight shell %}
docker-compose build
{% endhighlight %}

{% highlight shell %}
db uses an image, skipping
Building web
Step 1 : FROM ruby:2.3.0
2.3.0: Pulling from library/ruby
efd26ecc9548: Pull complete
...
Digest: sha256:1759dbfc94b572b340f5e415734029a1e1cef460d5343fe38c757b0df7ef64ee
Status: Downloaded newer image for ruby:2.3.0
 ---> 7ca70eb2dfea
Step 2 : RUN apt-get update -qq && apt-get install -y build-essential libmysqlclient-dev
 ---> Running in e9fbe7cda7ab
Reading package lists...
Building dependency tree...
Reading state information...
The following extra packages will be installed:
  dpkg-dev fakeroot libalgorithm-diff-perl libalgorithm-diff-xs-perl
  libalgorithm-merge-perl libdpkg-perl libfakeroot libfile-fcntllock-perl
  libmysqlclient18 libtimedate-perl mysql-common
Suggested packages:
  debian-keyring
The following NEW packages will be installed:
  build-essential dpkg-dev fakeroot libalgorithm-diff-perl
  libalgorithm-diff-xs-perl libalgorithm-merge-perl libdpkg-perl libfakeroot
  libfile-fcntllock-perl libtimedate-perl
The following packages will be upgraded:
  libmysqlclient-dev libmysqlclient18 mysql-common
3 upgraded, 10 newly installed, 0 to remove and 87 not upgraded.
Need to get 4622 kB of archives.
After this operation, 4951 kB of additional disk space will be used.
Get:1 http://security.debian.org/ jessie/updates/main libmysqlclient-dev amd64 5.5.52-0+deb8u1 [952 kB]
Get:2 http://httpredir.debian.org/debian/ jessie/main libtimedate-perl all 2.3000-2 [42.2 kB]
Get:3 http://security.debian.org/ jessie/updates/main mysql-common all 5.5.52-0+deb8u1 [80.1 kB]
Get:4 http://security.debian.org/ jessie/updates/main libmysqlclient18 amd64 5.5.52-0+deb8u1 [674 kB]
Get:5 http://httpredir.debian.org/debian/ jessie/main libdpkg-perl all 1.17.27 [1075 kB]
Get:6 http://httpredir.debian.org/debian/ jessie/main dpkg-dev all 1.17.27 [1548 kB]
Get:7 http://httpredir.debian.org/debian/ jessie/main build-essential amd64 11.7 [7114 B]
Get:8 http://httpredir.debian.org/debian/ jessie/main libfakeroot amd64 1.20.2-1 [44.7 kB]
Get:9 http://httpredir.debian.org/debian/ jessie/main fakeroot amd64 1.20.2-1 [84.7 kB]
Get:10 http://httpredir.debian.org/debian/ jessie/main libalgorithm-diff-perl all 1.19.02-3 [51.7 kB]
Get:11 http://httpredir.debian.org/debian/ jessie/main libalgorithm-diff-xs-perl amd64 0.04-3+b1 [12.2 kB]
Get:12 http://httpredir.debian.org/debian/ jessie/main libalgorithm-merge-perl all 0.08-2 [13.5 kB]
Get:13 http://httpredir.debian.org/debian/ jessie/main libfile-fcntllock-perl amd64 0.22-1+b1 [36.4 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 4622 kB in 17s (258 kB/s)
(Reading database ... 21075 files and directories currently installed.)
Preparing to unpack .../libmysqlclient-dev_5.5.52-0+deb8u1_amd64.deb ...
Unpacking libmysqlclient-dev (5.5.52-0+deb8u1) over (5.5.47-0+deb8u1) ...
...
Setting up mysql-common (5.5.52-0+deb8u1) ...
...
Processing triggers for libc-bin (2.19-18+deb8u4) ...
 ---> f0b579b19d06
Removing intermediate container e9fbe7cda7ab
Step 3 : RUN mkdir /docker_api
 ---> Running in d08b36a8f53b
 ---> 7d5c77f2921f
Removing intermediate container d08b36a8f53b
Step 4 : WORKDIR /docker_api
 ---> Running in b6a7ad88304d
 ---> f8b80d283b8d
Removing intermediate container b6a7ad88304d
Step 5 : ADD Gemfile /docker_api/Gemfile
 ---> 9da2e4d785a7
Removing intermediate container 93486c2ce704
Step 6 : ADD Gemfile.lock /docker_api/Gemfile.lock
 ---> 2dd29b8a141b
Removing intermediate container b810d12335cc
Step 7 : RUN bundle install
 ---> Running in 841fbdcd49eb
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Resolving dependencies....
Installing ...
Bundle complete! 6 Gemfile dependencies, 42 gems now installed.
Bundled gems are installed into /usr/local/bundle.
 ---> 4cbe4c68aa4a
Removing intermediate container 841fbdcd49eb
Step 8 : ADD . /docker_api
 ---> 41e7b704317f
Removing intermediate container 2023cf4039d7
Successfully built 41e7b704317f
{% endhighlight %}

* List Docker images

{% highlight shell %}
docker images

REPOSITORY      TAG      IMAGE ID       CREATED         SIZE
dockerapi_web   latest   41e7b704317f   8 minutes ago   815.3 MB
ruby            2.3.0    7ca70eb2dfea   5 months ago    725.5 MB
{% endhighlight %}

* Run commands against "web" container of Docker Image (defined in docker-compose.yml) to create Rails API app structure 

{% highlight shell %}
docker-compose run web rails-api new .
{% endhighlight %}

* Port conflict encountered with local MySQL
{% highlight conf %}
ERROR: for db  Cannot start service db: driver failed programming external connectivity
on endpoint dockerapi_db_1 (d2cbca006c1d4f9f14783b21ced37af4c33c2dfc2bda6d8a595b2d467f6ee1e6): 
Error starting userland proxy: Bind for 0.0.0.0:3306 failed: port is already allocated
ERROR: Encountered errors while bringing up the project.
{% endhighlight %}

* Delete all Docker Containers (the `-a` flag lists all containers and `-q` flag only returns Docker Container IDs)
{% highlight shell %}
docker rm $(docker ps -a -q)
{% endhighlight %}

* Alternative approach with force remove 
{% highlight shell %}
docker rm -f `sudo docker ps -a -q`
{% endhighlight %}

* Delete all Docker Images
{% highlight shell %}
docker rmi $(docker images -q)
{% endhighlight %}

* Create Dockerfile (define Docker Container housing app and its dependencies). Dockerfile places app code in an image for building a container housing Ruby, Bundler and all dependencies inside.

{% highlight conf lineno %}
FROM ruby:2.3.0
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /myapp
WORKDIR /myapp
ADD Gemfile /myapp/Gemfile
ADD Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
ADD . /myapp
{% endhighlight %}

* Create Gemfile (bootstrap) to initially load Rails and then overwritten by rails new.

{% highlight conf lineno %}
source 'https://rubygems.org'
gem 'rails', '4.2.4'
{% endhighlight %}

* Create empty Gemfile.lock to build Dockerfile.

{% highlight shell %}
touch Gemfile.lock
{% endhighlight %}

* Create docker-compose.yml describing services of app (database and web app), describes how to retrieve Docker image of each service (db runs on pre-made MySQL image, and web app is built from current directory), and describes configuration needed to link each service together and expose the web apps port.

{% highlight conf lineno %}
version: '2'
services:
  db:
    image: mysql
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
{% endhighlight %}
		    
* Build project to generate Rails skeleton app using `docker-compose run`. Compose will build the image for web service using Dockerfile, then run `rails new` inside new container, using that image to generate fresh app:

{% highlight shell %}
docker-compose run web rails new . --force --database=mysql --skip-bundle
{% endhighlight %}

{% highlight conf %}
Creating network "dockertest_default" with the default driver
Pulling db (mysql:latest)...
latest: Pulling from library/mysql
...: Pull complete
Digest: sha256:c8f03238ca1783d25af320877f063a36dbfce0daa56a7b4955e6c6e05ab5c70b
Status: Downloaded newer image for mysql:latest
Creating dockertest_db_1
Building web
Step 1 : FROM ruby:2.3.0
2.3.0: Pulling from library/ruby
...: Pull complete
Digest: sha256:1759dbfc94b572b340f5e415734029a1e1cef460d5343fe38c757b0df7ef64ee
Status: Downloaded newer image for ruby:2.3.0
 ---> 7ca70eb2dfea
Step 2 : RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
 ---> Running in 4427986d2a90
Reading package lists...
Building dependency tree...
Reading state information...
The following extra packages will be installed:
  dpkg-dev fakeroot libalgorithm-diff-perl libalgorithm-diff-xs-perl
  libalgorithm-merge-perl libc-ares2 libdpkg-perl libfakeroot
  libfile-fcntllock-perl libpq5 libtimedate-perl libv8-3.14.5
Suggested packages:
  debian-keyring postgresql-doc-9.4
The following NEW packages will be installed:
  build-essential dpkg-dev fakeroot libalgorithm-diff-perl
  libalgorithm-diff-xs-perl libalgorithm-merge-perl libc-ares2 libdpkg-perl
  libfakeroot libfile-fcntllock-perl libtimedate-perl libv8-3.14.5 nodejs
The following packages will be upgraded:
  libpq-dev libpq5
2 upgraded, 13 newly installed, 0 to remove and 61 not upgraded.
Need to get 5198 kB of archives.
After this operation, 12.2 MB of additional disk space will be used.
Get:1 http://security.debian.org/ jessie/updates/main libpq-dev amd64 9.4.9-0+deb8u1 [164 kB]
Get:2 http://security.debian.org/ jessie/updates/main libpq5 amd64 9.4.9-0+deb8u1 [124 kB]
Get:3 http://httpredir.debian.org/debian/ jessie/main libc-ares2 amd64 1.10.0-2 [76.7 kB]
Get:4 http://httpredir.debian.org/debian/ jessie/main libtimedate-perl all 2.3000-2 [42.2 kB]
Get:5 http://httpredir.debian.org/debian/ jessie/main libdpkg-perl all 1.17.27 [1075 kB]
Get:6 http://httpredir.debian.org/debian/ jessie/main dpkg-dev all 1.17.27 [1548 kB]
Get:7 http://httpredir.debian.org/debian/ jessie/main build-essential amd64 11.7 [7114 B]
Get:8 http://httpredir.debian.org/debian/ jessie/main libfakeroot amd64 1.20.2-1 [44.7 kB]
Get:9 http://httpredir.debian.org/debian/ jessie/main fakeroot amd64 1.20.2-1 [84.7 kB]
Get:10 http://httpredir.debian.org/debian/ jessie/main libalgorithm-diff-perl all 1.19.02-3 [51.7 kB]
Get:11 http://httpredir.debian.org/debian/ jessie/main libalgorithm-diff-xs-perl amd64 0.04-3+b1 [12.2 kB]
Get:12 http://httpredir.debian.org/debian/ jessie/main libalgorithm-merge-perl all 0.08-2 [13.5 kB]
Get:13 http://httpredir.debian.org/debian/ jessie/main libfile-fcntllock-perl amd64 0.22-1+b1 [36.4 kB]
Get:14 http://httpredir.debian.org/debian/ jessie/main libv8-3.14.5 amd64 3.14.5.8-8.1 [1269 kB]
Get:15 http://httpredir.debian.org/debian/ jessie/main nodejs amd64 0.10.29~dfsg-2 [648 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 5198 kB in 17s (295 kB/s)
(Reading database ... 21075 files and directories currently installed.)
Preparing to unpack .../libpq-dev_9.4.9-0+deb8u1_amd64.deb ...
Unpacking libpq-dev (9.4.9-0+deb8u1) over (9.4.6-0+deb8u1) ...
Preparing to unpack .../libpq5_9.4.9-0+deb8u1_amd64.deb ...
Unpacking libpq5:amd64 (9.4.9-0+deb8u1) over (9.4.6-0+deb8u1) ...
Selecting previously unselected package libc-ares2:amd64.
Preparing to unpack .../libc-ares2_1.10.0-2_amd64.deb ...
Unpacking libc-ares2:amd64 (1.10.0-2) ...
Selecting previously unselected package libtimedate-perl.
Preparing to unpack .../libtimedate-perl_2.3000-2_all.deb ...
Unpacking libtimedate-perl (2.3000-2) ...
Selecting previously unselected package libdpkg-perl.
Preparing to unpack .../libdpkg-perl_1.17.27_all.deb ...
Unpacking libdpkg-perl (1.17.27) ...
Selecting previously unselected package dpkg-dev.
Preparing to unpack .../dpkg-dev_1.17.27_all.deb ...
Unpacking dpkg-dev (1.17.27) ...
Selecting previously unselected package build-essential.
Preparing to unpack .../build-essential_11.7_amd64.deb ...
Unpacking build-essential (11.7) ...
Selecting previously unselected package libfakeroot:amd64.
Preparing to unpack .../libfakeroot_1.20.2-1_amd64.deb ...
Unpacking libfakeroot:amd64 (1.20.2-1) ...
Selecting previously unselected package fakeroot.
Preparing to unpack .../fakeroot_1.20.2-1_amd64.deb ...
Unpacking fakeroot (1.20.2-1) ...
Selecting previously unselected package libalgorithm-diff-perl.
Preparing to unpack .../libalgorithm-diff-perl_1.19.02-3_all.deb ...
Unpacking libalgorithm-diff-perl (1.19.02-3) ...
Selecting previously unselected package libalgorithm-diff-xs-perl.
Preparing to unpack .../libalgorithm-diff-xs-perl_0.04-3+b1_amd64.deb ...
Unpacking libalgorithm-diff-xs-perl (0.04-3+b1) ...
Selecting previously unselected package libalgorithm-merge-perl.
Preparing to unpack .../libalgorithm-merge-perl_0.08-2_all.deb ...
Unpacking libalgorithm-merge-perl (0.08-2) ...
Selecting previously unselected package libfile-fcntllock-perl.
Preparing to unpack .../libfile-fcntllock-perl_0.22-1+b1_amd64.deb ...
Unpacking libfile-fcntllock-perl (0.22-1+b1) ...
Selecting previously unselected package libv8-3.14.5.
Preparing to unpack .../libv8-3.14.5_3.14.5.8-8.1_amd64.deb ...
Unpacking libv8-3.14.5 (3.14.5.8-8.1) ...
Selecting previously unselected package nodejs.
Preparing to unpack .../nodejs_0.10.29~dfsg-2_amd64.deb ...
Unpacking nodejs (0.10.29~dfsg-2) ...
Setting up libpq5:amd64 (9.4.9-0+deb8u1) ...
Setting up libpq-dev (9.4.9-0+deb8u1) ...
Setting up libc-ares2:amd64 (1.10.0-2) ...
Setting up libtimedate-perl (2.3000-2) ...
Setting up libdpkg-perl (1.17.27) ...
Setting up dpkg-dev (1.17.27) ...
Setting up build-essential (11.7) ...
Setting up libfakeroot:amd64 (1.20.2-1) ...
Setting up fakeroot (1.20.2-1) ...
update-alternatives: using /usr/bin/fakeroot-sysv to provide /usr/bin/fakeroot (fakeroot) in auto mode
Setting up libalgorithm-diff-perl (1.19.02-3) ...
Setting up libalgorithm-diff-xs-perl (0.04-3+b1) ...
Setting up libalgorithm-merge-perl (0.08-2) ...
Setting up libfile-fcntllock-perl (0.22-1+b1) ...
Setting up libv8-3.14.5 (3.14.5.8-8.1) ...
Setting up nodejs (0.10.29~dfsg-2) ...
update-alternatives: using /usr/bin/nodejs to provide /usr/bin/js (js) in auto mode
Processing triggers for libc-bin (2.19-18+deb8u4) ...
 ---> 88a885514990
Removing intermediate container 4427986d2a90
Step 3 : RUN mkdir /myapp
 ---> Running in c972e78cfe6d
 ---> 12d4dfd73b23
Removing intermediate container c972e78cfe6d
Step 4 : WORKDIR /myapp
 ---> Running in db0d109a7127
 ---> ee3f0d824fa1
Removing intermediate container db0d109a7127
Step 5 : ADD Gemfile /myapp/Gemfile
 ---> 0c2da1162413
Removing intermediate container 55ff0454d072
Step 6 : ADD Gemfile.lock /myapp/Gemfile.lock
 ---> 728cfdc694b5
Removing intermediate container 943da5a3b68f
Step 7 : RUN bundle install
 ---> Running in 8f9865758468
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Resolving dependencies....
...
Installing rails 4.2.4
Bundle complete! 1 Gemfile dependency, 36 gems now installed.
Bundled gems are installed into /usr/local/bundle.
 ---> 62400e236692
Removing intermediate container 8f9865758468
Step 8 : ADD . /myapp
 ---> 74c029433bf6
Removing intermediate container 0931dcf187cb
Successfully built 74c029433bf6
WARNING: Image for service web was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
       exist  
      create  README.rdoc
{% endhighlight %}

* Uncomment `therubyracer` in Gemfile to have a Javascript runtime and add specific `mysql2` Gem version:

{% highlight conf %}
gem 'therubyracer', platforms: :ruby
gem 'mysql2', '0.3.20'
{% endhighlight %}

* Build image again with updated Gemfile, which changes to the Dockerfile itself

{% highlight shell %}
docker-compose build
{% endhighlight %}

{% highlight conf %}
db uses an image, skipping
Building web
Step 1 : FROM ruby:2.3.0
 ---> 7ca70eb2dfea
Step 2 : RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
 ---> Using cache
 ---> 88a885514990
Step 3 : RUN mkdir /myapp
 ---> Using cache
 ---> 12d4dfd73b23
Step 4 : WORKDIR /myapp
 ---> Using cache
 ---> ee3f0d824fa1
Step 5 : ADD Gemfile /myapp/Gemfile
 ---> 9e45149ecac7
Removing intermediate container 863c837882da
Step 6 : ADD Gemfile.lock /myapp/Gemfile.lock
 ---> 5b3c45843812
Removing intermediate container 1870cc9ab378
Step 7 : RUN bundle install
 ---> Running in 08d6e877b4bc
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Resolving dependencies...
...
Bundle complete! 13 Gemfile dependencies, 60 gems now installed.
Bundled gems are installed into /usr/local/bundle.
Post-install message from rdoc:
Depending on your version of ruby, you may need to install ruby rdoc/ri data:

<= 1.8.6 : unsupported
 = 1.8.7 : gem install rdoc-data; rdoc-data --install
 = 1.9.1 : gem install rdoc-data; rdoc-data --install
\>= 1.9.2 : nothing to do! Yay!
 ---> 6117c97bb2dd
Removing intermediate container 08d6e877b4bc
Step 8 : ADD . /myapp
 ---> 5573ce2c917b
Removing intermediate container 0b1e507a6dd7
Successfully built 5573ce2c917b
{% endhighlight %}

* Connect to MySQL DB. Rails expects a DB to be running on localhost now that the app is bootable, so point it at the DB Container instead. Change DB and Username to align with defaults set by the MySQL Image.Replace config/database.yml with:

{% highlight conf %}
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: mysql
  password:
  host: db

development: &default
  <<: *default
  database: mysql

test:
  <<: *default
  database: myapp_test
{% endhighlight %}
		  
* Install gems including mysql2
	
{% highlight shell %}
bundle install
{% endhighlight %}

* Boot up app

{% highlight shell %}
docker-compose up
{% endhighlight %}

{% highlight conf %}
Starting dockertest_db_1
Starting dockertest_web_1
Attaching to dockertest_db_1, dockertest_web_1
db_1   | error: database is uninitialized and password option is not specified 
db_1   |   You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
dockertest_db_1 exited with code 1
web_1  | [2016-08-30 09:49:12] INFO  WEBrick 1.3.1
web_1  | [2016-08-30 09:49:12] INFO  ruby 2.3.0 (2015-12-25) [x86_64-linux]
web_1  | [2016-08-30 09:49:12] INFO  WEBrick::HTTPServer#start: pid=1 port=3000
{% endhighlight %}

* Create DB in another terminal window.

{% highlight shell %}
docker-compose run web rake db:create
{% endhighlight %}

{% highlight conf %}
Starting dockertest_db_1
#<Mysql2::Error: Unknown MySQL server host 'db' (0)>
Couldn't create database for {"adapter"=>"mysql2", "encoding"=>"utf8", "pool"=>5, "username"=>"mysql", "password"=>nil, "host"=>"db", "database"=>"mysql"}, {:charset=>"utf8", :collation=>"utf8_unicode_ci"}
(If you set the charset manually, make sure you have a matching collation)
#<Mysql2::Error: Unknown MySQL server host 'db' (0)>
Couldn't create database for {"adapter"=>"mysql2", "encoding"=>"utf8", "pool"=>5, "username"=>"mysql", "password"=>nil, "host"=>"db", "database"=>"myapp_test"}, {:charset=>"utf8", :collation=>"utf8_unicode_ci"}
(If you set the charset manually, make sure you have a matching collation)
{% endhighlight %}




* Start Dockerised web server. If image not found locally, Docker will pull from Docker Hub.
In a web browser, go to http://localhost/.
    - Note: ports are exposed on the private IP addresses of the VM and forwarded to localhost

{% highlight shell %}
docker run -d -p 80:80 --name webserver nginx
{% endhighlight %}