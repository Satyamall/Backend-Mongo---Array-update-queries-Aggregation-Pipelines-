
# Mongo - Array update queries, Aggregation Pipelines.


# Operations are done:-

# $push
- pushing single values
- pushing multiple values - **$each**
- with modifiers, like **$sort, $slice**

# $pull
- pull a particular value
- many with condition
- use **$in** with pull

# $pop
- -1, 1 like stack and queue,

# $addToSet
- add data and maintaining the set property.

# Aggregations.

db.users.aggregate([
    {
        $match
    },
])
<!-- match -->

1- use match to filter out some data in aggregation
2- use group along with it to group by country, and count of results in each country
3- show average rating, max rating and min rating as well.
4- sort by rating in descending order, limit to top 10 results
5- project to only results where country belongs to US, do not show \_id, location
6- use sortByCount to show all companies grouped by country and counts.



# 1. insertMany in collection posts.
```js
db.posts.insertMany([
    {
        "title": "Masai 101",
        "author_id": "1",
        "author": "Satya",
        "comments": [
            {
                "body": "Hello",
                "author": "Sid",
                "author_id": "2"
            }
        ],
        "tags": ["101", "Programming"]
    },
    {
        "title": "Masai 102",
        "author_id": "3",
        "author": "Jaswant",
        "comments": [
            {
                "body": "Hello",
                "author": "Satya",
                "author_id": "1"
            }
        ],
        "tags": ["101"]
    }
])
```
```js
 db.posts.find()
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '101' ]
  }
]

# 2. $push. by the help of updateOne.
```js
db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: "102"}})
```
**Shows->**
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

```js
db.posts.find()
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '101', '102' ]
  }
]

```js
db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: ["103","Programming"]}})
```
```js
db.posts.find()
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '101', '102', [ '103', 'Programming' ] ]
  }
]


# 3. $push - $each.
```js
db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: {$each: ["103","Programming"]}}})
```
```js
db.posts.find()
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '101', '102', [ '103', 'Programming' ], '103', 'Programming' ]
  }
]


# 4. $push - $each - $sort , $slice.
```js
db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: {$each: ["104","204"], $sort: {tags: -1},$slice: 3}}})
```
```js
db.posts.find()
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '101', '102', [ '103', 'Programming' ] ]
  }
]

```js
db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: {$each: ["104","204"], $sort: {tags: 1},$slice: 3}}})
```
```js
db.posts.find()
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [ { body: 'Hello', author: 'Sid', author_id: '2' } ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '101', '102', [ '103', 'Programming' ] ]
  }
]


# 5. New collections students insertMany.
```js
db.students.insertMany([ { name: "Satya", scores: [96,94,70,67,64] }])   
```
**Output->**
{
  acknowledged: true,
  insertedIds: { '0': ObjectId("6208bd817092788c90649a18") }
}

```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 96, 94, 70, 67, 64 ]
  }
]


# 6. $push - $each - $sort, $slice with the help of updateOne.
```js
 db.students.updateOne({ name: "Satya" }, { $push: { scores: {$each: [35, 100, 45], $sort: { scores: -1 }, $slice: 3 }} })
```
```js
 db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 96, 94, 70 ]
  }
]

# 7. $sort directly apply in push operation and it gives the correct answer.
```js
db.students.updateOne({ name: "Satya" }, { $push: { scores: {$each: [35, 100, 45], $sort: -1 , $slice: 3 }} })
```
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 96, 94 ]
  }
]


# 8. with collections posts.
```js
db.posts.updateOne({title: "Masai 101"},{$push: {comments: {$each: [{body: "Hi", author: "Satya",author_id: "1" },{body: "Nice!",author: "Sid", author_id: "2"}]}}})
```
```js
db.posts.find({})
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [
      { body: 'Hello', author: 'Sid', author_id: '2' },
      { body: 'Hi', author: 'Satya', author_id: '1' },
      { body: 'Nice!', author: 'Sid', author_id: '2' }
    ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '101', '102', [ '103', 'Programming' ] ]
  }
]


# 9. $pull with the help of updateOne.
```js
db.posts.updateOne({title: "Masai 102"},{$pull: {tags: "101"}})
```
```js
db.posts.find({})
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [
      { body: 'Hello', author: 'Sid', author_id: '2' },
      { body: 'Hi', author: 'Satya', author_id: '1' },
      { body: 'Nice!', author: 'Sid', author_id: '2' }
    ],
    tags: [ '101', 'Programming' ]
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '102', [ '103', 'Programming' ] ]
  }
]

