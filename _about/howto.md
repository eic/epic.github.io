---
title: How-to
name: howto
layout: default

tables:
  table1:
    width: 20%
    columns: [30%, 70%]
    headers: [col1, col2]
    rows:
      - [ a, b ]
      - [ x, y ]
  table2:
    width: 20%
    columns: [50%, 50%]
    headers: [col1, col2]
    rows:
      - [ a,
      "this is an example of a very wide column which can potentially span many text rows in the same cell" ]
      - [ x,
      "two lines
      in YAML" ]
---
{% include layouts/title.md %}

* TOC
{:toc}
<hr/>
#### Overview
The following will be of interest to the collaborators interested in contributing to this
site or involved in its maintenance.

We use GitHub to manage the code for this site. Please take a look at the
{% include navigation/findlink.md name='github_site' tag='GitHub repository' %}
to get an idea of the general organization of the data, layouts and supporing logic.
The idea is to shape the code and content in a way that is easy to navigate
and modify. The following sections explain how this is achieved.
Development of this site involves the following aspects:
* adding and modifying the structured data content
* adding "assets" i.e. documents, images etc - although this needs to be done sparingly: one
needs to keep in mind that there are practical limits on the size of any repository, as well
as hard limits on repositories of sites which are hosted as "GitHub pages"
* interfacing the navigation tools on this site, which for the most part consists
of the top navigation bar and the dropdown menus

<hr/>
#### Navigation Mechanics
##### The Menu System
Entries in the navigation bar on top are named similar to the folders in this
project, for example the "Software" is associated with the folder "_software" etc.
These folders are treated as "collections" by the Jekyll framework and they need to be declared
in the main configuration file
<a href="https://github.com/eic/eic.github.io/blob/master/_config.yml" target="_blank">_config.yml</a>.
As one can see in 
<a href="https://github.com/eic/eic.github.io/blob/master/_includes/layouts/navbar.html" target="_blank">the navigation bar code</a>,
its menu entries are generated automatically based on the content of the file
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/menus.yml" target="_blank">_data/menus.yml</a>.
The content and order of top entries in the navigation bar as well as content and ordering of all items in all dropdown
menus are defined in that file.

If a menu entry in
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/menus.yml" target=_blank>_data/menus.yml</a>
is an external link, there is no additional source code (i.e. markdown page content is not necessary).
Examples are easy to spot in the menu definition file
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/menus.yml" target=_blank>_data/menus.yml</a>.

Menu entry descriptions in *menu.yml* can also have optional attributes:
* **"div"**: to include a divider right above an entry in a dropdown menu the
following line should be added respective section of the _data/menus.yml file: "*div: yes*".
* **"exclude"** -  mainly for development purposes; if a section of the menu
needs to be ignored when building the site one can just easily add "*exclude: yes*" to the repective section.
* **"label"**: to insert a label into a dropdown menu. The label won't have any links or page associations, it's just a visual guide.

##### Front Matter
"Front Matter" is a piece of YAML code (typically short) embedded on the very top of a
Markdown-formatted document which can contain any sort of data, for example variables
used to render the page or define a tag so it can be more easily referenced in this way.
The data in the Front Matter is accessible in a way similar to object attributes.
Importantly, when present the Front Matter serves to inform {% include navigation/findlink.md name='Jekyll' %}
that this particular page needs to be rendered into HTML for inclusion in the site being built. Otherwise
it won't be included in the site. In summary, most of the files used to build the site
are expected to be in the Markdown (MD) format, and **each MD file is to be equipped with the
"front matter" section**, which could look like the excerpt below.
```yaml
---
title: My Cool Software
name: my_cool_software
category: mysoftware
layout: default
---
```

<hr/>
#### Helper macros
##### Tables
**All tables on this site have added CSS styling to enhance appearance and readability.**

To define  tables within pages one can use built-in Markdown table styling capabilities,
for example this snippet of Markdown code

```
| Language                 | Lines of code                       | Description |
|--------------------------|-------------------------------------|-------------|
| C++                      | 101                                 | Module 1    |
| Python                   | 37                                  | Module 2    |
| Ruby                     | 53                                  | Parser      |
{:.table-bordered}
```

...will be rendered as

| Language                 | Lines of code                       | Description |
|--------------------------|-------------------------------------|-------------|
| C++                      | 101                                 | Module 1    |
| Python                   | 37                                  | Module 2    |
| Ruby                     | 53                                  | Parser      |
{:.table-bordered}

<br/>
Main disadvantage of this method is difficulty in editing large table cells
(i.e. cells with a large amount of content).
There is also an option to embed HTML directly. Both Markdown and plain HTML
options will work but have limitations for example multiple tables present
on the same page may end up having diferent widths, and column within individual tables
may also have inconsistent column widths. This detracts from the page readability.

For greater control and ease of editing data there is also a macro called
**big_table.md** deployed on this site
which is illustrated in the following example. Assume the following content has been added
to the *front matter* of a page:
```yaml
tables:
  table1:
    width: 20%
    columns: [30%, 70%]
    headers: [col1, col2]
    rows:
      - [ a, b ]
      - [ x, y ]
```
...and the source of the page contains:
> {% raw %}
> {% include layouts/big_table.md table=page.tables.table1 %}
> {% endraw %}

This will result in the following content on the page:
{% include layouts/big_table.md table=page.tables.table1 %}
<br/>
Note how the table and column widths are configured in the example above.
More tables can be easily added by putting the content in blocks of the front matter
and then referring to them using the macro, as needed.

Individual cells can contain long strings and/or strings split over multiple lines as
long as these are double-quote delimited in the front matter. Example:
```yaml
tables:
  table2:
    width: 20%
    columns: [50%, 50%]
    headers: [col1, col2]
    rows:
      - [ a,
      "this is an example of a very wide column which can potentially span many text rows in the same cell" ]
      "two lines
      in YAML" ]
```
{% include layouts/big_table.md table=page.tables.table2 %}


<br/>
##### Links
Frequently used **external** links are located in the following registry:
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/links.yml" target=_blank>_data/links.yml</a>.

They can be easily inserted in pages in a consistent manner by their mneumonic name using
a macro as decribed below. This simplifies handling user-unfriendly links and ensures they
remain the same across the site and makes
it easy to point to a different resource when necessary. For example, this piece of code
> {% raw %}
> {% include navigation/findlink.md name='Jekyll' %}
> {% endraw %}

will result in the link: {% include navigation/findlink.md name='Jekyll' %}. If you would like the displayed
link name to be different, the optional "tag" argument for the macro will do that. Example:
> {% raw %}
> {% include navigation/findlink.md name='github_site' tag='repository' %}
> {% endraw %}

will result in the following link: {% include navigation/findlink.md name='github_site' tag='repository' %}.
Links will automatically open in a new tab. There is a similar macro for use with links to **internal**
pages on the site. Example:
> {% raw %}
> {% include navigation/pagelink.md folder=site.about name='howto' tag='"How-to" page' %}
> {% endraw %}

will result in: {% include navigation/pagelink.md folder=site.about name='howto' tag='"How-to" page' %} (this happens
to point to the page you are reading now).
Primary advantage of this macro is that it allows renaming the source file - while keeping the same "name" attribute
in the Front Matter section the links will still be correct.

##### Images
Users and developers have complete freedom in how they incorporate images into pages on this site.
In many cases handling images will be facilitated by adding an image to the following registry:
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/gallery.yml" target=_blank>_data/gallery.yml</a>.
Note that items in *gallery.yml* contains references to paths of images. While any image can reside in any folder it
is recommended to keep the image files under *asset/images* in one of the following four folders:
* site
* software
* support
* tutorials

The *site* folder is used for keeping elements of the site look and feel, the *software* one is used
to keep various software-related diagrams, and the rest are self-explanatory. Once an image is added to
to the appropriate folder and *gallery.yml* one cab use a simple macro to refer to it by its mneumonic
name and automatically add it to pages on this site, as illustrated in the following example. The PNG
file containing the "news banner" image is added to the *site* folder under *assets/images* and this
lines are added to *gallery.yaml*:
```yaml
- name: news_banner
  path: '/assets/images/site/EICUG-SWG-News-Banner.png'
  title: 'EICUG SWG News Banner'
  type: logo
```
Then, this line of code included in the page
> {% raw %}
> {% include images/image.md name='news_banner' width='400' %}
> {% endraw %}

...will produce

{% include images/image.md name='news_banner' width='400' %}

If you are adding a software-related diagram, it should go into the "software" folder and
the *gallery.yml* should reference it accordingly.
This helps to ensure consistency of image links throughout the site and remove
the need to include cumbersome HTML in the page code. It also makes it possible to move
the content across folders if necessary and only make changes to the path in one place.

##### Documents
Similar to the logic presented above, it is preferable to create references to documents (both internal to the site
but also as references to cloud storage such as Zenodo) in an organized manner. Similar to examples above,
there is a registry of documents on the site:
<a href="https://raw.githubusercontent.com/eic/eic.github.io/master/_data/documents.yml" target=_blank>_data/documents.yml</a>.
It is quite small as the time of writing but is expected to grow. Documents can be referred to their names and links
autogeneraged by using a macro such as one in this example:
{% raw %}
{% include documents/doc.md name='Jung-MCValidation.pdf' %}
{% endraw %}

One of the advantages of this approach is that the source of the document can be relocated within the site
or even uploaded to an external resources while the links in the site code will remain valid provided
corrections are made in just one place: _documents.yml.

<hr/>
#### Managing Data
Jekyll is fairly flexible when it comes to storing and manipulating structured data.
The data component of the site can reside in the "front matter" section of the Markdown-formatted
files or in separate YAML (or JSON, CSV etc) data sources. The front matter approach works well
for small sites. For scalability, it is recommended to rely mostly on dedicated data files (i.e.
files in the "<a href="https://github.com/eic/eic.github.io/tree/master/_data" target="_blank">_data</a>" folder)
and keep the content of the front matter sections of individual MD files to a minimum.

The {% include navigation/findlink.md name='Liquid' %} template language
features a variety of filters that can be applied to the data stored in YAML and other data sources
as well as in the front matter blocks of pages, and decent (not perfect) support for collections,
iterators and flow control constructs.

Information assets on this site (e.g. imaged, PDF files etc) are stored in the aptly named
"<a href="https://github.com/eic/eic.github.io/tree/master/assets/" target="_blank">assets</a>"
folder and its subsiduaries. This convention should be kept going forward.

#### Formatting
We aim to provide a uniform look and feel across the site. To that end, whenever possible
the head (title) of each page is formatted by using the standard include layouts/title.md - please
look a the code for examples of its use. It's renders the page title from the front matter sections.

Headers of sections within a page are currently formatted in Markdown as "header level 4" i.e. prepended
with four hash characters like in **"#### My section header"**. Take a look at the header (Formatting)
of this section to get an idea of how it's rendered.

Blockquotes are used to put emphasis on quoted text. The standard way of doing it in Markdown is to
prepend the lines that need this formatting with the '>' character. For example:
```
>Blockquote!
```
...will produce
>Blockquote!

Language-aware code highlighting capability has been integrated and
can be easily used via the fenced code blocks i.e. blocks delimited by
triple backticks, with an optional shortcut for the programming language name.
For example,



    ```bash
    export foo="foo"
    echo $foo
    ls -l .
    ```

will be rendered as:
```bash
export foo="foo"
echo $foo
ls -l .
```

and

    ```python
    def my_function():
      print("Hello from a function")
    ```

will show as:

```python
def my_function():
  print("Hello from a function")
```


For C++, the opening line should be triple backticks followed by "c++"
similar to examples above and the code will be rendered accordingly such as:

```c++
class MyClass {       // The class
  public:             // Access specifier
    int myNum;        // Attribute (int variable)
    string myString;  // Attribute (string variable)
};
```

<hr/>
#### Development and Deployment
##### Ruby and Jekyll
To productively participate in the development of this site one needs to learn the basics of the
{% include navigation/findlink.md name='Jekyll' %} framework and install it on
a development machine. This is relatively straighforward but may depend on the OS specifics.

Once the environment is in place, any modification can be validated immediately since
the locally running development server provided by Jekyll will render the site
on the local host. Basic knowledge of the {% include navigation/findlink.md name='Liquid' %}
template language and in particular the "filters" that are part of it is extremely helpful.

##### GitHub Pages
As already mentioned, we use GitHub to manage the code for this site. A fork/pull request process is optimal,
but if you prefer direct push for some reason please get in touch with the group. To get access to the code,
create a clone of the website repository and move to the directory created:

  ```bash
  git clone https://github.com/eic/eic.github.io.git
  cd eic.github.io
  ```

The repository is configured for ''GitHub Pages''.
This means that once modified contents are committed to the repository and you perform *git push*, the
{% include navigation/findlink.md name='github_pages' tag='GitHub pages system' %} will build the
site and make it publicly available at [https://eic.github.io/](https://eic.github.io/). Errors in the
updated code may result either in failed deployment or a broken instance of the site. For that reason
it is desirable to assess the result of changes before publishing them.
To achieve this, you need to install Jekyll on your local machine, as detailed below.

##### Jekyll
Instructions can be found on Jekyll [web site](https://jekyllrb.com/docs/installation/) but the short story is:

* Install {% include navigation/findlink.md name='ruby_downloads' tag='Ruby' %} and {% include navigation/findlink.md name='rubygems_downloads' tag='RubyGems' %}
* Install Bundler (a Ruby package manager): `gem install bundler`
* Install/update your Jekyll installation (must be done regularly):
  ```bash
  bundler update
  ```
* Run Jekyll (in the eic.github.io directory):
  ```bash
  bundler exec jekyll serve
  ```

Once Jekyll has been started you can view the web site by connecting to `localhost:4000`.
Changes made to files are immediately compiled and reflected on the
displayed site (at the next page load). The optimal workflow is to make changes and debug
entirely locally before doing a 'git commit' and pushing to GitHub.

Examples of Jekyll commands
```bash
# Start a new Jekyll project
## NB. this is a little tricky e.g. you would need to pick a correct
## Gemfile and perhaps use --force
## A little experimentation is in order
jekyll new myblog

# Clean stale CSS etc, also run after modifyig _config.yml.
bundle exec jekyll clean
# Serve content while filtering out some annoying warning messages
bundle exec jekyll serve 2>&1 | grep -v ': Using'
```

If you get this far, you are ready to start modifying the site. A word of caution - please check the output
of the jekyll compiler for error messages. Normally the code compiles and you just reload the page to see
the result, but if there was an error and you are not checking the compiler's windown the behavior of the
page will be puzzling.


[top](#how-to)

