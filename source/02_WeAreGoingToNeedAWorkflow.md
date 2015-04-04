# We are going to need a workflow

I'm pulled in a thousand different directions by a thousand different solutions. iBooks Author looks nice. Nathan Barry
went with that right? I think that is also how Josh Long wrote Execute. But what about epub? Isn't epub just html at 
its core? Maybe I'll write in HTML and use an HTML to PDF converter? That would allow me to craft all sorts of
customizations in CSS. I could really style things awesomely. I could release a web version with fancy JS stuff and 
blow people's minds with a really unique ebook release.

But what about organization of my writings -- writing stuff in pure HTML is going to be a mess. I guess I could use a
static site generator and write in a templating language to keeps things organized into chapters. But maybe I should 
use a ebook authoring tool that handles this for me? What about Scrivener? Don't screenplay authors use that? What if 
I want to deliver a screenplay for my book in case they decide to adapt into a movie?

Choosing a workflow from a place of ignorance is an exercise in futility. Any choice we make will be clouded by a lack
of knowledge, too many options and possibly delusions of grandeur.

If we want to find a workflow that works we are going to have start with very basic questions. Questions
we can see the answers to immediately. If we answer the simple questions first then the complex ones will become
clearer. Answering some basic questions will give us constraints and illuminate the path ahead.

## Where will we distribute the book?

For distribution it would be nice to have something that works well for the writing phase as well as the publishing
phase. Perhaps even something that generates our ebooks from our source formats for us. Such a service exists and it's
called [Leanpub](https://leanpub.com).

What does Leanpub use as its source format? Leanpub uses Markdown. Thus, we will need to use Markdown; we now have our
first constraint.

## What final format does it need to be in when distributed?

There are a zillion different options for ebook formats so whatever we choose it's going to have to be capable of
converting into a variety of formats. We will need something that can go into PDF, epub, kindle and mobi formats. It
just so happens that with the exception of PDF all these formats are HTML at there core. 

What is a good option for writing HTML ebooks? Markdown of course. Markdown is simple, flexible, looks great as
plaintext and most importantly, it is built with HTML as the intended output format.

How then do we turn Markdown into PDFs? The answer lies in LaTeX, which will be our in-between format of choice. LaTeX
is to PDF as Markdown is to HTML. They're best buds and get together all the time to just chat and mark things up. 

Does LaTeX mean we will have to maintain two separate formats? Nope. Markdown can be easily converted into LaTeX
using tools like Kramdown.

## LaTeX

LaTeX is one of those things you probably know about but have never bothered learning. I mean, it's for technical
papers right? When have you ever needed to write a technical paper. One day when you have the time, you plan to study 
it but not now. For now, you'll just use Markdown. Markdown works for now, but one day you're going to have to touch 
LaTeX if you want to move beyond the HTML realm.

I could cover LaTeX the normal way; mention the apps you use to use it, the commands you use to install it, the syntax
you use to craft it, and then send you on your way. Most books on LaTeX are written this way. I could tread the same
ground other books do and I'm sure you'd get some benefit. After all, countless books cover things that way, it's not a
completely useless approach. 

You might even be motivated enough to play with LaTeX a bit. Perhaps you will even write a few a things in it. In the
long run though, you'll slowly find its usefulness lacking. Markdown will just be quicker and more productive. When you
write Markdown you'll get things done. When you use LaTeX you will feel like you're fighting it; spending more time on
your tool than on writing. When you truly need the power of LaTeX you'll do what you always do, live without it or hack
a solution onto your favorite Markdown parser.

I know you'll do this because I do it. We all do it. We learn new tech because we are curious, but if the logistics
don't work out, if the practicalities of using it don't align with execution, actually getting stuff done, then we 
don't use it. It's the same reason we never blog on our amazing overly designed custom built blogging engines, it just 
isn't productive. One day a Medium, Tumblr or Jekyll comes along and we finally take the plunge and simplify enough to 
just focus on our writing. Things need to be too easy or they won't be made use of. Your tools need to blend into the
background and get out of the way of creation or you'll never use them.

If we are actually going to make real use of LaTeX it's going to have to work for us, not us for it. It will have to
complement our workflow, not supplement it. We will adopt a workflow that makes LaTeX work for us.

## Leanpub

Let's visit Leanpub, get registered, and create a test book. 

After we get signed up, Leanpub will inform of us the basics. We will upload our ebook files via Dropbox. They will be
written in plain text using Markdown.

We will get a link sent to us for our first book. Get Dropbox setup if you haven't already.

I> You can utilize the web interface for Dropbox. No need to download/install.

Visit the getting started page for your book *leanpub.com/bookname/getting_started* e.g *https://leanpub.com/markdown-to-ebook/getting_started*

The first thing we can take away from this page is that we will use Markdown. If we incorporate fancy stuff it
will be by customizing the conversion of our Markdown. I actually like this as it keeps our text straightforward and
simple, we will only have to use convoluted LaTeX when absolutely necessary.

We need to know a little about the Markdown flavor we are dealing with though. After some quick googling I found this 
[HN comment](https://news.ycombinator.com/item?id=4998144):

    Hey, co-founder of Leanpub here.

    If you're interested in the differences between Pandoc and Leanpub, there are two places to look. Leanpub is based
    off of Kramdown, so there's the Kramdown documentation[1]. We have made a few extensions to Kramdown to support
    things that books need, so you'll also want to look at the Leanpub manual[2].

    If you have any questions, send us an email at hello@leanpub.com.

    [1]: http://kramdown.rubyforge.org/syntax.html [2]: https://leanpub.com/help/manual

So, Leanpub uses Kramdown, this is good news. This makes incorporating Leanpub into our workflow even easier. 
