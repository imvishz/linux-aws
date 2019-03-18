# d3-PEVN
A Project to visualize data as a Barchart using D3, Vue, Express, Node and PostgreSQL.

# Project Structure 

client - This Folder Consists of the Frontend of the Project 

server - This Folder Consists of the Backend of the Project

data - This Folder Consists of the DB dump

# Packages Used in Frontend

Axios - Promise based HTTP client for the browser and node.js
        'npm install --save axios' 

D3 - D3 (Data-Driven Documents or D3.js) is a JavaScript library for visualizing data using web standards
     'npm install --save d3' 

# Packages Used in Backend

Express - 'npm install express'

Body Parser - 'npm install body-parser'

Knex - 'npm install knex --save'
       'npm install pg'

Cors - 'npm install cors' 

# Backend Setup

Clone the project 

Setup the PostgreSQL and import the DB file db_dump into your database

Modify the configuration of database in server/server.js

Run 'npm install' in the server folder

Run 'npm start' 

Backend will run on `http://localhost:3000`

# Frontend Setup

Run 'npm install' in the client folder

Run 'npm run dev' 

Frontend will run on `http://localhost:8080`

# Functionality

The Barchart will display the data as month-wise by default

To display the data as Weekly select the sort by drop down as weekly,
which will show the select month drop down choose the month to display the data for that particular month

