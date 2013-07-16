# Leanpubbin'

The best way to get started with Leanpub is just to dive in, into a sample book. Open up the folder for your first book
. You should see _convert\_html_ and _manuscript_ folders. 

Open up the manuscript folder. You will see a Book.txt file and a chapter1.txt file.

Book.txt is an index of files. Files are ignored (not include in the book) until added to the index. Leanpub takes 
Book.txt, parses it line by line and pulls in each file and parses it with Kramdown. At some point in the future
Leanpub takes this joined Kramdown file and converts it to pdf, epub and mobi formats.

If we wanted to create the structure for a Leanpub book manually we would do the following:

Create the manuscript folder:

{:lang="sh"}
    $ mkdir manuscript

Go into the folder:

{:lang="sh"}
    $ cd manuscript

Create a Book.txt file:

{:lang="sh"}
    $ touch Book.txt

Now we add chapters to our index:

    001_Introduction.md

and so on..

We need to follow two rules; 1. Heading 1's (a single pound symbol `#`) are chapter titles. Heading 2s (two pound
symbols `##`) are section tiles. 

When formated as Markdown a Leanpub book looks like:

{:lang="md"}
    # Chapter Title

    Chapter contents

    ## Section Title

    Section contents

## Leanpub Markdown flavor

Leanpub Markdown is based on Kramdown. The only thing you really need to know beyond 
[Kramdown's documentation](http://kramdown.rubyforge.org/quickref.html) is:

1. Use h1's for chapter titles. 
2. Use h2's for section titles.

The h1's and h2's will be parsed and turned into your Table of Contents, so it's important you don't utilize them for
things other than chapter titles and section titles.

### Images in Leanpub

You're probably going to want to use some images. All assets are linked relative to your manuscript folder. You place
your images in _manuscript/images_. You can then link to them in the typical Markdown fashion.

For example;

{:lang="md"}
    ![Adventure Time](images/finn.png)

They need to be 300ppi if you intend to distribute them via print. Create them at 300ppi anyway just in case. 

You can pass attributes like width and height to Leanpub images and they will be reflected in the final output. For
example:

{:lang="md"}
    {width=60%,float=left}

    ![This is the Image Floated Left](images/LeanpubLogo1200x610_300ppi.png)

However, I recommend you don't take advantage of Leanpub's image attributes. They are not parsed like Kramdown's and
their syntaxes are incompatible. It will result in stray text (e.g `{width=60%}`_ appearing when you use Kramdown to
generate PDFs. The best solution is to simply not rely on image attributes. If you need image control in the final
published PDF, then use attribute lists, which are compatible with Kramdown. 

The Kramdown syntax looks like this instead:

{:lang="md"}
    ![Image](images/screengummi.png)
    {:width=100%}

If you need your images in 300ppi (e.g after taking a screenshot) the easiest thing to do is run an imagemagick command
against it:

{:lang="sh"}
    $ convert -units PixelsPerInch image -density 300 outimage

Setting the density will be adequate if all you intend to do is distribute your book digitally. If you do print you'll
have to make sure to take them at retina resolutions and then do some post re-sampling. Something like:

{:lang="sh"}
    $ convert -units PixelsPerInch image -resample 300 outimage

See: the [how-to-insert-an-image](https://leanpub.com/help/manual#how-to-insert-an-image) 
part of the Leanpub manual for more details.

## Previewing Leanpub locally

Previewing is central to a Leanpub workflow, you'll be doing it a lot. So much, that manually previewing is likely to
get annoying. To keep your sanity you are going to need a variety of methods to quickly preview changes. Methods that
don't require you to wait five minutes for Leanpub. Leanpub should be reserved for previewing the final product.

T> Previews from Leanpub will be generated into your Dropbox folder in _/Preview_. You can manually edit these if you 
T> want to get clever but I highly recommend you only use them for previewing.


### Preview with Sublime

Thankfully someone has built an extension for this called 
[sublimetext-markdown-preview](https://github.com/revolunet/sublimetext-markdown-preview).
We can install it using Package Manger;

1. *Shift* + *CRTL* + *P*. 
2. Install Package: _Markdown Preview_.

When you have it installed you can just *CRTL* + *SHFIT* + *P* and preview in the browser.

### Preview with VIM

There are a few scripts for this. Here is [one](http://www.vim.org/scripts/script.php?script_id=3994) 

### Preview with Guard and HTML

A good cross-platform, cross-editor, solution, is to monitor your Markdown files and generate HTML, then you can just
use your browser to preview. This solution has the added benefit of allowing us to generate previews using Kramdown,
which means our previews will support everything Leanpub does. 

How do we do this? We will use a rubygem called *guard* and a guard script that monitors Markdown files.

First we have to install Guard. The process is fairly simple:

{:lang="sh"}
    $ gem install guard

A> Add `sudo` if installing to system. Drop the `sudo` on Mac. Also drop sudo when using rbenv. On windows, drop 
A> everything, run around screaming, then install a decent Operating System.

In your ebook folder run:

{:lang="sh"}
    $ guard init

Now install the markdown monitor:

{:lang="sh"}
    $ gem install guard-markdown

Add an example guard for Markdown to your Guardfile by running:

{:lang="sh"}
    $ guard init markdown

It will generate something like this:

{:lang="ruby"}
    guard 'markdown', :convert_on_start => true, :dry_run => true do  
      watch (/source_dir\/(.+\/)*(.+\.)(md|markdown)/i) { |m| "source_dir/#{m[1]}#{m[2]}#{m[3]}|output_dir/#{m[1]}#{m[2]}html"}
    end

We want to change the source dir to `/manuscript, set the output_dir, and enable it:

{:lang="ruby"}
    guard 'markdown', :convert_on_start => true do  
      watch (/manuscript\/(.+\/)*(.+\.)(md|markdown)/i) { |m| "manuscript/#{m[1]}#{m[2]}#{m[3]}|preview_html/#{m[1]}#{m[2]}html"}
    end

Launch it with:

{:lang="sh"}
    $ guard

A> Run this in the ebook's folder

Now we need to make sure our browser gets reloaded when the files change. We can do this with _guard-livereload_ and a
[LiveReload](http://livereload.com) browser extension. Once you have your relevant browser extension installed do the following:

{:lang="sh"}
    $ gem install guard-livereload

Then add the following to your Guardfile:

{:lang="ruby"}
    guard 'livereload' do
      watch(%r{preview_html/.+\.(html)})
    end

Note: You might want to add _preview\_html_ to your .gitignore file as well.

### Other Preview Apps

For Mac I recommend [MouApp](http://mouapp.com). For Linux there is
[ReText](http://sourceforge.net/p/retext/home/ReText)

Remember to follow the golden rule of *Keeping it Simple*. Your Markdown productivity shouldn't be tied to any
specific app. Choose things that complement your workflow not supplement it.
