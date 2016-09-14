+++
date = "2016-07-20T23:24:14+04:00"
draft = true
title = "Thoughts on Ruby syntax"
tags = [ "Development", "Ruby" ]

+++

With ruby metaprogramming, many doors are really open to extend your code. I will start by giving practical examples on how you can use classes, modules, methods and many other related topics.

In the beginning, let's start by defining a class:

``` ruby 
class Base
    # this method will be associated with any "instance" object of the "Base" class
    def writeSomeCode
        puts "Hello, I'm writing some code!"
    end
end
```

For example, let's try do the following:

``` ruby
[3] pry(main)> Base.writeSomeCode
NoMethodError: undefined method 'writeSomeCode' for Base:Class
from (pry):7:in '__pry__'

[6] pry(main)> b.writeSomeCode
Hello, I'm writing somec ode!
=> nil

```

That's obvious, we can't call the method directly as it belongs to in the instance of the "Base" class. However, we can reference the `self` keyword:

``` ruby 
class Base
    class << self # Can also be written as: class << Base
        def writeSomeCode
            puts "Hello, I'm writing some code!"
        end
    end
end
```

That's can be simlpy considered as Class Method definition where these methods are tied to the class itself. In another scenario, we might need to limit the method access to certain instance.

``` ruby 
[60] pry(main)> o = Object.new
=> #<Object:0x007fb8499a8d60>
[61] pry(main)> class << o
[61] pry(main)*   def oTest
[61] pry(main)*     puts "oTest print!"
[61] pry(main)*   end  
[61] pry(main)* end  
=> nil
[62] pry(main)> o.class
=> Object
[63] pry(main)> o.o
o.oTest      o.object_id  
[63] pry(main)> o.oTest
oTest print!
=> nil
[65] pry(main)> oo = Object.new
=> #<Object:0x007fb84a3ff110>
[66] pry(main)> oo.oTest
NoMethodError: undefined method `oTest' for #<Object:0x007fb84a3ff110>
from (pry):114:in `__pry__'
[67] pry(main)> 
```

Passing a code block into such siutation is very beneficial. A prominent example is that when we need to make dynamic calls
