---
title: "Learning Hugo(1)"
author: "Haobo Gu"
tags: [Hugo]
date: 2019-08-23T15:35:00+08:00
draft: true
---

<!--more-->

## Content Source Organization

The top level of content folder is `./content/`. Under this folder, the contents can be organized and nested at any level. The following is an example:

```sh
.
└── content
    ├── about
    │   └── index.md  				// <- https://example.com/about/
    ├── posts
    │   ├── firstpost.md   		// <- https://example.com/posts/firstpost/
    │		├── secondpost.md  		// <- https://example.com/posts/secondpost/
    │   └── happy
    │				├── ness.md  			// <- https://example.com/posts/happy/ness/
    │       └── kittens       // <-- Section, because contains _index.md
    │      			└── _index.md
    └── quote
        ├── first.md       		// <- https://example.com/quote/first/
        └── second.md      		// <- https://example.com/quote/second/
```

The page origanization conforms to the content source organization. However, each type of pages has an ***index page***, which is the front page when you goes to this category. For instance, the index page of `tags` looks like: ![image-20190823154917184](http://haobo-markdown.oss-cn-zhangjiakou.aliyuncs.com/markdown/2019-08-23-074917.png)

The correspondence between source file paths and site urls is:

```sh
# Content file path
.         url
.       ⊢--^-⊣
.        path    slug
.       ⊢--^-⊣⊢---^---⊣
.           filepath
.       ⊢------^------⊣
content/posts/_index.md


# Site urls
                     url ("/posts/")
                    ⊢-^-⊣
       baseurl      section ("posts")
⊢--------^---------⊣⊢-^-⊣
        permalink
⊢----------^-------------⊣
https://example.com/posts/index.html
```

### Override Destination Paths in Front Matter

In front matter, the `slug`, `url`, and `layour` can be overridden. For example, if a post's path is `content/posts/old-post.md`, by default, its url would be `https://example.com/posts/old-post/`. We can set the slug in front matter to change the url:

```yaml
title: New Post
slug: "new-post"
```

 Then the url is changed to `https://example.com/posts/new-post/`

The url can be also overridden by `url` option in front matter:

```yaml
title: New Post
url: https://example.com/blog/new-url/
# OR
url: /blog/new-url/
```

In the second case, the url will be changed to `baseUrl + /blog/new-url/`.

For more information about url configuration, see [URL Management](https://gohugo.io/content-management/urls/)

## Config Markdown Renderer

The default markdown renderer in Hugo is [Blackfriday](https://github.com/russross/blackfriday). There are lots of configurations in `config.toml` about Blackfriday:

| Config name     | Default value | Explanation                                                  |
| --------------- | ------------- | ------------------------------------------------------------ |
| taskLists       | true          | Markdown TO-DO List                                          |
| smartypants     | true          | Smart punctuation subsitutions, such as `(c)`-> ©            |
| fractions       | true          | Render fractions in a collapsed format                       |
| extensions      | []            | Enable [Blackfriday's Markdown extensions](https://gohugo.io/content-management/formats/#blackfriday-extensions) |
| HrefTargetBlank | false         | Open **absolute** links in a new window or tab               |

