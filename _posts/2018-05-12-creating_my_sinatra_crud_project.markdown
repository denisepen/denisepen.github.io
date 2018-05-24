---
layout: post
title:      "Creating My Sinatra CRUD Project"
date:       2018-05-12 14:07:50 -0400
permalink:  creating_my_sinatra_crud_project
---


Creating this project was so much fun and very fulfilling.  There is nothing like creating your very own (and very first) web application: writing the code, making tweaks to have it work just the way you want it to, and eventually using it for it's intended purpose.  I created an app that would allow me to log my daily workouts. It keeps track of the days that I worked out, the total time that I worked out, as well as the total mileage of all my workouts.  You can choose from a list of given exercises such as running, walking, biking, weight lifting, etc. You can also create your own exercise so that any activity that you perform can be logged into the database.  

Creating this app also alerted me to my weaknesses. After completing this project I walked away with  a list of To Do's and skills I need to brush up on before moving forward. 

One improvement I need to make is to better understand how to use Git and GitHub  I am terribly afraid to use Git because I have made so many mistakes. With this project I messed up again.  When I initially began this project  I simply opened the Learn IDE and began writing the code. I worked like this for two days before I realized I needed to have the project in it's own repository. I went to youtube and watched videos and learned how to move data from one repository to another - and I moved the files successfully to their own repository.  However, when I made this move I (wrongly) assumed that I had moved the new repository into the Sinatra project repository - and therefore could simply open the Learn IDE and be in the new repository.  I was wrong. I had moved the files from the Sinatra project folder into the new repository (as I should have).  At this point I panicked and used the Ask A Question feature to get help. Fortunately, this was easy to fix and the instructor patiently walked me through the correct way to set this up - and the correct way to access the files in the future.  

I learned quite a bit working on this project. One of the biggest takeaways is how data associations are created with Active Record. Each of the models in this project (a User model and a Workout model) inherited from Active Record and because of that I had access to macros which provided methods for relating the models and their associated data. For this project a  workout belongs to a user and a user has many workouts.  The models looked like this:

```
class User < ActiveRecord::Base
has_many :workouts

end


class Workout < ActiveRecord::Base
  belongs_to :user
	
	end


```

You can see that each model has a macro: User uses the :'has_many' macro (because the user has many workouts) and Workout uses the ':belongs_to' macro (because a workout belongs to a user). By using these macros, not only are we notifying each model of it's association to other models, but we're also receiving several methods **from** these macros that we can use to build those associations.  

According to  http://guides.rubyonrails.org these are the additional methods:

>   Methods Added by belongs_to:
> 
> 
> When you declare a belongs_to association, the declaring class automatically gains 6 methods related to the association: 

> association
> 
> association=(associate)
> 
> build_association(attributes = {})
> 
> create_association(attributes = {})
> 
> create_association!(attributes = {})
> 
> reload_association``
> 


Association is the object that the model belongs to. In the case of the workouts the association is the user and these methods translate to:

user

user=(user)

build_user(user attributes)

create_user(user attributes)

create_user!(user attributes)

reload_user


So by adding the :belongs_to macro to workouts we can now call the method 'workout.user' or we can create and simultaneously associate a user by using 'creaue_user' and passing in the user attibutes.  Without these macros we would be forced to write each of these methods for each model that belongs to another model.  This is very repetitive and not very DRY. 

In addition, when a model belongs to another model, the first model has an id that corresponds to it's parent model id.  So in this case the workout must have a user_id in it's database to make the associations valid. The user_id in the workouts database is a foreign key and it points to the primary key (or id) of the user in the users database.


The ':has_many' macro also supplies several useful methods for building relationships:


> When you declare a has_many association, the declaring class automatically gains 17 methods related to the association:
> 
> collection
> 
> collection<<(object, ...)
> 
> collection.delete(object, ...)
> 
> collection.destroy(object, ...)
> 
> collection=(objects)
> 
> collection_singular_ids
> 
> collection_singular_ids=(ids)
> 
> collection.clear
> 
> collection.empty?
> 
> collection.size
> 
> collection.find(...)
> 
> collection.where(...)
> 
> collection.exists?(...)
> 
> collection.build(attributes = {}, ...)
> 
> collection.create(attributes = {})
> 
> collection.create!(attributes = {})
> 
> collection.reload
> 

This gives us the ability to build methods such as, 'user.workouts'  which would supply a collection of workouts for a specific user, or 'user.workouts.empty?' which would let us know if the collection of workouts is empty or if there are actually workouts stored for this user.  

By connecting the models in a Sinatra application to Active Record we gain so much more functionality than we would have otherwise.  As we can see in the case of the :has_many macro, there are seventeen methods built and ready to use simply by making that simple association.  This eliminates the complexity of working with the models and accessing the database so creating the application is relatively simple and strightforward.

Overall, I had a blast working on this project and I look forward to learning more about Sinatra, Rails, Active Recocrd, Git/Github and so much more on my journey. 
