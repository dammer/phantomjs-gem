# Phantomjs as a Rubygem

I am lazy as hell, and wanted to be able to install [PhantomJS](http://phantomjs.org) via Rubygems/Bundler
when using [poltergeist](https://github.com/jonleighton/poltergeist).

It keeps installations of phantomjs in `$HOME/.phantomjs/VERSION/PLATFORM`. When you call `Phantomjs.path`, it
will return the path to the phantomjs executable in there. If that is not present, it will first fetch and
install the prebuilt packages suitable for the current plattform (currently Linux 32/64 or OS X supported).

**TL;DR:** Instead of manually installing phantomjs on your machines, use this gem. It will take care of it.

## Example

    require 'phantomjs'
    Phantomjs.path # => path to a phantom js executable suitable to your current platform. Will install before return when not installed yet.

## Usage with Poltergeist/Capybara

Add this to your `Gemfile`:

    group :test do
      gem 'phantomjs', :require => 'phantomjs/poltergeist'
    end

This will automatically require (and install) phantomjs and configure Capybara in the same way as noted below for
manual setup

### Manual setup

Add `gem 'phantomjs', :group => :test` to your `Gemfile` and run `bundle`. In your test/spec helper, re-configure
the Poltergeist capybara driver to use the phantomjs package from this gem:

    require 'phantomjs' # <-- Not required if your app does Bundler.require automatically (e.g. when using Rails)
    Capybara.register_driver :poltergeist do |app|
      Capybara::Poltergeist::Driver.new(app, :phantomjs => Phantomjs.path)
    end

## A note about versions.

The gem version consists of 4 digits: The first 3 indicate the phantomjs release installed via this gem,
the last one is the internal version of this gem, in case I screw things up and need to push another release
before

## Contributing

**Warning**: The `spec_helper` calls `Phantomjs.implode` when it is loaded, which purges the `~/.phantomjs`
directory. This is no bad thing, it just means every time you run the specs you'll download and install all
three packages over, so tread with caution please :)

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Copyright

(c) 2012 Christoph Olszowka

Note that this project merely simplifies the installation of the entirely separate PhantomJS project
via a Ruby gem. You can find the license information for PhantomJS in PHANTOMJS_LICENSE.BSD and further
information at http://http://phantomjs.org/