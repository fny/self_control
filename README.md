# SelfControl 🎛

Control flow with an audit trail for conditional statements. Track your ifs and elses to make debugging gnarly conditionals a breeze.

## About

SelfControl was sponsored by Greenlight COVID-19 monitoring to
debug deeply nested logic used to determine whether students and teachers are fit to return to school.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'self_control'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install self_control

## Usage

I'm an extremely complicated person. Figuring out when I want to ice cream is hard.

```ruby
require 'self_control'

@temperature = 70
@income = 10

def sunny?; false; end
def having_cravings?; true; end
def hot?; @temperature > 97; end
def have_enough_money; @income > 2; end
def temperature_moderate?; @temperature > 50; end

flow = SelfControl::Flow.new(self) do
  If(:hot?) {
    Return(:yes)
  }.Elsif(And(:sunny?, :have_enough_money?)) {
    Return(:yes)
  }.Else {
    If(:temperature_moderate?) {
      If(:having_cravings?) {
        Return(:yes)
      }
    }
  }
  Return(:no)
end

flow.result # => :yes
puts(flow.trace)
# root:
#   branch 1.0:
#     if: hot? => false
#     elsif and: boolean => false
#       sunny? => false
#       have_enough_money => true
#     else: boolean => true
#     branch 2.0:
#       if: temperature_moderate? => true
#       branch 3.0:
#         if: having_cravings? => true

```

Within the specified target (in this case `self`):

You can also execute statements directly inside a branch, but you won't be able to see what was evaluated in the trace.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

To type check run:

```
solargraph typecheck
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/fny/self_control.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
