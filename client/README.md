### Docker Steps Client- Frontend 13-03-2019
Clone the repository.

$ git clone https://github.com/imvishz/d3-PEVN.git

$ cd d3-PEVN/server/

$ docker build -t frontend .

$ docker image ls

$ docker run -p 8080:8080 -d frontend


### Install NodeJS in your system if you face any error regarding NPM commands.

$ curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -

$ sudo yum install nodejs