```js
db.posts.updateOne({title: "Masai 101"},{$pull: {tags: {$in: ["101","Programming"]}}})
```
```js
db.posts.find({})
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [
      { body: 'Hello', author: 'Sid', author_id: '2' },
      { body: 'Hi', author: 'Satya', author_id: '1' },
      { body: 'Nice!', author: 'Sid', author_id: '2' }
    ],
    tags: []
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '102',['103','Programming'] ]
  }
]

```js
db.posts.updateOne({title: "Masai 102"},{$pull: {tags: {$in: [["103","Programming"]]}}})
```
```js
db.posts.find({})
```
**Output->**
[
  {
    _id: ObjectId("6208b9397092788c90649a16"),
    title: 'Masai 101',
    author_id: '1',
    author: 'Satya',
    comments: [
      { body: 'Hello', author: 'Sid', author_id: '2' },
      { body: 'Hi', author: 'Satya', author_id: '1' },
      { body: 'Nice!', author: 'Sid', author_id: '2' }
    ],
    tags: []
  },
  {
    _id: ObjectId("6208b9397092788c90649a17"),
    title: 'Masai 102',
    author_id: '3',
    author: 'Jaswant',
    comments: [ { body: 'Hello', author: 'Satya', author_id: '1' } ],
    tags: [ '102' ]
  }
]


# 10. $pull with update in collections students.
```js
db.students.update({},{$pull: {scores:{$lte: 96}}})
```
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100 ]
  }
]


# 11. $push - $each - $sort
```js
db.students.update({},{$push: {scores:{$each: [100,50,30,20],$sort: -1}}}) 
```
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 100, 50, 30, 20 ]
  }
]


# 12. $pop.
```js
db.students.update({},{$pop: {scores: 1}}) 
```
- Last element will be remove like stack.
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 100, 50, 30 ]
  }
]

```js
 db.students.update({},{$pop: {scores: -1}})
 ```
- Remove from front like queue.
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30 ]
  }
]



# 13. $addToSet with updateMany.
```js
db.students.updateMany({},{$addToSet: {scores: 90}})
```
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90 ]
  }
]

# - Add again with the set don't  added the data.

```js
db.students.updateMany({},{$addToSet: {scores: 90}})
```
```js
db.students.find()
```
**Output->Still as previous**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90 ]
  }
]

# - 100 is already present but it not add 100 once again.
```js
db.students.updateMany({},{$addToSet: {scores: 100}})
```
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90 ]
  }
]


# 14. $addToSet - $each with updateMany.
```js
db.students.updateMany({},{$addToSet: {scores: {$each: [100,50,99,80]}}})
```
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90, 99, 80 ]
  }
]

```js
db.students.updateMany({},{$push: {scores: {$each: [100,50,99,80]}}})
```
```js
db.students.find()
```
**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [
      100,  50, 30, 90, 99,
       80, 100, 50, 99, 80
    ]
  }
]


# 15. Aggregations.

db.users.aggregate(
    [
        {
            $match
        },
    ]
)


```js
db.companies.aggregate([{$match: {rating: {$gte: 90}}}])
```
- aggregate all companies and match where rating is greater than 90



# $group
```js
db.companies.aggregate([
    { $match: { rating: {$gte: 90}}},
    { $group: {
        _id: {origin_country: "TH"}
    }}
])
```
**Output->**
[ { _id: { origin_country: 'TH' } } ]

```js
db.companies.aggregate([
    { $match: { rating: {$gte: 90}}},
    { $group: {
        _id: "$origin_country",
        count: {$sum: 1}
    }}
])
```
**Output->**
[
  { _id: 'KM', count: 1 }, { _id: 'AF', count: 1 },
  { _id: 'UZ', count: 1 }, { _id: 'YE', count: 1 },
  { _id: 'NO', count: 1 }, { _id: 'UY', count: 2 },
  { _id: 'EE', count: 1 }, { _id: 'CR', count: 1 },
  { _id: 'MY', count: 1 }, { _id: 'KP', count: 1 },
  { _id: 'PE', count: 3 }, { _id: 'PH', count: 7 },
  { _id: 'PA', count: 1 }, { _id: 'CA', count: 4 },
  { _id: 'KE', count: 1 }, { _id: 'ZA', count: 1 },
  { _id: 'KH', count: 1 }, { _id: 'MX', count: 1 },
  { _id: 'JP', count: 1 }, { _id: 'CI', count: 1 }
]
Type "it" for more

```js
 db.companies.find({origin_country: "CN",rating: {$gte: 90}}).count()
 ```
**Output->** 19

```js
 db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 1 } } }])
```
**Output->**
[
  { _id: 'KM', count_abc: 1 },
  { _id: 'AF', count_abc: 1 },
  { _id: 'UZ', count_abc: 1 },
  { _id: 'EC', count_abc: 1 },
  { _id: 'YE', count_abc: 1 },
  { _id: 'NO', count_abc: 1 },
  { _id: 'UY', count_abc: 2 },
  { _id: 'EE', count_abc: 1 },
  { _id: 'MY', count_abc: 1 },
  { _id: 'CR', count_abc: 1 },
  { _id: 'KP', count_abc: 1 },
  { _id: 'PE', count_abc: 3 },
  { _id: 'PH', count_abc: 7 },
  { _id: 'CA', count_abc: 4 },
  { _id: 'PA', count_abc: 1 },
  { _id: 'ZA', count_abc: 1 },
  { _id: 'KE', count_abc: 1 },
  { _id: 'MX', count_abc: 1 },
  { _id: 'KH', count_abc: 1 },
  { _id: 'JP', count_abc: 1 }
]
Type "it" for more

```js
db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } } }])
```
**$sum: 2 means multiplying by number 2**
**Output->**
[
  { _id: 'KM', count_abc: 2 },
  { _id: 'AF', count_abc: 2 },
  { _id: 'UZ', count_abc: 2 },
  { _id: 'EC', count_abc: 2 },
  { _id: 'YE', count_abc: 2 },
  { _id: 'NO', count_abc: 2 },
  { _id: 'UY', count_abc: 4 },
  { _id: 'EE', count_abc: 2 },
  { _id: 'MY', count_abc: 2 },
  { _id: 'CR', count_abc: 2 },
  { _id: 'KP', count_abc: 2 },
  { _id: 'PE', count_abc: 6 },
  { _id: 'PH', count_abc: 14 },
  { _id: 'CA', count_abc: 8 },
  { _id: 'PA', count_abc: 2 },
  { _id: 'ZA', count_abc: 2 },
  { _id: 'KE', count_abc: 2 },
  { _id: 'MX', count_abc: 2 },
  { _id: 'KH', count_abc: 2 },
  { _id: 'JP', count_abc: 2 }
]
Type "it" for more

```js
 db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } , average: {$avg: "$rating"}}}])
