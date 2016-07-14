Ruby basics

methods:

.reverse - reverses given values e.g. "maxine".reverse => "enixam"
.length - returns length
.max - returns maximum value (e.g. from an array)
.sort - orders values
.values - will return the values in an object
File/directory methods:
.cp(src, dest, options) - will copy files from a specified source to a specified destination
.mtime(src) - returns time object of when file was added
! - putting this at the end of a method will change the original object, not make a new copy with the changes.
to_s converts values to strings.
to_i converts values to integers (numbers.)
to_a converts values to arrays.

40.to_s.reverse => "04"
"maxine" * 5 => "maxinemaxinemaxinemaxinemaxine"

to declare a new variable (no 'var' needed):
ticket = [22, 77, 14]

You can use square brackets to find a value in an object and change it to a different value e.g.
ticket[22] = 'porridge' this will replace 22 with 'porridge' in the array.

Hashes:

A Hash is a dictionary-like collection of unique keys and their values.
they are similar to Arrays, but where an Array uses integers as its index, a Hash allows you to use any object type.

Hashes enumerate their values in the order that the corresponding keys were inserted.

You can create a hash like:

books = {} or
books = Hash.new(0)

books = {"Gravity's Rainbow"=>:splendid, "The Secret History"=>:splendid, "Purple Hibiscus"=>:splendid}

To print a list of the keys in the hash, type books.keys

Symbols:

Tiny, efficiently reusable code words with a colon:

:splendid
:vegetarian
:Indian hot

Blocks:

Chunks of code which can be tacked on to many of Ruby's methods. Here's the code you used to build a scorecard:

books.values.each { |rate| ratings[rate] += 1 }.

and to print something 5 times:
5.times { prin­­t "Odel­­ay!" }

Code blocks can also be instantiated using 'do' and 'end'

This code opens a file in 'append' mode using the "a" parameter and 'do' tells Ruby that the code block is starting and anything inside will be added to the end of the file.
'end' tells Ruby when the code to be added has stopped.
File.open(­"/Home/com­ics.txt", "a") do |f|
..	f << "Cat and Girl:­ http:­//catandgi­rl.com/"
..	end

Dir - Dir has a collection of methods for checking out file directories

Dir.entries "/" - entries is being called on the Dir variable and lists everything in it from the top of the directory (indicated by "/").

Method arguments: Pretty much anything listed after a method such as
Dir.entries "/" - "/" is the argument
print poem - poem is the argument
print "pre", "event", "ual", "ism" -- This bit has several arguments
