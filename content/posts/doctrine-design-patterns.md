---
title: "My Doctrine Design Patterns"
date: 2024-12-10T14:26:42+01:00
draft: false
categories:
  - Development
  - Doctrine
  - Symfony
tags:
  - dev
  - doctrine
  - symfony
---
# My custom Doctrine Design Patterns

## Entity workflow control with Decorators

### ORM entity

```
use Doctrine\ORM\EntityManagerInterface;

class PostEntity implements Post
{
    public function save(EntityManagerInterface $entityManager): Post
    {
        $entityManager->persist($this->entity());
        $entityManager->flush();

        return $this->act();
    }

    public function entity(): PostEntity
    {
        return $this;
    }

    public function done(): Post
    {        
        return new Done($this);
    }
}
```
### Workflow State Decorator
```

class Approved implements Post
{
    public function __construct(private Post $origin)
    {
    }

    public function save(EntityManagerInterface $entityManager): Post
    {
        $entityManager->persist($this->entity());
        $entityManager->flush();

        return $this->act();
    }

    public function entity(): PostEntity
    {
        return $this->origin->entity()->approved();
    }

    public function done(): Post
    {
        $this->origin->done();
        /**
        * new state subsequent workflow code
        */

        return new Done($this);
    }
}
```
### 'Disabling' decorator (ensures workflow is ran only once by an object)
```

class Done implements Post
{
    public function __construct(private Post $origin) 
    {
    }

    public function save(EntityManagerInterface $entityManager): Post
    {
        return $this;
    }

    public function entity(): PostEntity
    {
        return $this->origin->entity();
    }

    public function done(): Post
    {
        return $this;
    }
}

```

## Entities filtering from database or memory

### Filter class
```
use Doctrine\ORM\QueryBuilder;

class Approved implements Filter
{
    public function query(QueryBuilder $queryBuilder): QueryBuilder
    {
        return $queryBuilder
            ->andWhere('p.status = :status')
            ->setParameter('status', (new PostStatuses())->approved() );
    }

    public function take(Post $item): bool
    {
        return $item->entity()->isApproved();
    }
}
```

### Array entities source

```

class ArrayPosts implements Posts
{
    private $filters;

    public function __construct(private array $items) 
    { 
       $this->filters = [];
    }

    public function toArray(): array
    {
        return array_filter(
            $this->items, 
            fn(Post $item) => array_reduce($this->filters, fn(bool $take, Filter $filter) => $take && $filter->take($item), true)
        );
    }

    public function take(Filter $filter): Posts
    {
        $new = clone $this;
        $new->filters[] = $filter;

        return $new;
    }
}
```

### Database entities source

```

class DatabasePosts implements Posts
{
    private $filters;

    public function __construct(private EntityManagerInterface $entityManager)
    { 
       $this->filters = [];
    }

    public function toArray(): array
    {
        $qb = $this->entityManager->createQueryBuilder();
        $qb->select('p')->from(PostEntity::class, 'p');

        $qb = array_reduce($this->filters, fn(QueryBuilder $queryBuilder, Filter $filter) => $filter->query($queryBuilder), $qb);

        return $qb->getQuery()
            ->getResult();
    }

    public function take(Filter $filter): Posts
    {
        $new = clone $this;
        $new->filters[] = $filter;

        return $new;
    }
}
```
