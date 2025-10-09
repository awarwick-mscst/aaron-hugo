# This is How It All Starts: Documenting My Hugo Odyssey

Hey there, fellow tinkerer. If you're anything like me—a developer who's spent too many late nights wrestling with bloated CMSes—you know the thrill of stripping things back to basics. Static sites? They're the minimalist's dream: fast, secure, and zero server-side drama. Today, I finally carved out time to dip my toes into Hugo, the Go-powered static site generator that's been on my radar for years. No more excuses. This post is my raw, unfiltered log of the setup process. Think of it as a breadcrumb trail for anyone following in my footsteps. Let's break it down, step by step, because if I'm doing this once, I might as well make it repeatable.

## Prerequisites: Gear Up with the Essentials

Before firing up Hugo, I needed the foundational tools. I'm on Windows (your mileage may vary), so I leaned on familiar installers to keep things snappy.

1. **Grab Git**: Version control is non-negotiable, even for a solo demo site. Headed to [git-scm.com](https://git-scm.com/) and downloaded the latest installer. Ran it with defaults—nothing fancy. A quick `git --version` in my terminal confirmed it was live.

2. **Install Go**: Hugo's built in Go, but you don't need the full toolchain unless you're hacking the source. Still, having it around never hurts. Snagged it from [golang.org](https://golang.org/) and let the installer handle the PATH magic. Verified with `go version`.

3. **Hugo via Winget**: Why complicate things? Windows' built-in package manager is a gem for this. Opened PowerShell as admin and ran:
   ```
   winget install Hugo.Hugo.Extended
   ```
   The "Extended" flavor includes goodies like SCSS processing and Chroma syntax highlighting—essential for any theme worth its salt. Post-install, `hugo version` spat out something like "hugo v0.XX.X-extended windows-amd64," and I was golden.

Pro tip: If winget gripes about permissions, double-check your execution policy with `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`.

## Carving Out a Workspace

Organization first, chaos later. I didn't want my Hugo experiments cluttering my root drive, so I created a dedicated spot in my user profile: `~/source`. It's simple, accessible, and plays nice with my existing habits.

- Navigated to `%USERPROFILE%` in Explorer.
- Made a new folder: `source`.
- Inside that, created `hugodemo` for my inaugural site. (We'll populate it next.)

This keeps things tidy—your source lives here, builds go to `public/`, and you're not hunting for files later.

## Scaffolding the Site: Hugo's Quickstart Magic

With tools in place, it was time to summon the beast. From my `source` directory, I dropped to the command line and let Hugo do the heavy lifting:

```
hugo new site hugodemo
cd hugodemo
```

Boom. In seconds, Hugo spun up a skeleton: `config.toml`, `archetypes/`, `content/`, `layouts/`, `static/`, `themes/`, and `public/`. It's bare-bones but battle-ready. No fluff, just a canvas.

## Picking and Plumbing in a Theme

Hugo ships theme-less by default, which is both a blessing and a curse. To see something pretty without wiring up my own CSS, I browsed [themes.gohugo.io](https://themes.gohugo.io/). Scrolling through the retro vibes, the [Terminal theme](https://github.com/panr/hugo-theme-terminal) jumped out—simple, monospaced elegance with duotone syntax highlighting. Perfect for a dev blog.

To integrate it without bloating my repo, I went the submodule route (clean, versioned, and Git-friendly). From the `hugodemo` root:

```
git submodule add -f https://github.com/panr/hugo-theme-terminal.git themes/terminal
```

(Note: I had to tack on `.git` and specify `themes/terminal`—Git's picky like that. If you're cloning fresh, init your repo first with `git init`.)

This pulls the theme into `themes/terminal` without copying files outright. Now, tweak `config.toml` to wire it up:

```toml
baseURL = "/"
languageCode = "en-us"
title = "My Hugo Demo"

theme = "terminal"

[params]
  contentTypeName = "posts"
```

That's it. No deep config dives yet—Terminal's defaults are solid.

## Igniting the Server: From Code to localhost

Ready to feast your eyes? Fire up the dev server:

```
hugo server -t terminal
```

Hugo churned for a millisecond, then beamed: "Web Server is available at //localhost:1313/ (bind address 127.0.0.1)". (Those forward slashes? Windows quirk—ignore 'em.) Popped open my browser, punched in `localhost:1313`, and there it was: a crisp, retro landing page staring back at me. Empty content directory? Sure, but the skeleton proved the pipes worked. Auto-reload meant tweaks to `config.toml` hot-swapped without a restart.

## Reflections from the Trenches

Total time: Under 30 minutes, including detours for typos and docs dives. Hugo's opinionated simplicity is its superpower—no npm installs, no Yarn lockfiles, just pure, declarative bliss. Next up? Populating that `content/posts/` dir with real markdown and maybe tweaking Terminal's color scheme via its [CSS generator](https://panr.github.io/terminal-css/).

If you're stalling on static sites like I was, this is your sign: Start small, document as you go. It's not just setup—it's momentum. What's your first Hugo experiment? Drop a comment if you hit snags; I'll be here, caffeinated and compiling.

*Last updated: October 07, 2025 | ~400 words | 2 min read*