# MongoDB and Mongosh

## Objectives

* Understand what a non-relational database is.
* Understand how queries are made to a non-relational database.
* Analyze and design databases with their corresponding collections.
*  Understand how to interact with the data stored in the database.

## Tools

* MongoDB
* Mongosh terminal

# Table of contents

  - [Objectives](#objectives)
  - [Tools](#tools)
  - [Tasks](#tasks)
  - [1.1 Develop the Project](#11-develop-the-project)
  - [1.2.1 INSERT DATA](#121-insert-data)
  - [1.2.2 UPDATE DATA](#122-update-data)
  - [1.2.3 GET DATA](#123-get-data)
  - [1.2.4 DELETE DATA](#124-delete-data)
  - [1.3 BONUS](#13-bonus)

## Tasks

## 1.1 Develop the Project

Create your own social network database with the following collections:
* Users
* Posts
  * Comments


**Creating the database:**

```
use socialMediaApr19
```
**Creating collections:**

```
db.createCollection("users");
db.createCollection("posts");
```

1.2. Running Queries

## 1.2.1 INSERT DATA

Insert at least 10 new users:
* Username
* Email
* Age 

Insert at least 15 new posts with at least 2 comments per post:
* Title
* Body
* Username
* Comments

### 🔷InsertOne()

```
db.users.insertOne({username: "Cowboy93", email: "example@example.co", age: 30});
```

```
db.posts.insertOne(
{
  title: "Morning coffee",
  body: "The best thing in the world is to have a coffee at sunrise when everybody else is still asleep",
  username: "Cowboy93",
  comments: [
          {
            username: "Foodie4Life",
            body: "Even better with pancakes!"
          },
          {
            username: "FitnessFanatic",
            body: "Only after my morning workout"
          }]
});
```

### 🔷InsertMany()

```
db.users.insertMany([
{username: "GamerGal22", email: "example1@msn.com", age: 25},
{username: "FitnessFanatic", email: "example2@msn.com", age: 35},
{username: "TravelBug", email: "example3@msn.com", age: 33},
{username: "Foodie4Life", email: "example4@msn.com", age: 28},
{username: "BookWorm", email: "example5@msn.com", age: 27},
{username: "MusicLover", email: "example6@msn.com", age: 40},
{username: "Fashionista", email: "example7@msn.com", age: 23},
{username: "PetLoverForever", email: "example8@msn.com", age: 31},
{username: "PhotographyPro", email: "example9@msn.com", age: 55}
]);
```

```
db.posts.insertMany([
  {title: "Travel Dreaming",body: "I can't wait to travel again! Where should I go next?", username: "TravelBug88", comments: [
          {username: "Foodie4Life", body: "Have you tried exploring the local cuisine in Italy? It's  a foodie's paradise!"},
          {username: "ArtEnthusiast",body: "Paris is always a good idea for art lovers like us. The Louvre is a must-visit!"}
         ]
  },
  {
     NEXT POST HERE
  },
  {
     AND NEXT...
  },
  {
      ...UNTIL THE LAST...
  }
]);
```
Example result:
```JSON
{
  "_id": {"$oid": "643fbabed1f5200430d6612e"},
  "username": "GamerGal22",
  "email": "example1@msn.com",
  "age": 25
}
```



 Insert at least 10 new users:
○ Username
○ Email
○ Age 


## 1.2.2 UPDATE DATA

Update publications:
* Update all fields of a post
* Change the body of a publication.


### 🔷updateOne()
updateOne({ *select post* }, {**$set:** {*set new values*}})
```
 db.posts.updateOne(
...    {title: "Morning coffee"},
...    {$set: {
...       title: "Breakfast",
...       body: "Going for brunch with friends!",
...       username: "TravelBug88",
...       comments: [
...          {username: "FitnessFanatic", body: "Count your calories!"},
...          {username: "BookWorm12", body: "Have fun"}
...       ]
...    }}
... );
```

```

db.posts.updateOne({title:"Movie Night"}, {$set:{body:"Going to see Avatar 3 in 4D!!"}});

```

Original:
```JSON
...
"title":"Movie Night"
"body":"Cozying up with some popcorn and a classic movie."
...
```
New:
```JSON
...
  "title": "Movie Night",
  "body": "Going to see Avatar 3 in 4D!!",
  ...
```



Update comments:
* Update a post's comment.

```
db.posts.updateOne({title:"Movie Night"}, {$set:{"comments.1.body":"Don't waste your money."}});
```
>__Note__  
**NUMBER 1** here refers to the index number of the comments array (the second comment in this case).  
It's also necessary to wrap the whole **comments.1.body** in **inverted commas** because the field name contains special characters (dots).

Original:
```JSON
...
{
    "comments": [
    {
      "username": "GamerGal22",
      "body": "I'm more of a gamer, but I can appreciate a good movie night too!"
    },
    {
      "username": "ArtEnthusiast",
      "body": "What's your favorite classic movie? I love 'Gone with the Wind'."
    }
  ]
}

...
```
New:
```JSON
...
 {
    "comments": [
    {
      "username": "GamerGal22",
      "body": "I'm more of a gamer, but I can appreciate a good movie night too!"
    },
    {
      "username": "ArtEnthusiast",
      "body": "Don't waste your money."
    }
  ]
}
  ...
```

Update users:
* Update all the fields of a user
* Change the email of two users, that is, make the query twice.


```
db.users.updateOne({username:"BookWorm"}, {$set:{username:"PartyAnimal", email:"updated@msn.com", age:19}});
```
```
db.users.updateOne({username: "PartyAnimal"}, {$set:{email:"updatedagain@msn.com"}});
db.users.updateOne({username: "TravelBug"}, {$set:{email:"updatenumber2@msn.com"}});
```

* Increase the age of a user by 5 years

### 🔶{$inc: {*select field: increase value* }}

```
db.users.updateOne({username:"Cowboy93"}, {$inc: {age: 5}});
```
From:
```JSON
"age": 30
```
To:
```JSON
"age": 35
```



## 1.2.3 GET DATA

* Select all posts
* Select the publications that match the indicated username

### 🔷find()
```
 db.posts.find();
```
```
db.posts.find({username:"TravelBug88"});
```

* Select all users older than 20
* Select all users under the age of 23
* Select all users who are between the ages of 25 and 30

### 🔶{$gt: *value*} (greater than)

```
db.users.find({age: {$gt:20}});
```
### 🔶{$lt:*value*} (less than)

```
socialMediaApr19> db.users.find({age: {$lt:20}});
```
To select a range, we can separate two comparison operators with a comma: 
```
db.users.find({age:{$gte:25,$lte:30}});
```
A more complex option would be to use $and - operator:
```
db.users.find({
  $and:[
    {age:{$gte:25}},
    {age:{$lte:30}}
    ]
  });
```


* Show users from youngest to oldest and vice versa

### 🔷sort({*field:* 1}) Ascending sort
```
db.users.find().sort({age:1});
```
### 🔷sort({*field:* -1}) Descending sort
```
db.users.find().sort({age:-1});
```

* Select the total number of users

### 🔷count()
 db.users.find().count();


* Select all posts and have them displayed with the following structure: Post title: "title one"

### 🔷forEach() with 🔷print()
```
db.posts.find().forEach((post)=>{print("Post title: " + post.title)});
```
Result:
```
Post title: Breakfast
Post title: Morning Coffee
Post title: Travel Dreaming
Post title: Monday Motivation
Post title: Movie Night
Post title: New Recipe Alert
Post title: Sunday Brunch
Post title: Fashion Finds
Post title: Sunday Funday
Post title: New Book Alert
Post title: Home Workout
Post title: Exploring the city
Post title: Sushi Night
Post title: Awful experience
Post title: Real Bargain!
```

* Select only 2 users
### 🔷limit(*number*)
Shows the first 2.
```
db.users.find().limit(2);
```

* Search by title 2 publications 
### 🔷createIndex() and 🔶{$text:{$search: *search string* }}

I created a search index, specifying the field to be "title", and type of search as "text": 
```
db.posts.createIndex({title:"text"});
```
Then I searched posts whose title includes "Sunday" and limited the search to two.
```
db.posts.find({
  $text:{
    $search:"Sunday"
    }
  }).limit(2);
```

## 1.2.4 DELETE DATA

* Remove all users older than 30
```
 db.users.deleteMany({age:{$gt:30}});
```
result: 6 users were deleted as informed by the terminal
```
{ acknowledged: true, deletedCount: 6 }
```

## 1.3 BONUS

* Select the total number of posts that have more than one comment

Option 1:

### 🔷countDocuments({*query*, *options*});
To count all documents, specify an empty document as the first parameter.

```
db.posts.countDocuments({},{comments:{$gt:1}});
```
>__Warning__  
This is a **mongosh method**. This is not for database commands or language-specific drivers, such as Node.js.

Option 2:
### 🔶{$exists: *true/false*}

Finds posts where comments.1 (comment array index 1) exists, which means there are at least two comments:

```
db.posts.find({ "comments.1": { $exists: true } })   
```

Option 3:
### 🔶{$expr: ---}
This allows for the expansion of the range of operators we have at our disposal by using operators normally used in **aggregation pipelines** (when we want to perform complex data transformations and calculations, such as filtering, sorting, grouping, and summarizing data).

```
db.posts.find({
   $expr: {
      $gt: [ { $size: "$comments" }, 1 ]
   }
});
```
>__NOTE__  
-The expression **{ $size: "$comments" }** returns the size of the comments array for a given document.  
-The gt operator returns true if the first operand is greater than the second operand.-First operand here is the length of the array, and second is 1. If the length of comments is greater than 1, the post is displayed.   
-In MongoDB's aggregation framework, the $ symbol is used to indicate that a field name is being referred to as a variable, rather than as a string literal. "$comments" is not being treated as a string literal that represents the name of the field, but as a variable that will be replaced by the actual value of the comments field for each document in the returned collection.

* Select the last post created
* Select 5 latest posts

Since no date was created, we can sort the posts by ID in descending order (latest first) and then limit the search to the number we want.
```
db.posts.find().sort({_id:-1}).limit(1);
```
```
db.posts.find().sort({_id:-1}).limit(5);
```

* Delete all posts that have more than one comment

Since all the posts have more than one comment, the following deletes all the posts that currently exist:

```
db.posts.deleteMany({ "comments.1": { $exists: true } })   
```

