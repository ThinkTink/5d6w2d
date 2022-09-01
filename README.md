# Introduction to this Application
You will be modifying an existing server that provides an API for a blogging website. The database for the API has a table of blog Posts, which include information about each blog post such as the text and author of the post, how many times the post has been “liked”, etc. Additionally, the database contains Users. Each blog post can have multiple authors, which correspond to users in the database (this association is stored in the database as UserPost). A new blog post must have at least one author that is a user already registered in the database.

Currently, the starting code has the following API routes already implemented:
- POST /api/register - Register a new user
- POST /api/login - Login for an existing user
- POST /api/posts - Create a new post

Only a logged in User can use this blogging website API, with the exception of the login and register routes.

# Getting Started

- System requirements
  - Node.JS v16
- Set environment variables.
  Create a `.env` file in the root directory, and copy the contents from [.env.sample](.env.sample)
- Install dependencies
  ```
  yarn
  ```
- Seed database
  ```
  yarn seed
  ```
- Start a dev server
  ```
  yarn dev
  ```

# Getting Started (Docker)

Instead of following the steps above, you can use Docker to set up your environment.

- System requirements
  - [Docker Compose](https://docs.docker.com/compose/install/)
- Run `docker-compose up` to spin up the dev server.
- Enter `Ctrl-C` in the same same terminal or `docker-compose down` in a separate terminal to shut down the server.

# Verify That Everything Is Set Up Correctly

You can use cURL or a tool like [Postman](https://www.postman.com/) to test the API.

#### Example Curl Commands

You can log in as one of the seeded users with the following curl command:

```bash
curl --location --request POST 'localhost:8080/api/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "thomas",
    "password": "123456"
}'
```

Then you can use the token that comes back from the /login request to make an authenticated request to create a new blog post

```bash
curl --location --request POST 'localhost:8080/api/posts' \
--header 'x-access-token: your-token-here' \
--header 'Content-Type: application/json' \
--data-raw '{
    "text": "This is some text for the blog post...",
    "tags": ["travel", "hotel"]
}'
```

# Helpful Commands

- `yarn prettier --write .` : Runs autoformatter
- `yarn test` : This repository contains a non-comprehensive set of unit tests used to determine if your code meets the basic requirements of the assignment. **Please do not modify these tests.**
- `yarn seed` : Resets database and populates it with sample data.

# Common Setup Errors

- If you encounter an error related to `secretOrPrivateKey` when attempting to make a request, please ensure you have created a .env file in the root directory as per the _Getting Started_ instructions.
- If you are on M1 mac, you might encounter `ERR_DLOPEN_FAILED` when you try to install dependencies locally then run the server in docker (or vice versa). In case of error, try removing the `node_modules` directory and restart `docker compose up`.
