Lab 1 part 1
1.  List all people. (200) 
  	A: db.people.find();

2. Count all people. (200).     
	
		A: db.people.find().count()

3. List all people in Arizona. (6)     
		
		A: db.people.find({state: ‘Arizona’}); 
4. List all males in Arizona. 

		A:  db.people.find({state: ‘Arizona’, gender: ‘Male’});
5. List all people in Arizona plus New Mexico.
  
		A: db.people.find( {$or: [{state: ‘Arizona’}, {state: ‘New Mexico’}]} );
6. List all people under age 40. (90)

		A: db.people.find( {age: {$lt: 40}} );
7. List all females in Florida between the ages of 40 and 45 (inclusive). (4)

		A: db.people.find( { age: {$lte: 45, $gte: 40} } ) ;
8. List people whose first name starts with "H". (2)

		A: db.people.find({first_name: /^H/ });
9. List all people in Michigan, sorted by first name. (6)

		A: db.people.find({state: ‘Michigan’}).sort({first_name: 1});
10. List all people who live in Virginia or are named Virginia.

		A: db.people.find({$or: [{state: 'Virginia'}, {first_name: 'Virginia'}]})
11. List the names of people under age 30. Only display their first and last name. (38)

		A: db.people.find( {age: {$lt: 30}}, {first_name: 1, last_name: 1}} );
12. List all people in Montana. Display all information except age. (2)

		A: db.people.find( {state: ‘Montana’},  {age: false});
13. List the email addresses of people with a ".edu" email. Only display the email. (12)

		A: db.people.find( {email: /\.edu$/},{email:1} );

Extended Challenges:

14. Count all people with at least one child under age four. (69)

      A: db.people.find( {'children.age': {$lt: 4}} ).count()

15. List people who have no children. (43)

      A: db.people.find({children: []});

16. List people who have at least one child. (157)

      A: db.people.find({children: {$ne: []}});




MONGODB LAB 1 (PART 2 - DATA MANIPULATION)

Task: Continue to work with the people data from part 1. Add, update, and delete documents as instructed below.

Build Specifications:
Write MongoDB shell commands to do the following tasks. Record these commands in a text document and submit the document. You do not need to record the results of the commands.

In people collection
1: Add a person to the collection. You pick the data, but they should have an empty array for children.

A: db.people.insertOne({first_name: "NewPerson", last_name: "LastName", email: "email@email.com", gender: "Female", age: 30, state: "Arizona", children: []});


2: Add another person. They should have at least two children.

A: db.people.insertOne({first_name: "NewPerson2", last_name: "LastName2", email: "email@email.com", gender: "Male", age: 35, state: "Michigan", children: [{name: "Kid1", age: 4}, {name: "Kid2", age: 8}]});


3: Update one person named Clarence. He moved from North Dakota to South Dakota.

A: db.people.updateOne({first_name: "Clarence"}, {$set: {state: "South Dakota"}});


4: Update Rebecca Hayes. Remove her email address.

A: db.people.updateOne({_id: ObjectId("61f3694f2a1a01cf4a64dbc3")}, {$unset: {email: ""}});


5: Update everyone from Missouri. They all had a birthday today, so add one to their age. (expect 4 matches)

A: db.people.updateMany({state: "Missouri"}, {$inc: {age: 1}});


6: Jerry Baker has updated information. Replace with a new document:
{ first_name: "Jerry", last_name: "Baker-Mendez", email: "jerry@classic.ly", gender:"Male", age: 28, state: "Vermont", "children": [{name: "Alan", age: 18}, {name: "Jenny", age: 3}] }

A: db.people.updateOne({_id: ObjectId("61f3694f2a1a01cf4a64dc11")},{$set: {first_name: "Jerry", last_name: "Baker-Mendez", email: "jerry@classic.ly", gender:"Male", age: 28, state: "Vermont", "children": [{name: "Alan", age: 18}, {name: "Jenny", age: 3}] }});


7: Delete Wanda Bowman.

A: db.people.deleteOne({_id: ObjectId("61f3694f2a1a01cf4a64dbe9")});


8: Delete everyone who does not have an email address specified. (expect 36 matches - maybe more depending what you added above)

A: db.people.deleteMany({email: null});


In submissions collection

9: Add several documents to a new submissions collection. Do it all in one command. (Remember, MongoDB will create the collection for you. Just start adding documents.)
title: "The River Bend", upvotes: 10, downvotes: 2, artist: <ID of Anna Howard>
title: "Nine Lives", upvotes: 7, downvotes: 0, artist: <ID of Scott Henderson>
title: "Star Bright", upvotes: 19, downvotes: 3, artist: <ID of Andrea Burke>
title: "Why Like This?", upvotes: 1, downvotes: 5, artist: <ID of Steven Marshall>
title: "Non Sequitur", upvotes: 11, downvotes: 1, artist: <ID of Gerald Bailey>

A: db.submissions.insertMany([
{title: "The River Bend", upvotes: 10, downvotes: 2, artist: ObjectId("61f3694f2a1a01cf4a64db92")}, 
{title: "Nine Lives", upvotes: 7, downvotes: 0, artist: ObjectId("61f3694f2a1a01cf4a64dbc0")}, 
{title: "Star Bright", upvotes: 19, downvotes: 3, artist: ObjectId("61f3694f2a1a01cf4a64dc43")}, 
{title: "Why Like This?", upvotes: 1, downvotes: 5, artist: ObjectId("61f3694f2a1a01cf4a64dbc9")}, 
{title: "Non Sequitur", upvotes: 11, downvotes: 1, artist: ObjectId("61f3694f2a1a01cf4a64db90")}
]);


10: Add 2 upvotes for "The River Bend".

A: db.submissions.updateOne({title: "The River Bend"}, {$inc: {upvotes: 2}});


11: Add a field round2 = true to all submissions with at least 10 upvotes. (expect 3 matches)

A: db.submissions.updateMany({upvotes: {$gt: 10}}, {$set: {round2: true}});


Extended Challenges:
12: Update Helen Clark. She had a baby! Add a child, name: Melanie, age: 0.

A: db.people.updateOne({_id: ObjectId("61f3694f2a1a01cf4a64dc4e")}, { $push: { children: { name: 'Melanie', age: 0 }} });


13: Joan Bishop has a child named Catherine. She just had a birthday and prefers to go by "Cat". In one query update the child's name to "Cat" and increment her age by one.

A: db.people.updateOne(
{ _id: ObjectId("61f3694f2a1a01cf4a64dc47") },
   { $set:
      {
        'children.3.name': 'Cat'
      },
   $inc:
    {
        'children.3.age': 1
      }
   }
)


14: List all submissions that have more downvotes than upvotes.

A: db.submissions.find({$expr:{$gt:['$downvotes', '$upvotes']}});
