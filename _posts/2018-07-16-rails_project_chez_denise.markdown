---
layout: post
title:      "Rails Project: Chez Denise"
date:       2018-07-16 16:57:05 -0400
permalink:  rails_project_chez_denise
---


For my rails project I created a website for a restaurant called Chez Denise. A user can visit the website  and order meals from the menu. It's also possible for a user to write a review about the restaurant.  

![](https://i.imgur.com/qs6tJLy.png)

I have five different models: User, Trip, Order, Meal, and Review. 

The reason I chose this as my project is because I wanted to build something that can actually be used in the real world, as well as something that I can continue to work on and update over time as my skillset grows.

A User has many Trips(to the restaurant) and also has many Reviews.  A Trip belongs to a user, has many Orders and has many Meals through Orders. The Order model creates the join between the Meal and the Trip and also has an Order date.  A Meal has many Orders and has many Trips through Orders.  


When I initially built this I only had 3 models, User, Order, and Meals.  However, I quickly realized that in order to keep track of every visit a User makes to the restaurant I would need another model, a Trip model. This allows me to group the number of Meals ordered on each Trip a User makes to the restaurant.  So, in case a User makes three Trips to the restaurant in a day, I can aggregate the Meals ordered for each Trip.  I can keep track of every single visit, every order placed, and every Meal per Trip.

A User can also leave a Review.  By doing this I am able to create nested routes that allow me to link the User to his or her own personal Review creation, edit, index, and show pages.  

```
resources :users, only: [:new, :show] do
      resources :reviews, only: [:new, :edit, :show, :index]
   end
```


This leads to the following routes:



![](https://i.imgur.com/I2YyqSp.png)

![](https://i.imgur.com/1cpG2ZU.png)

![](https://i.imgur.com/iB4gKhD.png)

![](https://i.imgur.com/EoVLa1A.png)


In addition to the typical method of authentication I also used Omniauth which allows Users to log in via Facebook.  I initially ran into trouble when I tried to use Omniauth.  The problem was that because the authentication is occuring via Facebook, the User validations I created were not being met so any user created through Facebook was not being saved.  I was able to overcome this by adding the following line of code to my authentication:

![](https://i.imgur.com/QTbSFAB.png)

This allows validations to be bypassed if omniauth is used.  


I made use of the Faker gem for this project.  This gem allowed me to seed the database with a list of fake users so I wouldn't need to create new users everytime.  My blog post about the Faker gem is [here.](http://deniseonaquest.com/do_you_know_about_the_faker_gem)

I learned quite a bit while creating this project.  Even though I feel like I learned alot and feel comfortable creating with Rails I realize there is so much more to learn. I look forward to learning more.
