# Advanced Information Management - CRUD DB System with Rust and MongoDB

This is a Backend System with CRUD (Create, Read, Update, and Delete) APIs using the Rust programming language and MongoDB as the database. 
The Rust API will run on a Warp HTTP server and use the MongoDB Rust driver to persist data in a MongoDB database.


## Pre-requisites

1. Docker / Docker Compose
2. Rust / Cargo
3. MongoDB
4. Postman
5. Make

## Set-up Procedures

1. In the base directory, run `make install` to install cargo dependencies
2. run `make dev` to run docker-compose and build and start mongodb
3. run `cargo r -r` to start the application
4. Upload file Post It App on Postman and check each API

## APIs

1. Health Checker (GET) - Standard health check to determine if app has been set-up corrrectly or not

Sample Request:

```
curl --location 'http://localhost:8000/api/healthchecker'
```

Sample Response:

``` json
{
"status": "success",
"message": "Build CRUD API with Rust and MongoDB"
}
```

2. Create Post It Note (POST) - Creates a post it note and returns a UUID in the response.
Title is used as 'Key', hence we cannot create a new note with 

Sample Request:

```
curl --location 'http://localhost:8000/api/notes/' \
--header 'Content-Type: application/json' \
--data '{
"title": "My first post it note",
"content": "I want to pass my AIM subject!!",
"category": "school-note"
}'
```

Sample Response:

``` json 
{
    "status": "success",
    "data": {
        "note": {
            "id": "67eba2e19b9d99ac29b6ad3e",
            "title": "My first post it note",
            "content": "I want to pass my AIM subject!!",
            "category": "school-note",
            "published": false,
            "createdAt": "2025-04-01T08:25:05.836Z",
            "updatedAt": "2025-04-01T08:25:05.836Z"
        }
    }
}
```


Sample Response (Duplicate Title):

``` json 
{
    "status": "fail",
    "message": "Duplicate key error"
}
```

3. Get Post It Note (GET) - Retrieves the post it note of the UUID passed from the URI.
An error message is shown if the UUID does not exist or has been deleted.


Sample Request:

```
curl --location 'http://localhost:8000/api/notes/67eba2e19b9d99ac29b6ad3e'
```

Sample Response:

``` json 
{
    "status": "success",
    "data": {
        "note": {
            "id": "67eba2e19b9d99ac29b6ad3e",
            "title": "My first post it note",
            "content": "I want to pass my AIM subject!!",
            "category": "school-note",
            "published": false,
            "createdAt": "2025-04-01T08:25:05.836Z",
            "updatedAt": "2025-04-01T08:25:05.836Z"
        }
    }
}
```


Sample Response (UUID does not exist):

``` json 
{
    "status": "fail",
    "message": "<UUID>"
}
```



4. Update Post It Note (PATCH) - Updates the post it note of the UUID passed from the URI.
   We can edit the title, content, category or published field.
   An error message is shown if the UUID does not exist or has been deleted.


Sample Request:

```
curl --location --request PATCH 'http://localhost:8000/api/notes/67eba2e19b9d99ac29b6ad3e' \
--header 'Content-Type: application/json' \
--data '{
    "title": "Updating my first note! ✅✅👇👇",
    "published": true
}'
```

Sample Response:

``` json 
{
    "status": "success",
    "data": {
        "note": {
            "id": "67eba2e19b9d99ac29b6ad3e",
            "title": "Updating my first note! ✅✅👇👇",
            "content": "I want to pass my AIM subject!!",
            "category": "school-note",
            "published": true,
            "createdAt": "2025-04-01T08:25:05.836Z",
            "updatedAt": "2025-04-01T08:25:05.836Z"
        }
    }
}
```


Sample Response (UUID does not exist):

``` json 
{
    "status": "fail",
    "message": "<UUID>"
}
```


5. Delete Post It Note (DELETE) - Deletes the post it note of the UUID passed from the URI.
   An error message is shown if the UUID does not exist or has been deleted.


Sample Request:

```
curl --location --request PATCH 'http://localhost:8000/api/notes/67eba2e19b9d99ac29b6ad3e' \
--header 'Content-Type: application/json' \
--data '{
    "title": "Updating my first note! ✅✅👇👇",
    "published": true
}'
```

Sample Response:

No Content 204 


Sample Response (UUID does not exist):

``` json 
{
    "status": "fail",
    "message": "Note with ID: 67eb9feb70b1bb39f4b3399e not found"
}
```


6. Get All Post It Notes (GET) - Retrieves all the post it notes in the database. The API is paginated

Sample Request:

```
curl --location 'http://localhost:8000/api/notes?page=1&limit=10'
```

Sample Response:

``` json 
{
    "status": "success",
    "results": 2,
    "notes": [
        {
            "id": "67ebaa1ff6c94f0fe0974157",
            "title": "My first post it note",
            "content": "I want to pass my AIM subject!!",
            "category": "school-note",
            "published": false,
            "createdAt": "2025-04-01T08:55:59.261Z",
            "updatedAt": "2025-04-01T08:55:59.261Z"
        },
        {
            "id": "67ebaa29f6c94f0fe0974158",
            "title": "My second post it note",
            "content": "I still want to pass my AIM subject!!",
            "category": "school-note",
            "published": false,
            "createdAt": "2025-04-01T08:56:09.535Z",
            "updatedAt": "2025-04-01T08:56:09.535Z"
        }
    ]
}
```


Sample Response (No Data):

``` json 
{
    "status": "fail",
    "message": "<UUID>"
}
```
