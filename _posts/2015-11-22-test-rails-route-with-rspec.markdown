---
layout: post
title: "Test custom Rails routes with RSpec"
date: 2015-11-22 22:17:27
categories: tests
comments: true
---

Recently I was reading how to test custom rails routes, so I would like to share it.

Image that we have the current custom route:
{% highlight ruby %}
put '/posts/:post_id/comments/sort' to: 'post_comments/sort#update'
{% endhighlight %}

and we want to test that the request is routable.

Our test will be:

{% highlight ruby %}
describe 'sort all comments of a post' do
  it 'tests that the request is routable' do
    expect(put: "/posts/#{post.id}/comments/sort").to route_to(
      controller: "posts/sort",
      action: "update",
      post_id: post.id.to_s
    )
  end
end
{% endhighlight %}

Notice that we convert the id to string, otherwise the test is going to fail.
The route_to matcher is very helpful when we want to test routes other than

standard RESTful routes.

For more examples you can check the Relish [documentation](https://relishapp.com/rspec/rspec-rails/docs/routing-specs/route-to-matcher){:target="_blank"} for RSpec route_to matcher.
