# Using Jasmine with the asset pipeline #

Jasmine does not currently support the asset pipeline, but there's a lovely gem called [jasmine-rails](https://github.com/searls/jasmine-rails) that will give us this support. Used in conjunction with [jasmine-headless-webkit](http://johnbintz.github.com/jasmine-headless-webkit/), you can run your specs in Terminal, similar to `rspec` rather than in the browser (and it's fast!).

## Add to your Gemfile

First, lets add the necessary dependencies.

    group :development, :test do
      gem 'jasmine-rails'
      gem 'jasmine-headless-webkit'
    end

## Initialize jasmine

    jasmine init

Then remove the extra files it generates.

    rm -rf spec/javascripts/helpers spec/javascripts/PlayerSpec.js
    rm -rf public/javascripts/Player.js public/javascripts/Song.js

## Update jasmine.yml

Because we're using the asset pipeline, we need to tell jasmine where our assets are.

    src_dir: app/assets/javascripts

    asset_paths:
      - vendor/assets/javascripts

    src_files:
      - "vendor/**/*.{js,coffee}"
      - "lib/**/*.{js,coffee}"
      - "app/**/*.{js,coffee}"

    spec_files:
      - "**/*[Ss]pec.{js,coffee}"

    helpers:
      - "helpers/**/*.{js,coffee}"

    spec_dir: spec/javascripts

## Make a sanity test

At this point, jasmine should be working. Lets make a sanity test to check. Create a file in `spec/javascripts` called `sanity_spec.coffee`.

    describe "Sanity Check", ->
      it "demonstrates a successful test", ->
        expect(1).toEqual 1

Then run `jasmine-headless-webkit -c` (that `-c` is for color).

    Running Jasmine specs...
    .
    PASS: 1 test, 0 failures, 0.007 secs.

Profit!

## A note on OSX Lion ##

If you're using Lion, you'll likely get a message like...

    Can't load, the file may be broken.

...in which case you'll want to load the gem directly from Github.

    gem 'jasmine-headless-webkit', :git => https://github.com/johnbintz/jasmine-headless-webkit.git

In some cases you may even need to install from source. Don't worry, it's not hard!

    git clone https://github.com/johnbintz/jasmine-headless-webkit.git
    cd jasmine-headless-webkit
    gem build jasmine-headless-webkit.gemspec
    gem install jasmine-headless-webkit.gem
