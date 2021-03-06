[![Build Status](https://travis-ci.org/contentful/contentful-bootstrap.rb.svg)](https://travis-ci.org/contentful/contentful-bootstrap.rb)

# Contentful Bootstrap

A small CLI tool for getting started with Contentful

## Contentful
[Contentful](http://www.contentful.com) is a content management platform for web applications,
mobile apps and connected devices. It allows you to create, edit & manage content in the cloud
and publish it anywhere via a powerful API. Contentful offers tools for managing editorial
teams and enabling cooperation between organizations.

## What does `contentful_bootstrap` do?
`contentful_bootstrap` let's you set up a new Contentful environment with a single command.

## How to Use

### Installation

```bash
$ gem install contentful_bootstrap
```

### Usage

You can create spaces by doing:

```bash
$ contentful_bootstrap create_space <space_name> [--template template_name] [--json-template template_path] [--mark-processed] [--config CONFIG_PATH]
```

You can also generate new Delivery API Tokens by doing:

```bash
$ contentful_bootstrap generate_token <space_id> [--name token_name] [--config CONFIG_PATH]
```

You can also generate JSON Templates from existing spaces by doing:

```bash
$ contentful_bootstrap generate_json <space_id> <delivery_api_access_token> [--output-file OUTPUT PATH] [--content-types-only]
```

You can update existing spaces from JSON Templates by doing:

```bash
$ contentful_bootstrap update_space <space_id> -j template_path [--mark-processed] [--skip-content-types]
```

### Built-in templates

Just getting started with Contentful? We have included the following built-in templates:

```
blog
gallery
catalogue
```

You can use these with the `--template` option to create some demo data and start developing
against our APIs right away. Once you've gotten comfortable, you can
[create your own templates](#json-templates) for quickly replicating testing & development spaces.

### Using from within other applications

Include `contentful_bootstrap` to your project's `Gemfile`

```ruby
gem "contentful_bootstrap"
```

Require `contentful_bootstrap`

```ruby
require 'contentful/bootstrap'
```

To Create a new Space

```ruby
Contentful::Bootstrap::CommandRunner.new.create_space("space_name")
```

Additionally, you can send an options hash with the following keys:

```ruby
options = {
  template: "blog", # Will use one of the predefined templates and create Content Types, Assets and Entries
  json_template: "/path/to/template.json", # Will use the JSON file specified as a Template
  mark_processed: false, # if true will mark all resources as 'bootstrapProcessed' and will be avoided for update_space calls (doesnt affect create_space)
  trigger_oauth: true # if true will trigger OAuth process
}
Contentful::Bootstrap::CommandRunner.new.create_space("space_name", options)
```

To Update an existing Space

```ruby
options = {
  json_template: "/path/to/template.json", # Will use the JSON file specified as a Template
  mark_processed: false, # if true will mark all resources as 'bootstrapProcessed and will be avoided on future update_space calls
  trigger_oauth: true, # if true will trigger OAuth process
  skip_content_types: false, # if true will avoid creating the content types
}
Contentful::Bootstrap::CommandRunner.new.update_space("space_id", options)
```

To Create a new Delivery API Token

```ruby
Contentful::Bootstrap::CommandRunner.new.generate_token("space_id")
```

Additionally, you can send an options hash with the following keys:

```ruby
options = {
  name: "Some Nice Token Name", # Will Create the Delivery API Token with the specified name
  trigger_oauth: true # if true will trigger OAuth process
}
Contentful::Bootstrap::CommandRunner.new.generate_token("space_id", options)
```

To Generate a JSON Template from an exising Space

```ruby
Contentful::Bootstrap::CommandRunner.new.generate_json(
  "space_id",
  access_token: "delivery_api_access_token",
  content_types_only: false # if true will not fetch Entries and Assets
)
```

Additionally, you can send an options hash with the following keys:
**NOTE**: The `:access_token` key is required in the options hash

```ruby
options = {
  access_token: "access_token" # REQUIRED
  filename: "template.json" # Will save the JSON to the specified file
}
Contentful::Bootstrap::CommandRunner.new.generate_json("space_id", options)
```

Optionally, `CommandRunner#new` will take a parameter for specifying a configuration path

### Configuration

Contentful Bootstrap will read by default from `~/.contentfulrc`, but you can provide your own
file by using the `--config CONFIG_PATH` parameter

If you don't have `~/.contentfulrc` created, you will be prompted if you want to create it

#### Configuration Format

The configuration file will be in `ini` format and looks like the following

```ini
[global]
CONTENTFUL_MANAGEMENT_ACCESS_TOKEN = a_management_access_token

[space_name]
SPACE_ID = some_space_id ; Space configurations are not required by this tool, but can be generated by it
CONTENTFUL_DELIVERY_ACCESS_TOKEN = a_delivery_acces_token
```

### JSON Templates

Using the `--json-template` option, you can create spaces with your own predefined content.
This can be useful for creating testing & development spaces or just starting new projects from
a common baseline. You can find a complete example [here](./examples/templates/catalogue.json)

Using the `--mark-processed` option alongside `--json-template` will mark all resources as `bootstrapProcessed`,
which will make it so `update_space` calls avoid already created resources. (A resource being either a Content Type, Entry or Asset).

## Contributing

Feel free to improve this tool by submitting a Pull Request. For more information,
please check [CONTRIBUTING.md](./CONTRIBUTING.md)
