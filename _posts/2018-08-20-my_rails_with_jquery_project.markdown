---
layout: post
title:      "My Rails With Jquery Project"
date:       2018-08-20 21:52:38 +0000
permalink:  my_rails_with_jquery_project
---


The purpose of this project was to build upon the previous Rails project and use jquery to make it dynamic.  In order to do this it was necessary to use the rails backend as an api, serialize the data, turn the data into javascript objects, and utilize these objects to display data to the page.  

My rails project was an online restaurant ordering system. This system has a menu and the customer (user) is able to add items to their order, checkout, and write reviews about the restaurant.  There is only one user that is an administrator, the restaurant owner, and the administrator is able to add new meals to the menu and review all customer orders and customer reviews (but cannot make or edit reviews).  When I initially created this online ordering system I wanted it to look very clean and uncluttered. By adding jquery to the project I was able to maintain this aesthetic while displaying additional data as needed.  For example, this is the basic menu page: 

![img](https://i.imgur.com/DBIzZrN.png)

Using jquery I am now able to click on each meal on the menu and display the meals description, cost and calorie count. There is also an "ADD" button that can be used to add the meal to the customer's cart.

![img](https://i.imgur.com/hZ7Y6lU.png)

I did the same on the User's show page.  Initially I only had the customer's name and e-mail address. However, when the "Orders" button is clicked a list of all of the customer's previous orders is displayed.  

In order to accmplish this it was necessary to alter the rails controllers for every model to render all of the necessary data in JSON (Javascript Object Notation).  The data was converted to JSON, or serialized using the 'active_model_serializers' gem.  Utilizing jquery I was then able to access this JSON data and manipulate it to create the  required views.  

One of the requirements of this project was to take the serialized data and turn it into Javascript objects and use the objects for data manipulation. By creating these objects I was also able to add methods to the object prototypes which allowed me to use the data in new and exciting ways.  Here is an example of an object I created:  

```
function Meal(id, name, description, calorie_count, price, category) {
      this.id = id;
      this.name = name;
      this.description = description;
      this.calorie_count = calorie_count;
      this.price = price;
      this.category = category;
      }

    Meal.prototype.summary = function() {
      return `<h4 style="font-size: 12px;" >` + this.description + "<br> <b>  Calories: </b>" + this.calorie_count + "<br> 
			<b>Price: </b> $" + parseInt(this.price).toFixed(2) + add+ "</h4>";
      };
			
			var newMeal= new Meal(data["id"], data["name"], data["description"], data["calorie_count"], data["price"],  
			data["category"] );
```

Here you can see the Meal constructor function that I used to create the newMeal object. I also created a 'summary' function on the Meal prototype that is used to summarize the data and present the newMeal's description, calorie count, and price as HTML that will then be appended to the DOM. The arguments used to buid the newMeal object( data["name"] and data["description"] ) are response data that is returned from a get request to the api. 

Overall, I really enjoyed building this project because it forced me to learn so much about javascript and jquery, and I also learned many new things abut Rails.  Now that I know how to integrate javascript and jquery into the Rails framework and use Rails as an api there's really no limit to what I can build!
