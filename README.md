## Overview

AvatarMagick is a [Dragonfly](https://github.com/markevans/dragonfly) plugin for generating Gmail style avatars like those pictured below

<p align="center">
	<img src="https://user-images.githubusercontent.com/138067/52684517-8a70a400-2f14-11e9-8412-04945bc7c839.png" alt="sample">
</p>

The plugin is configurable and options can be set to control background color, text color, font, and size. For more information, please see [this blog post](http://bjedrocha.com/rails/2016/01/06/default-gmail-style-avatars-in-rails/) and accompanying [example project](https://github.com/bjedrocha/avatar-magick-example). 

#### Dragonfly compatibility

The plugin works with the latest versions (1.x) of Dragonfly with support for older versions (0.9.x) in the pipeline.

## Installation

It is assumed you're already using Dragonfly. To install AvatarMagick, simply add this line to your application's Gemfile:

```ruby
gem 'avatar_magick'
```

And then execute:

    $ bundle
    
Once installed, you'll need to register the AvatarMagick plugin along with the [ImageMagick](http://markevans.github.io/dragonfly/imagemagick/) plugin that's included with the Dragonfly gem.

#### Rails

If you're using Dragonfly within your Rails application, you'll already have a `config/initializers/dragonfly.rb` file where your Dragonfly configuration settings are stored. Edit the file and add the plugin within the `configure` block

```ruby
Dragonfly.app.configure do
	plugin :imagemagick
	plugin :avatarmagick
	
	# rest of settings...
end
```

#### Sinatra/Rack/Other

Simply add the plugin within the `configure` block

```ruby
require 'dragonfly'

Dragonfly.app.configure do
  plugin :imagemagick
  plugin :avatarmagick
end
```

## Configuration

You can configure defaults for text color, background color, size, and font. If not specified, AvatarMagick will use the following defaults

```
color: FFFFFF
background_color: 000000
size: '120x120'
font: 'Arial-Regular'
```

To overwrite, simply provide new values for any of the above within Dragonfly's `configure` block. For example, if you wanted the default background to be red (FF0000) instead of black (000000) and the default size to be 200px by 200px

```ruby
Dragonfly.app.configure do
	plugin :imagemagick
	plugin :avatarmagick, background_color: 'FF0000', size: '200x200'
end
```

Or, if you wanted to change the default font

```ruby
Dragonfly.app.configure do
	plugin :imagemagick
	plugin :avatarmagick, font: 'OpenSans'
end
```

##### Choosing Fonts

To see what fonts are available, open up the terminal and type `convert -list font`. You can select any font listed when configuring or when calling `generate`.

## Usage

Once the plugin is installed and registered, you can use it like any of the other built-in generators (text, plain, etc.)

#### Basic

```ruby
image = Dragonfly.app.generate(:initial_avatar, "Bart Jedrocha")
```

will produce

![basic](https://user-images.githubusercontent.com/138067/52684554-a6744580-2f14-11e9-8468-dec84a102f01.png)

The plugin will extract at most 3 initials from the passed-in string (e.g. Bill James Pheonix MacKenzie will produce an avatar with the initials BJP). This should be sufficient for the vast majority of cases.

#### Playing with options

You can pass options to control the background color, text color, size, and font used to generate the avatar

```ruby
Dragonfly.app.generate(:initial_avatar, "Martin", {background_color: 'ff8f00'})
```

will produce

![bgcolor](https://user-images.githubusercontent.com/138067/52684569-b1c77100-2f14-11e9-88dd-3fc7355ed515.png)

```ruby
Dragonfly.app.generate(:initial_avatar, "James Hetfield", {background_color: 'f48fb1', color: '333333'})
```

will produce

![txtcolor](https://user-images.githubusercontent.com/138067/52684674-036ffb80-2f15-11e9-808b-26160a6f67e5.png)

```ruby
Dragonfly.app.generate(:initial_avatar, "Amanda Smith", {background_color: '00695c', size: '200'})
```

will produce

![size](https://user-images.githubusercontent.com/138067/52684661-fbb05700-2f14-11e9-8128-12bb2e667481.png)

```ruby
Dragonfly.app.generate(:initial_avatar, "Oliver Murphy", {background_color: 'b71c1c', font: 'Georgia'})
```

will produce

![font](https://user-images.githubusercontent.com/138067/52684652-f4894900-2f14-11e9-9b0e-c66e4d68bb35.png)

## Development

After checking out the repo, run `bash bin/setup` to install dependencies. Then, run `ruby bin/console` for an interactive prompt that will allow you to experiment. Please note that before being able to use the generator, you'll need to register both the [ImageMagick](http://markevans.github.io/dragonfly/imagemagick/) plugin and the AvatarMagic plugin

	irb > Dragonfly.app.configure do
	irb >   plugin :imagemagick
	irb >   plugin :avatarmagick
	irb > end

To install this gem onto your local machine, run `bundle exec rake install`.

## Roadmap

1. Add support for older versions (0.9.x) of Dragonfly

## Contributing

1. Fork it ( https://github.com/[my-github-username]/avatar_magick/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
