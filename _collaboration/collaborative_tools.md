---
title: Collaborative Tools
name: collaborative_tools
layout: default
---

{% include layouts/title.md %}

#### Overview

* The ePIC Collaboration is currently using [Wiki](https://wiki.bnl.gov/EPIC/index.php?title=Main_Page){:target="_blank"}
as its main web presence tool.
Migration to this website, motivated by ease of long-term maintenance and efficient coordination of the
team contributions, is work in progress. See sections below for technical information.
* For meetings and agendas, there is a dedicated [ePIC Indico area](https://indico.bnl.gov/category/402/){:target="_blank"} hosted at BNL.
* Ther is an extensive set of mailing lists hosted at BNL. For more information about access please contact the corresponding working group convener or a member of the leadership team.

---

#### About this website

This website is work in progress. Below is the initial set of information intended to help you
get started to make contributions to the content published here.

We leverage a very popular and efficient _static website generator_ technology:
[Jekyll](https://jekyllrb.com/){:target="_blank"}.
It allows the authors to use the easy-to-read [Markdown](https://www.markdownguide.org/){:target="_blank"}
annotation, which is then automatically, and transparently for the user, converted into HTML. This way
the cumbersome HTML editing as avoided. A simple example of the Markdown text such as used on this page
may look like this:

```markdown
External links: [Markdown](https://www.markdownguide.org/){:target="_blank"}

Itemized list:
* __item 1__ in bold
* _item 2_ in italics
```
...which is rendered as:

---

External links: [Markdown](https://www.markdownguide.org/){:target="_blank"}

Itemized list:
* __item 1__ in bold
* _item 2_ in italics

---

Data content is handled in a way that effectively serves as a database,
without the need to deploy an actual database server. This is achieved by storing the site data (when needed)
in YAML-formatted files, which can be queried and parsed automatically.
The most optimal setup for the content development is to install Jekyll on your machine using the information
contained in the Jekyll link above. This requires a modicum of effort but is certainly not too difficult. It is
also possible to edit the material directly on _GitHub_ (see the section below) using the state-of-the-art
editor -- __VScode__. It is imperative that such direct edits, if they are necessary, are done in your own branch
(as opposed to "main") so as to not accidentally damage the ePIC website by making unintended or invalid changes.

##### GitHub: Managing the Content and Serving the Content

We use a [GitHub repository](https://github.com/eic/epic.github.io){:target="_blank"} to manage the code for this site.
Basic git/GitHub literacy is very helpful for efficient participation in the development of this site's content. In terms
of getting your new or updated content into the repo, the branch/pull request (PR) process is optimal.To get access to the code,
create a clone of the website repository and move to the directory created:

```bash
git clone git@github.com:eic/epic.github.io.git
cd epic.github.io
```

You should be able to create your own branch using the GitHub web interface, it's fairly straightforward. Then,
you can use a command line like the one below, to create a working copy of the branch on your workstation or
laptop:

```bash
git checkout my_cool_branch
```

After you commit the updated material to your branch and push it to GitHub, you will probably want to create
a "pull request", which will prompt the administrators of this site to review your commits and merge them
into main. After that the updated material will be rendered on the webpage. This is achieved by using the mechanism
of ''GitHub Pages'', which is a free service provided by GitHub and which automaticall renders the site
at the url: [https://eic.github.io/epic.github.io/](https://eic.github.io/epic.github.io/){:target="_blank"}

##### Documents, Images, Data

The ePIC Collaboration is still working on its policies and technical solutions to store and manage all sorts
of digital materials (e.g. PFD files, images, DOC files etc). This is work in progress. In the meantime,
storage of this type  of materials on this website is __strongly discouraged__ due to GitHub limitations and
other factors. Links to Google Drive, Dropbox or other such cloud meadia should be used instead. Once ePIC
picks the document management solution (e.g. Invenio RDM, Zenodo or something else) this will be revisited.
