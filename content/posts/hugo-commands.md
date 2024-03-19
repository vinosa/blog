---
title: "Hugo Commands"
date: 2024-03-19T11:20:16+01:00
draft: false
categories:
- Development
- Hugo
tags:
- dev
- hugo
---
### Install
```
snap install hugo --channel=extended

or 

sudo apt-get install hugo
```

### Test installation

```
hugo help 
```
### Create new site

```
hugo new site <site> -f yml
hugo new site --force . // in existing repository folder
```

### Add a new theme

```
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

### Add some content

```
hugo new posts/my-first-post.md
```

### Add folder root

```
hugo new notes/history/_index.md
```

### Start Hugo Server (with drafts enabled)

```
hugo server -D  --navigateToChanged // http://localhost:1313/
```

### Deploy (to public folder)

```
hugo
```
### Force Stop server

```
 killall -9 hugo
```

### Build static pages

```
hugo -D
```

