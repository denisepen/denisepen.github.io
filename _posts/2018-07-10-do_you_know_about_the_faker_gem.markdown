---
layout: post
title:      "Do You Know About The Faker Gem?"
date:       2018-07-10 19:55:55 +0000
permalink:  do_you_know_about_the_faker_gem
---


Everyone knows that when you're getting started on a new project creating seed data to play with initially is a big part of the process.  When creating a new rails project, immediately after getting a model set up I will go into the seeds file and create a few objects with the necessary attributes to test the project as I continue to add functionality.  For example, if I want to create a new user I will do something like this: 

```
User.create!(first_name:  "Denise",
              last_name:  "Owner",
             email: "denise@thebestrestaurant.org",
             password:              "foobar",
             password_confirmation: "foobar",
             admin: true)
```

But I found this great new gem that can automate the creation of new objects for your seed file.  [The Faker gem](https://github.com/stympy/faker).   You can use it to create all sorts of things. A few examples are names, music, movies, food and so many other things.  I used it in my project to create a list of users for my User model:

```
User.create!(10.times do |n|
  first_name  = Faker::Name.first_name
  last_name  = Faker::Name.last_name
  email = "#{first_name}_#{n+1}@gmail.org"
  password = "#{first_name}password"
  User.create!(first_name:  first_name,
                last_name: last_name,
               email: email,
               password:              password,
               password_confirmation: password)
 ```

This code generated ten random users for me with all of the necessary attributes that I needed to test while building my project.  Here are some of the users that this created: 


> Name: Giovanny Friesen 
> Email: Giovanny_1@gmail.org 

> Name: Audra Ledner 
> Email: Audra_2@gmail.org 

> Name: Monique Feeney 
> Email: Monique_3@gmail.org 


This was an incredible time saver and I can see so may uses for this gem in future projects.  

Take a look at the [documentation](https://github.com/stympy/faker) and see all of the fun and bizarre things that you can "Fake".  What gems have you found to be useful or interesting?  