```
 **Output->**
 [
  { _id: 'KM', count_abc: 2, average: 98 },
  { _id: 'AF', count_abc: 2, average: 94 },
  { _id: 'UZ', count_abc: 2, average: 96 },
  { _id: 'EC', count_abc: 2, average: 92 },
  { _id: 'YE', count_abc: 2, average: 95 },
  { _id: 'NO', count_abc: 2, average: 95 },
  { _id: 'UY', count_abc: 4, average: 95 },
  { _id: 'EE', count_abc: 2, average: 95 },
  { _id: 'MY', count_abc: 2, average: 94 },
  { _id: 'CR', count_abc: 2, average: 99 },
  { _id: 'PE', count_abc: 6, average: 95.33333333333333 },
  { _id: 'KP', count_abc: 2, average: 99 },
  { _id: 'PH', count_abc: 14, average: 94.71428571428571 },
  { _id: 'CA', count_abc: 8, average: 97.25 },
  { _id: 'PA', count_abc: 2, average: 96 },
  { _id: 'ZA', count_abc: 2, average: 98 },
  { _id: 'KE', count_abc: 2, average: 95 },
  { _id: 'MX', count_abc: 2, average: 91 },
  { _id: 'KH', count_abc: 2, average: 97 },
  { _id: 'JP', count_abc: 2, average: 93 }
]
Type "it" for more

```js
db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } , average: {$avg: "$rating"},max: {$max: "$rating"},min: {$min: "$rating"}}}])
```
**Output->**
[
  { _id: 'KM', count_abc: 2, average: 98, max: 98, min: 98 },
  { _id: 'AF', count_abc: 2, average: 94, max: 94, min: 94 },
  { _id: 'UZ', count_abc: 2, average: 96, max: 96, min: 96 },
  { _id: 'EC', count_abc: 2, average: 92, max: 92, min: 92 },
  { _id: 'YE', count_abc: 2, average: 95, max: 95, min: 95 },
  { _id: 'NO', count_abc: 2, average: 95, max: 95, min: 95 },
  { _id: 'UY', count_abc: 4, average: 95, max: 99, min: 91 },
  { _id: 'EE', count_abc: 2, average: 95, max: 95, min: 95 },
  { _id: 'MY', count_abc: 2, average: 94, max: 94, min: 94 },
  { _id: 'CR', count_abc: 2, average: 99, max: 99, min: 99 },
  { _id: 'KP', count_abc: 2, average: 99, max: 99, min: 99 },
  {
    _id: 'PE',
    count_abc: 6,
    average: 95.33333333333333,
    max: 97,
    min: 94
  },
  {
    _id: 'PH',
    count_abc: 14,
    average: 94.71428571428571,
    max: 99,
    min: 92
  },
  { _id: 'CA', count_abc: 8, average: 97.25, max: 98, min: 96 },
  { _id: 'PA', count_abc: 2, average: 96, max: 96, min: 96 },
  { _id: 'ZA', count_abc: 2, average: 98, max: 98, min: 98 },
  { _id: 'KE', count_abc: 2, average: 95, max: 95, min: 95 },
  { _id: 'MX', count_abc: 2, average: 91, max: 91, min: 91 },
  { _id: 'KH', count_abc: 2, average: 97, max: 97, min: 97 },
  { _id: 'JP', count_abc: 2, average: 93, max: 93, min: 93 }
]
Type "it" for more

```js
db.companies.aggregate([ { $match: {origin_country: "CA" } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } , average: {$avg: "$rating"}}}])      
```
**Output->**
[ { _id: 'CA', count_abc: 34, average: 56.8235294117647 } ]


# $limit :
```js
db.companies.aggregate([ { $match: {origin_country: "CA" } },{$limit: 10},{ $group: { _id: "$origin_country", count: { $sum: 1 } , average: {$avg: "$rating"}}}])
```
**Output->**
[ { _id: 'CA', count: 10, average: 48.5 } ]


# $sort and $limit :
```js
db.companies.aggregate([ { $match: {origin_country: "CA" } },{$sort: {rating: -1}},{$limit: 10},{ $group: { _id: "$origin_country", count: { $sum: 1 } , average: {$avg: "$rating"}}}])
```
**Output->**
[ { _id: 'CA', count: 10, average: 78.7 } ]


# .explain()
```js
db.companies.aggregate([ { $match: {origin_country: "CA" } },{$sort: {rating: -1}},{$limit: 10},{ $group: { _id: "$origin_country", count_abc: { $sum: 1 } , average: {$avg: "$rating"}}}]).explain()
```
**Output->**
{
  explainVersion: '1',
  stages: [
    {
      '$cursor': {
        queryPlanner: {
          namespace: 'masai.companies',
          indexFilterSet: false,
          parsedQuery: { origin_country: { '$eq': 'CA' } },
          queryHash: '179EC7D7',
          planCacheKey: 'AF03904D',
          maxIndexedOrSolutionsReached: false,
          maxIndexedAndSolutionsReached: false,
          maxScansToExplodeReached: false,
          winningPlan: {
            stage: 'PROJECTION_SIMPLE',
            transformBy: { origin_country: 1, rating: 1, _id: 0 },
            inputStage: {
              stage: 'SORT',
              sortPattern: { rating: -1 },
              memLimit: 104857600,
              limitAmount: 10,
              type: 'simple',
              inputStage: {
                stage: 'COLLSCAN',
                filter: { origin_country: { '$eq': 'CA' } },
                direction: 'forward'
              }
            }
          },
          rejectedPlans: []
        },
        executionStats: {
          executionSuccess: true,
          nReturned: 10,
          executionTimeMillis: 4,
          totalKeysExamined: 0,
          totalDocsExamined: 1004,
          executionStages: {
            stage: 'PROJECTION_SIMPLE',
            nReturned: 10,
            executionTimeMillisEstimate: 0,
            works: 1017,
            advanced: 10,
            needTime: 1006,
            needYield: 0,
            saveState: 2,
            restoreState: 2,
            isEOF: 1,
            transformBy: { origin_country: 1, rating: 1, _id: 0 },
            inputStage: {
              stage: 'SORT',
              nReturned: 10,
              executionTimeMillisEstimate: 0,
              works: 1017,
              advanced: 10,
              needTime: 1006,
              needYield: 0,
              saveState: 2,
              restoreState: 2,
              isEOF: 1,
              sortPattern: { rating: -1 },
              memLimit: 104857600,
              limitAmount: 10,
              type: 'simple',
              totalDataSizeSorted: 3013,
              usedDisk: false,
              inputStage: {
                stage: 'COLLSCAN',
                filter: { origin_country: { '$eq': 'CA' } },
                nReturned: 17,
                executionTimeMillisEstimate: 0,
                works: 1006,
                advanced: 17,
                needTime: 988,
                needYield: 0,
                saveState: 2,
                restoreState: 2,
                isEOF: 1,
                direction: 'forward',
                docsExamined: 1004
              }
            }
          },
          allPlansExecution: []
        }
      },
      nReturned: Long("10"),
      executionTimeMillisEstimate: Long("1")
    },
    {
      '$group': {
        _id: '$origin_country',
        count_abc: { '$sum': { '$const': 1 } },
        average: { '$avg': '$rating' }
      },
      maxAccumulatorMemoryUsageBytes: { count_abc: Long("80"), average: Long("88") },
      totalOutputDataSizeBytes: Long("261"),
      usedDisk: false,
      nReturned: Long("1"),
      executionTimeMillisEstimate: Long("1")
    }
  ],
  serverInfo: {
    host: 'LAPTOP-0RDA0M4E',
    port: 27017,
    version: '5.0.6',
    gitVersion: '212a8dbb47f07427dae194a9c75baec1d81d9259'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600
  },
  command: {
    aggregate: 'companies',
    pipeline: [
      { '$match': { origin_country: 'CA' } },
      { '$sort': { rating: -1 } },
      { '$limit': 10 },
      {
        '$group': {
          _id: '$origin_country',
          count_abc: { '$sum': 1 },
          average: { '$avg': '$rating' }
        }
      }
    ],
    cursor: {},
    '$db': 'masai'
  },
  ok: 1
}


# createIndex:
```js
db.companies.createIndex({origin_country: 1})
```
**Output->**
origin_country_1


# getIndexes:
```js
db.companies.getIndexes()
```
**Output->**
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { origin_country: 1 }, name: 'origin_country_1' }
]


# $group :
```js
db.companies.aggregate([
    { $match: {origin_country: "CA"}},
    { $sort: {rating: -1}},
    { $limit: 5}
])
```
**Output->**
[
  {
    _id: ObjectId("61ff7cbb84fad959b1793d91"),
    id: 854,
    company_name: 'Rhyzio',
    email: 'ncorainnp@icio.us',
    rating: 98,
    location: '44673 Manufacturers Alley',
    origin_country: 'CA'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793d46"),
    id: 779,
    company_name: 'Topiclounge',
    email: 'jmiramslm@instagram.com',
    rating: 98,
    location: '8 Arkansas Drive',
    origin_country: 'CA'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793b76"),
    id: 315,
    company_name: 'Midel',
    email: 'cmargrett8q@weebly.com',
    rating: 97,
    location: '06506 Green Court',
    origin_country: 'CA'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793da7"),
    id: 876,
    company_name: 'Skinix',
    email: 'ievisonob@mit.edu',
    rating: 96,
    location: '3973 Buena Vista Drive',
    origin_country: 'CA'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793e19"),
    id: 990,
    company_name: 'Photofeed',
    email: 'nhovingtonrh@simplemachines.org',
    rating: 77,
    location: '5824 5th Pass',
    origin_country: 'CA'
  }
]


# $project:
```js
db.companies.aggregate([ { $match: { origin_country: "CA" } }, { $sort: { rating: -1 } }, { $limit: 5 },{$project: {_id: 0, location: 0}}])
```
**Output->**
[
  {
    id: 854,
    company_name: 'Rhyzio',
    email: 'ncorainnp@icio.us',
    rating: 98,
    origin_country: 'CA'
  },
  {
    id: 779,
    company_name: 'Topiclounge',
    email: 'jmiramslm@instagram.com',
    rating: 98,
    origin_country: 'CA'
  },
  {
    id: 315,
    company_name: 'Midel',
    email: 'cmargrett8q@weebly.com',
    rating: 97,
    origin_country: 'CA'
  },
  {
    id: 876,
    company_name: 'Skinix',
    email: 'ievisonob@mit.edu',
    rating: 96,
    origin_country: 'CA'
  },
  {
    id: 990,
    company_name: 'Photofeed',
    email: 'nhovingtonrh@simplemachines.org',
    rating: 77,
    origin_country: 'CA'
  }
]


# sortByCount: 
```js
db.companies.aggregate([
    { $match: {origin_country: "CA"}},
    { $sortByCount: "rating"}
])
```
```js
db.companies.aggregate([ { $match: { origin_country: "CA" } }, { $sortByCount: "$rating" }])
```
**Output->**
[
  { _id: 43, count: 2 },
  { _id: 98, count: 2 },
  { _id: 11, count: 1 },
  { _id: 77, count: 1 },
  { _id: 96, count: 1 },
  { _id: 71, count: 1 },
  { _id: 72, count: 1 },
  { _id: 44, count: 1 },
  { _id: 97, count: 1 },
  { _id: 8, count: 1 },
  { _id: 76, count: 1 },
  { _id: 19, count: 1 },
  { _id: 17, count: 1 },
  { _id: 38, count: 1 },
  { _id: 58, count: 1 }
]

```js
db.companies.aggregate([ { $match: { origin_country: "CA" } }, { $sortByCount: "$origin_country" }])
```
**Output->** [ { _id: 'CA', count: 17 } ]

```js
db.companies.aggregate([ { $match: { rating: {$gte: 90} } }, { $sortByCount: "$origin_country" }])
```
**Output->**
[
  { _id: 'CN', count: 19 }, { _id: 'ID', count: 8 },
  { _id: 'PH', count: 7 },  { _id: 'GR', count: 7 },
  { _id: 'PL', count: 7 },  { _id: 'FR', count: 7 },
  { _id: 'CA', count: 4 },  { _id: 'BR', count: 4 },
  { _id: 'PE', count: 3 },  { _id: 'PT', count: 3 },
  { _id: 'UY', count: 2 },  { _id: 'RU', count: 2 },
  { _id: 'CO', count: 2 },  { _id: 'US', count: 2 },
  { _id: 'SE', count: 2 },  { _id: 'IE', count: 2 },
  { _id: 'KM', count: 1 },  { _id: 'AF', count: 1 },
  { _id: 'UZ', count: 1 },  { _id: 'EC', count: 1 }
]
Type "it" for more


# $group:
```js
db.companies.aggregate([ 
    { $match: { rating: {$gte: 90} } }, 
    { $group: {_id: "$origin_country", count: {$sum: 1}}},
    { $sort: {count: -1}}
])
```
**Output->**
[
  { _id: 'CN', count: 19 }, { _id: 'ID', count: 8 },
  { _id: 'PH', count: 7 },  { _id: 'GR', count: 7 },
  { _id: 'PL', count: 7 },  { _id: 'FR', count: 7 },
  { _id: 'CA', count: 4 },  { _id: 'BR', count: 4 },
  { _id: 'PE', count: 3 },  { _id: 'PT', count: 3 },
  { _id: 'UY', count: 2 },  { _id: 'RU', count: 2 },
  { _id: 'CO', count: 2 },  { _id: 'US', count: 2 },
  { _id: 'SE', count: 2 },  { _id: 'IE', count: 2 },
  { _id: 'KM', count: 1 },  { _id: 'AF', count: 1 },
  { _id: 'UZ', count: 1 },  { _id: 'EC', count: 1 }
]
Type "it" for more


# $sort, $limit, $unset
```js
db.companies.aggregate([ 
    { $match: { rating: { $gte: 50 } } }, 
    { $sort: {rating: 1}},
    {$limit: 10},
    {$unset: "field_name"}
])
```
**$unset means directly show the output**
**Output->**
[
  {
    _id: ObjectId("61ff7cbb84fad959b1793c39"),
    id: 510,
    company_name: 'Photojam',
    email: 'dgiraudele5@globo.com',
    rating: 50,
    location: '579 Goodland Street',
    origin_country: 'JP'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793db8"),
    id: 893,
    company_name: 'Wordify',
    email: 'aodeaos@google.com.hk',
    rating: 50,
    location: '1244 Elka Place',
    origin_country: 'BR'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793dfc"),
    id: 961,
    company_name: 'Kwilith',
    email: 'aantonacciqo@google.nl',
    rating: 50,
    location: '0 Jenifer Parkway',
    origin_country: 'CN'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793c68"),
    id: 557,
    company_name: 'Linkbuzz',
    email: 'marstingallfg@so-net.ne.jp',
    rating: 50,
    location: '449 Hoffman Junction',
    origin_country: 'RU'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793bd4"),
    id: 409,
    company_name: 'Aibox',
    email: 'handersonbc@goo.gl',
    rating: 50,
    location: '4361 Macpherson Avenue',
    origin_country: 'PH'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793dda"),
    id: 927,
    company_name: 'Voonix',
    email: 'imcsheepq@freewebs.com',
    rating: 50,
    location: '814 Superior Place',
    origin_country: 'UA'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793d8c"),
    id: 849,
    company_name: 'Divape',
    email: 'flanglandsnk@theglobeandmail.com',
    rating: 50,
    location: '3 Oak Center',
    origin_country: 'FR'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793d36"),
    id: 763,
    company_name: 'Divavu',
    email: 'jdoulll6@si.edu',
    rating: 50,
    location: '70056 Riverside Hill',
    origin_country: 'NL'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793a67"),
    id: 44,
    company_name: 'Wordtune',
    email: 'cgollin17@joomla.org',
    rating: 51,
    location: '0127 8th Terrace',
    origin_country: 'CZ'
  },
  {
    _id: ObjectId("61ff7cbb84fad959b1793d50"),
    id: 789,
    company_name: 'Browsecat',
    email: 'hcoleshilllw@lycos.com',
    rating: 51,
    location: '53417 Buhler Avenue',
    origin_country: 'FR'
  }
]


# directly by $group without $match
```js
db.companies.aggregate([{$group: {_id:"$origin_country"}},{$limit: 2}])    
```
**Output->**
[ { _id: 'AF' }, { _id: 'AL' } ]


# Notes of Mongo- Array update queries and Aggregation Piplines

#Array Update Operators

**$push**
The $push operator appends a specified value to an array.

The $push operator has the form:

Starting in MongoDB 5.0, update operators process document fields with string-based names in lexicographic order. Fields with numeric names are processed in numeric order. See Update Operators Behavior for details.

If the field is absent in the document to update, $push adds the array field with the value as its element.

If the field is not an array, the operation will fail.

If the value is an array, $push appends the whole array as a single element. To add each element of the value separately, use the $each modifier with $push. For an example, see Append Multiple Values to an Array. For a list of modifiers available for $push, see Modifiers.

Starting in MongoDB 5.0, mongod no longer raises an error when you use an update operator like $push with an empty operand expression ( { } ). An empty update results in no changes
```js
db.students.insertOne( { _id: 1, scores: [ 44, 78, 38, 80 ] } )

