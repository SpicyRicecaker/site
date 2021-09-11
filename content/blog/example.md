---
title: Setting up a blog (and this website)
description: How in the world do we get this blog started?
date: 2021-09-11
---

Rationale: I want make a blog for a couple reasons. One is that when I go through setting up something complicated, like configuring neovim, oftentimes it's helpful for me to document the steps, so they're easier to retrace later (if I lose the configuration, for example). Explaining my decisionmaking process to my future self would also be very helpful, so that the next time I go into setting it up I don't have to reread every piece of documentation out there. It might be helpful for you, dear reader, as well. It's a win-win. It's also a place that I can hopefully vent if I need to.

# Layout

## Choosing a Framework

I really want to try making it with SvelteKit. The setup is easy, and default config with hot-module reloading makes development on the project simply a joy. Svelte is probably the most performant frontend framework there is. 

## How do I add a new post

The best way to write notes on a desktop would be in markdown format. I know I want to use it to write my blog posts. So the question then becomes: how do I integrate svelte with markdown?

## Looking at exemplars

### Elianiva's Site

[Elianiva](https://github.com/elianiva), a contributor to various neovim projects, has their own [website](https://github.com/elianiva/elianiva.my.id) made with sveltekit. They include YAML metadata with title, description, date information at the top of the markdown file for each blog. Then when a user clicks on `posts`, the content of all of the blogs are retrieved via `fs.read`, which is fed into a data parsing library called [gray matter](https://github.com/jonschlinkert/gray-matter)
```yaml
---
title: titlehere
description: descriptionhere
...
---
```
Gray matter then returns a list of data on blogs `.json` format, which can be used in the blogs listings.
```json
[

    {title: "bob", description: "a blog about bob"},
    {title: "joe", description: "a blog about joe"},
    // ...
]
```
The body of the blog posts are written in `.svx` format, a filename extension representing markdown documents with jsx in them. This is then preprocessed via a library called [MDsveX](https://github.com/pngwn/MDsveX), which allows for svelte in markdown. 

Keep in mind that all of this data is generated beforehand, because sveltekit can function like an SSG (static-site-generator). Therefore, it's not like the markdown files are being read and rendered every time a different user clicks on a post. The data is already there when the developer compiles it.

This was one of my biggest worries in making a post without sveltekit, since if I was using pure svelte I probably would've ended up with an implementation where I fetched and parsed the markdown files everytime a user wanted to load a blog. 

Finally, the file structure is a bit convoluted. Each post is nested under `/src/routes/post/(folder)/(post).svx`.

### Official svelte.dev site

The official [svelte.dev](https://svelte.dev/blog/) blog seems to take a similar approach, except using less third-party libraries. There is still YAML metadata, but it's just parsed using simple regex. Actual blog posts are written in plain markdown, and fed into [marked](https://github.com/markedjs/marked), a well-known markdown -> html parser written in javascript. 

I'm in support of the file structure, which just really just includes posts at `/content/(post).svx`.

## Choosing a layout

I chose to use the well-known marked over mdsvex for rendering markdown, but I will use gray-matter instead of regex to retrieve the frontmatter. Honestly though, the most important part for me is to get static generation working.

I'll include all my posts under `/content`.

WIP TBC