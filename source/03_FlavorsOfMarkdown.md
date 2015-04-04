# Flavors of Markdown

You might think you know Markdown, but chances are you only know _a Markdown_. The Markdown you know might not be the
Markdown you get to use. All Markdown implementations handle the basics but you might not be aware of what the basics
[^MarkdownExtensionsSpec] actually are. Automatic link parsing, for example, is not part of the standard, it's just
something that everyone and their biological birthing entity implements. There are three major flavors of extended
Markdown you are likely to encounter.

[^MarkdownExtensionsSpec]:  
    An official extension spec is being worked on but the details are pretty sparse. See 
    [The Future of Markdown](http://www.codinghorror.com/blog/2012/10/the-future-of-markdown.html) 
    and [this commit](https://github.com/vmg/sundown/commit/37728fb2d7137ff7c37d0a474cb827a8d6d846d8)

## Github flavored Markdown 

This [implementation](http://github.github.com/github-flavored-markdown) has been made enormously popular by the ubiquity of Github in the development community. It
has a few features not found in the original Markdown spec, namely; fenced code blocks and automatic link parsing. The
de facto implementation of this flavor is [redcarpet](https://github.com/vmg/redcarpet)

## MultiMarkdown

Like many Markdown flavors, [MultiMarkdown](http://fletcherpenney.net/multimarkdown) originated as the de facto implementation of Markdown for a
language; originally written for Perl, MultiMarkdown has since become a [C library](https://github.com/jgm/peg-markdown). It is one of the 
most well documented flavors but it lacks a decent adoption rate/userbase.

## Markdown Extra

[This](http://michelf.ca/projects/php-markdown/extra) variant came originally from PHP but has since been implemented
in several languages. Markdown Extra has a few extra features like; footnotes, extensions, attribute lists etc.

We will be using an implementation of Markdown Extra called Kramdown. There are two reasons for this:

1. [Leanpub](https://leanpub.com) uses it.
2. It is written in Ruby which is easy to hack on.

For the most part, the only two flavors you will have to deal with are Github's and Markdown Extra. The differences
between the two aren't too significant. Most of what Github does is extra for Github and won't effect your needs for
ebook writing. Utilizing Github features just adds stuff when your Markdown is used on Github and utilizing Kramdown
features just adds stuff when using Kramdown.

The only difference you need to care about is the syntax for fenced code blocks. Github utilizes three \`'s and Kramdown uses
three `~~~`.

They look like:

*Github*

    ```ruby
    require 'redcarpet'
    markdown = Redcarpet.new("Hello World!")
    puts markdown.to_html
    ```

*Kramdown*

    ~~~ ruby
    require 'redcarpet'
    markdown = Redcarpet.new("Hello World!")
    puts markdown.to_html
    ~~~

Since you cant modify the parsing on Github, if you plan to publish the same Markdown on Github and need the code
highlighting you'll need to modify Kramdown to use the same indicator as Github for starting and closing fenced code
blocks. 

A> Alternatively you can just use indentation to specify your code blocks which is compatible across most 
A> implementations

Changing `~` to a \` will get it parsing like Github. To do that just search for `FENCED_CODEBLOCK_START` in the
Kramdown files. [Monkey-patching](#monkeypatching) is the simplest way to get the modification into Kramdown. In the
interest of clarity, the following code snippet will accomplish said changes:

{.lang="ruby"}
    module Kramdown
      module Parser
        class Kramdown
          FENCED_CODEBLOCK_START = /^`{3,}/
          FENCED_CODEBLOCK_MATCH = /^(`{3,})\s*?(\w+)?\s*?\n(.*?)^\1`*\s*?\n/m
        end
      end
    end

If you plan to use Leanpub you'll need to utilize indented codeblocks instead. You'll indent things using four 
spaces then manually specify your language using attribute lists. Like this:

{:lang="md"}
    This is a codeblock

    {:lang="ruby"}
        cats = 'demons'

Beyond that, just follow the [docs for Kramdown](http://kramdown.rubyforge.org/quickref.html) and you'll be fine.

## Learn your Markdown

You might be tempted to avoid learning Markdown and to add a Markdown GUI to your workflow. I urge you to resist that
temptation. Jumping around in app menus for simple things like links and headings will kill your workflow. There is a
reason why some writers from past eras still choose to use typewriters, being 100% comfortable with your tools is
extremely important for productivity. You'll need to know your Markdown inside and out if you ever hope to finish a
book.

Any app that attempts to replace the Markdown text process is going to interfere with your workflow in the long run. If
you use Markdown apps only use them to complement your writing, with fancy previews and nifty publishing tools. Don't
use an app as a crutch. Using crutches will make you disabled.
