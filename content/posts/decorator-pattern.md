---
title: "Decorator Design Pattern"
description: "A useful design pattern implementation in PHP"
date: 2023-01-10T11:46:10+01:00
draft: false
categories:
- Development
- PHP
tags:
- dev
- php
---
# Decorator Design Pattern Implementation

## How to decorate classes

```php
interface NameInterface{
	
}

class Name implements NameInterface
{
	public function __construct(
			protected $first,
			protected $last) {
	}
	
	public function __toString()
	{
		return $this->first . ' ' . $this->last;
	}
}

abstract class AbstractName extends Name
{	
	public function __construct(protected NameInterface $origin) {
		$this->first = $origin->first;
		$this->last = $origin->last;
	}
}

class PrefixedName extends AbstractName
{
	public function __construct(protected NameInterface $origin, private string $prefix) {
		parent::__construct($origin);
	}
	
	public function __toString()
	{
		return $this->prefix . ' ' . (string) $this->origin;
	}
}
```

