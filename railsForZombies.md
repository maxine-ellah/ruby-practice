DEEP IN THE CRUD

Rails docs: http://guides.rubyonrails.org/index.html

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

Validations

When we write 'Tweet' with a capital t anywhere in our app, we are referencing the 'tweet' model. what gets returned is an instance of the class which has the values for which ever tweet we've specified. the 'Tweet' class is a model.

if you want to specify that the :status column always has a value when a new zombie is created you can use a validator like this:

class Tweet < ActiveRecord::Base
  validates_presence_of :status
end

If you now tried to create a new zombie with an empty :status value and did t.save, it would evaluate to false.

t = Tweet.new
t.save => false

to find out what went wrong, you can use the .errors method on the class e.g. which returns a hash with the value of status to be the error message.

t.errors.messages => {status: ["can't be blank"]}

Rails has HEAPS of built-in validators

You can also use a different syntax for writing validations:

validates :status, presence: true - this also checks if the :status column has a value

validates :status, presence: true, length: { minimum: 3 }

Relationships

You could create a zombies table to store each individual zombie and its details, then, add a new column to the tweets table called 'zombie_id'.

Then, with two tables now, you'd need to define the relationship between them. You'd need to express that in the tweets table a tweet 'BELONGS_TO' a zombie. You'd put this info in the model of the tweet:

class Tweet < ActiveRecord::Base

  belongs_to :zombie

end

And in the other direction, you'd also need to specify that a zombie 'HAS MANY' tweets, like this:

class Zombie < ActiveRecord::Base

  has_many :tweets

end


Symbols:

similar in use to keys in key value pairs.

To read a specific value from a hash :
books[:key] => value
