# A Sublime Markdown Experience

If you write a book in Markdown chances are very high that you are going to write a lot of Markdown. Well, unless you
have hired one of those ghost Markdown writers or made a deal with a Markdown demon. It is going to be helpful to tweak
your environment to be specific to Markdown. Thankfully Sublime Text 2 is flexible enough that it can quickly become 
the best friend of any Markdown writer.

## Count my chars, measure my words, tell me the lines, express my worth

Word count is a powerful tool for judging both the completion of your work and to measure your self worth as a writer.
Are you procrastinating? Are you zooming along like a super human Stephen King? Only word count can tell you. Well, 
that, and the quality of your writing. But who needs quality writing when you have measurements like word count.

Adding word counts to Sublime Text 2 is quite easy, just Install the package [WordCount](https://github.com/SublimeText/WordCount).
Once installed, you will have a word count in the status bar below your files. Now drive up that count!

## Getting Sublime

I wish this section was longer and reaching Nirvana for Markdown editing was harder that way I could elevate my word
count. Unfortunately, it's as simple as installing two extensions; [SmartMarkdown](https://github.com/demon386/SmartMarkdown) and
[MarkdownEditing](https://github.com/ttscoff/MarkdownEditing). Together, these two will add everything you need. _SmartMarkdown_ will handle
things like smart folding, adjusting heading levels etc. _MarkdownEditing_ will handle all your nifty shortcuts for
inserting Markdown things.

Not to worry though, I can up my word count a bit with some configuration suggestions. 

You are going to want to use a better theme for Markdown editing, one that gives you nice highlighting for everything,
one that is designed for writing not coding. MarkdownEditing comes with an excellent theme inspired by Byword that 
looks very nice. You can enable it for Markdown files by adding it to your _MultiMarkdown.sublime-settings_ and 
_Markdown.sublime-settings_ files like so:

{:lang="json"}
    {
      "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Focus.tmTheme"
    }

You might also want to enable Markdown syntax on `.txt` files for when you're utilizing Leanpub. You can force Leanpub
to use `.md` but you might be the type of person that likes `.txt` to be treated as Markdown. You can do this easily by
adding the following to _Markdown.sublime-settings_:

{:lang="json"}
    {
      "extensions":
      [
        "txt"
      ]
    }

You might also want to try out the ApplySyntax plugin which will handle detecting syntax based on the contents of a
file. Get it via package control or [Github](https://github.com/facelessuser/ApplySyntax)

### Bugs with MarkdownEditing

There is a bug in MarkdownEditing on installations without the Menlo font. The font is set in the settings for the
syntax manually. Doing this causes bizarre issues, like rulers disappearing and fonts not resizing.  You can fix it by
resetting your preferred font in the MultiMarkdown .sublime-settings and Markdown.sublime-settings files:

{:lang="json"}
    {
      "font_face": "Menlo"
    }

Font settings being set in a Package also causes issues with using commands for increasing/decreasing text size. You 
can fix this by commenting out `font_size` in `MarkdownEditing packages folder/Markdown.sublime-settings` (click browse
packages to find it).

May your Markdown experience be Sublime!
