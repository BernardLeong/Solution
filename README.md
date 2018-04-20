
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


