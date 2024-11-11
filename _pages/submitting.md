---
layout: page
title: submitting
permalink: /submitting/
description:
nav: true
nav_order: 3
---

### A more open process

As with the previous edition of the Blog Post track, we forgo the requirement for total anonymity. 
The blog posts **must be anonymized for the review process**, but users will submit their anonymized blog posts via a pull request to the blog track's repository (in addition to a submission on OpenReview).
The pull request will trigger an automated pipeline that will build and deploy your post onto a website dedicated to the reviewing process.
<!-- The post will be merged into the staging repository, where it will be deployed to a separate Github Pages website.  -->
Reviewers will be able to access the posts directly through a public URL (generated by the Github action), and will submit their reviews on OpenReview.
Reviewers should refrain from looking at the git history for the post, which may reveal information about the authors.

This still largely follows the Double-Blind reviewing principle; it is no less double-blind than when reviewers are asked to score papers that have previously been released to [arXiv](https://arxiv.org/), an overwhelmingly common practice in the ML community.
This approach was chosen to lower the burden on both the organizers and the authors; in 2022, many submissions had to be reworked once deployed due to a variety of reasons.
By allowing the authors to render their websites to Github Pages prior to the review process, we hope to avoid this issue entirely. 
<!-- We also avoid the issue of having to host the submissions on a separate server during the reviewing process. -->

However, we understand the desire for total anonymity. 
Authors that wish to have a fully double-blind process might consider creating new GitHub accounts without identifying information which they will only be use for this track.
For an example of a submission in the past which used an anonymous account in this manner, you can check out the [World Models blog post (Ha and Schmidhuber, 2018)](https://worldmodels.github.io/) and the [accompanying repository](https://github.com/worldmodels/worldmodels.github.io).

### Template

The workflow you will use to participate in this track should be relatively familiar to you if have used [Github Pages](https://pages.github.com/). Specifically, our website uses the [Al-Folio](https://github.com/alshedivat/al-folio) template.
This template uses Github Pages as part of its process, but it also utilizes a separate build step using [Github Actions](https://github.com/features/actions) and intermediary [Docker Images](https://www.docker.com/).

**We recommend paying close attention to the steps presented in this guide. 
Small mistakes here can have very hard-to-debug consequences.**

### Contents

- [Quickstart](#quickstart)
- [Download the Blog Repository](#download-the-blog-repository)
- [Creating a Blog Post](#creating-a-blog-post)
- [Local Serving](#local-serving)
   - [Method 1: Using Docker](#method-1-using-docker)
   - [Method 2: Using Jekyll Manually](#method-2-using-jekyll-manually)
      - [Installation](#installation)
      - [Manual Serving](#manual-serving)
- [Submitting Your Blog Post](#submitting-your-blog-post)
- [Reviewing Process](#reviewing-process)
- [Camera Ready (TBD)](#camera-ready)


### Quickstart

This section provides a summary of the workflow for creating and submitting a blog post. 
For more details about any of these steps, please refer to the appropriate section.


1. Fork or download our [repository](https://github.com/iclr-blogposts/2025). 

2. Create your blog post content as detailed in the [Creating a Blog Post](#creating-a-blog-post) section.
    In summary, to create your post, you will: 
    - Create a Markdown or HTML file in the `_posts/` directory with the format `_posts/2025-04-28-[SUBMISSION NAME].md`. If you choose to write the post in HTML, then the extension of this last file should be .html instead of .md. NOTE: HTML posts are not officially supported, use at your own risk!
    - Add any static image to `assets/img/2025-04-28-[SUBMISSION NAME]/`.
    - Add any interactive HTML figures to `assets/html/2025-04-28-[SUBMISSION NAME]/`. 
    - Put your citations into a bibtex file in `assets/bibliography/2025-04-28-[SUBMISSION NAME].bib`. 

    **DO NOT** touch anything else in the repository.
    We will utilize an automated deployment action which will filter out all submissions that modifiy more than the list of files that we just described above.
    Read the [relevant section](#creating-a-blog-post) for more details.
    **Make sure to omit any identifying information for the review process.**

3. To render your website locally, you can build a docker container via `$ ./bin/docker_run.sh` to serve your website locally. 
    Alternatively, you can setup your local environment to render the website via conventional `$ bundle exec jekyll serve --future` commands. 
    More information for both of these configuratoins can be found in the [Local Serving](#local-serving) section.

4. To submit your website, create a pull request to the main repository. Make sure that this PR's title is `_posts/2025-04-28-[SUBMISSION NAME]`. This will trigger a GitHub Action that will build your blogpost and write the host's URL in a comment to your PR.

5. If accepted, we will merge the accepted posts to our main repository. See the [camera ready](#camera-ready) section for more details on merging in an accepted blog post.

**Should you edit ANY files other your new post inside the `_posts` directory, and your new folder inside the `assets` directory, your pull requests will automatically be rejected.**

You can view an example of a successful PR [here](https://github.com/iclr-blogposts/2025/pull/48). You can view an example of a PR with erroneous files [here](https://github.com/iclr-blogposts/2025/pull/51).

### Download the Blog Repository

Download or fork our [repository](https://github.com/iclr-blogposts/2025). 
You will be submitting a pull request this repository.

### Creating a Blog Post

To create a blog post in Markdown format, you can modify the [example]({% post_url 2025-04-28-distill-example %}) Markdown post `_posts/2025-04-28-distill-example.md` and rename it to `_posts/2025-04-28-[SUBMISSION NAME].md`, where `[SUBMISSION NAME]` is the name of your submission. You can see the result of the sample post .

While most users will want to create a post in the Markdown format, it is also possible to create a post in HTML format. For this, modify instead the example  `_posts/2025-04-28-distill-example2.html` and rename it to `_posts/2025-04-28-[SUBMISSION NAME].html`. (NOTE: HTML is not officially supported, use at your own risk).


You must modify the file's header (or 'front-matter') as needed.



 ```markdown
 ---
layout: distill
title: [Your Blog Title]
description: [Your blog post's abstract - no math/latex or hyperlinks!]
date: 2025-04-28
future: true
htmlwidgets: true

# anonymize when submitting 
authors:
  - name: Anonymous 

# do not fill this in until your post is accepted and you're publishing your camera-ready post!
# authors:
#   - name: Albert Einstein
#     url: "https://en.wikipedia.org/wiki/Albert_Einstein"
#     affiliations:
#       name: IAS, Princeton
#   - name: Boris Podolsky
#     url: "https://en.wikipedia.org/wiki/Boris_Podolsky"
#     affiliations:
#       name: IAS, Princeton
#   - name: Nathan Rosen
#     url: "https://en.wikipedia.org/wiki/Nathan_Rosen"
#     affiliations:
#       name: IAS, Princeton 

# must be the exact same name as your blogpost
bibliography: 2025-04-28-distill-example.bib  

# Add a table of contents to your post.
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
toc:
  - name: [Section 1]
  - name: [Section 2]
  # you can additionally add subentries like so
    subsections:
    - name: [Subsection 2.1]
  - name: [Section 3]
---

# ... your blog post's content ...
```

You must change the `title`, `discription`, `toc`, and eventually the `authors` fields (**ensure that the
submission is anonymous for the review process**).

<!-- Add any tags that are relevant to your post, such as the areas your work is relevant to. -->
Read our [sample blog post]({% post_url 2025-04-28-distill-example %}) carefully to see how you can add image assets, and how to write using $$\LaTeX$$!
Read about rendering your post locally [below](#serving).

**Important: make sure your post is completely anonymized before you export and submit it!**

Before going any further, it will be useful to highlight exactly what folders and files you are going to add or modify.
Even if you use one of our simpler quickstart methods, this will always be what's happening 
behind the scenes.

If you clone our repo or download a release, you will find a directory structure that looks like 
the following (excluding all files and directories that are not relevant to your submission): 

```bash
your_blogpost_repo/
│
├── _posts
│   ├── 2025-04-28-[YOUR SUBMISSION].md         # <--- Create this markdown file; this is your blogpost
│   └── ...
├── assets
│   ├── bibliography
│   │   ├── 2025-04-28-[YOUR SUBMISSION].bib    # <--- Create this bibtex file
│   │   └── ...
│   ├── html
│   │   ├── 2025-04-28-[YOUR SUBMISSION]        # <--- Create this directory and add interactive html figures
│   │   │   └──[YOUR HTML FIGURES].html
│   │   └── ...
│   ├── img
│   │   ├── 2025-04-28-[YOUR SUBMISSION]        # <--- Create this directory and add static images here
│   │   │   └──[YOUR IMAGES].png
│   │   └── ...
│   └── ...
└── ...
```

In summary, to create your post, you will: 

- Create a Markdown (or HTML) file in the `_posts/` directory with the format `_posts/2025-04-28-[SUBMISSION NAME].md` (`_posts/2025-04-28-[SUBMISSION NAME].html` in the case of an HTML file). 
- Add any static image assets will be added to `assets/img/2025-04-28-[SUBMISSION NAME]/`.
- Add any interactive HTML figures will be added  to `assets/html/2025-04-28-[SUBMISSION NAME]/`. 
- Put your citations into a bibtex file in `assets/bibliography/2025-04-28-[SUBMISSION NAME].bib`. 

**DO NOT** touch anything else in the blog post!
If you do, our automated pipeline will reject your PR and you will have to undo those changes in order for it to be accepted!

Note that `2025-04-28-[YOUR SUBMISSION]` serves as a tag to your submission, so it should be the
same for all three items.
For example, if you're writing a blog post called "Deep Learning", you'd likely want to make your
tag `2025-04-28-deep-learning`, and the directory structure would look like this:

```bash
your_blogpost_repo/
│
├── _posts
│   ├── 2025-04-28-deep-learning.md         # <--- Create this markdown file; this is your blogpost
│   └── ...
├── assets
│   ├── bibliography
│   │   ├── 2025-04-28-deep-learning.bib    # <--- Create this bibtex file
│   │   └── ...
│   ├── html
│   │   ├── 2025-04-28-deep-learning        # <--- Create this directory and add interactive html figures
│   │   │   └──[YOUR HTML FIGURES].html
│   │   └── ...
│   ├── img
│   │   ├── 2025-04-28-deep-learning        # <--- Create this directory and add static images here
│   │   │   └──[YOUR IMAGES].png
│   │   └── ...
│   └── ...
└── ...
```

### Local serving

So far we've talked about how to get the relevant repository and create a blog post conforming to our requirements.
Everything you have done so far has been in Markdown, but this is not the same format as web content (typically HTML, etc.).
You'll now need to build your static web site (which is done using Jekyll), and then *serve* it on some local webserver in order to view it properly.
We will now discuss how you can *serve* your blog site locally, so you can visualize your work before you open a pull request on the staging website so you can submit it to the ICLR venue.

#### Method 1: Using Docker 

To render your website locally, we follow the instructions for [Local setup using Docker (Recommended on Windows)](https://github.com/iclr-blogposts/iclr-blogposts.github.io/blob/master/README.md#local-setup-using-docker-recommended-on-windows), but specifically you will need to create your own docker container rather than pull it from Dockerhub (because we modified the Gemfile).

Create and run the Docker image:

```bash
./bin/docker_run.sh
```

Remove the `Gemfile.lock` file if prompted.
This will create a docker image labeled as `al-folio:latest`. 
Don't use `dockerhub_run.sh`; this may result in issues with missing jekyll dependencies.


#### Method 2: Using Jekyll Manually

For users wishing to not use a Docker container, you can install Jekyll directly to your computer and build the site using Jekyll directly.
This is done at your own risk, as there are many potential points of error!
Follow the instructions for rendering the website via the conventional method of `$ bundle exec jekyll serve --future`

##### Installation

You will need to manually install Jekyll which will vary based on your operating system.
The instructions here are only for convenience - you are responsible for making sure it works on your system and we are not liable for potential issues that occur when adding your submissions to our repo!

**Ubuntu/Debian**

1. Install Ruby

    ```bash
    sudo apt install ruby-full
    ```

2. Once installed, add the following to your `.bashrc` or whatever terminal startup script you may use (this is important because otherwise gem may complain about needing sudo permission to install packages):

    ```bash
    export GEM_HOME="$HOME/.gem"
    export PATH="$HOME/.gem/bin:$PATH"
    ```

3. Install Jekyll and Bundler:

    ```bash
    gem install jekyll bundler
    ```

**MacOS and Windows**

Mac and Windows users can find relevant guides for installing Jekyll here:

- [Windows guide](https://jekyllrb.com/docs/installation/windows/)
- [MacOS guide](https://jekyllrb.com/docs/installation/macos/)

##### Manual Serving

Once you've installed jekyll and all of the dependencies, you can now serve the webpage on your local machine for development purposes using the `bundle exec jekyll serve` command.

You may first need to install any project dependencies. In your terminal, from the directory containing the Jekyll project run:

```bash
bundle install
```

This will install any plugins required by the project. 
To serve the webpage locally, from your terminal, in the directory containing the Jekyll project run:

```bash
bundle exec jekyll serve --future --port=8080 --host=0.0.0.0
```

You should see something along the lines of:

```
> bundle exec jekyll serve
Configuration file: /home/$USER/blog_post_repo/_config.yml
            Source: /home/$USER/blog_post_repo
       Destination: /home/$USER/blog_post_repo/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
       Jekyll Feed: Generating feed for posts

        ... you may see a lot of stuff in here related to images ...

                    done in 0.426 seconds.
 Auto-regeneration: enabled for '/home/$USER/blog_post_repo'
    Server address: http://0.0.0.0:8080/2025/
  Server running... press ctrl-c to stop.
```

If you see this, you've successfully served your web page locally!
You can access it at server address specified, in this case `http://0.0.0.0:8080/2025/` (and the blog posts should once again be viewable at the `blog/` endpoint).


### Submitting your Blog Post

To submit your blog post:

1. **Anonymize your blog post.** Strip all identifying information from your post, including the 
  author's list (replace with `Anonymous`).
2. Double check that your post matches the formatting requirements, including (but not limited to):
    - **Only modify** files in the following locations (failure to do so will result in your PR 
      automatically being closed!):
        - a Markdown (or HTML) file in `_posts/` with the format `_posts/2025-04-28-[SUBMISSION NAME].md` 
          (or `.html`)
        - static image assets added to `assets/img/2025-04-28-[SUBMISSION NAME]/`
        - interactive HTML figures added  to `assets/html/2025-04-28-[SUBMISSION NAME]/`
        - citations in a bibtex file in `assets/bibliography/2025-04-28-[SUBMISSION NAME].bib`
    - Have a short 2-3 sentence abstract in the `description` field of your front-matter ([example](https://github.com/iclr-blogposts/2025/blob/295ab5b4c31f2c7d421a4caf41e5481cbb4ad42c/_posts/2025-04-28-distill-example.md?plain=1#L4-L6))
    - Have a table of contents, formatted using the `toc` field of your front-matter ([example](https://github.com/iclr-blogposts/2025/blob/295ab5b4c31f2c7d421a4caf41e5481cbb4ad42c/_posts/2025-04-28-distill-example.md?plain=1#L36-L47))
    - Your bibliography uses a `.bibtex` file as per the sample post
3. Open a pull request against the `main` branch of the [2025 repo](https://github.com/iclr-blogposts/2025). 
  Fill in the checklist provided in the PR template. The title of your pull request should be 
  exactly the name of your markdown/html file.
    - i.e. `_posts/2025-04-28-[SUBMISSION NAME].md` would require a PR name `2025-04-28-[SUBMISSION NAME]` 
4. **(TBD - will be set up soon!)** Your post will automatically run two pipelines: one to verify that you have not modified any other 
  file in the repo, and another that will create a unique URL for your contributed blog post.
    - Verify that everything looks correct in the given URL.
    - If the pipelines failed, check if it was because of improper formatting (i.e. you modified 
      restricted files). If this is the case, fix the issues. If the issue persist, please ping one of the repo admins.
      
5. Submit the name of your blog post and its URL to our OpenReview through [this link](https://openreview.net/group?id=ICLR.cc/2025/BlogPosts&referrer=%5BHomepage%5D(%2F)).

> **Note:** If you wish to make updates to your submission, you should update the content in the 
> PR that you already opened. 

### Reviewing Process

Reviewers will be required to only view the live content of the reviewing website - the website to which the Pull Requests push to. 
We ask that they act in good faith, and refrain from digging into the repository's logs and closed Pull Requests to find any identifying information on the authors.

### Camera-ready

**TBD** - instructions will be provided closer to the submission deadline.

### Full guide coming soon!