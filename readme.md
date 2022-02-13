
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

- db.posts.insertMany([
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

# - db.posts.find()

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

#  - db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: "102"}})

**Shows->**
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

# - db.posts.find()

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

# - db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: ["103","Programming"]}})

- db.posts.find()

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

# - db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: {$each: ["103","Programming"]}}})

- db.posts.find()

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

#  - db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: {$each: ["104","204"], $sort: {tags: -1},$slice: 3}}})

- db.posts.find()

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


# -  db.posts.updateOne({_id: ObjectId("6208b9397092788c90649a17")},{$push: {tags: {$each: ["104","204"], $sort: {tags: 1},$slice: 3}}})

- db.posts.find()

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

- db.students.insertMany([ { name: "Satya", scores: [96,94,70,67,64] }])   

**Output->**
{
  acknowledged: true,
  insertedIds: { '0': ObjectId("6208bd817092788c90649a18") }
}

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 96, 94, 70, 67, 64 ]
  }
]


# 6. $push - $each - $sort, $slice with the help of updateOne.

# -  db.students.updateOne({ name: "Satya" }, { $push: { scores: {$each: [35, 100, 45], $sort: { scores: -1 }, $slice: 3 }} })

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 96, 94, 70 ]
  }
]

# 7. $sort directly apply in push operation and it gives the correct answer.

# - db.students.updateOne({ name: "Satya" }, { $push: { scores: {$each: [35, 100, 45], $sort: -1 , $slice: 3 }} })

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 96, 94 ]
  }
]


# 8. with collections posts.

# - db.posts.updateOne({title: "Masai 101"},{$push: {comments: {$each: [{body: "Hi", author: "Satya",author_id: "1" },{body: "Nice!",author: "Sid", author_id: "2"}]}}})

- db.posts.find({})

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

# - db.posts.updateOne({title: "Masai 102"},{$pull: {tags: "101"}})

- db.posts.find({})

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

# - db.posts.updateOne({title: "Masai 101"},{$pull: {tags: {$in: ["101","Programming"]}}})

- db.posts.find({})

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

# - db.posts.updateOne({title: "Masai 102"},{$pull: {tags: {$in: [["103","Programming"]]}}})

- db.posts.find({})

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

# - db.students.update({},{$pull: {scores:{$lte: 96}}})

-  db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100 ]
  }
]


# 11. $push - $each - $sort

# - db.students.update({},{$push: {scores:{$each: [100,50,30,20],$sort: -1}}}) 

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 100, 50, 30, 20 ]
  }
]


# 12. $pop.

# -  db.students.update({},{$pop: {scores: 1}}) 
- Last element will be remove like stack.

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 100, 50, 30 ]
  }
]

# - db.students.update({},{$pop: {scores: -1}})
- Remove from front like queue.

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30 ]
  }
]



# 13. $addToSet with updateMany.

# - db.students.updateMany({},{$addToSet: {scores: 90}})

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90 ]
  }
]

# - Add again with the set don't  added.

- db.students.updateMany({},{$addToSet: {scores: 90}})

-  db.students.find()

**Output->Still as previous**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90 ]
  }
]

# - 100 is already present but it not add 100 once again.

- db.students.updateMany({},{$addToSet: {scores: 100}})

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90 ]
  }
]


# 14. $addToSet - $each with updateMany.

# - db.students.updateMany({},{$addToSet: {scores: {$each: [100,50,99,80]}}})

- db.students.find()

**Output->**
[
  {
    _id: ObjectId("6208bd817092788c90649a18"),
    name: 'Satya',
    scores: [ 100, 50, 30, 90, 99, 80 ]
  }
]

# - db.students.updateMany({},{$push: {scores: {$each: [100,50,99,80]}}})

- db.students.find()

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


# - db.companies.aggregate([{$match: {rating: {$gte: 90}}}])

- aggregate all companies and match where rating is greater than 90



# $group

- db.companies.aggregate([
    { $match: { rating: {$gte: 90}}},
    { $group: {
        _id: {origin_country: "TH"}
    }}
])

**Output->**
[ { _id: { origin_country: 'TH' } } ]

- db.companies.aggregate([
    { $match: { rating: {$gte: 90}}},
    { $group: {
        _id: "$origin_country",
        count: {$sum: 1}
    }}
])

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


 - db.companies.find({origin_country: "CN",rating: {$gte: 90}}).count()
**Output->** 19

 - db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 1 } } }])

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


- db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } } }])

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


 - db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } , average: {$avg: "$rating"}}}])

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

- db.companies.aggregate([ { $match: { rating: { $gte: 90 } } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } , average: {$avg: "$rating"},max: {$max: "$rating"},min: {$min: "$rating"}}}])

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


- db.companies.aggregate([ { $match: {origin_country: "CA" } }, { $group: { _id: "$origin_country", count_abc: { $sum: 2 } , average: {$avg: "$rating"}}}])      

**Output->**
[ { _id: 'CA', count_abc: 34, average: 56.8235294117647 } ]


# $limit :

-db.companies.aggregate([ { $match: {origin_country: "CA" } },{$limit: 10},{ $group: { _id: "$origin_country", count: { $sum: 1 } , average: {$avg: "$rating"}}}])

**Output->**
[ { _id: 'CA', count: 10, average: 48.5 } ]


# $sort and $limit :

-db.companies.aggregate([ { $match: {origin_country: "CA" } },{$sort: {rating: -1}},{$limit: 10},{ $group: { _id: "$origin_country", count: { $sum: 1 } , average: {$avg: "$rating"}}}])

**Output->**
[ { _id: 'CA', count: 10, average: 78.7 } ]


# .explain()

-db.companies.aggregate([ { $match: {origin_country: "CA" } },{$sort: {rating: -1}},{$limit: 10},{ $group: { _id: "$origin_country", count_abc: { $sum: 1 } , average: {$avg: "$rating"}}}]).explain()

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
-db.companies.createIndex({origin_country: 1})

**Output->**
origin_country_1


# getIndexes:
-db.companies.getIndexes()

**Output->**
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { origin_country: 1 }, name: 'origin_country_1' }
]


# $group :

- db.companies.aggregate([
    { $match: {origin_country: "CA"}},
    { $sort: {rating: -1}},
    { $limit: 5}
])

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

- db.companies.aggregate([ { $match: { origin_country: "CA" } }, { $sort: { rating: -1 } }, { $limit: 5 },{$project: {_id: 0, location: 0}}])

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

- db.companies.aggregate([
    { $match: {origin_country: "CA"}},
    { $sortByCount: "rating"}
])

-b.companies.aggregate([ { $match: { origin_country: "CA" } }, { $sortByCount: "$rating" }])

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

-db.companies.aggregate([ { $match: { origin_country: "CA" } }, { $sortByCount: "$origin_country" }])
**Output->** [ { _id: 'CA', count: 17 } ]

- db.companies.aggregate([ { $match: { rating: {$gte: 90} } }, { $sortByCount: "$origin_country" }])

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
-db.companies.aggregate([ 
    { $match: { rating: {$gte: 90} } }, 
    { $group: {_id: "$origin_country", count: {$sum: 1}}},
    { $sort: {count: -1}}
])

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

- db.companies.aggregate([ 
    { $match: { rating: { $gte: 50 } } }, 
    { $sort: {rating: 1}},
    {$limit: 10},
    {$unset: "field_name"}
])

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

-db.companies.aggregate([{$group: {_id:"$origin_country"}},{$limit: 2}])    

**Output->**
[ { _id: 'AF' }, { _id: 'AL' } ]