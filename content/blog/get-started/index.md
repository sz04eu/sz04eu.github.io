---
title: "How I Used AI to Build and Deploy a Personal Blog on macOS"
summary: "How I used an AI assistant as a debugging partner to build a HugoBlox blog on an Intel Mac and deploy it to GitHub Pages."
date: 2026-10-25
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
authors:
  - me
tags:
  - Hugo
  - Hugo Blox
  - GitHub Pages
  - macOS
  - Markdown
  - AI
status: published

---
{{< toc mobile_only=true is_open=true >}}


## 1. Introduction
After I finished my PhD, I felt a strange kind of emptiness. For years, my life was strictly organized around research, deadlines, and writing papers. When all of that ended, I felt a bit lost. I needed a new project to keep me busy and focused.

So, I decided to build a personal blog. I wanted a place to record my life, share my thoughts, and document this new chapter. I also wanted it to be free, and I wanted to have full control over my content. There was just one big problem: I am not a web developer.
Working with Node.js packages, dealing with submodules, and figuring out GitHub Actions can be very hard. It is easy to waste a whole weekend just trying to start the website server, instead of actually writing blog posts.

Because of this, I decided to try a different way. This is where AI helped me. I used the AI assistant as a partner to fix problems, such as missing files or security blocks. Most importantly, it helped me understand *why* the error happened.

In this post, I will share how I built and launched this website on an older Intel Mac using MacPorts. I will show the exact mistakes I made and how I checked each AI fix to make sure I was moving forward.

## 2. My Setup
Before I get into the technical steps, I need to explain my setup. 
Here is what I am working with:

- **Hardware:** MacBook Air 2020
- **Processor:** 1.2 GHz Quad-Core Intel Core i7
- **Memory:** 16 GB
- **OS:** macOS Sequoia 15
- **Package Manager:** MacPorts
- **Core Stack:** Hugo Extended, Go, Node.js, and pnpm

I used MacPorts and installed my tools using `sudo port install`. 
Also, running an older Intel chip means I sometimes ran into different build problems compared to newer Macs.
The first steps were quite easy. 
I used MacPorts to install the main tools (Hugo Extended, Go, Node.js, and pnpm). 
Then, I downloaded the HugoBlox blog template and copied it into my GitHub repository. 
I changed my basic site settings and felt very happy for a moment. 

