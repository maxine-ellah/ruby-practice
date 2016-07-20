#DEEP IN THE CRUD

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
Tweet.find(2, 1, 3) - returns array of tweets
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
t.attributes = { status: "I'm a new thing", zombie: "chomper"} - here we send a hash to update multiple rows/columns

t = Tweet.find(2)
t.update(
  status: "I'm a new thing", zombie: "chomper") - .update method automatically saves this to the table.


DELETE:

t = Tweet.find(2).destroy

Tweet.destroy_all - destroys all tweets/rows


#MODELS TASTE LIKE CHICKEN

Validations

When we write 'Tweet' with a capital t anywhere in our app, we are referencing the 'tweet' model. what gets returned is an instance of the class which has the values for which ever tweet we've specified. the 'Tweet' class is a model.

if you want to specify that the :status column always has a value when a new zombie is created you can use a validator like this:

class Tweet < ActiveRecord::Base
  validates_presence_of :status
end

If you now tried to create a new zombie with an empty :status value and did t.save, it would evaluate to false.

t = Tweet.new
t.save => false

to find out what went wrong, you can use the .errors method on the class e.g. which returns a hash with the value of 'status' to be the error message.

t.errors.messages => {status: ["can't be blank"]}

Rails has HEAPS of built-in validators

You can also use a different syntax for writing validations:

validates :status, presence: true - this also checks if the :status column has a value

validates :status, presence: true, length: { minimum: 3 } - this does the same as above and checks that the length is at least 3.

Relationships

You could create a zombies table to store each individual zombie and its details, then, add a new column to the tweets table called 'zombie_id'.

Then, with two tables now, you'd need to define the relationship between them. You'd need to express that in the tweets table a tweet 'BELONGS_TO' a zombie. You'd put this info in the Tweet model:

class Tweet < ActiveRecord::Base

  belongs_to :zombie

end

And in the other direction, you'd also need to specify that a zombie 'HAS MANY' tweets, like this:

class Zombie < ActiveRecord::Base

  has_many :tweets

end

Below, the number of tweets belonging to a zombie is accessed by simply writing zombie.tweets.count. The tweets table doesn't need to be looped through because in our Zombie class we've said that a zombie 'HAS_MANY' tweets.

<ul>
  <% zombies.each do |zombie| %>
    <li>
      <%= zombie.name %>
      <% if zombie.tweets.count >1 %>
      <p>SMART ZOMBIE</p>
      <% end %>
    </li>
  <% end %>
</ul>


#THE VIEWS AIN'T ALWAYS PRETTY

When ruby is used inside HTML it is written in a file that usually looks like this: filename.html.erb - .erb stands for Embedded Ruby

These : <% %> tell our app that anything written inside of them this is ruby code and that we want it to be exectuted.

This code will find the tweet with ID 1

<% tweet = Tweet.find(1) %>

But to print anything we need to write this:

<h1><%= tweet.status %></h1>

<%= - this tells our app anything written after this is to be printed.

In order to be as DRY as possible, it's encouraged to have any boilerplate HTML in a separate file. By default, Rails creates this file for us called application.html.erb. Every page we create will then use this template by default. You can then keep the code for your views in their own files.

To tell the application template (application.html.erb) to load what we want it to show we use the <%= yield %> command.

Create a link:

<%= link_to tweet.zombie.name, tweet.zombie %>

link_to is a Rails helper method which generates a link for us. The first argument is the link text and the second is the url/path/where we want the link to go.

link to an edit page:

<ul>
  <% zombies.each do |zombie| %>
    <li>
      <%= link_to zombie.name, edit_zombie_path(zombie) %>
    </li>
  <% end %>
</ul>

Looping through:

We can use a code block like this:

<% Tweet.all.each do |tweet| %>
<p><%= tweet.status %></p>
<p><%= tweet.zombie.name %></p>
<% end %>

Tweet - refers to a class which is our model
.all - returns an array of all the tweets
|tweet| - this returns a single tweet from the array

to include links in the loop:

<% Tweet.all.each do |tweet| %>
<p><%= link_to tweet.status, tweet %>
</p>
<p><%= link_to tweet.zombie.name, tweet.zombie %></p>
<% end %>

Conditionals:

<% tweets = Tweet.all %> - set a variable to the tweets
<% tweets.each do |tweet| %> - start a code block which loops through each tweet
<% end %>
<% if tweets.size == 0 %> - this says, 'after looping through the model, if the conditional is true, do this (whatever is inside the conditional)'
<em>No Tweets Found</em>
<% end %>

#CONTROLLERS MUST BE EATEN

Controllers:

This is what a controller looks like in Rails:

class TweetsController < ApplicationController

  def show
    @tweet = Tweet.find(1)
  end

end

Inside the controller, there is a method/action called 'show'. This by default will render out the first tweet to our show.html.erb view. This is good because we shouldn't have this kind of code inside our view, it should be kept in the controller and referenced in the view with the use of the @ symbols which gives the views access to variables.
If you don't want the 'show' view to render out by default you can specify a different view like this:

def show
  @tweet = Tweet.find(1)
  render action: 'status'
end

'render action: 'status'' will render out the 'status' view instead.

###Params

if you want to be able to access a tweet of any ID e.g. by entering a different id in the URL: /tweets/3, /tweets/7 etc. that's fine, you just need to give the 'show' method in the controller the following argument: Tweet.find(params[:id])
Now when you enter a different ID in the URL Rails will generate a new hash called params that looks like this:

####params = { id: "1" }

that will then look up the proper tweet and render it to the view.
