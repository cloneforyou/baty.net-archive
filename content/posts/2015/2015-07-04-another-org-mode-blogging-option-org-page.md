---
author: Jack
categories:
- Journal
date: "2015-07-04T19:55:00+00:00"
tags:
- Blogging
- org-mode
title: Another Org Mode Blogging Option – org-page
url: /2015/another-org-mode-blogging-option-org-page/
---

Even though I've been posting to baty.net via Org2Blog I'm still interested in other blogging options available from within Org Mode. There's a long [list of options here][1]. I started with [Org-page][2] for no particular reason other than it looked fairly simple. I'm new to the whole Emacs and Org Mode world so starting simple is better.

Here are some quick notes on what I've learned so far.

In my .spacemacs config…

<pre class="example">(setq op/repository-directory "~/Sites/notes")   ;; the repository location
(setq op/site-domain "http://notes.baty.net/")
(setq op/site-main-title "Notes by Jack Baty")
(Setq op/site-sub-title "Notes on work and play")
(setq op/personal-disqus-shortname "jackbaty")
</pre>

I created a new repo using…

<pre class="example">M-x op/new-repository
</pre>

This creates and configures a new Git repository. There are two branches.

<ul class="org-ul">
  <li>
    source: This is where the source content lives, all in .org files
  </li>
  <li>
    master: This is where the rendered HTML files are maintained.
  </li>
</ul>

The trick is to write and commit on "source" first, _then_ publish using…

<pre class="example">M-x op/do-publication
</pre>

Running `do-publication` will prompt for a number of options. The answers I use are…

<ul class="org-ul">
  <li>
    Publish all files? n (then accept the following prompt for a git commit, e.g. HEAD^1)
  </li>
  <li>
    Publish to directory? n (this causes the rendered files to be rendered to the master branch)
  </li>
  <li>
    Auto commit? y (commits the rendered HTML files to the Git master branch)
  </li>
  <li>
    Auto push to remote? y (may just as well)
  </li>
</ul>

Now I have a static site rendered to HTML files in my Git repo. The final step is to publish them to my web server. I do this using rsync. Here's the script.

<div class="org-src-container">
  <pre class="src src-bash">#!/bin/bash

echo -e " 33[0;32mDeploying updates to notes.baty.net... 33[0m"
cd ~/Sites/notes

# make sure we're on master since that's where the HTML files live
git co master

rsync -v -rz --checksum --delete --no-perms /Users/jbaty/Sites/notes/ host:/server/path/to/notes

# Switch back to source so we can keep on blogging
git co source
</pre>
</div>

For now, the results can be seen at <http://notes.baty.net/>

 [1]: http://orgmode.org/worg/org-blog-wiki.html
 [2]: https://github.com/kelvinh/org-page