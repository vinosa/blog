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

    public function act(): Post
    {        
        return new Done($this);
    }
}
```
### Workflow State Decorator
```

class Approved implements Post
{
    private $origin;

    public function __construct(
        Post $origin
    ) {
        $this->origin = $origin;
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

    public function act(): Post
    {
        $this->origin->act();
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
    private $origin;

    public function __construct(
        Post $origin
    ) {
        $this->origin = $origin;
    }

    public function save(EntityManagerInterface $entityManager): Post
    {
        return $this;
    }

    public function entity(): PostEntity
    {
        return $this->origin->entity();
    }

    public function act(): Post
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

    public function item(Post $item): bool
    {
        return $item->entity()->isApproved();
    }
}
```

### Array entities source

```

class ArrayPosts implements Posts
{
    private $items;
    private $filters;

    public function __construct(
        array $items
    ) {
       $this->items = $items; 
       $this->filters = [];
    }

    public function toArray(): array
    {
        return array_filter(
            $this->items,
            function (Post $item)
            {
                return array_reduce(
                    $this->filters,
                    function(bool $take, Filter $filter) use ($item)
                    {
                        return $take && $filter->item($item);
                    },
                    true
                );
            }
        );
    }

    public function filter(Filter $filter): Posts
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
    private $entityManager;
    private $filters;

    public function __construct(
        EntityManagerInterface $entityManagerInterface
    ) {
       $this->entityManager = $entityManagerInterface; 
       $this->filters = [];
    }

    public function toArray(): array
    {
        $qb = $this->entityManager->createQueryBuilder();
        $qb->select('p')->from(PostEntity::class, 'p');

        $qb = array_reduce(
            $this->filters,
            function (QueryBuilder $queryBuilder, Filter $filter) {
                return $filter->query($queryBuilder);
            },
            $qb
        );

        return $qb
            ->getQuery()
            ->getResult();
    }

    public function filter(Filter $filter): Posts
    {
        $new = clone $this;
        $new->filters[] = $filter;

        return $new;
    }
}
```
