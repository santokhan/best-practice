Approach 1 looks convenient, but it’s actually the wrong direction for a relational database.

### Why Approach 1 is problematic

* **`INT[]` is not standard SQL** (only supported in some DBs like PostgreSQL, not MySQL).
* You **can’t enforce foreign keys on array elements**.
  This line is invalid in most systems:

  ```sql
  FOREIGN KEY (image_ids) REFERENCES Images(id)
  ```
* Hard to query:

  * Find all stories using an image → messy
  * Join images → inefficient or impossible without hacks
* Breaks normalization (you’re storing multiple values in one column)

---

### Why Approach 2 is the correct design

This is the proper **many-to-many relationship** pattern.

```sql
CREATE TABLE StoryImages (
  id INT AUTO_INCREMENT PRIMARY KEY,
  story_id INT NOT NULL,
  image_id INT NOT NULL,
  FOREIGN KEY (story_id) REFERENCES Stories(id),
  FOREIGN KEY (image_id) REFERENCES Images(id)
);
```

#### Advantages:

* Fully normalized (clean relational design)
* Proper foreign key enforcement
* Easy queries:

  ```sql
  SELECT * 
  FROM StoryImages si
  JOIN Images i ON si.image_id = i.id
  WHERE si.story_id = 1;
  ```
* Scales well (you can add metadata later like order, caption, etc.)

---

### Best Practice (slight improvement)

You’ll likely need **image ordering** and maybe captions later:

```sql
CREATE TABLE StoryImages (
  id INT AUTO_INCREMENT PRIMARY KEY,
  story_id INT NOT NULL,
  image_id INT NOT NULL,
  position INT DEFAULT 0,
  FOREIGN KEY (story_id) REFERENCES Stories(id),
  FOREIGN KEY (image_id) REFERENCES Images(id)
);
```

---

### What about `featured_image_id`?

Keep it in `Stories` — that’s perfectly fine:

```sql
featured_image_id INT,
FOREIGN KEY (featured_image_id) REFERENCES Images(id)
```

---

### Final verdict

* ❌ Approach 1 → Not recommended / breaks relational principles
* ✅ Approach 2 → Correct, scalable, production-ready

---
