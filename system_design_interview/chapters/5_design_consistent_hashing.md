# Chapter 5 - Design Consistent Hashing

Consistent hasing is a technique used to distribute requests across servers

## The rehashing problem

If you have `n` cache servers, it's common to use the following method to balance load:

```python
server_index = hash(key) % n
```

This works well when the size of the server pool is fixed but if we increase `n`, we get different indexes for the same key. This causes a storm of cache misses.

## Consistent hashing 

> A special kind of hashing such that when a hash table is re-sized only k/n keys need to be remapped on average where k is the number of keys and n is the number of slots

- **Hash space and hash ring:** if SHA-1 is used as our hash function, the hash space ranges from 0 to 2^160 - 1. `x0` corresponds to 0 and `xn` corresponds to 2^160 - 1. By connecting both ends, we get a hash ring.
- **Hash servers:** We map servers based on IP onto the ring.
- **Hash keys:** Keys are hashed onto the hash ring.
- **Server lookup:** To determine which server a key is stored on, we go clockwise on the ring until a server is found.
- **Add server:** If we add a server, only a fraction of keys need to be redistributed.

There are two issues with this basic approach:

1. Impossible to keep the same size of partitions on the ring if a server is added or removed. The partitions can get either very small or very large.
2. It is also possible to have non-uniform key distributions on the same ring.

### Virtual nodes

- Each server is represented virtually and can consist of multiple partitions on the actual ring.
- As the number of virtual nodes increases, the distributions of keys becomes more balanced. 
- Trade-off is that more space is needed to represent these

### Finding affected keys

- **Added a server:** When a server is added, the affected range moves anticlockwise from the new server until the next one.
- **Removing a server:** When a server is removed, the affected range also moves anticlockwise until the next server is found.
