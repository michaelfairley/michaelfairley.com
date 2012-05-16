---
title: Fast Tests on JRuby (RSpec + Nailgun + autotest)
layout: post
---

JRuby is awesome. The JVM's startup time is not:

{% highlight console %}
$ rvm use 1.9.3
Using /Users/michaelfairley/.rvm/gems/ruby-1.9.3-p125
$ time ruby -e "puts 'Hello'"
Hello

real	0m0.077s
user	0m0.031s
sys	0m0.013s
$ rvm use jruby
Using /Users/michaelfairley/.rvm/gems/jruby-1.6.7
$ time ruby -e "puts 'Hello'"
Hello

real	0m1.955s
user	0m1.268s
sys	0m0.212s
{% endhighlight %}

This makes doing TDD in JRuby more painful than it ought to be.

Luckily, JRuby has built in support for [Nailgun](http://www.martiansoftware.com/nailgun/), which can prevent the need for starting up a new JVM each time we want to run Ruby, making things a brazilian times faster.

Using Nailgun is simple. First, start up the Nailgun server:

{% highlight console %}
ruby --ng-server
{% endhighlight %}

And then pass `--ng` to ruby whenever you use it:

{% highlight console %}
$ time ruby --ng -e "puts 'Hello'"
Hello

real	0m0.376s
user	0m0.001s
sys	0m0.004s
{% endhighlight %}

Much better.

Unfortunately, a teensy bit of monkey patching is required to get Nailgun playing nicely with autotest.

Dump this anywhere in your .autotest file:

{% highlight ruby %}
# From https://github.com/rspec/rspec-core/blob/master/lib/autotest/rspec2.rb,
# but with "--ng" added.
class Autotest::Rspec2 < Autotest
  def make_test_cmd(files_to_test)
    files_to_test.empty? ? '' :
      "#{prefix}#{ruby}#{suffix} --ng -S '#{RSPEC_EXECUTABLE}' --tty \
      #{normalize(files_to_test).keys.flatten.map { |f| "'#{f}'"}.join(' ')}"
  end
end
{% endhighlight %}

And then run autotest with Nailgun:

{% highlight console %}
ruby --ng -S autotest
{% endhighlight %}

Boom. Now you have a subsecond test cycle in JRuby.