## 3. Building the Site
I decided to use Hugo as the base engine because it is very fast.
I chose **HugoBlox** which is built on top of Hugo. 
I can change settings using simple YAML files and write text in Markdown. 
- 👉 [**Create a new site**](https://hugoblox.com/templates/)
- 📚 [**Personalize your site**](https://docs.hugoblox.com/)
- 💬 [**Hugo Blox community**](https://discord.gg/z8wNYzb) or [**Hugo community**](https://discourse.gohugo.io)

**The First Hurdle**

With everything installed, I opened my terminal, went to my project folder, and typed my first command with confidence:

```bash
pnpm dev
```

This is the standard command to start the local development server so I can preview my site in a browser. I expected to see a local web address pop up so I could start designing. 

Instead, I got my first wall of red text. 

The terminal simply said:

```bash
zsh: command not found: pnpm
```

## 4. Debugging with AI

Once the initial excitement of setting up the template faded, the real work began. Here is a look at the exact errors I hit, how the AI helped me understand them, and how I checked the fixes myself before moving on.

### Pitfall 1: `zsh: command not found: pnpm`

**The Error:**  
Like I mentioned before, when I typed `pnpm dev`, the terminal told me it had no idea what `pnpm` was. 

**What AI Suggested:**
The AI explained that simply installing Node.js doesn't always install `pnpm`, or the terminal doesn't know where to find it yet. It suggested installing `pnpm` globally. Because I use MacPorts instead of Homebrew, the standard instructions online did not work. The AI helped me find the right steps for my system: making sure Node.js was installed through MacPorts, and then using Node's built-in `corepack` tool to turn on `pnpm`.

**How I Verified It:**
Instead of just guessing, I typed `pnpm -v` in the terminal. Once it showed a version number (like `10.14.0`), I knew the system could see it. Only then did I try running `pnpm dev` again.

### Pitfall 2: `binary with name "go" not found in PATH`

**The Error:**
After fixing `pnpm`, I tried the dev command again. Hugo started building, but then it crashed with a module error: `failed to download modules: binary with name "go" not found in PATH`.

**What AI Suggested:**
The AI explained the "why": HugoBlox uses Hugo Modules to manage its themes, and those modules need the **Go** programming language to run behind the scenes. Because I am on an Intel Mac using MacPorts, the AI first gave me a Homebrew command (`brew install go`). I had to correct it and ask for the MacPorts version (`sudo port install go`).

**How I Verified It:**
Even after installing Go, it didn't work immediately because MacPorts puts programs in `/opt/local/bin`, and my current terminal window didn't see it yet. I tested the installation by typing `go version`. Once that worked, I knew the path was correct, and Hugo could finally download its files.

### Pitfall 3: `"tailwindcss" is not whitelisted in policy "security.exec.allow"`

**The Error:**
This was the hardest wall to break. The site built, but then crashed with a massive error saying that `tailwindcss` was blocked by Hugo's security policy. 

**What AI Suggested:**
This is where using AI as a *debugging partner* really helped. The AI explained that in a recent update (Hugo v0.165.0+), the creators changed the default security rules and removed `tailwindcss` from the allowed list. The AI gave me the exact code needed to fix it, reminding me to edit my own site's configuration file, not the theme's file. 

**How I Verified It:**
I opened my `config/_default/hugo.yaml` file and found the security section. I manually added `'^tailwindcss$'` to the list. I saved the file, restarted the server, and watched the terminal screen. When the error went away and the server started, I opened `http://localhost:1313` in my browser to make sure the style and design were actually loading. 

## 5. Deploying to GitHub Pages

Getting the site to run on my own computer was a huge relief, but my real goal was to put it online for everyone to see. At first, I was very confused about how GitHub Pages works with static site generators.

**Why we don't upload the `public/` folder**

When Hugo builds a site, it takes Markdown files and settings and creates a folder named `public/`. This folder contains the final HTML, CSS, and JavaScript files that a web browser can read. 

My first thought was to just upload this `public/` folder directly to GitHub. **Do not do this.** If you do, you will have to manually build the site on your computer every single time you write a new post, and then upload the whole folder again. It takes too much time and gets messy. 

Instead, we upload the **source files** (our Markdown files, settings, and themes) and let GitHub do the building for us automatically.

**The Setup: Workflows vs. Settings**

To make GitHub build the site automatically, you need to understand two main files:

1. **`config/_default/*.yaml` (The Settings):** This is like a recipe for your website. It tells Hugo your site's name, your web address, and what theme to use. Hugo reads this file when building your site.
2. **`.github/workflows/*.yaml` (The Deployment Script):** This acts like a robot. It lives in a hidden folder called `.github`. It tells GitHub Actions exactly *when* and *how* to build your site. Basically, it says: "Every time I upload new content, open a temporary virtual computer, install Hugo, build the site, and publish the `public/` folder."

**Configuring GitHub Pages**

Once my workflow file was ready, I went to my code repository on GitHub and clicked on **Settings > Pages**. 

Under "Build and deployment", I found the "Source" setting. I changed it from "Deploy from a branch" to **"GitHub Actions"**. 

As soon as I uploaded my updated files to the `main` branch, it started working. I clicked the "Actions" tab on GitHub and watched the deployment script run. A minute later, my website was live on the internet!

## 6. What I Learned
Building this blog gave me a place to write my posts, and it also taught me how to use AI as a helpful partner.
Looking back, building this website was frustrating at times, but I felt great to finish it. 

Using AI for this project completely changed how I deal with technical problems. 
It is a helpful tool for reading error messages and explaining changes in new software updates. 
However, AI is not a magic solution. You still need to test the fixes and understand *why* they work. AI can save your debug time, but if you do not understand the reason behind it, you will get stuck again. 


## License
Released under the [MIT](https://github.com/HugoBlox/kit/blob/main/LICENSE.md) license.
