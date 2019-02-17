# GraphQL using AWS App Sync

This tutorial explains how to take advantage of the GraphQL features of AWS App Sync. 

## Pending

* Specify the names and folders of the files included in git

## DynamoDB Setup

Create two tables in AWS DynamoDB:

* Movies - Primary key: 'id'  (number)
* Reviews - Primary key: 'id' (number)

## Lambda Functions

Import in Lambda the following six functions (Node.js 8.x):

* MovieList - Used to list all the entries in the table Movies
* MovieAdd - Used to add a new entry in the table Movies
* MovieGet - Used to retrieve a single element in the table Movies
* ReviewList - Used to list all the entries in the table Reviews
* ReviewAdd - Used to add a new entry in the table Reviews
* ReviewGet - Used to retrieve a single element in the table Reviews

During the creation of the first lambda, you will need to create a new custom role. Edit that role in the IAM console and attach to it the policy AmazonDynamoDBFullAccess.

## API Gateway

Create in AWS API Gateway the Movies API with the following resources and methods:

* /
  * /movies
    * GET - Associate it with MovieList lambda 
    * POST - Associate it with MovieAdd lambda
    * /{id}
      * GET - Associate it with MovieGet lambda

Be careful to always select "Use Lambda Proxy integration".
Create as well the Reviews API with this list of resources and methods:

* /
  * /reviews
    * GET - Associate it with ReviewList lambda. Add a URL query string parameter named 'idMovie'
    * POST - Associate it with ReviewAdd lambda
    * /{id}
      * GET - Associate it with ReviewGet lambda

Deploy both APIs in a new stage named 'beta'.

## Populate data

Use the Postman collections to interact with the APIs and create an initial dataset. You will need to create a new Postman environment with these variables:

* movies
* reviews

Use the different requests in the collection to verify that all the endpoints are working properly.

## App Sync

Create a new AWS App Sync API named Movies. Use the file schema.graphql to create the Schema.

Create two datasources of type HTTP:

* movies - Linking to the URL in API Gateway of the Movies API
* reviews - Linking to the URL in API Gateway of the Reviews API

Define resolvers for the following types:

* Movie.reviews - Data Source: reviews. Configure the request mapping template using the provided file
* Mutation.newMovie - Data Source: movies.Configure the request mapping template using the provided file 
* Query.getMovie - Data Source: movies. Configure the request mapping template using the provided file 
* Query.getMovies - Data Source: movies. Configure the request mapping template using the provided file
* Query.getReviews - Data Source: reviews. Configure the request mapping template using the provided file


## Test GraphQL API

Use the file queries.graphql to verify the GraphQL using the Queries tab provided by AWS App Sync

Alternatively you can use the GraphQL Playground of Prisma ( <https://github.com/prisma/graphql-playground> ) to test the API. You will need to get the API Key from the Settings tab and pass it as a x-api-key header.
Mention to graphql playground prisma. Keys as headers








