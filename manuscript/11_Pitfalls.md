# Pitfalls

Chances are you are gong to fall into a pit sooner or later. Holes abound in the land of code and Markdown. These
sections help you avoid frustration and increase the delight of crafting your ebook.

## Markdown Issues

Markdown can go wrong, don't let it go wrong on you. If it does go wrong, you can turn to the following headings for
help. Trust them, they're headings.

### Duplicate Link Issues

    Warning: Duplicate link ID '1' - overwriting

This occurs when you have duplicate links in your Markdown. This can happen when you have two of the same links or a
link without a title. The solution is to use link references:

{:lang="md"}
    A [link][kramdown hp] to the homepage.

    [kramdown hp]: http://kramdown.rubyforge.org "hp"

### Footnote ID issues

    Warning: Footnote marker '1' already appeared in document, ignoring newly found marker

To resolve this use unique Footnote tags instead of numbers. For example:

{:lang="md"}
    This has a footnote [^cats]

    [^cats]: The footnote

### Footnote problems

Sometimes you want to stick some stuff in your footnotes that doesn't quite work out formatting wise. Usually, it will
be something like code snippets. The solution is to place your footnotes on a separate page and link to them via
fragment IDs. It works by specifying a block element with an ID so you can link to it. 

You can add an ID to any block element. For example, headers:

