---
layout: post
title: "rake task to generate new jekyll posts"
date: 2015-11-21 21:03:46
categories: blog
comments: true
---

# Writing a Rake Task to generate new post for jekyll blogs

This is a simple rake file that generates new posts.
It is very easy to use it, just create a new ```Rakefile``` and paste the following code:
{% highlight ruby %}
DATE = Time.now.strftime("%Y-%m-%d")
TIME = Time.now.strftime("%H:%M:%S")
POST_DIR = '_posts'

desc "Generate new post"
task :new_post do

  puts 'Please write the post title'
  @name = STDIN.gets.chomp
  @title = @name.downcase.strip.gsub(' ', '-')
  @file = "#{POST_DIR}/#{DATE}-#{@title}.markdown"

  if File.exists?("#{file}")
    raise 'file already exists'
  else
    File.open(@file, 'w') do |file|
      file.write(
        "---\nlayout: post\ntitle: \"#{@name}\"\ndate: #{DATE} #{TIME}\ncategories:\n---\n"
      )
    end
  end
end
{% endhighlight %}

You can run the task by typing the following command:
```$ rake new_post```

The task is straight forward, it does very simple things,
just asking for a title and then creates a new file according to jekyll's requirments
under the _posts folder.

Of cource the task is far from perfect and can be improved.
