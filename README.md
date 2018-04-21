
# My explanation

Firstly let us assume two simple things:
* This data is not categorized
* The schema is open for change

# This is the current ERD I envision from the schema 
(may be simplified or I may be wrong)
![erd](https://user-images.githubusercontent.com/29495816/39039991-1924eb58-44b9-11e8-99d8-fe02fcd948e1.png)

This tells us that the Gender, Location and Department are seperate categories from each employee, it would prefer working this schema in a nested / indexed ERD and still remain NoSQL. I am purely assuming and not factoring in the cost to create such a DB.

#Proposed ERD

![proposed_erd](https://user-images.githubusercontent.com/29495816/39040800-67a989fe-44ba-11e8-95f3-e574dacd3f9a.png)


This is my proposed ERD. In here we see that each employee has one to one relationship with a Location, one Location has one Department and one Department has one Gender. 

At this point, lets also assume two key factors : 
* Each employee cannot have more than one work locations and department 
* All employee regardless of both gender are sharing the same survey score as long as they are in the same location and department.
* Scores are placed in child element of Location (Department / Gender)

I would also envision the schema for each employee to look like this... 

```

  {
  "Driver":  {
    1,
    "employee": {
      "number" : {increment logic}
      "required" : ["Department" , "Location" ,"Gender" , Survey Score],
      "properties" : {
        "Location": {
          "type": "object",
          "oneOf": [
              { "Singapore" },
              { "Malaysia" },
              { "Vietnam" }
          ]
            "Department": { 
              "type": "object",
              "oneOf": [
                  { "IT" },
                  { "Sales" },
                  { "Marketing" }
            scores: [
              { value: 5, answer_count: 16}
            ]
        }
      }
    }
  }

```

For this I think we may not need a user account to collect all the surveys, we just need two things from the user, an enumerable value to ask the user his/her location and department also to ensure absolute anonymity (being anonymous). For now we are done with the user. 

# Answer first question. How can we transform this json schema to fulfil the report 1 and report 2.

### At this point lets use my envisioned schema for abit.


To flexible access any data, we just need to access it with this basic logic (no code).
Example from report 1 and report 2

* report 1: Take location, take all scores from IT, Marketing and Sales as long as its the same location

```
push thses scores in an array
all the department scores / number
```
and display / parse into views if needed.

Visa Versa for Vietnam and Malaysia.

* report 2: Same concept for Location score, since Department and Gender is a child element of Location, we can direct access.

On a related note, we could also have more flexibility in this way. We can also say we just want all IT department scores for three locations, we just need to access all the IT department survey scores for all countries and do and divison with Location.length . 

For good user experience , we can also compare in a chart how the same department(lets say IT) survey score differencate in various countries. (assuming that the client is a large company)

We may not need to do this in schema / databse, just in views.

I have not touched with the gender survey scores, but if we want we can simply place it into the schema as a sibling element with Department, just felt that as a web application design POV we may not need to have different survey for different genders.

It may look like this with gender included

```
  {
  "Driver":  {
    1,
    "employee": {
      "number" : {increment logic}
      "required" : ["Department" , "Location" ,"Gender" , Survey Score],
      "properties" : {
        "Location": {
          "type": "object",
          "oneOf": [
              { "Singapore" },
              { "Malaysia" },
              { "Vietnam" }
          ]
            "Department": { 
              "type": "object",
              "oneOf": [
                  { "IT" },
                  { "Sales" },
                  { "Marketing" }
              ]
            "Gender": { 
              "type": "object",
              "oneOf": [
                  { "Male" },
                  { "Female" },
              ]
            }
          scores: [
              { value: 5, answer_count: 16}
          ]  
        }
      }
    }
```
# Using old schema

If we are using the old schema to fulfil report 1, we may do something like this.  

For example: 

App.location === "singapore"

Push all the Singapore location with driver 1 in arrays. Then Javascript reduce (add) the scores and divide it with the array.length. 

Same for all drivers and Locations.

to fulfil report 2, we may do something similar.

Under Driver 1, use if else condition for various conditions namely Location, Department and Gender. 

For example: 

All App.location scores, 
push in array
add up the numbers in array / array.length

All App.department scores, 
push in array
add up the numbers in array / array.length

All App.gender scores, 
push in array
add up the numbers in array / array.length

Lastly,
Push all these integers in array / 3



# What is the complexity (Big O) of your solution?
I will aim for O(1) — Constant Time

As long as you know the key value, which in this case an enum, just access the value.

# Is it the best way to store scores?
In my opinion, maybe not. Actually for this survey instance, I would prefer a relational database. It more organized and users will not have a chance to input an extra field since the question doesn,t matter, we care only on the scores. My suggested schema I wrote above is my small opinion that it may be a slightly better way. I aim to show a variety of data to my users if I am developing such an app.

# If we use NoSQL database to persist the scores, which data store should we use? How does it help?

I would use MongolDB using the Embedding method. It give great support for one-to one relationships(for this instance), sub-documents are small in size, child-data "belongs-to" parent data and hierchical structures are quite visible in the schema.  

# How do you organize the scores base on different attributes, driver (and questions)

As mention, the application will ask for user to select its Location and Deprtment, my vision for such an app is such that there will be a redirection to different views / question sheet based on the users location and department. The scores will be saved such that this is the score where user location = "" and user department = "" .
