# Prepping for Publishing.

Once we have our book written it's time to distribute it. By now we have the essential in-between formats: latex,  pdf
and html but we are going to need to wrap these up in the various wrappers for various ebook readers. We are close,  but that last 1% can be killer enough to kill a product, or at least delay it. s

## Converting to epub

Before we get started converting things we need to know what is we are converting to. Mobi and epub are at their core 
just html. The formats are just a wrapper around html files. If we are going to get to epub the best route is
through html.

Converting to epub will consist of three steps;

1. Get our Markdown to HTML using Kramdown
2. Tidy up the HTML into XHTML . Epub is very picky about its html and wants it perfect.
3. Convert the XHTML to Epub using [Calibre's](http://calibre-ebook.com) ebook-convert command line tool.
4. Edit the epub using [Sigil](http://code.google.com/p/sigil), an epub editor.

First install html tidy:

*Ubuntu*

{:lang="sh"}
    $ sudo apt-get install tidy

*Homebrew*

{:lang="sh"}
    $ sudo brew install tidy

And Calibre:

*Ubuntu*

{:lang="sh"}
    $ sudo apt-get install calibre

*Mac*

Visit the [download page](http://calibre-ebook.com/download) and install it.

Then install Sigil:

*Ubuntu*

{:lang="sh"}
    $ sudo add-apt-repository ppa:rgibert/ebook
    $ sudo apt-get update
    $ sudo apt-get install sigil

*Mac*

Visit the [download page](http://code.google.com/p/sigil/downloads/list) and install it.

Now run kramdown to generate the html:

{:lang="sh"}
    $ kramdown Joined.md -o html --template document > Joined.html

A> If you want any custom css styling you can add it to your template and pass it into Kramdown that way. Try to not
A> manually edit output files in a way that cant be automated at some point.

Then we clean it up:

{:lang="sh"}
    $ tidy -asxhtml -output tidy.xhtml Joined.html

And finally we convert it into epub:

{:lang="sh"}
    $ ebook-convert tidy.xhtml Joined.epub

Once you have an epub you can tweak it using an epub editor like Sigil to add things like metadata.

## From epub to mobi and beyond

Once we have an epub getting to similar wrapper formats is just a matter of passing it through ebook-convert and then
tweaking the final file with an editor for that format. For example;

*Mobi*

{:lang="sh"}
    $ ebook-convert Joined.epub Joined.mobi

*AZW3*

{:lang="sh"}
    $ ebook-convert Joined.epub Joined.azw3

Take a look at the documentation for ebook-convert if you get stuck. The docs are [at](http://manual.calibre-ebook.com/cli/ebook-convert.html) and written
in good old Sphinx style.

## Cover Images

Cover images are a bit more complicated than you might first think. This is because we cant simply add a full width
image page to the book file itself. Having the cover in the book would result in two
cover pages in epub readers. We are going to have to craft a separate solution for each format.

For LaTeX, we can just add it to a document template and then pass it into Kramdown when we need a PDF with a cover.

To do this on LaTeX we need the following packages:

{:lang="tex"}
    \usepackage[overlay]{textpos}
    \usepackage{wallpaper}
    \usepackage{graphicx}

Then place this right after the `\begin{document}`:

{:lang="tex"}
    \begin{textblock*}{297mm}(0mm,0mm)
    \ThisTileWallPaper{\paperwidth}{\paperheight}{images/title_page.jpg}
    \end{textblock*}
    \cleardoublepage

You can then just pass this to Kramdown when you generate the LaTeX. Adding cover pages in epub and mobi can be
accomplished with [Calibre](http://calibre-ebook.com), an ebook conversion tool you have probably heard of before.

Covers can simply be passed into Calibre. For example:

{:lang="sh"}
    $ ebook-convert tidy.xhtml Joined.epub --cover images/title_page.jpg

## Some brief notes on styling things

You might be tempted to really go heavy on your customization of your ebooks. You may even spend hours searching for
tips on how to customize PDFs, which are a complete pain to style. Don't waste your time. Adding custom styles
significantly interferes with their readability. Readers like (and often expect) things to look a certain way; it's why
readability extensions are so popular.

Rely on the default styles as much as possible and only customize when it's necessary to improve the UX of the book.
This minimalist approach seems like it might interfere with creativity but it only redirects it to where it matters
most.