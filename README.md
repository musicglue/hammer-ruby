# hammer-ruby
This is the repo used for building MRI ruby binaries for the [heroku ruby buildpack](https://github.com/heroku/heroku-buildpack-ruby).

## Building a Ruby
This tool uses [hammer](https://github.com/hone/hammer) for building binaries on heroku. You'll need to install the hammer gem first.

```sh
$ gem install hammer --pre
```

Next you can build the binary. For instance `2.0.0-p353`.

```sh
$ hammer build --env VERSION:2.0.0-p353
```

This puts a `*.tgz` binary in the `builds/` folder.

NOTE: This has only been tested with Ruby 1.8.7 and higher.

### Enviroment Variables
hammer supports passing build options as environment variables as we saw above for specifying the ruby version with `VERSION`. 

NOTE: hammer uses a `:` instead of a `=` for setting the options.

* `VERSION` - This is the ruby version being used. It's expected to be in the format: `"#{MAJOR}.#{MINOR}.#{TEENY}-p#{PATCH}"`
* `BUILD` - If this flag is set (can be set to anything for true), then hammer-ruby will build a "build" ruby. This sets the prefix to `"/tmp/ruby-#{MAJOR}.#{MINOR}.#{TEENY}"`. This is required for ruby `1.8.7` and `1.9.2` since the `--enable-load-relative` flag does not work properly. `--enable-load-relative` allows a ruby to be executed from anywhere and not just the prefix directory. By required, you need to build two binaries: one where `BUILD` is false (runtime ruby) and where `BUILD` is true (build ruby).
* `DEBUG` - If this flag is set (can be set to anything for true), then hammer-ruby will set debug flags in the binary ruby being built.
* `RUBYGEMS_VERSION` - This allows one to specify the Rubygems version being used. This is only required for Ruby 1.8.7 since it doesn't bundle Rubygems with it.
* `GIT_URL` - If this option is used, it will override fetching a source tarball from <http://ftp.ruby-lang.org/pub/ruby> with a git repo. This allows building ruby forks or trunk. This option also supports passing a treeish git object in the URL with the `#` character. For instance, `git://github.com/hone/ruby/ruby.git#ruby_1_8_7`.
* `S3_BUCKET_NAME` - This option is the S3 bucket name containing of dependencies for building ruby. If this option is not specified, hammer-ruby defaults to "heroku-buildpack-ruby". The dependencies needed are `libyaml-0.1.4.tgz` and `libffi-3.0.10.tgz`.
