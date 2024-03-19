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
### Create new site

```
hugo new site <site> -f yml
hugo new site --force . // in existing repository folder
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

