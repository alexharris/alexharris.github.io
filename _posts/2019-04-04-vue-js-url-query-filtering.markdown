---
layout: post
title:  "Vue.js URL Query Parameter Filtering"
date:   2019-04-03 03:45:02 -0700
categories: vue javascript
---
One project that I am currently working on is a simple publishing platform for archival collections called [Small Archives](https://smallarchives.com), built with Vue.js. Users create archives of items such as images, videos, and PDFs, which are then displayed, along with metadata, in a variety of ways. Currently, I am working on building out some of the filtering functionality so that visitors can easily view only certain facets of a given collection.

Currently, an archive has a main view for the header info, and then each type of listing (list, grid, etc) has its own subcomponent. For the initial filters, which only appeared on the main archive page, it was easy enough to pass values from the various filter form fields to the components using Vue's `props`.





You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
