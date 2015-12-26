---
layout: post
title: "ActiveRecord without Rails"
date: 2015-12-26 11:48:54
categories: posts
comments: true
---

# Simple guide on how to use ActiveRecord without Rails
I am a big fun of ActiveRecord and recently I was experimenting with it on my free time. On this post I am going to show a very simple way on how to use it without having installed Rails.

I have used the following folder structure for the example app:

{% highlight bash %}
├── Gemfile
├── Rakefile
├── config
│   └── database.yml
├── db
│   └── migrate
├── models
└── setup.rb
{% endhighlight %}

What we have to do is just to include *activerecord gem* inside our Gemfile and then ```bundle install``` in the terminal. For this example we are going to use the *sqlite3 adapter*.

**Gemfile:**
{% highlight ruby%}
source 'https://rubygems.org'

gem 'activerecord'
gem 'sqlite3'
{% endhighlight %}

Then we are going to write a migration under the ```db/migrate/``` folder and the corresponding model under the ```models/``` folder.
Notice the *001* on the migration's filename, is used for the order the migrations are being executed.

**db/migrate/001_create_users.rb:**
{% highlight ruby %}
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :first_name
      t.string :last_name
    end
  end
end
{% endhighlight %}

**models/user.rb**
{% highlight ruby %}
class User < ActiveRecord::Base
end
{% endhighlight %}

Three more things to be done and we are ready. First we need a rake task to execute the migrations (*Rakefile*), secondly a configuration file for the database (*database.yml*) and lastly an application setup file (*app.rb*).

**Rakefile**
{% highlight ruby %}
namespace :db do
  desc 'Run migrations'
  task migrate: :enviroment do
    ActiveRecord::Migrator.migrate('db/migrate', ENV["VERSION"] ? ENV["VERSION"].to_i : nil )
  end
end

task :enviroment do
  load 'app.rb'
end
{% endhighlight %}

**config/database.yml**
{% highlight yaml%}
development:
  adapter: sqlite3
  database: db/database.sqlite3
  pool: 5
  timeout: 5000
{% endhighlight %}

**app.rb**
{% highlight ruby %}
require 'active_record'
require 'sqlite3'
require 'logger'

load 'models/user.rb'

ActiveRecord::Base.logger = Logger.new('development.log')  # active_record logs
configuration = YAML::load(IO.read('config/database.yml'))
ActiveRecord::Base.establish_connection adapter: 'sqlite3', database: 'db/database.sqlite3'
{% endhighlight %}

That's it! Now we can run the migration as we normally do for a Rails app, ```rake db:migrate``` and we are ready.

#### Play time!

Fire up *irb* or *pry* in the terminal and require the *app.rb* file (```require './app'```). Now we are ready to use ActiveRecord without having rails. I definitely recommend the [api.rubyonrails.org](http://api.rubyonrails.org/classes/ActiveRecord/Base.html){:target="_blank"} and [http://guides.rubyonrails.org](http://guides.rubyonrails.org/active_record_querying.html){:target="_blank"} for more information, I found them extremely useful.

Hope you find this post helpful enough for very basic usage of ActiveRecord without Rails. 😀
