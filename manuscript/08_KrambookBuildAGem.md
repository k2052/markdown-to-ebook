# Krambook: Build a Gem

It's time to package up all these custom Kramdown extensions into a Gem.

T> If you so desire you can skip this section and simply visit the [repo](http://github.com/k2052/krambook) and
T> install the completed gem.

First create a directory for the gem:

{:lang="sh"}
    $ mkdir krambook
    $ cd krambook

Now we need to add a gemspec file:

{:lang="sh"}
    $ touch krambook.gemspec

Add the contents:

{:lang="ruby"}
    #!/usr/bin/env gem build
    # encoding: utf-8

    Gem::Specification.new do |s|
      s.name                      = 'krambook'
      s.rubyforge_project         = 'krambook'
      s.authors                   = ["K-2052"]
      s.email                     = 'k@2052.me'
      s.summary                   = 'Kraft you ebooks with Kramdown.'
      s.homepage                  = 'http://github.com/bookworm/krambook'
      s.description               = 'Kraft you ebooks with Kramdown. Intended as a collection 
                                     of helpers for working with Markdown based ebooks. 
                                     Currently operates like a local version of LeanPub.'
      s.required_rubygems_version = '>= 1.3.6'
      s.version                   = '0.0.1'
      s.date                      = Time.now.strftime("%Y-%m-%d")

      s.files            = `git ls-files`.split("\n") | Dir.glob("{lib}/**/*")
      s.test_files       = `git ls-files -- {test,spec,features}/*`.split("\n")
      s.executables      = `git ls-files -- bin/*`.split("\n").map{ |f| File.basename(f) }
      s.require_paths    = ['lib']
      s.rdoc_options     = ['--charset=UTF-8']
      
      s.add_dependency('kramdown')
      s.add_dependency('bundler', '~> 1.0')
      s.add_dependency('thor')
      s.add_dependency('active_support')
    end

Most of these lines are self explanatory. We use git to pull our files and we add _Thor_ as a dependency. Thor is a
wrapper around command line and file generation stuff. Think rake, but on steroids. Thor will allow us to construct
minimal code for handling arguments.

Add a Gemfile for bundler's sake:

{:lang="ruby"}
    source 'https://rubygems.org'

    gemspec

This tells bundler to use the gemspec as a source for the gems. Run `$ bundle install` to make sure things are
installed.

Now we need to a base file to load up our gem file files when someone does `require 'krambook'`:

{:lang="sh"}
    $ mkdir lib
    $ cd lib
    $ touch krambook.rb

Open up `krambook.rb` and define a class for the CLI interface:

{:lang="ruby"}
    require 'kramdown'
    require 'thor'

    module Krambook
      class CLI < Thor
        include Thor::Actions

        class_option :type, :desc => 'The type to convert to', :aliases => '-t', :default => 'kramdown', :type => :string
        class_option :help, :type => :boolean, :desc => 'Show help usage' 
        default_task :join

        def join
        end
      end
    end

Now on to our convert method. We need to add the code we created earlier and modify it to to use arguments passed to it.

{:lang="ruby"}
    def join
      files            = []
      joined_file_path = File.expand_path './manuscript/' + 'Joined.md'
      File.delete(joined_file_path) if File.exist?(joined_file_path)

      File.open(File.expand_path('./manuscript/Book.txt'), 'r') do |f|
        f.each_line do |line|
          files << line.gsub(/\s+/, "")
        end
      end

      File.open(joined_file_path, 'a') do |f|
        files.each do |file_path|
          full_file_path = File.expand_path './manuscript/' + file_path
          f.puts Kramdown::Document.new(IO.read(full_file_path)).send("to_#{options[:format]}".to_sym) if File.exists?(full_file_path)
          f.puts '' unless file_path = files.last
        end
      end
    end

Let's add that external file inclusion converter we created earlier to `lib/kramdown/converter`:

{:lang="ruby"}
    module Kramdown
      module Converter
        class Includey < ::Kramdown::Converter::Kramdown
          def convert_codeblock(el, indent)
            if el.attr.include?('include')
              file_path = File.expand_path(Dir.pwd + '/' + el.attr['include'])
              el.value  = IO.read(file_path) if File.exists?(file_path)
            end

            attr = el.attr.dup
            lang = extract_code_language!(attr)  
            "~~~ #{lang}\n" + el.value.split(/\n/).map {|l| l.empty? ? "" : "#{l}"}.join("\n") + "\n~~~\n"
          end
        end
      end
    end

Remember to require it at the top of `lib/krambook.rb`

{:lang="ruby"}
    require 'kramdown'
    require 'kramdown/converter/includey'

We want this to work from CLI so do the following.

{:lang="sh"}
    $ mkdir bin
    $ touch bin/krambook
    $ chmod a+x bin/krambook

Then add the following to _bin/krambook_:

{:lang="ruby"}
    #!/usr/bin/env ruby

    $LOAD_PATH.unshift File.join(File.dirname(__FILE__), '..', 'lib')

    require 'krambook'
    Krambook::CLI.start

We can test this by running the bin file in a directory with a book. For example:

{:lang="sh"}
    $ ruby /home/k2052/creations/krambook/bin/krambook

We get the following back:

{:lang="sh"}
    [WARNING] Attempted to create command "convert" without usage or description. Call desc if you want this method to be available as command or declare it inside a no_commands{} block. Invoked from "/home/k2052/creations/krambook/lib/krambook.rb:12:in `<class:CLI>'".
    Could not find command "".

We can fix both issues by adding a desc before the join method definition. 

Add the following to `krambook.rb`:

{:lang="ruby"}
    desc 'join', 'Joins the markdown files'
    def join

Run it again and you should have a _Joined.md'_ in the manuscript folder. Now it's just a matter of submitting our gem
to rubygems.

*Note:* Obviously you don't actually perform this step as I'm the maintainer of the Krambook gem.

First add a README.md file:

{:lang="sh"}
    $ touch README.md

Then do all the git stuff:

{:lang="sh"}
    $ git init .
    $ git add .
    $ git commit -m "first commit" -a

If your bin file is ignored you may have to do:

{:lang="sh"}
    $ gi]t add bin/ --force

Then we need to build the gem and submit:

{:lang="sh"}
    $ gem build krambook.gemspec
    $ gem push krambook-0.0.1.gem

Done!
