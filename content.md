# Debugging in Ruby on Rails with the `debug` Gem

## Introduction to Debugging
Now we're going to delve into debugging - a critical skill for every developer. We'll use the [debug](https://github.com/ruby/debug) gem, a robust tool for diagnosing and resolving issues in Ruby on Rails applications.

## Why Debugging Matters
Debugging is the process of identifying and fixing errors or bugs in your code. It's crucial for making sure your applications run smoothly and behave as expected. Up to now you've likely used `puts`, `pp`, or other logging techniques to help 'make the invisible visible' and resolve bugs in your code. The debug gem provides a way to inspect your code execution, examine variables, and control the flow to understand exactly what's happening at any point.

## Setting Up the debug Gem
Before we start, ensure the debug gem is included in your Rails project:

Add the gem to your Gemfile:

```ruby
gem 'debug', group: :development
```

Run `bundle install` to install the gem.

With that set up, you're ready to begin debugging!

## Debugging with `debugger` Alias
In Ruby, binding.break is a method used to initiate a debugging session. However, there's a more commonly used alias: `debugger`. This alias works exactly the same way and is preferred by many developers for its brevity and clarity.

<aside>
The `debug` gem provides several aliases for initiating a debugging session, including `binding.break` and `debugger`. You can use whichever you prefer; they all provide the same functionality.
</aside>

## Using `debugger` in a Rails Application
Suppose we have a Rails application with a `Post` model. We're encountering issues with creating a post. You suspect the issue lies in the create action of the `PostsController`. By placing debugger in this action, you can inspect the @post object and step through the code to identify where things go wrong.

```ruby
class PostsController < ApplicationController
  def create
    @post = Post.new(post_params)
    debugger # Debugging starts here
    if @post.save
      redirect_to @post
    else
      render :new
    end
  end
end
```

Once your app evaluates the `debugger` statement, it'll enter the debugging session:

```
Processing by PostsController#create as HTML
[2, 11] in ~/projects/rails-guide-example/app/controllers/posts_controller.rb
     4|
     5|   def create
     6|     @post = Post.new(post_params)
=>   7|     debugger
     8|     if @post.save
     9|       redirect_to @post
    10|     else
    11|       render :new
    12|     end
    13|   end
    14|
=>#0    PostsController#create at ~/projects/rails-guide-example/app/controllers/posts_controller.rb:7
  #1    ActionController::BasicImplicitRender#send_action(method="index", args=[]) at ~/.rbenv/versions/3.0.1/lib/ruby/gems/3.0.0/gems/actionpack-7.1.0.alpha/lib/action_controller/metal/basic_implicit_render.rb:6
  # and 72 frames (use `bt' command for all frames)
(rdbg)
```

## Navigating the Debugging Session
When your Rails server hits the debugger line, it will pause, and you'll enter the debug mode in your console. Here's how to navigate:

`step` or `s`: Move to the next line of code, stepping into methods.
`next` or `n`: Move to the next line without stepping into methods.
`continue` or `c`: Resume normal execution until the next breakpoint.
`vars`: Display local variables and their values.
`p <expression>`: Evaluate and print any Ruby expression.

## Advanced Debugging Techniques
**Conditional Breakpoints**: Set breakpoints that trigger under specific conditions, like `debugger if params[:debug]`.
**Watching Variables**: Use `watch @post` to monitor changes in the @post variable.
**Backtrace Inspection**: Use `backtrace` to view the call stack and understand how the code reached the current point.

## Debugging Views and Partials
You can also debug views in Rails:

```erb
<%# app/views/users/new.html.erb %>
<%= form_with model: @user do |form| %>
  <% debugger %>
  <%= form.label :name %>
  <%= form.text_field :name %>
  ...
<% end %>
```

When this view is rendered, the debugger will pause, allowing you to inspect the state of your view, including any instance variables like @user.

TODO: javascript debugger

## Practice Exercise: Debugging a Rails Application
Now, let's put your new debugging skills into practice. Follow these steps in your existing Rails project:

- Identify a feature or a bug in your application where you suspect an issue.
- Insert `debugger` at a strategic location in your code.
- Interact with your application to trigger the debugger.
- Use the debug console to inspect variables, step through the code, and diagnose the issue.
- Once you've identified the problem, make the necessary corrections in your code.
  
## Conclusion and Next Steps
You've now learned the essentials of debugging in Ruby on Rails using the debug gem and the debugger alias. Remember, effective debugging is key to developing reliable and efficient applications.

## Further Learning
- Explore More Commands: The debug gem offers a wide array of commands and capabilities. Spend time experimenting with them to understand their full potential.
- Debugging in Different Environments: While we focused on the development environment, remember that you can also use debugging in testing. It can be particularly helpful for understanding why a test is failing.
- Integrate Debugging with Your IDE: Many Integrated Development Environments (IDEs) offer built-in support or plugins for Ruby debugging. Explore how to connect the debug gem with your IDE for a seamless debugging experience. [Here are details on how you can set that up in VSCode](https://code.visualstudio.com/docs/editor/debugging)
- Read the [Official Guide on debugging Rails applications](https://guides.rubyonrails.org/debugging_rails_applications.html)

## Final Exercise
Try adding a new feature or fixing a complex bug in your application. Use the debugger to step through the entire flow of the feature or bug fix. This exercise will give you deeper insights into the interactions within your Rails application and enhance your problem-solving skills.

Happy debugging! 🪰
