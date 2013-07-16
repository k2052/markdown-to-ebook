# Hacker meet book. Book meet hacker.

This was written because I was annoyed. Annoyed by all the annoying parts of writing an ebook. Annoyed by;
managing code snippets, differing markdown implantations, ebooks editors that were more about dialogs than writing, the
confusing syntax of LaTeX and the insanity of format divergence.

All these annoyances take energy to deal with, energy is inspiration and energy is a finite resource . I wanted to stop
wasting inspiration on things that weren't writing. I wanted my tools for writing to free me to write not control me,
not shackle me. I wanted my tools to work for me not me for them. I wanted to spend my time writing not
thinking about how I would transform that writing into a book. I wanted a book to be a natural result of my workflow, 
innate to the process.

The more energy you expend on stupid things the less you have to write. Writing quality suffers, procrastination sets
in, you stop working and before you know it you have lost all energy to complete the book. This book attempts to
eliminate drag, to streamline you to a finished book. It doesn't cover everything, it gives you just enough knowledge 
so you can focus on what matters, the writing.

This book follows the hackers ethos of not obscuring things. Problem areas are not glossed over, they are put right out
in the open, so you can learn from them. Where I struggled you will know I did and why I did. I wrote this book through
countless frustrations with Markdown parsers, LaTeX packages and varying bugs; to ignore that would skip a crucial part
of how I learned what I know.

Most technical books appear smooth when read, but they were not written smoothly. To get that snippet down to something
slim and slick requires a lot of breaking things and figuring out what doesn't work. Making things look easy is hard
work. I tried not to make things look easier than they are. By showing you my failures I hope to help you avoid
them.

This is not a book for those that like quick summaries and step by step processes. It's a book for those that like to
learn something. The only way to eliminate frustration is to internalize your knowledge. To make it so much a part of
you that it cant be ignored. 

Unless you truly know how to do something then doing it is going to get in the way and take energy away from your
writing. You cant be looking up how to create a section every time you want to create a section. Such a process will
kill any chance of finishing. Things have to flow smoothly, without thought, so you can just write.

With this in mind, I have kept the approach to tools as simple as possible, so they can be internalized as quickly as
possible. For writing our source files we will use an implementation of Markdown called Kramdown. Markdown is simple
enough to internalize in a day and most importantly, it is flexible enough that it can be adapted to support a variety
of extras. Kramdown has been chosen as our implementation for a few additional reasons;

1. It is used in a variety of publishing platforms (specifically [Leanpub](https://leanpub.com))
2. It closely resembles many other implementations like PHP Markdown Extended. 
3. It's well documented.
4. It's coded in Ruby. Tools like pandoc are just as powerful as Kramdown but pandoc is unfortunately written in
Haskell. Haskell would only increase the learning curve and add more annoying parts to getting our
ebook published.

You wont need to know much to get through this book but you will need to be comfortable hacking around. Comfortable is 
a dangerous word for communication, one's interpretation can greatly vary from the intended meaning. Let's go over some
examples to help clarify what I mean by _comfortable_

Can you do this?

{:lang="sh"}
    homebrew install

Or if you are on Linux this?

{:lang="sh"}
    sudo apt-get install

If this leaves you scratching your head then you are not ready for this book. If you are left feeling comfortable and 
at ease, not queasy and fearful, then you are ready.

We are going to edit Ruby files and add things, so some rudimentary ability to read and edit Ruby will be necessary. If
you are comfortable enough to edit Gemfiles and install gems using bundler then you'll be fine. Ruby won't be necessary
for your work flow in the end, but if you want to move beyond the book some Ruby knowledge will be beneficial.

How to use this book? Read it. Now I know what you might be thinking: "He means digest this fucker, really reflect on
it." Or perhaps you think I mean something really simple yet profound like "Become the book.".  

Neither. 

I left instructions vague so you can decide how to approach the book. Don't follow rules in how you learn. You know how
you learn. Follow your gut. 

If it feels right to read the book non-stop then do it. If it feels right to read the TOC,
skim over the chapters, and then leave it until you need to know the contents, then do that. If it feels right to only
look up the sections you need to get what you want done, then just do that. 

How you read this book is up to you. Just read it.
