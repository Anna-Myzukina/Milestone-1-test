# Milestone-1-test
[How to use rake db commands in the correct way](https://dev.to/neshaz/how-to-use-rake-db-commands-in-the-correct-way--50o2)

https://voormedia.github.io/rails-erd/
https://github.com/voormedia/rails-erd


Models
For code about your database or domain objects, the model is your first stop in Rails. Models are powerful, easy to test, reusable across applications and more like non-Rails code than most of Rails – familiar, even if you don’t know Rails yet.

If there’s a good way to put the code in your model, that’s usually a safe bet and a good idea.

Write tests too, of course!

Controllers
It’s easy to put lots of code in your controllers, but it’s almost always a mistake. Business logic for your app should get out of the controller and into the model as quickly as possible. Logic about how things are shown to the user should go into the view. In general, the controller should be a tiny, thin glue layer putting together your other components.

Views
Having lots of logic in your views is a huge anti-pattern. Don’t do it. It’s hard to test, it’s hard to find, it’s hard to write sandwiched in between the HTML… Just don’t.

Instead, your views should contain HTML, variables that turn into HTML, and calls to helper methods that generate HTML — or whatever your final output format is. There should be no logic in there to test. No conditionals, no loops, no non-display methods. If you add an output format, there should be no code to repeat because all the interesting data transforms already happened, and no other output format cares about your HTML-only helpers. Right?

Helpers
Rails “helpers” are very specifically view helpers. They’re automatically included in views, but not in controllers or models. That’s on purpose.

Code in the application helper is included in all the views in your application. Code in other helpers is included in the corresponding view. If you find yourself writing big loops, method calls or other logic in the view but it’s clearly display logic, move it into a method in the helper.

The /lib Directory
Every Rails app starts with a /lib directory, but not much explanation of it.

Remember that helpers are specifically view helpers? What if you wanted a controller helper? Or a model helper? Sometimes you can use a parent controller or parent model, but that’s not always the best choice.

If you want to write a helper module for non-view logic, the /lib directory is usually the best place to put it. For example, logging code or some kinds of error handling may be a cross-cutting concern like that.

Also, if you’re putting everything in the ApplicationController or ApplicationHelper, those can get big. Consider factoring some of that code out into helpers, or into /lib.

Stuff in /lib isn’t always automagically included for you like controllers and models. So you may need to explicitly require the file, not just use the name of the class.

Gems
Sometimes you have reusable pieces in your application. A controller or model might be needed by multiple different Rails apps. A particular piece of logic for logging or display might be useful to a lot of different folks. You might even find a different way of doing things that most Rails apps would benefit from.

These are all cases where you want to create a new gem and have your applications use it instead of sharing a directory of code.

These days it’s really easy to create a new gem, so don’t be intimidated. If you haven’t worked through the first free chapter of Rebuilding Rails, this may be a good time to do it — it’ll show you how to quickly, easily create and use a new gem.

Assets
In a few cases, you’re not even writing Ruby code. Instead, it may be Sass, Scss, JavaScript or CoffeeScript. In this case, it generally belongs under app/assets.

Concerns and Exceptions
Rails has a very specific, very unusual setup. I think it’s a good idea for small apps, but only use Rails until it hurts. If your application gets too big or complicated, the Rails code organization may hurt more than it helps you.

There are several “grow out of Rails” approaches to apply alternate architectures to the framework. From Hexagonal Rails to Objects on Rails to the more general Clean Ruby DCI approach. I won’t tell you which to use, but I’ll tell you that you’re better off starting with plain, simple Rails and growing out of it.

Most Rails apps, and even more Rails controllers, don’t need to get all that big. They often don’t need to change much. Why go complicated when simple is working great?



# Project: Building Facebook

* NOTE before start chreate project using this command (name_of_project it`s example, you should change it on your own name of your project for example: facebook-clone...)

        rails new name_of_project --database=postgresql


## Milestone 1:

* In this project you should create ERD(Entity Relationship Diagram) diagram, it`s image of your future database. I used online tool [lucidchart](https://www.lucidchart.com/) to draw that diagram, create folder "docs" and add to folder that image with diagram. Follow this video to understand how it works [Entity Relationship Diagram (ERD) Tutorial - Part 1](https://www.youtube.com/watch?v=QpdhBUYk7Kk&vl=en)
* Watch next video to be familiar with Primary and Foreign Keys [Entity Relationship Diagram (ERD) Tutorial - Part 2](https://www.youtube.com/watch?v=-CuY5ADwn24)

Here is a screenshot with example of such image with diagram:
![screen](https://github.com/Anna-Myzukina/facebook-milestones/blob/master/docs/ERD-facebook.jpeg)


## Milestone 2:

* In this Milestone you need to setup project as it is described in Odin requirements. It means that you should add necessary gems to your Gemfile.
I recommended to add next gems:

* gem ['bootstrap-sass', '3.3.7'](https://github.com/twbs/bootstrap-rubygem)
* gem ['bcrypt',         '3.1.12'](https://github.com/codahale/bcrypt-ruby)
* gem ['faker',          '1.7.3'](https://github.com/faker-ruby/faker)
* gem ['will_paginate', '3.1.6'](https://github.com/mislav/will_paginate)
* gem ['bootstrap-will_paginate', '1.0.0'](https://github.com/yrgoldteeth/bootstrap-will_paginate) 
* gem ['rubocop'](https://www.rubocop.org/en/stable/)
* gem ['devise'](https://github.com/plataformatec/devise)
* gem ['omniauth-facebook'](https://github.com/mkdynamic/omniauth-facebook)
* gem ["font-awesome-rails"]()
* gem ['gravatar_image_tag'](https://github.com/mdeering/gravatar_image_tag)

group :development, :test do
* gem ["rspec-rails"](https://github.com/rspec/rspec-rails) 
* gem ['sqlite3', '1.3.13'](https://github.com/sparklemotion/sqlite3-ruby)
end
        
 
 Than use next command in your terminal
 
      $  bundle install
      $  bundle updete

1. Cause we added bootstrap to Gemfile in next step we should create file and add bootstrap to our application

        touch app/assets/stylesheets/custom.scss

Inside this file app/assets/stylesheets/custom.scss add next code

        @import "bootstrap-sprockets"; 
        @import "bootstrap";

2. Cause we added font-awesom in your application.css, include next:

/*
 *= require font-awesome
 */

 Inside this file app/assets/stylesheets/custom.scss add next code

        @import "font-awesome";

On this site https://fontawesome.com/?from=io you can choose any icons you want


## Milestone 3:

I this project we Create models with associations and implement all requested features for users and posts. Add authentication with Devise as described in requirements.

        rails generate scaffold User first_name:string last_name:string email:string password:string birthday:string gender:string

        rails db:migrate

First, if you have already run the migrations generated by the scaffold command, you have to perform a rollback first.

        rake db:rollback

You can create scaffolding using:

        rails generate scaffold MyFoo 

(or similar), and you can destroy/undo it using

        rails destroy scaffold MyFoo
        
That will delete all the files created by generate, but not any additional changes you may have made manually.


Next, you need to run the generator:

$ rails generate devise:install

$ rails generate devise User

Then run 

$ rails db:migrate

