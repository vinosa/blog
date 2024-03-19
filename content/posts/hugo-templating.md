---
title: "Hugo Templating"
date: 2024-03-19T11:31:59+01:00
draft: false
categories:
- Development
- Hugo
tags:
- dev
- hugo
---
### Common layout

in baseof.html :
```
{{ block "main" . }}
{{end}}
```
In single.html or list.html :
```
{{define "main" }}
 bla bla
{{end}}
```

### Variables
```
<title>{{ .Title } {.Content}</title>
```

### Custom variables
```
{{ $address := "123 Main St." }}
{{ $address }}
```

### Including partial templates
```
{{ partial "header.html" . }}
```

### Foreach
```
{{ range $elem_index, $elem_val := $array }}
   {{ $elem_index }} -- {{ $elem_val }}
{{ end }}

{{ range $elem_key, $elem_val := $map }}
   {{ $elem_key }} -- {{ $elem_val }}
{{ end }}
```

### With (if exists)
```
{{ with .Params.title }}
    <h4>{{ . }}</h4>
{{ else }}
    {{ .Summary }}
{{ end }}
```

### Global context
```
{{ $title := .Site.Title }}
<ul>
{{ range .Params.tags }}
  <li>
    <a href="/tags/{{ . | urlize }}">{{ . }}</a>
            - {{ $.Site.Title }}
  </li>
{{ end }}
</ul>
```

### Removing white space
```
<div>
  {{- .Title -}}
</div>
```

