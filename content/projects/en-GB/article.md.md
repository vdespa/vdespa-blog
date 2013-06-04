# article.md #

## Short introduction ##

*article.md* brings to Joomla! the power of Github & co. combined with a lightweight markup language, Markdown. Basically you can store and edit your content as Markdown documents and store them on github (storing them locally on your server is also possible).

<!--BREAK-->

> Please note that this extension is in a very early development stage. I would not recommend it for inexperienced users or for large websites.

## What is this extension NOT ##

- This is not a Markdown editor; you can create Markdown files using a basic text editor or something more user friendly like MarkdownPad 2.


## Basic usage ##

1. Install the extension.
2. Enable and configure the plugin `article.md`; basic configuration options are shown below:
    TODO: add image
3. Create a Markdown document and uploaded it to a github repository. Ideally under the following syntax:
    `/{content-folder}/{category-slug}/{laguage-tag}/{file-name}`
example: `/content/blog/en-GB/myfile.md` 
3. Create an article and place it inside a the following code.
    {"mdFile":"myfile.md"}
4. Save it and check the respective page.

## 

## Download ##

The first preview release is scheduled for 1st of June 2013. If you are interested in testing, drop me a message via Twitter @vdespa or an email.

## Requirements ##

- Joomla 3.1 (it might just work in 2.5. but I haven't fully tested it)
- PHP 5.3+

## License ##

This extension is free to download and use. It is released as Open Source under the GNU General Public License and MIT license.

The *PHP Markdown Lib* which is included with the files is released under the MIT license. 

## Support ##

The full source code can be found on Github.

No official support is provided.  However, bugs and issues can be reported on the issue tracker.

 