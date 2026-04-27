# Which one is the right approach

Image table on db

```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  uuid VARCHAR(36) NOT NULL,
  filename VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Approach 1**

We can store image_ids: int[] on Story data table.

```sql
CREATE TABLE Story (
  id INT AUTO_INCREMENT PRIMARY KEY,
  uuid VARCHAR(36) NOT NULL,
  featured_image_id INT NOT NULL,
  image_ids INT[] NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  FOREIGN KEY (featured_image_id) REFERENCES Images(id)
  FOREIGN KEY (image_ids) REFERENCES Images(id)
);
```

**Approach 2**

We can use a relational table like StoryImages.

```sql
CREATE TABLE StoryImages (
  id INT AUTO_INCREMENT PRIMARY KEY,
  story_id INT NOT NULL,
  image_id INT NOT NULL,
  FOREIGN KEY (story_id) REFERENCES Stories(id),
  FOREIGN KEY (image_id) REFERENCES Images(id)
);
```
