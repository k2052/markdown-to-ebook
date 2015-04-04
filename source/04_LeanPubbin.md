# Leanpubbin': A Brief Introduction To Leanpub

The best way to get started with Leanpub is just to dive in -- into a sample book. Visit [https://leanpub.com/books/new](https://leanpub.com/books/new) and follow the steps. 

Leanpub will have created a folder for your ebook on Dropbox. Open up the folder. You should see _convert\_html_ and _manuscript_ folders.

Open up the manuscript folder. You will see a Book.txt file and a chapter1.txt file.

*Book.txt* is an index of files. Files are ignored (not included in the book) until they're added to the index. Leanpub
takes Book.txt, parses it line by line, joins each file and then parses the joined file with Kramdown. At some point in
the future Leanpub takes the Kramdown file and converts it to pdf, epub and mobi formats.

If we wanted to create the structure for a Leanpub book manually we could do the following:

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

When formated as Markdown a Leanpub book looks like:

{:lang="md"}
    # Chapter Title

    Chapter contents

    ## Section Title

    Section contents

## Leanpub Markdown flavor

Leanpub Markdown is based on Kramdown. The main thing you need to know beyond 
[Kramdown's documentation](http://kramdown.rubyforge.org/quickref.html) is:

1. Use h1's for chapter titles. 
2. Use h2's for section titles.

The h1's and h2's will be parsed and turned into your Table of Contents, so it's important you don't utilize them for
things other than chapter titles and section titles.

### Images in Leanpub

All assets are linked relative to your manuscript folder. Images are placed in _manuscript/images_. Link to them using
the normal Markdown image syntax.

For example;

{:lang="md"}
    ![Adventure Time](images/finn.png)

They need to be at 300ppi if you intend to distribute you book via print. They will be upscaled to 300ppi automatically
so if you intend to distribute entirely through digital means you can forgo creating them at 300ppi. It might be a good
idea to create them at 300ppi anyway just to keep your options open.

If you need your images in 300ppi (e.g after taking a screenshot) the easiest thing to do is run an imagemagick command
against it:

{:lang="sh"}
    $ convert -units PixelsPerInch image -density 300 outimage

Setting the density will be adequate if all you intend to do is distribute your book digitally. If you do print you'll
have to make sure to take them at retina resolutions and then do some post re-sampling. Something like:

{:lang="sh"}
    $ convert -units PixelsPerInch image -resample 300 outimage

You can pass attributes like width and height they will be reflected in the final output. For
example:

{:lang="md"}
    {width=60%,float=left}

    ![This is the Image Floated Left](images/LeanpubLogo1200x610_300ppi.png)

I> See the [how-to-insert-an-image](https://leanpub.com/help/manual#how-to-insert-an-image) part of the Leanpub manual
I> for more details.

However, I recommend you don't take advantage of Leanpub's image attributes. They are not parsed like Kramdown's and
their syntaxes are incompatible. If you use Leanpub's syntax will result in stray text (e.g `{width=60%}`_ appearing
when you use Kramdown to generate PDFs. The best solution, is to simply not rely on image attributes.

If you absolutely need image control in the final published PDF then use attribute lists, which are compatible with
Kramdown.

The Kramdown syntax looks like this:

{:lang="md"}
    ![Image](images/screengummi.png)
    {:width=100%}

## Previewing Leanpub locally

Previewing is central to a Leanpub workflow, you'll be doing it a lot. So much, that manually previewing is likely to
get cumbersome. To keep your sanity you are going to need a variety of methods to quickly preview changes. Methods that
don't require you to wait for Leanpub to process your book.

T> Previews from Leanpub will be generated into your Dropbox folder in _/Preview_. You can manually edit these if you 
T> want to get clever but I highly recommend you only use them for previewing.

### Previewing with Sublime

Thankfully someone has built an extension for this called [sublimetext-markdown-preview](https://github.com/revolunet
/sublimetext-markdown-preview). We can install it using Package Manger;

1. *Shift* + *CRTL* + *P*. 
2. Install Package: _Markdown Preview_.

Once you have it installed you can just hit *CRTL* + *SHFIT* + *P* (P is for preview) and a browser window will open
with a preview.

### Previewing with VIM

There are a few scripts for this. Here is [one](http://www.vim.org/scripts/script.php?script_id=3994) 

### Previewing with Guard and HTML

A good cross-platform, cross-editor, solution, is to monitor your Markdown files and generate HTML when they change,
then you can just use your browser to preview the result. This solution has the added benefit of allowing us to 
generate previews using Kramdown, which means our previews will support everything Leanpub does.

To do this we will use [guard](http://guardgem.org) and a guard script that monitors Markdown files.

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
[LiveReload](http://livereload.com) browser extension. Once you have the relevant browser extension installed do the
following:

{:lang="sh"}
    $ gem install guard-livereload

Then add the following to your Guardfile:

{:lang="ruby"}
    guard 'livereload' do
      watch(%r{preview_html/.+\.(html)})
    end

T> You might want to add _preview\_html_ to your .gitignore file as well.

### Other Preview Apps

For Mac I recommend [MouApp](http://mouapp.com). For Linux there is
[ReText](http://sourceforge.net/p/retext/home/ReText)

Remember to follow the golden rule of *Keeping it Simple*. Your Markdown productivity shouldn't be tied to any
specific app. Choose things that complement your workflow not supplement it.
