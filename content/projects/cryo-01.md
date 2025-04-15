---
title: "Cryo: Building a replicated B-Tree Database from Scratch - Part 1: Why ?"
date: 2025-04-15
updated: 2025-04-15
---
How I’m Building a Replicated B-Tree Database from the Ground Up.
<!-- more -->
I've recently been working on a side project called [**Cryo**](https://github.com/DavisRayM/cryo), a replicated B-Tree database written from scartch in Rust. Well... "replicated" is a bit aspirational. Right now, Cryo is a single instance BTree store with a basic CLI interface (inspired by SQLite) and supports simple select, insert, update and delete operations.

This blog series is mainly for me to document and share the journey; What i've built so far, what broke in the process, what I've learned and where I'm headed next. I'll be breaking it down over multiple posts, each focusing on a specific layer or challenge in the project.

But this first part is more of a sketchbook than a tutorial.

### The origins of the project

The idea for Cryo first started taking shape during my Masters study. Between long hours in data structures, distributed systems and parallel systems courses and late-night reading sessions with the book *[Database Internals](https://www.oreilly.com/library/view/database-internals/9781492040330/)* by Alex Petrov, I found myself getting more and more curious about the inner working of databases; especially the storage layer.

Textbooks and lectures covered the basic introduction to how B-Trees worked *in theory*, but I wanted to see what I would discover and learn if I actually built one. What would break ? What would I misunderstand and most important What would I learn?

That itch turned into a project; Cryo.

Cryo started with a question I couldn’t shake:
**“What would it take to build a real, replicated, persistent, queryable database from scratch?”**

I wasn’t aiming to replace SQLite or match RocksDB in performance—I just wanted to peel back the layers and *actually understand* what goes into building one.

I chose B-Trees because they’re elegant, widely used, and have stood the test of time. With variants of the original structure improved and utilized by production grade database i.e [Postgresql](https://www.postgresql.org/docs/current/btree.html). And replication because… well, that part excites me; everything in this day and age is replicated one way or another. Wouldn't it be great to truly understand how it's implemented ? How Raft or Paxos truly works; *Paxos especially was interesting to learn in class but I didn't quite get how to implement it*.

### What Cryo Can Do Right Now

So far, Cryo is a command-line tool that supports:

- Creating a single database file on disk
- Retrieving, inserting, updating, and deleting fixed-size rows
- Persisting data using a basic B-Tree structure

Records currently follow a fixed schema:
```sql
(id INTEGER, username CHAR[32], email CHAR[255])
```

All rows are stored in fixed-size slots, which has made the storage layout simpler to start with, but limiting. Users are tied to the specific row structure and does not allow them to define any schema or structure for the data they'd like to store.

### Next Up: Variable Row Sizes

The next big step is moving beyond the fixed row format. Supporting variable-sized rows (and eventually more flexible schemas) means rethinking how rows are encoded, how pages are laid out, and how free space is managed inside each page.

It’s going to make things trickier, but it’s also where the database starts to feel more “real.” I’ll probably break that work into its own post once I dive in.

### What’s Ahead in This Series

Over the next few parts, I’ll be unpacking the deeper mechanics of Cryo:

- How the B-Tree is implemented, including node layout and balancing
- How data is written to disk and organized in pages
- The eventual replication system
- Design trade-offs I’ve made and bugs I’ve had to wrestle with

If you want to check out the code or follow along, it’s all open-source:  
👉 [github.com/DavisRayM/cryo](https://github.com/DavisRayM/cryo)

### Up Next: B-Trees, Splits & Pages

In Part 2, I’ll get into how Cryo’s B-Tree actually works; the core logic behind inserts, node splits, and page storage. Stay tuned.