db.students.updateOne(
   { _id: 1 },
   { $push: { scores: 89 } }
)
Append Multiple Values to an Array
Use $push with the $each modifier to append multiple values to the array field.

The following example appends each element of [ 90, 92, 85 ] to the scores array for the document where the name field equals joe:

db.students.updateOne(
   { name: "joe" },
   { $push: { scores: { $each: [ 90, 92, 85 ] } } }
)
```
Use $push Operator with Multiple Modifiers
Add the following document to the students collection:
```js
db.students.insertOne(
   {
      "_id" : 5,
      "quizzes" : [
         { "wk": 1, "score" : 10 },
         { "wk": 2, "score" : 8 },
         { "wk": 3, "score" : 5 },
         { "wk": 4, "score" : 6 }
      ]
   }
)
```
The following $push operation uses:

the $each modifier to add multiple documents to the quizzes array,
the $sort modifier to sort all the elements of the modified quizzes array by the score field in descending order, and
the $slice modifier to keep only the first three sorted elements of the quizzes array.
```js
db.students.updateOne(
   { _id: 5 },
   {
     $push: {
       quizzes: {
          $each: [ { wk: 5, score: 8 }, { wk: 6, score: 7 }, { wk: 7, score: 6 } ],
          $sort: { score: -1 },
          $slice: 3
       }
     }
   }
)
```
After the operation only the three highest scoring quizzes are in the array:


{
  "_id" : 5,
  "quizzes" : [
     { "wk" : 1, "score" : 10 },
     { "wk" : 2, "score" : 8 },
     { "wk" : 5, "score" : 8 }
  ]
}

**$pull**
The $pull operator removes from an existing array all instances of a value or values that match a specified condition.

The $pull operator has the form:

{ $pull: { <field1>: <value|condition>, <field2>: <value|condition>, ... } }

To specify a in an embedded document or in an array, use dot notation.

Starting in MongoDB 5.0, update operators process document fields with string-based names in lexicographic order. Fields with numeric names are processed in numeric order. See Update Operators Behavior for details.

If you specify a and the array elements are embedded documents, $pull operator applies the as if each array element were a document in a collection. See Remove Items from an Array of Documents for an example.

If the specified to remove is an array, $pull removes only the elements in the array that match the specified exactly, including order.

If the specified to remove is a document, $pull removes only the elements in the array that have the exact same fields and values. The ordering of the fields can differ.
```js
db.stores.insertMany( [
   {
      _id: 1,
      fruits: [ "apples", "pears", "oranges", "grapes", "bananas" ],
      vegetables: [ "carrots", "celery", "squash", "carrots" ]
   },
   {
      _id: 2,
      fruits: [ "plums", "kiwis", "oranges", "bananas", "apples" ],
      vegetables: [ "broccoli", "zucchini", "carrots", "onions" ]
   }
] )
```
The following operation removes

"apples" and "oranges" from the fruits array
"carrots" from the vegetables array
```js
db.stores.updateMany(
    { },
    { $pull: { fruits: { $in: [ "apples", "oranges" ] }, vegetables: "carrots" } }
)
```
Result:

{
  _id: 1,
  fruits: [ 'pears', 'grapes', 'bananas' ],
  vegetables: [ 'celery', 'squash' ]
},
{
  _id: 2,
  fruits: [ 'plums', 'kiwis', 'bananas' ],
  vegetables: [ 'broccoli', 'zucchini', 'onions' ]
}
Remove All Items That Match a Specified $pull Condition
Create the profiles collection:
```js
db.profiles.insertOne( { _id: 1, votes: [ 3, 5, 6, 7, 7, 8 ] } )
```
The following operation will remove all items from the votes array that are greater than or equal to ( $gte ) 6:
```js
db.profiles.updateOne( { _id: 1 }, { $pull: { votes: { $gte: 6 } } } )
```
After the update operation, the document only has values less than 6:

{ _id: 1, votes: [ 3, 5 ] }

**$pop**
The $pop operator removes the first or last element of an array. Pass $pop a value of -1 to remove the first element of an array and 1 to remove the last element in an array.

The $pop operator has the form:

{ $pop: { <field>: <-1 | 1>, ... } }
```js
db.students.insertOne( { _id: 1, scores: [ 8, 9, 10 ] } )
```
```js
db.students.updateOne( { _id: 1 }, { $pop: { scores: -1 } } )
```
// { _id: 1, scores: [ 9, 10 ] }
```js
db.students.updateOne( { _id: 10 }, { $pop: { scores: 1 } } )
```
// { _id: 10, scores: [ 9 ] }
**$identifier**
https://docs.mongodb.com/manual/reference/operator/update/positional-filtered/#mongodb-update-up.---identifier--
**$[]**
https://docs.mongodb.com/manual/reference/operator/update/positional-all/

**$addToSet**
https://docs.mongodb.com/manual/reference/operator/update/addToSet/
Update Array modifiers
Update Operator Modifiers:

**$each**
Modifies the $push and $addToSet operators to append multiple items for array updates.
    
**$position**
Modifies the $push operator to specify the position in the array to add elements.
    
**$slice**
Modifies the $push operator to limit the size of updated arrays.

**$sort**
Modifies the $push operator to reorder documents stored in an array.
$sort The $sort modifier orders the elements of an array during a $push operation.

To use the $sort modifier, it must appear with the $each modifier. You can pass an empty array [] to the $each modifier such that only the $sort modifier has an effect.

{
  $push: {
     <field>: {
       $each: [ <value1>, <value2>, ... ],
       $sort: <sort specification>
     }
  }
}
```js
db.students.insertOne(
   {
     "_id": 1,
     "quizzes": [
       { "id" : 1, "score" : 6 },
       { "id" : 2, "score" : 9 }
     ]
   }
)
```
```js
db.students.updateOne(
   { _id: 1 },
   {
     $push: {
       quizzes: {
         $each: [ { id: 3, score: 8 }, { id: 4, score: 7 }, { id: 5, score: 6 } ],
         $sort: { score: 1 }
       }
     }
   }
)
```

{
  "_id" : 1,
  "quizzes" : [
     { "id" : 1, "score" : 6 },
     { "id" : 5, "score" : 6 },
     { "id" : 4, "score" : 7 },
     { "id" : 3, "score" : 8 },
     { "id" : 2, "score" : 9 }
  ]
}
Sort Array Elements That Are Not Documents
```js
db.students.insertOne( { "_id" : 2, "tests" : [  89,  70,  89,  50 ] } )
```

The following operation adds two more elements to the scores array and sorts the elements:
```js
db.students.updateOne(
   { _id: 2 },
   { $push: { tests: { $each: [ 40, 60 ], $sort: 1 } } }
)
```
Update Array Using Sort Onlyicons/link.png
```js
db.students.updateOne(
   { _id: 3 },
   { $push: { tests: { $each: [ ], $sort: -1 } } }
)
```
The following $push operation uses:
the $each modifier to add multiple documents to the quizzes array, the $sort modifier to sort all the elements of the modified quizzes array by the score field in descending order, and the $slice modifier to keep only the first three sorted elements of the quizzes array.
```
db.students.insertOne(
   {
      "_id" : 5,
      "quizzes" : [
         { "wk": 1, "score" : 10 },
         { "wk": 2, "score" : 8 },
         { "wk": 3, "score" : 5 },
         { "wk": 4, "score" : 6 }
      ]
   }
)
```
```js
db.students.updateOne(
   { _id: 5 },
   {
     $push: {
       quizzes: {
          $each: [ { wk: 5, score: 8 }, { wk: 6, score: 7 }, { wk: 7, score: 6 } ],
          $sort: { score: -1 },
          $slice: 3
       }
     }
   }
)
```

{
  "_id" : 5,
  "quizzes" : [
     { "wk" : 1, "score" : 10 },
     { "wk" : 2, "score" : 8 },
     { "wk" : 5, "score" : 8 }
  ]
}
#Aggregation
Aggregation operations process multiple documents and return computed results

**Aggregation Pipeline Stages**
An aggregation pipeline consists of one or more stages that process documents:

Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, and calculate values. The documents that are output from a stage are passed to the next stage. An aggregation pipeline can return results for groups of documents. For example, return the total, average, maximum, and minimum values.

In the db.collection.aggregate() method and db.aggregate() method, pipeline stages appear in an array. Documents pass through the stages in sequence.

**Pipeline**

**$match**
Filters the documents to pass only the documents that match the specified condition(s) to the next pipeline stage.

The $match stage has the following prototype form:

{ $match: { <query> } }

$match takes a document that specifies the query conditions.

Place the $match as early in the aggregation pipeline as possible. Because $match limits the total number of documents in the aggregation pipeline, earlier $match operations minimize the amount of processing down the pipe.

**$group**
Groups input documents by the specified _id expression and for each distinct grouping, outputs a document. The _id field of each output document contains the unique group by value. The output documents can also contain computed fields that hold the values of some accumulator expression.
```js
{
  $group:
    {
      _id: <expression>, // Group By Expression
      <field1>: { <accumulator1> : <expression1> },
      ...
    }
 }
