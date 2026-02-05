---
title: "Creating a post"
date: 2026-02-03T13:00:00-06:00
draft: false
---
# Posting to Hugo is easy

So now the site exists. The theme is wired up. The local server is humming along on `localhost:1313`.  

And then you hit the real question:

**How do you actually post something?**

This is where Hugo stops being “a static site generator” and starts being a habit. Writing posts is the entire point, so let’s document that workflow while it’s still fresh in my head.

No cloud, no deployment pipelines, no polish. Just creating content and seeing it live on a locally hosted Hugo site.

---

## Starting the Engine (Again)

If you followed along with my first post, you already know the drill. From the root of your Hugo site:

```powershell
hugo server -D
````

The `-D` flag tells Hugo to include draft posts. This matters more than you think, because Hugo is very opinionated about what it shows you.

Once the server starts, you’ll see something like:

```
Web Server is available at http://localhost:1313/
```

Leave this running. Hugo will watch your files and hot-reload as you work.

---

## Creating a Post the Hugo Way

Sure, you *could* manually create a Markdown file. But Hugo gives you a cleaner, repeatable way to do it.

From the same directory:

```powershell
hugo new posts/my-first-post.md
```

Instantly, Hugo drops a new file into:

```
content/posts/
```

No guessing folder structure. No typos. One command, one file, done.

Pro tip: keep filenames lowercase and hyphenated. URLs are forever, and future-you will thank you.

---

## Understanding the Front Matter

Open the new file in your editor. At the top, you’ll see something like this:

```yaml
---
title: "My First Post"
date: 2026-02-04T09:00:00-06:00
draft: true
---
```

This block is called **front matter**, and it’s how Hugo knows what to do with your content.

A few key points:

* `title` becomes the page title and link text
* `date` controls ordering
* `draft: true` keeps the post out of production builds

As long as that draft flag exists, the post will *only* appear when running Hugo with `-D`.

That’s a feature, not a bug.

---

## Writing the Post (Markdown, Not Magic)

Below the front matter is just Markdown. No templating. No shortcodes required to get started.

I tend to structure posts loosely like this:

```markdown
## What I’m documenting

What problem I was solving or what I was learning.

## The steps

Commands, config, decisions, mistakes.

## Notes from the trenches

Things I’d forget if I didn’t write them down.
```

Code blocks are simple:

```powershell
hugo server -D
```

Inline commands stand out nicely too: `hugo new`.

Nothing fancy. The power is in consistency.

---

## Seeing It Live on localhost

With the server still running, jump back to your browser:

```
http://localhost:1313/
```

If everything’s wired correctly, your post should now appear in the list. Click it. Read it. Scroll it.

Now try editing the file and saving it.

Hugo rebuilds instantly. No refresh loops. No waiting. This is where the workflow really clicks.

---

## Drafts vs Reality

Here’s the gotcha that bites everyone once.

If you stop the server and restart it **without** the `-D` flag:

```powershell
hugo server
```

Your draft post will disappear.

That’s expected behavior.

When you’re ready for the post to be considered “real,” update the front matter:

```yaml
draft: false
```

Or remove the line entirely. Restart the server, and if the post still shows up, it’s ready for prime time.

---

## Common Stumbles

A few things I tripped over early on:

* Post not showing up?
  → Check `draft: true` and whether you started Hugo with `-D`.

* Page won’t render?
  → Front matter indentation matters. YAML is unforgiving.

* Nothing updates?
  → Make sure `hugo server` is still running and didn’t error out in the terminal.

When in doubt, the terminal output usually tells the truth.

---

## Why This Matters

This is the real value of Hugo for me: frictionless documentation.
No login screens. No databases. No editor lag. Just files, commits, and momentum.

Once posting is muscle memory, the site stops being “a project” and starts being a logbook.

Next up, I’ll get into how I organize posts, handle images, and eventually push this whole thing to the cloud. But for now, this is enough.

Write locally. Preview instantly. Repeat.

If you’re following along and hit a snag, you know where to find me — probably staring at a terminal, wondering why I didn’t document the last thing sooner.
