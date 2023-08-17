# On the Road to Adventure

The time has come to discover new adventures, to explore new spaces. So I'm about to enter uncharted territory, or to be precise, a land I only glimpsed over 10 years ago, Ruby on Rails.
But first, let's go to the [getting started page](https://guides.rubyonrails.org/getting_started.html).

# First step

Discovering Ruby On Rails requires a few prerequisites, which I'm about to validate on my good old Manjaro Linux distribution.

## Is Ruby installed ?
```shell
ruby --version
ruby 3.0.5p211 (2022-11-24 revision ba5cf0f7c5) [x86_64-linux]
```
**Yes**

## IS SQLite installed ?

```shell
sqlite3 --version
3.42.0 2023-05-16 12:36:15 831d0fb2836b71c9bc51067c49fee4b8f18047814f2ff22d817d25195cf3alt
```

**Yes**


All that's left to do is install rails

```shell
gem install rails
```

## Is Rails successfuly installed ?

```
rails --version                                                                 
zsh: correct 'rails' to '_rails' [nyae]? 
```

**No**, damned...
Let's check the installation log:

```shell
WARNING:  You don't have /home/jbd/.local/share/gem/ruby/3.0.0/bin in your PATH,
          gem executables will not run.
```

Sounds like a good avenue to investigate.

Let's edit our PATH to add gem executable
```shell
export PATH=/home/jbd/.local/share/gem/ruby/3.0.0/bin:$PATH
```

Now, we can retry

```shell
rails -version                                                                        Rails 7.0.6
```

Tada !! \o/

As I was afraid of the gem configuration, I find a command to check the environement:

```shell
gem environment
```

# Let's Play with RoR

## Create a New Blog Application

```shell
rails new blog
```

Wow, it creates a full structure with a lot of files, and sadly it outputs errors.

```shell
# ...
Bundler::PermissionError: There was an error while trying to write to `/usr/lib/ruby/gems/3.0.0/cache`. It is likely that you need to grant
write permissions for that path.
# ...
```

