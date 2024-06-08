---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: base 
---

Hi there, welcome to my blog.

I am compiling a collection of interesting exercises that I came across during my research.
Most problems have short descriptions, can be solved in a few lines, and hopefully inspires some thoughts.
They are listed as follows.
Email me if you want a solution and/or find a mistake.

{% for puzzle in site.puzzles %}
  <h3>
    <a href="{{ puzzle.url }}">
      {{ puzzle.title }}
    </a>
  </h3>
{% endfor %}

<h2>Blogs</h2>

{% for blog in site.blogs %}
  <h3>
    <a href="{{ blog.url }}">
      {{ blog.title }}
    </a>
  </h3>
{% endfor %}