# Ruby on Rails Super Duper Basics
Cobbled from this tutorial: https://guides.rubyonrails.org/getting_started.html

We are starting at 3.2: Creating the Application.

Anything in (()) is something you should name yourself. 

## Creating the Application
- Create a folder on your computer where you want the thing to live.  
- $ rails new ((name))
- $ cd ((name)) 
- $ code . 

## Getting Started
- $ rails server
    - Ctrl + C to stop the server (even on Mac)
    - Recommended to have a SECOND command line open for this
- Check http://localhost:3000 for "Yay! You're on Rails!"

### First Controller, Routes, & View
- $ rails generate controller ((Name)) ((any pages to create))
    - In the tutorial, you create "Welcome index" aka the Welcome controller and the index page
- VS Code: Edit the new view 
- VS Code: Update routes at config/routes.rb
    - Lots of ways to add routes, in the tutorial you add `resources : articles` here
    - See also Academty Twitch 11/9 & 11/10 for other ways

### Editing Your Controller
- VS Code: Add functions to your controller
    - `def new` `end` is what's in the tutorial
    - Whatever other functions you want your controller to do
    - Any "page" you want to add to your view needs at least `def function end`

### Editing Your View
- VS Code: Add HTML or whatever to the view associated with the function you added to your controller

### Workflow Within a Controller
- Add a function to your controller, make a view page for it, add HTML to that view page. 
     - It seems like this is the basic loop here although your mileage may vary

### Adding a Model
- $ rails generate model ((Name))((column:coltype)) 
    - The formula here is the name (singular) followed by the column name and then the column type. The formatting on the column:coltype is finicky.
    - columnname:references will create an ID and associate two tables
    - Check under db/migrate to see that it's making the changes you want
- $ rails db:migrate
    - Check the db/schema.rb to make sure that everything looks like you want
    - If not, you can $ rails db:rollback and do it over
    - There's another method for adding a column to a table that I have not gotten to work, but it looks like this: $ rails generate migration add_fieldname_to_tablename fieldname:string

### Other Stuff
- Ruby "strong parameters" requires you to say which parameters you'll allow. See the private method at the bottom of the code example below, or Section 5.6 of the tutorial.
- Adding validation: Section 5.10 of the tutorial
- Linking from page to page in RoR: `<% link_to 'Link Text', functionname_controllername_path %>`
- Pass through Ruby in a .html.erb (embedded Ruby) file with `<% ruby goes here %>` OR `<%= ruby that has a return to display goes here%>`

### Adding Styling
- VS Code: app/assets/stylesheets/application (and there are other stylesheets generated for each controller too)

## CRUD Controller Example Code
{
    class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
 
  def show
    @article = Article.find(params[:id])
  end
 
  def new
    @article = Article.new
  end
 
  def edit
    @article = Article.find(params[:id])
  end
 
  def create
    @article = Article.new(article_params)
 
    if @article.save
      redirect_to @article
    else
      render 'new'
    end
  end
 
  def update
    @article = Article.find(params[:id])
 
    if @article.update(article_params)
      redirect_to @article
    else
      render 'edit'
    end
  end
 
  def destroy
    @article = Article.find(params[:id])
    @article.destroy
 
    redirect_to articles_path
  end
 
  private
    def article_params
      params.require(:article).permit(:title, :text)
    end
end
}

