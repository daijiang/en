---
layout: post
title:  Some useful keyboard shortcuts for Atom editor
categories: [notes]
tags: [Atom, computer]
---

I am trying to switch to Github's new editor [Atom](https://atom.io/). Here is a note about things I found useful for me.

I like the syntax of Vim when moving cursors around. Thus [**vim-mode**](https://github.com/atom/vim-mode) is a must.

I also like the **multi-cursor** feature from *sublime text*, which I feel is a must for me. Shortcuts within Atom:

- `ctrl-D` if you select a world, then you hit `ctrl-D` and Atom will select next same word for you. Then you can either type directly (which will replace the old word) or use left or right arrow to append things.
- `ctrl-leftclick` you can use this to select locations for multi-cursor wherever you want.
- `shift-alt-down` or `shift-alt-up` to put multi-cursor at multiple lines. Or you can select multiple lines first, then `selection -- split into lines` (in Mac, you can use `cmd-shift-L`, sadly, for windows and linux so far, no similar shortcut for this [in sublime, we can use `ctrl-shift-L`].).

These pretty much cover most of usage of multi-cursor, but I still missing `shift-rightclick_and_drag` feature from *sublime text*.

 *update soon*