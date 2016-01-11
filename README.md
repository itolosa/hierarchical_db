# HierarchicalDb

This gem has the implementation of **[Hierarchical databases](http://www.sitepoint.com/hierarchical-data-database/)**

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'hierarchical_db'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install hierarchical_db

Next you have to add a migration into your model with attributes **lft** and **rgt**. For example, if you have a model called **Territory** (to manage countries, cities and all the other hierarchies) with the following relationship:
```ruby
class Territory < ActiveRecord::Base
  belongs_to :parent_territory, class_name: 'Territory'
end
```
You have to add a migration like this:
```ruby
class AddSortedTreeFields < ActiveRecord::Migration
  def change
    add_column :territories, :lft, :integer
    add_column :territories, :rgt, :integer
  end
end
```
Into the model you have to include Hierarchies adding the code below:
```ruby
class Territory < ActiveRecord::Base
  include HierarchicalDb #we added this
  belongs_to :parent_territory, class_name: 'Territory'
end
```
Finally we add two alias methods that are useful and necessary to deal with hierarchies:
```ruby
class Territory < ActiveRecord::Base
  include HierarchicalDb 
  belongs_to :parent_territory, class_name: 'Territory'
  # alias methods
  alias_method :children, :territories #we added this
  alias_method :parent, :parent_territory #we added this
end
```
## Usage

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake false` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/hierarchical_db. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

