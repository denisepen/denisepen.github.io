---
layout: post
title:      "My CLI Data Gem Project"
date:       2018-03-27 14:56:47 +0000
permalink:  my_cli_data_gem_project
---

To create my CLI data gem I decided to use one website that I visit often - [ketodash.com/recipes](http://).

[ketodash.com/recipes](http://) is a website that hosts a collection of recipes for people who follow the ketogenic diet.  
The ketogenic diet is known as the low carb, high fat diet or  'LCHF' diet.  If you adhere to this diet, most of your daily calories come from fat (usually plant or fish based) with the rest coming from vegetables.  Carbohydrate intake is usually limited to 20 g or less.  As someone who follows this way of eating it is imperative that I constantly have a list fo recipes at the ready which led me to ketodash.com. 

On the 'recipes' page is a list of the names of over 300 recipes. This list labels each recipe by its meal type, such as snack, dessert, drink, etc. Each recipe also has its nutrition information included so you can see the number of calories, the amount of fat, or the other important macronutrient values.


For my project I wanted to be able to offer the user three things:

   1. The ability to see a list of all the recipes. 

   2. The option to access the recipes based on their type - so, for example, if someone wanted to only look at the dessert recipes, they could, without having to scroll through all 300+ recipes.

   3. Once the user has chosen a recipe, they are able to view the nutrition information as well as get a URL that they can use to access the recipe itself, with ingredients and instructions.  


To begin this project I scraped the ketodash/recipes page and created recipe objects. I gave each recipe object a name, type, URL, and properties for each macronutrient. Once each recipe object was created it was added to the recipes class array.

```
doc = Nokogiri::HTML(open("https://ketodash.com/recipe"))
    @@recipes = []
     doc.css("div.col-sm-6").collect do |dish|

        recipe_1 = self.new
        recipe_1.name = dish.css("div.card-header h5 a").attribute("title").text
        recipe_1.url = "ketodash.com#{dish.css("div.card-header h5 a").attribute("href").text}"
        recipe_1.type = dish.css("div.card-header h5 span").text
        recipe_1.calories = dish.css("div.card-block ul li")[0].text
        recipe_1.protein = dish.css("div.card-block ul li")[1].text
         recipe_1.fat = dish.css("div.card-block ul li")[2].text
        recipe_1.carbs = dish.css("div.card-block ul li")[3].text
        @@recipes << recipe_1
```


To print out each recipe I created a method that would allow me to iterate over the @@recipes array and print out each recipe with an index:

 ```
 def self.all
    @@recipes.each.with_index(1) do |recipe, i|
      puts " #{i}. #{recipe.name}" 
    end
  end
```

To access the recipes by type I once again iterated over the @@recipes array and pulled out the recipes if they matched the given type. These matching recipes were then put into an array of the specific type. I then iterated over these 'type' arrays and was able to print out the list of recipes by type by index. 

```
def self.dessert
  @dessert = []
  @@recipes.each do |recipe|
    if recipe.type == "Treat"
      @dessert << recipe
    end
  end

    @dessert.each.with_index(1) do |dessert, i|
      puts "#{i}. #{dessert.name}"
  end
end
```

This completed my recipes class definition. I then moved onto building the CLI.  

I knew what data I wanted the user to be able to acces, so I created methods to allow just that. I presented a menu that asked the user to either choose to have the complete list of recipes or to choose the recipe by type.  No matter which option they chose they were given a list of recipes - either a list of all recipes on the webpage or a list of recipes by specific type.  Once they had the list of recipes they could either choose a recipe and receive the recipe's nutrition information or they could exit and go back to the main menu.  To choose a particular recipe they simply have to input the recipes index number. Once the nutrition information was printed they could either continue going through the recipes or exit. Here is a snippet of code from my CLI menu method:

  ```
when "4"
    list_desserts
    puts ""
    puts "Please choose a dish by number for nutrition information or type exit to return to the main menu"
      input = gets.strip
       if input == "exit"
        menu
      else
        puts @recipes_dessert[input.to_i-1].name
        puts @recipes_dessert[input.to_i-1].calories
        puts @recipes_dessert[input.to_i-1].protein
        puts @recipes_dessert[input.to_i-1].fat
        puts @recipes_dessert[input.to_i-1].carbs
        puts "URL: #{@recipes_dessert[input.to_i-1].url}"
        puts ""
        menu
      end
```

Let's explore this code.  If the user chooses to view only dessert recipes they will input the number '4'.  When they choose '4' they will get a list of all dessert recipes. If they want to choose from one of those recipes they can enter the index number of that recipe. Once that index number has been entered they receive the recipe's name, calories, protein, and a host of other properties. They can then either choose another recipe from the dessert recipe list or exit to the main menu.  

My CLI's call method consists of two other methods, #menu, and #goodbye (which tell you goodbye once you exit the program). The menu method does all the heavy lifting.

```
def call
  menu
  goodbye
end
```

While building this CLI data gem I utilized everything I learned in Flatiron's Ruby curriculum.  I utilized logic and conditionals, booleans, arrays, iterators, created objects and assigned them properties. I had to use instance methods and class methods, and consider the object's lifecycle.  Overall, working on this project was a wonderful way to finetune my undertanding of all these concepts.









