---
layout: base
title: Exercises
permalink: /about/
---

I am compiling a collection of exercises that I came across during my research.
Most problems have short descriptions, and can be solved in a few lines.
Some of them are original and might (hopefully) inspire some thoughts.
<!-- Presumably, LLMs would struggle with these problems due to their originality. -->

Feel free to email me if you {find a mistake, want to discuss a solution, managed to solve them by prompting LLMs}.

{% for puzzle in site.puzzles %}
  <h3>
    <a href="{{ puzzle.url }}">
      {{ puzzle.title }}
    </a>
  </h3>
{% endfor %}
