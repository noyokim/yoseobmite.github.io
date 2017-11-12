---
layout: post
title:  "Config Markdown"
date:   2017-11-09 09:12:00 +07:00
author: Young-Seob Kim
categories: [scrap]
tags: [markdown]
---

```bash
Yep, I was looking for this too when I was upgrading to Jekyll 3. After deciphering all the plugins that the processing goes through I ended up with this:

markdown: kramdown
# !github-pages! mandatory › https://help.github.com/articles/using-jekyll-with-pages/#configuration-settings-you-cannot-change
# Since Jekyll 3 the default highlighter is Rouge (replaced Pygments.rb in v44)
highlighter: rouge

# More › http://kramdown.gettalong.org/quickref.html
# Options › http://kramdown.gettalong.org/options.html
kramdown:
  input:          GFM
  # https://github.com/jekyll/jekyll/pull/4090
  syntax_highlighter: rouge

  # Rouge Highlighter in Kramdown › http://kramdown.gettalong.org/syntax_highlighter/rouge.html
  # span, block element options fall back to global
  syntax_highlighter_opts:
    # Rouge Options › https://github.com/jneen/rouge#full-options
    css_class: 'highlight'
    #line_numbers: true # bad idea, spans don't need linenos and would inherit this option
    span:
      line_numbers: false
    block:
      line_numbers: true
      start_line: 1
      
```