```
Accumulator Operator

$accumulator Returns the result of a user-defined accumulator function.

$sum Returns a sum of numerical values. Ignores non-numeric values.

$max Returns the highest expression value for each group.

$avg Returns an average of numerical values. Ignores non-numeric values.

$count Returns the number of documents in a group. Distinct from the $count pipeline stage.

Others

**$project**
Passes along the documents with the requested fields to the next stage in the pipeline. The specified fields can be existing fields from the input documents or newly computed fields.

```js
db.books.aggregate( [ { $project : { title : 1 , author : 1 } } ] )
```
Conditionally Exclude Fields You can use the variable REMOVE in aggregation expressions to conditionally suppress a field.

Consider a books collection with the following docume
```js
db.books.aggregate( [
   {
      $project: {
         title: 1,
         "author.first": 1,
         "author.last" : 1,
         "author.middle": {
            $cond: {
               if: { $eq: [ "", "$author.middle" ] },
               then: "$$REMOVE",
               else: "$author.middle"
            }
         }
      }
   }
] )
```
Others
**$sort
$sortByCount
$limit
$sum
$lookup
$cond
$expr**
Aggregation Pipeline Operators

Class notes


```js
db.posts.insertMany([
{
"title": "Masai 101",
"author_id": "1",
"author": "Albert",
"comments": [
{
"body": "Hello",
"author": "Sid",
"author_id": "2"
},
],
"tags": ["101", "Programming",]
},
{
"title": "Masai 102",
"author_id": "3",
"author": "Nrupul",
"comments": [
{
"body": "Hello",
"author": "Albert",
"author_id": "1"
},
],
"tags": ["101",]
},
])
```
#
```js
db.students.updateOne( { name: "Albert" }, { $push: { scores: { $each: [ 35, 100, 45 ], $sort: -1 , $slice: 3 } } } )
```
```js          
db.posts.updateOne( { title: "Masai 102" }, { $pull: { tags: "101"}} )
```
# $push

- pushing single values
- pushing multiple values - $each
- with modifiers, like sort, slice
- pull
  - pull a particular value
  - many with condition
  - use $in with pull
- $pop
  - -1, 1
- $addToSet

- Problem to try:
- sort without adding or removing anything to an array

# Aggregations
```js
db.users.aggregate(
[
{
$match
},
]
)

