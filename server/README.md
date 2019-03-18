### Docker Steps Client- Backend 13-03-2019
Clone the repository.

$ git clone https://github.com/imvishz/d3-PEVN.git

$ cd d3-PEVN/server/

$ docker build -t backend .

$ docker image ls

$ docker run -p 3000:3000 -d backend


### Install NodeJS in your system if you face any error regarding NPM commands.

$ curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -

$ sudo yum install nodejs
