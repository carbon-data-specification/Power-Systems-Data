# Power Systems Data website

This is the website of the working group.

It's goal is to create more visibility for the working group and to provide a place for the working group to publish its specifications.

## Install

The website runs through Jekyll, a static site generator. To install Jekyll, follow the instructions on the [Jekyll website](https://jekyllrb.com/docs/installation/).

## Serve

`jekyll serve`


### Troubleshooting

* `jekyll serve`:
  * With ruby 3.0.0 together with Jekyll 4.2.2, you might encounter an error similar to `jekyll/commands/serve/servlet.rb:3:in 'require': cannot load such file -- webrick (LoadError)`. It's because `webrick` is missing. From [here](https://github.com/jekyll/jekyll/issues/8523) we can see that running `bundle add webrick` should fix it.