<!-- match -->

db.users.find( { } )

aggreate all companies and mathc where rating is gte 90

db.companies.aggregate( [ { $match: { rating: { $gte: 90 } } }])

# $group

db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } } }])

db.companies.aggregate([ { $match: { rating: { $gte: 90 }, origin_country: "CA" } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 }, average: { $avg: "$rating" } } }])

db.companies.aggregate([ { $match: { origin_country: "CA" } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 }, average: { $avg: "$rating" } } }])

db.companies.aggregate([
{ $match: { origin_country: "CA" } },
{ $limit: 5 } ,
{ $group: { _id: "$origin_country", count: { $sum: 1 }, average: { $avg: "$rating" } } }]
)

db.companies.aggregate([
{ $match: { origin_country: "CA" } },
{ $sort: { rating: -1 } },
{ $limit: 5 } ,
{ $group: { _id: "$origin_country", count: { $sum: 1 }, average: { $avg: "$rating" } } }]
)

db.companies.aggregate([
{ $match: { origin_country: "CA" } },
{ $sort: { rating: -1 } },
{ $limit: 5 } ,
{ $project: { _id: 0, location: 0 } }
]
)

<!-- sortByCount -->

db.companies.aggregate([
{ $match: { rating: {$gte: 50 } },
{ $sortByCount: "$rating" }
]
)

db.companies.aggregate([
{ $match: { rating: {$gte: 50 } },
{ $sort: { rating: 1 },
{ $limit: 10 },
{ $unset: "field_name" }
]
)
```
1. use match to filter out some data in aggregation
2. use group along with it to group by country, and count of results in each country
3. show average rating, max rating, min rating as well,
4. sort by rating in descending order, limit to top 10 results
5. project to only results where country belongs to US , do not show \_id, location
6. use sortByCount to show all companies grouped by country, and counts
