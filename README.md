# Gravatarify
    
Removes any hassles building those pesky gravatar urls, it's not there arent any alternatives [out](http://github.com/mdeering/gravitar_image_tag),
[there](http://github.com/chrislloyd/gravtastic), but none seem to support stuff like `Proc`s for the default picture url, or
the multiple host names supported by gravatar.com (great when displaying lots of avatars).

Best of it? It works with Rails, probably Merb and even plain old Ruby :)

## Install

TODO: need to gemify it...

## Using the view helpers

Probably one of the easiest ways to add support for gravatar images is with the included view helpers:

    <%= gravatar_tag @user.email %>
    
This builds a neat `<img/>`-tag, if you need to pass in stuff like the size etc. just:

    <%= gravatar_tag @user.email, :size => 25, :rating => :x, :class => "gravatar" %>
    
This will display an "X" rated avatar which is 25x25 pixel in size and the image tag will have the class `"gravatar"`.
If more control is required, or just the URL, well then go ahead and use `gravatar_url` instead:

    %img{ :src => gravatar_url(@user.email, :size => 16), :width => 16, :height => 16, :alt => @user.name, :class => "avatar avatar-16"}/
    
Yeah, a `HAML` example, creating an `<img/>`-tag by using `gravatar_url`.

## Using the model helpers

Another way (especially cool) for models is to do:

    class User < ActiveRecord::Base
     gravatarify
    end
   
Thats it! Well, at least if the `User` model responds to `email` or `mail`. Then in the views all left to do is:

    <%= image_tag @user.gravatar_url %>
   
Neat, isn't it? Of course passing options works just like with the view helpers:

    <%= image_tag @user.gravatar_url(:size => 16, :rating => :r) %>
   
Defaults can even be passed to the `gravatarify` call, so no need to repeat them on every `gravatar_url` call.

    gravatarify :employee_mail, :size => 16, :rating => :r
   
All gravatars will now come from the `employee_mail` field, not the default `email` or `mail` field and be in 16x16px in size
and have a rating of 'r'. Of course these can be override in calls to `gravatar_url` like before.

### PORO - plain old ruby objects (yeah, POJO sounds smoother :D)

Not using Rails, ActiveRecord or DataMapper? It's as easy as including `Gravatarify::ObjectSupport` to your
class:

    require 'gravatarify'
    class PoroUser
      include Gravatarify::ObjectSupport
      gravatarify
    end
    
Tadaaa! Works exactly like the model helpers, so it's now possible to call `gravatar_url` on instances
of `PoroUser`.

