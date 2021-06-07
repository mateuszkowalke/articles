# Building graphql backend in Go using gqlgen

## Initial setup

First things first, we need to create our project's directory:

```sh
mkdir gql-blog
cd gql-blog
```
Next initiate our project:
```sh
go mod init example.com/gql-blog
```
And now the fun begins. We'll be using the gqlgen package by 99designs. First we need to "go get it":
```sh
go get github.com/99designs/gqlgen
```
Most likely you'll need to add a few more dependencies, as stated in gqlgen command output. After you do that, we can start our new GraphQL server by running:
```sh
go run server.go
```

## First lookaround

The command `gqlgen init` created suggested project structure for us. First let's take a look in the `/graph` directory. There we can see the most important part of our graphql server - the schema. This article won't focus too much on the Schema Definition Language (SDL), so if you're looking for deeper dive please check out [this link](https://www.howtographql.com/basics/2-core-concepts/).

Our service will provide data for my blog. We're going to write the schema like this:

```gql
type Blogpost {
    id: ID!
    title: String!
    file: String!
    rating: Number
    comments: [Comment!]!
    author: User!
    published: Date!
}

type User {
  id: ID!
  email: String!
  username: String!
  password: String!
}

type Comment {
  id: ID!
  text: String!
  author: User!
  published: Date!
}

input NewBlogpost {
  title: String!
  file: String!
  author: User!
}

input NewUser {
  email: String!
  username: String!
  password: String!
}

type Mutation {
  createNewBlogpost(input: NewBlogpost!): Blogpost!
  createNewUser(input: NewUser!): User!
}
```
Now what have we just written?

At the top we are declaring `Blogpost`, `User` and `Comment` type. These are the types of data we are going to be using in our blog. They describe what fields we can find on each type. Also notice the fields themselves are typed, so working on frontend we always know what to expect from server after issuing specific query.

Something worth noticing is the `comments` field on `Blogpost` object type. It's an array of `Comment` objects. The `[Comment!]!` part means that there always will be an array (but the array can be empty) and it can contain only `Comment` objects.