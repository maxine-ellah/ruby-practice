DEEP IN THE CRUD

Tables in Rails

Accessing tables:

When we use the word 'Tweet', uppercase and singular, that allows us to access the 'tweets' table which is lowercase and plural.

when getting values from a table we use the dot syntax e.g.
t = Tweets.find(3) - this returns the row with the ID 3
t.status - returns the value under the 'status' column
t.name - returns value in 'name' column

CREATE:

note: Rails sets ID values for you (like table.increment in knex)

t = Tweet.new - creates a new row in the table for our zombie
t.status = "I love brains" - sets the value in 'status' column
t.save - saves it in table.

Alternate way to CREATE:

t = Tweet.new(status: "I love Brains", zombie: "jim")
t.save

or

Tweet.create(status: "I love brains", zombie: "jim")

READ:

Tweet.find(2)
Tweet.find(2, 1, 3) -returns array of tweets
Tweet.first - returns first tweet/row
Tweet.last - same as above, but last tweet/row
Tweet.all - returns all tweets/rows
Tweet.count
Tweet.order
Tweet.limit(10)
Tweet.all(name: 'maxine')

Chaining methods together can be done just like in knex:

Tweet.where(zombie: 'max').order(:status).limit(10)



UPDATE:

t = Tweet.find(2) - finds tweet/row with id 2
t.zombie = "chomper" - sets zombie value/column to "chomper"
t.save

Alternate way to UPDATE:

To set multiple attributes:
t = Tweet.find(2)
t.attributes = { status: "I'm a new thing", zombie: "chomper"} - here we send a hash

t = Tweet.find(2)
t.update(
  status: "I'm a new thing", zombie: "chomper") - .update method automatically saves this to the table.


DELETE:

t = Tweet.find(2).destroy

Tweet.destroy_all - destroys all tweets/rows


MODELS TASTE LIKE CHICKEN



Symbols:

similar in use to keys in key value pairs.

To read a specific value from a hash :
books[:key] => value
