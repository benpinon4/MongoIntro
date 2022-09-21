# MongoIntro
# Part One 
First query of adding 10 blogs

    db.blogs.insertMany(blogsArray)
    - Insert a new blog into the collection
    db.blogs.insert([{
        createdAt: new Date(),
        title: "Balls",
        text: "Voluptates dicta ut dolorem dolor. Tempora animi facilis dignissimos nostrum tempora soluta asperiores. Aut velit aspernatur rerum doloribus. Voluptate non voluptate consequatur. Et qui recusandae aliquid autem consequatur necessitatibus dolore aut.\nSimilique tempora iure magni. Ut eos eveniet amet officiis ab dicta explicabo est eos. Aperiam sunt ullam ipsam iste. Quam dolorum omnis minus sapiente. Est perferendis aut at non aut est ducimus asperiores porro. Rerum et vero perspiciatis ea facere omnis dolor dolores qui.\nNon voluptates aliquam et praesentium vero adipisci odio. Atque et molestias. Accusamus optio suscipit consequatur necessitatibus possimus ut nostrum et pariatur. Aut nam repellendus ut voluptate molestias et sint. Excepturi et ullam dolor ipsa nulla sapiente et. Suscipit quo consequatur voluptatem qui neque voluptatem voluptate ex.",
        author: "Ben Dover",
        lastModified: new Date(),
        categories: ["nulla", "quasi", "beatae"],
        id: "69",
        objectId: 11
    }])

- Find one blog by author

    db.blogs.find({
        author: "Ben Dover"
    }) 

- Find all blogs whose objectId is greater than 5

    db.blogs.find({
        objectId: {$gt: 5}
    })

- Find all blogs whose createdAt is after April 1, 2022

    db.blogs.find({
        createdAt: {$gt: new Date("2022-04-01")}
    })

# Part Two 
- Find all blogs where the field lastModified does not exist and 

    db.blogs.find({
        lastModified: {$exists: false}
    })

- Find all blogs where the createdAt type is a date

    db.blogs.find({
        "createdAt": {$type: "date"}
    })

- Combine the above two queries into one to find all blogs in which lastModified does not exist and createdAt is the type date

    db.blogs.find({
    lastModified: {$exists: false},"createdAt":{$type: "date"}
    })

- Find a blog with a specific phrase (or a more complex regular expression if you want to practice regexs) in the text

    db.blogs.find({
    text: {$regex: /Voluptates dicta ut dolorem dolor/}
    })

- Find all blogs that have "qui" in the categories array

    db.blogs.find({
    categories: {$in:["qui"]}
    })
# Part 3
- Find all blogs in which the lastModified does not exist and set it

    db.blogs.updateMany(
    {lastModified: {$exists:false}},
    {$set:{lastModifed: new Date()}
    })

- From now on, all the following queries should update lastModified to be the current datetime 
    
    db.blogs.updateMany({
        createdAt: {$gt: new Date("05-01-2022")}
    },{$set: {lastModifed: new Date()}
    })

- Find all blogs created after May 2022 and add "lorem" as a new category in the categories array

    db.blogs.updateMany({
        createdAt: {$gt: new Date("05-01-2022")}
    },{$addToSet: {categories: "lorem"}, 
        $set: {lastModifed: new Date()}
    })

- Find all blogs that have the category "voluptas" and pull "voluptas" from the categories
    
    db.blogs.updateMany({
        categories:  {$in: ["voluptas"]}
    },{$addToSet:  {categories: "voluptas"}, 
        $set: {lastModifed: new Date()}
    })

- Find all blogs with "corrupti" in the categories and delete those blogs
    
    db.blogs.deleteMany({
    categories:  {$in: ["corrupti"]}
    }