I came across a [github issue](https://github.com/rubygems/rubygems/issues/6272#issuecomment-1381683835) that give several soltions.
I can fix the permission/change the user or define a BUNDLE_PATH env variable.
I prefer to set the variable to avoid adding permission or complexity with a new user.
Let's define the local gem cache as BUNDLE_PATH

```shell
export BUNDLE_PATH=/home/jbd/.local/share/gem/ruby/3.0.0/cache
```

Clean the generated project
```shell
rm -rf blog
```

And retry the rail command:
```shell
rails new blog 
```

Bingo! No more permission issue.
Before this mishap, I'd say that the project structure was quite full,
Let's check the content on the [rails getting started guide](https://guides.rubyonrails.org/getting_started.html)
I let you read the guide as well.

Now, let's start the server

```shell
bin/rails server                                                                                                                                         ✔ 
=> Booting Puma
=> Rails 7.0.7 application starting in development 
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.6.6 (ruby 3.0.6-p216) ("Birdie's Version")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 159020
* Listening on http://127.0.0.1:3000
* Listening on http://[::1]:3000
Use Ctrl-C to stop
```

Awesome, and it shows the access log.

```shell
Started GET "/" for 127.0.0.1 at 2023-08-15 21:15:58 +0200
Processing by Rails::WelcomeController#index as HTML
  Rendering /home/jbd/.local/share/gem/ruby/3.0.0/cache/ruby/3.0.0/gems/railties-7.0.7/lib/rails/templates/rails/welcome/index.html.erb
  Rendered /home/jbd/.local/share/gem/ruby/3.0.0/cache/ruby/3.0.0/gems/railties-7.0.7/lib/rails/templates/rails/welcome/index.html.erb (Duration: 1.0ms | Allocations: 932)
Completed 200 OK in 12ms (Views: 3.7ms | ActiveRecord: 0.0ms | Allocations: 10049)
```

Everything seems to be working, so I'm moving on to the next step:
[Say Hello in Rails](https://guides.rubyonrails.org/getting_started.html#say-hello-rails).

I edit the routing configuration:
```ruby
Rails.application.routes.draw do
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"

  get "/articles", to: "articles#index"
end
```
Then generates the controller:

```shell
bin/rails generate controller Articles index --skip-routes
 create  app/controllers/articles_controller.rb
      invoke  erb
      create    app/views/articles
      create    app/views/articles/index.html.erb
      invoke  test_unit
      create    test/controllers/articles_controller_test.rb
      invoke  helper
      create    app/helpers/articles_helper.rb
      invoke    test_unit
```

Then edit the view `app/views/articles/index.html.erb` with: 
```html
<h1>Hello, Rails!</h1>
```

Then, checks the url http://localhost:3000/articles
Awesome, I can see my modification.

Next, the index page.
I edit the `config/routes.rb` file to uncomment the root line.

Then, check the root url: http://localhost:3000/
And yes we have the same message then /articles.

So, this web application follows the [MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) pattern.
We have the view and the controller, let's play with the model.

```shell
rails generate model Article title:string body:text                                                                                                      ✔ 
      invoke  active_record
      create    db/migrate/20230817184550_create_articles.rb
      create    app/models/article.rb
      invoke    test_unit
      create      test/models/article_test.rb
      create      test/fixtures/articles.yml
```

A new bunch of files is generated.
I apply db migration:

```shell
rails db:migrate                                                                                                                                         ✔ 
== 20230817184550 CreateArticles: migrating ===================================
-- create_table(:articles)
   -> 0.0011s
== 20230817184550 CreateArticles: migrated (0.0011s) ==========================
```

It executes the migration file previously generated.
And I learn that it uses [Active Record Migration](https://guides.rubyonrails.org/active_record_migrations.html).
It is based on a Ruby DSL and abstracts the database interactions.

Now, the guide propose to play with a Rails feature called console.

```shell
rails console
Loading development environment (Rails 7.0.7)
irb(main):001:0>
```

Then, I add a new article with a simple line:

```shell
article = Article.new(title: "Hello Rails", body: "I am on Rails!")
=> #<Article:0x0000557453c9fe88 id: nil, title: "Hello Rails", body: "I am on Rails!", created_at: nil, updated_at: nil>
```

The object is created but not saved into the database, let's do it:

```shell
article.save
  TRANSACTION (0.1ms)  begin transaction
  Article Create (0.3ms)  INSERT INTO "articles" ("title", "body", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "Hello Rails"], ["body", "I am on Rails!"], ["created_at", "2023-08-17 18:57:10.021086"], ["updated_at", "2023-08-17 18:57:10.021086"]]
  TRANSACTION (1.3ms)  commit transaction
=> true
```

Wow, is it really saved ?

```shell
article
=> #<Article:0x0000557453c9fe88 id: 1, title: "Hello Rails", body: "I am on Rails!", created_at: Thu, 17 Aug 2023 18:57:10.021086000 UTC +00:00, updated_at: Thu, 17 Aug 2023 18:57:10.021086000 UTC +00:00>
```

Yes, and moreover, the `id`, `created_at` and `updated_at` have been generated.
Let's try some commands:

```shell
Article.find(1)
  Article Load (0.3ms)  SELECT "articles".* FROM "articles" WHERE "articles"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
=> #<Article:0x0000557453ff6348 id: 1, title: "Hello Rails", body: "I am on Rails!", created_at: Thu, 17 Aug 2023 18:57:10.021086000 UTC +00:00, updated_at: Thu, 17 Aug 2023 18:57:10.021086000 UTC +00:00>

Article.all
  Article Load (0.1ms)  SELECT "articles".* FROM "articles"
=> [#<Article:0x00007f25448289d8 id: 1, title: "Hello Rails", body: "I am on Rails!", created_at: Thu, 17 Aug 2023 18:57:10.021086000 UTC +00:00, updated_at: Thu, 17 Aug 2023 18:57:10.021086000 UTC +00:00>]
```

For more details, see [Active Record Basics](https://guides.rubyonrails.org/active_record_basics.html) and [Active Record Query Interface](https://guides.rubyonrails.org/active_record_querying.html) 

\o/, we played with the model, now let's use it in the webapp.
I edit `app/controllers/articles_controller.rb` to show all the articles on the index action.

```ruby
def index
    @articles = Article.all
end
```

Then, I add it to the view by editing `app/views/articles/index.html.erb`

```erb
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <%= article.title %>
    </li>
  <% end %>
</ul>
```
