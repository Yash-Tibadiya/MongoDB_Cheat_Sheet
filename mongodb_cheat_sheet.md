# MongoDB Cheat Sheet

## Show All Databases

```bash
show dbs
```

## Show Current Database

```bash
db
```

## Create Or Switch Database

```bash
use DB_NAME
```

## Drop

```bash
db.dropDatabase()
```

## Create Collection

```bash
db.createCollection('posts')
```

## Show Collections

```bash
show collections
```

## Insert Row

```bash
db.posts.insert({
  title: 'Post One',
  body: 'Body of post one',
  category: 'News',
  tags: ['news', 'events'],
  user: {
    name: 'John Doe',
    status: 'author'
  },
  date: Date()
})
```

## Insert Multiple Rows

```bash
db.posts.insertMany([
  {
    title: 'Post Two',
    body: 'Body of post two',
    category: 'Technology',
    date: Date()
  },
  {
    title: 'Post Three',
    body: 'Body of post three',
    category: 'News',
    date: Date()
  },
  {
    title: 'Post Four',
    body: 'Body of post three',
    category: 'Entertainment',
    date: Date()
  }
])
```

## Get All Rows

```bash
db.posts.find()
```

## Get All Rows Formatted

```bash
db.posts.find().pretty()
```

## Find Rows

```bash
db.posts.find({ category: 'News' })
```

## Sort Rows

```bash
# asc
db.posts.find().sort({ title: 1 }).pretty()
# desc
db.posts.find().sort({ title: -1 }).pretty()
```

## Count Rows

```bash
db.posts.find().count()
db.posts.find({ category: 'news' }).count()
```

## Limit Rows

```bash
db.posts.find().limit(2).pretty()
```

## Chaining

```bash
db.posts.find().limit(2).sort({ title: 1 }).pretty()
```

## Foreach

```bash
db.posts.find().forEach(function(doc) {
  print("Blog Post: " + doc.title)
})
```

## Find One Row

```bash
db.posts.findOne({ category: 'News' })
```

## Find Specific Fields

```bash
db.posts.find({ title: 'Post One' }, {
  title: 1,
  author: 1
})
```

## Update Row

```bash
db.posts.update({ title: 'Post Two' },
{
  title: 'Post Two',
  body: 'New body for post 2',
  date: Date()
},
{
  upsert: true
})
```

## Update Specific Field

```bash
db.posts.update({ title: 'Post Two' },
{
  $set: {
    body: 'Body for post 2',
    category: 'Technology'
  }
})
```

## Increment Field (\$inc)

```bash
db.posts.update({ title: 'Post Two' },
{
  $inc: {
    likes: 5
  }
})
```

## Rename Field

```bash
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    likes: 'views'
  }
})
```

## Delete Row

```bash
db.posts.remove({ title: 'Post Four' })
```

## Sub-Documents

```bash
db.posts.update({ title: 'Post One' },
{
  $set: {
    comments: [
      {
        body: 'Comment One',
        user: 'Mary Williams',
        date: Date()
      },
      {
        body: 'Comment Two',
        user: 'Harry White',
        date: Date()
      }
    ]
  }
})
```

## Find By Element in Array (\$elemMatch)

```bash
db.posts.find({
  comments: {
     $elemMatch: {
       user: 'Mary Williams'
       }
    }
  }
)
```

## Add Index

```bash
db.posts.createIndex({ title: 'text' })
```

## Text Search

```bash
db.posts.find({
  $text: {
    $search: "\"Post O\""
    }
})
```

## Greater & Less Than

```bash
db.posts.find({ views: { $gt: 2 } })
db.posts.find({ views: { $gte: 7 } })
db.posts.find({ views: { $lt: 7 } })
db.posts.find({ views: { $lte: 7 } })
```

## Regex Search

```bash
db.posts.find({ title: /Post/ })
```

<hr />

# Update/delete examples using updateOne, updateMany, deleteOne, deleteMany with $set, and $unset.

## 1) UPDATE

- updateOne() modifies only the first matching document.
- updateMany() modifies all matching documents.
- $set: Updates (or adds) the specified fields.
- $unset: Removes the specified fields.

Update One:

```bash
db.posts.updateOne( { title: "Post One" }, { $set: { body: "Updated body for Post One" }, $unset: { category: "" } } );
```

Update Many:

```bash
db.posts.updateMany( { category: "News" }, { $set: { category: "Updated News" } } );
```

## 2) DELETE

- deleteOne() removes only the first matching document.
- deleteMany() removes all matching documents.
  Delete One:

```bash
db.posts.deleteOne( { title: "Post Four" } );
```

Delete Many

```bash
db.posts.deleteMany( { category: "Entertainment" } );
```

<hr />

# Comparison operator examples:

$ne (Not equals) - Find documents where 'category' is NOT 'News':

```bash
db.posts.find({ category: { $ne: "News" } });
```

$in (Matches Any Of) - Find documents where 'category' is either 'News' OR 'Technology':

```bash
db.posts.find({ category: { $in: ["News", "Technology"] } });
```

$nin (Not In) - Find documents where 'category' is neither 'News' nor 'Technology':

```bash
db.posts.find({ category: { $nin: ["News", "Technology"] } });
```

<hr />

# Logical operator examples:

$and (Matches All) - Find documents where 'category' is "News" AND 'user.name' is "John Doe":

```bash
db.posts.find({ $and: [ { category: "News" }, { "user.name": "John Doe" } ] });
```

$nor (Matches None) - Find documents where 'category' is neither "News" NOR "Technology":

```bash
db.posts.find({ $nor: [ { category: "News" }, { category: "Technology" } ] });
```

$or (Matches Any) - Find documents where 'category' is "News" OR "Technology":

```bash
  db.posts.find({ $or: [ { category: "News" }, { category: "Technology" } ] });
```

$not (With $lte & $regex) - Find documents where 'views' is NOT less than or equal to 100 (i.e., greater than 100), and the 'title' does not start with "Post":

```bash
  db.posts.find({ $and: [ { views: { $not: { $lte: 100 } } }, { title: { $not: /^Post/ } } ] });
```