{:lang="md"}
    # Header
    {:#the-fragment-id}

Or on paragraphs: 

{:lang="md"}
    Text about things here. Have you heard about the thing? Have you read the thing? Have you seen
    the thing? Do you believe in the thing? Can you feel the thing? Can you sense the thing? Can you touch the thing? Can you taste the thing? 
    {:#the-fragment-id}

Any block element will do. Once you have a block element to link to, creating a link is simple:

{:lang="md"}
    [I am a link. This is my purpose, but it does not define me, dl's do.](#the-fragment-id)

This will generate a link to the section of the place where _#the-fragment-id_ is. It works on HTML, LaTeX and PDF
formats out of the box. Only _why knows what it works on in the box.

### Sometimes you need to hide

Occasionally you need to to hide a element from Kramdown parsing and simply return nothing. For example, in this book I
have a section called `## Links` in which I have placed all my link definitions. Link definitions don't show up in the
final output because they are not meant to, they're just metadata.

This creates a problem though, the heading `## Links` shows up, but with nothing in its section. It would be nice 
to hide the heading as well so we don't have an empty section that confuses readers

This is fairly easy to add and simply involves overriding the convert method on any converter class to look like:

{:lang="ruby"}
    # Dispatch the conversion of the element +el+ to a +convert_TYPE+ method using the +type+ of
    # the element.
    #
    # Will return an empty string if the class 'hide' is present
    def convert(el, opts = {})
      hide = el.attr['class'].to_s =~ /\bhide\b/
      if hide
        ''
      else
        send("convert_#{el.type}", el, opts) 
      end
    end

This will return nothing (and thus hide the block from output) if the _hide_ class is present. We can use it like this:

{:lang="md"}
    ## Links
    {:.hide}

We could test this now by running a Markdown document through it but unfortunately it would fail to work. What have we missed? Let's add an inspect to the convert method and then run stuff through it again. 

The inspect looks like this:

{:lang="ruby"}
    def convert(el, opts = {})
      puts el.inspect
      # .... Rest of method
    end

Running this results in the following output:

{:lang="sh"}
    <kd:root nil>
    ...

We don't get any other elements outputted. This means everything goes through the root element. Taking a look at
_convert\_root_ reveals that we need to override the inner method:

{:lang="ruby"}
    def convert_root(el, opts)
      inner(el, opts)
    end

All elements with the exception of the root element are passed through the inner method. The inner method takes
each child element and loops through them appending results for each one. It eventually returns a string containing the entire converted document:

{:lang="ruby"}
    el.children.each_with_index do |inner_el, index|
      options[:index] = index
      options[:result] = result
      result << send("convert_#{inner_el.type}", inner_el, options)
    end
    result

Make the following changes to the inner method:

{:lang="ruby"}
    # Return the converted content of the children of +el+ as a string.
    def inner(el, opts)
      result = ''
      options = opts.dup.merge(:parent => el)
      el.children.each_with_index do |inner_el, index|
        options[:index] = index
        options[:result] = result
        hide = inner_el.attr['class'].to_s =~ /\bhide\b/
        if hide
          result << ''
        else
          result << send("convert_#{inner_el.type}", inner_el, options)
        end        
      end
      result
    end

Now running the Markdown through the converter results in the Links heading being hidden as expected.

## LaTeX to PDF 

    ! Package Listings Error: Couldn't load requested language.

This is due to a bad language definition on a codeblock. Check your codeblock to make sure the language is supported by
Listings. See [LaTeX supported Languages](http://en.wikibooks.org/wiki/LaTeX/Source_Code_Listings#Supported_languages) for a list of supported languages

### And so we copied gibberish

If you have issues with copying text from your generated PDFs it is usually due to encoding issues or missing font
glyphs. You can usually resolve the issues by adding the following lines to your template:

{:lang="erb"}
    <% if RUBY_VERSION >= '1.9' %>
    \usepackage[<%= encmap[@body.encoding.name] %>]{inputenc}
    <% else %>
    \usepackage[mathletters]{ucs}
    \usepackage[utf8x]{inputenc}
    <% end %>
    \usepackage[T1]{fontenc}
    \usepackage{lmodern}

Resolving the text encoding issues is done with:

{:lang="erb"}
    \usepackage[utf8x]{inputenc}

And the font issues can be resolved with:

{:lang="erb"}
    \usepackage[T1]{fontenc}
    \usepackage{lmodern}

The default font encoding on TeX is 7-bit, this limits fonts to 128 glyphs. 128 glyph fonts lack many characters
(accents etc) and will cause issues when you try to copy the missing chars. T1 font encoding is 8-bit and allows fonts
that have 256 glyphs. The _lmodern_ package includes all those good old latin glyphs that keep latin languages
functioning.

### Issues with new lines

By default TeX likes to format new paragraphs like:

{:lang="tex"}
         This is a new paragraph.  Can 
    you see the indent?  
         This is another paragraph. 

In most circumstances this is undesirable. There are two easy solutions; 

Abbreviate the paragraph with `\noindent`:

{:lang="tex"}
    A paragraph.

    \noindent A second paragraph

*Or*

Set the package parfill to do this by default:

{:lang="tex"}
    \usepackage[parfill]{parskip}

To get that in your documents just add it to your Kramdown LaTeX template.

## HTML, Epub, Mobi etc

Markdown and PDF aren't alone with their problems. HTML can have problems to.

### Code without styles.

CodeRay relies on css that you'll need to add to your template. To get it you can do:

{:lang="sh"}
    $ coderay stylesheet > coderay.css

Just add that CSS to the HTML template that you pass to Kramdown and you'll be good to go.

### Code highlighting wrong language

    CodeRay::Scanners could not load plugin :bash; falling back to :text

If we run the HTML converter against code blocks we will discover that CodeRay doesn't support the same name for 
formats as LaTeX. The only solution to this is to add a dictionary that associates LaTeX names with their equivalent 
CodeRay name or the CodeRay name with it's LaTeX equivalent.

The only question then, is who do we accept as the authority? CodeRay or Listings? CodeRay is more of a standard than
Listings and it supports short names which Listings does not. It's an easy choice.

Now we just need to know the names CodeRay uses for languages and the names Listings uses. The CodeRay short names are
based on Pygments so we can use it as a reference; consult the short names [here](http://pygments.org/docs/lexers). The
Listing supported languages can be found
[here](http://en.wikibooks.org/wiki/LaTeX/Source_Code_Listings#Supported_languages)s

Open up _kramdown/converter/latex.rb_ and add a hash for the associations to the initialize method:

{:lang="ruby"}
    @codeblock_lang_to_listings = {
      :ruby => 'Ruby',
      :sh   => 'bash',
      :text => 'Clean',
      :tex  => 'TeX',
      :js   => 'VBScript',
      :json => 'VBScript'
    }

To use this we only need to modify the lang var in _convert_codeblock_:

{:lang="ruby"}
    lang = extract_code_language(el.attr)
    lang = 'Clean' unless lang
  
    if @codeblock_lang_to_listings.include?(lang.to_sym)
      lang =  @codeblock_lang_to_listings[lang.to_sym] 
    else
      lang = 'Clean'
    end

That is it!
