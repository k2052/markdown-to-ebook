# An Introduction

Writing a book can be daunting, learning the tools to write a book can be even more daunting. A quick google search will turn up a multitude of services, tools, apps and frameworks designed to write a book. Apps like; iBooks, Adobe Acrobat, inDesign Skyreader, Kindle Publisher etc. Formats like; PDF, ePUB, LaTeX....

It's not so far fetched that a writer when faced with this mess of "solutions" might simply choose a trusty old
typewriter or even pen and paper. Or he might get so caught up writing tools for writing that he never gets the time to
write.

This book is a result of my search for a get out of the way method of writing ebooks. Something that keeps my focus on
the writing and not on the process of writing. It doesn't cover everything, it gives you just enough knowledge so you
can focus on what matters, the writing.

This book follows the hackers ethos of not obscuring things. Problem areas are not glossed over, they are put right out
in the open, so you can learn from them. Where I struggled you will know I did and why I did. I wrote this book through
countless frustrations with Markdown parsers, LaTeX packages and varying bugs; to ignore that would skip a crucial part
of how I learned what I know.

Most technical books appear smooth when read, but they were not written smoothly. To get that snippet down to something
slim and slick requires a lot of breaking things and figuring out what doesn't work. Making things look easy is hard
work. I tried not to make things look easier than they are. By showing you my failures I hope to help you avoid
them.

This is not a book for those that like quick summaries and step by step processes. It's a book for those that like to
learn something. The only way to eliminate frustration is to internalize your knowledge, to make it so much a part of
you that it cant be ignored. 

Unless you truly know how to do something then doing it is going to get in the way and take energy away from your
writing. You cant for instance, be looking up how to create a section every time you want to create a section. Such a
workflow will kill a book before it is even start. Things have to flow smoothly, without thought, so you can just write.

With this in mind, I have kept the approach to tools as simple as possible, so they can be internalized as quickly as
possible. Our book' source files will be written in Markdown. Markdown is simple enough to internalize in a day and 
most importantly, it is flexible enough that it can be adapted to support a variety of extras.

Kramdown has been chosen as our flavor/implementation of Markdown. This is for a few reasons;

1. It is used in a variety of publishing platforms (specifically [Leanpub](https://leanpub.com))
2. It closely resembles many other implementations like PHP Markdown Extended. 
3. It's well documented.
4. It's coded in Ruby. Tools like pandoc are just as powerful as Kramdown but pandoc is unfortunately written in
Haskell. Haskell would only increase the learning curve and add more annoying parts to getting our
ebook published.

## What you need to know

You wont need to know much to get through this book but you will need to be comfortable hacking around. Comfortable is 
a dangerous word for communication, one's interpretation can vary greatly from what is intended. Let's go over some
examples to help clarify what I mean by _comfortable_

Can you do this?

{:lang="sh"}
    homebrew install

Or if you are on Linux this?

{:lang="sh"}
    sudo apt-get install

If this leaves you scratching your head then you are not ready for this book. If you are left feeling comfortable and 
at ease, not queasy and fearful, then you are ready.

Some rudimentary ability to read and edit Ruby will also be necessary. If you are comfortable enough to edit Gemfiles 
and install gems using bundler then you should be fine.