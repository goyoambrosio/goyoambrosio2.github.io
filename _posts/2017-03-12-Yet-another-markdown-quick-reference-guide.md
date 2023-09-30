---
title: "Yet another markdown quick reference guide"
related : true
categories:
  - Quick reference
tags: 
  - guide
  - reference
  - info
  - markdown
---

There are lots of markdown quick reference guides out there. I make this just
for my personal utilization with annotations and includes some copy'n'paste from
other public guides.

This is a live document that I will evolve with contributions that seem to me of
interest.

## Emphasis

    *Emphasize*
    _Emphasize_
    **Strong**
    __Strong__

*Emphasize*
_Emphasize_
**Strong**
__Strong__

## Inline links

    A [link](http://example.com "Title")

A [link](http://example.com "Title")

## Referenced links

    Some text with [a link][1] and
    another [link][2].
    
    [1]: http://example.com/ "Title"
    [2]: http://example.org/ "Title"

Some text with [a link][1] and
another [link][2].

[1]: http://example.com/ "Title"
[2]: http://example.org/ "Title"

*The reference section can be anywhere in the document*

## Inline Images

    Logo: ![Alt](/assets/images/logo.png "Title")

Logo: ![Alt](/assets/images/2017/03/goyo-site-logo.jpg "Title")

*The “Alt” text (alternative text) makes images accessible to visually impaired*

## Referenced images

    Logo: ![Alt][3]
    
    [3]: /assets/images/logo.png "Title"

Logo: ![Alt][3]

[3]: /assets/images/2017/03/goyo-site-logo.jpg "Title"

## Linked images

    Linked logo: [![alt text](/assets/images/logo.png)](http://example.com/ "Title")

Linked logo: [![alt text](/assets/images/2017/03/goyo-site-logo.jpg)](http://example.com/ "Title")


## Footnotes

    This is a footnote [^1] example
    
    [^1]: Footnote text

This is a footnote [^1] example

[^1]: Footnote text

## Line breaks

    This is a first line  
    This is the second line after the line break

This is a first line  
This is the second line after the line break


## Bulled lists

    * Item
    * Item
    - Item
    - Item
    + Item
    + Item

* Item
* Item
- Item
- Item
+ Item
+ Item


## Numbered lists

    1. Item
    2. Item
    3. Item

1. Item
2. Item
3. Item

## Mixed lists

    1. Item
    2. Item
       * Mixed
       * Mixed  
    3. Item

1. Item
2. Item
   * Mixed
   * Mixed  
3. Item

## Blockquotes

    > Quoted text.
    > > Quoted quote.
    
    > * Quoted 
    > * List

> Quoted text.
> > Quoted quote.

> * Quoted 
> * List


## Preformatted

    Begin each line with 
    four spaces or more to 
    make text look
    e x a c t l y 
    like  you  type i
    t.
    
## Code

    `This is code`
    
    ~~~~
    This is a 
    piece of code 
    in a block
    ~~~~
    
    ```
    This too
    ```

`This is code`

~~~~
This is a 
piece of code 
in a block
~~~~

```
This too
```
## Syntax hightlighting

    ```css
    #button {
    	border: none;
    }
    ```
    
    ```javascript
    var s = "JavaScript syntax highlighting";
    alert(s);
    ```
     
    ```python
    s = "Python syntax highlighting"
    print s
    ```

```css
#button {
	border: none;
}
```

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```
 
```python
s = "Python syntax highlighting"
print s
```


## Headers

    Header 1
    ========
    
    Header 2
    --------
    
    # Header 1
    ## Header 2
    ### Header 3 
    #### Header 4 ####
    ##### Header 5 #####
    ###### Header 6 ######


## Definition lists

    WordPress
    :  A semantic personal publishing platform 
    
    Markdown
    :  Text-to-HTML conversion tool

WordPress
:  A semantic personal publishing platform 

Markdown
:  Text-to-HTML conversion tool

## Tables

Tables aren't part of the core Markdown spec, but they are part of GFM and Markdown Here supports them. They are an easy way of adding tables to your email -- a task that would otherwise require copy-pasting from another application.


    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | col 3 is      | right-aligned | $1600 |
    | col 2 is      | centered      |   $12 |
    | zebra stripes | are neat      |    $1 |

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the 
raw Markdown line up prettily. You can also use inline Markdown.

    Markdown | Less | Pretty
    --- | --- | ---
    *Still* | `renders` | **nicely**
    1 | 2 | 3

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

## Inline HTML

You can also use raw HTML in your Markdown, and it'll mostly work pretty well.

    <dl>
      <dt>Definition list</dt>
      <dd>Is something people use sometimes.</dd>
    
      <dt>Markdown in HTML</dt>
      <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
    </dl>

<dl>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>

## Horizontal Rule

    Three or more...
    
    ---
    
    Hyphens
    
    ***
    
    Asterisks
    
    ___
    
    Underscores

Three or more...

---

Hyphens

***

Asterisks

___

Underscores


## Line Breaks

    Here's a line for us to start with.
    
    This line is separated from the one above by two newlines, so it will be a *separate paragraph*.
    
    This line is also a separate paragraph, but...
    this line is only separated by a single newline, so it's a separate line in the *same paragraph*.

Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

This line is also a separate paragraph, but...
This line is only separated by a single newline, so it's a separate line in the *same paragraph*.

I'm using GFM
([GitHub Flavored Markdown](https://help.github.com/categories/writing-on-github/))
line breaks so there's no need to use MD's two-space line breaks.

## YouTube videos

You can type

    [![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](http://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)
    
but losing the image sizing and border.

It's better to include it  with HTML code 

    <a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE" target="_blank">
        <img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg" alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" />
    </a>
    
## More info

[Official project page](https://daringfireball.net/projects/markdown/)

[GitHub Flavored Markdown](https://help.github.com/categories/writing-on-github/)

[Atlassian Markdown syntax guide](https://confluence.atlassian.com/bitbucketserver/markdown-syntax-guide-776639995.html)

[Wordpress Markdown quick reference](https://en.support.wordpress.com/markdown-quick-reference/)
    

