---
name: jekyll-new-post
description: Create a new Jekyll blog post with the correct filename format and YAML front matter for this Chirpy-themed Jekyll site. Use when the user asks to create a new blog post, new article, new post, write a new blog, add a post, or any similar request related to creating content in the _posts/ directory.
---

# Jekyll New Post

Create a new blog post file in `_posts/` for this Jekyll site.

## Conventions

- **Filename**: `YYYY-MM-DD-{slug}.md`
  - `slug`: lowercase English words separated by hyphens (e.g., `study-thinking`, `perspective-projection`)
  - Do NOT use Chinese characters or spaces in the filename slug
- **Front matter**:
  ```yaml
  ---
  title: "Post Title"
  description: "Short description"
  date: YYYY-MM-DD HH:MM +0800
  categories: [CATEGORY_NAME]   # uppercase recommended
  tags: [tag_name]              # always lowercase
  ---
  ```
- **Timezone**: Always use `+0800`
- **Language**: Post content is written in Chinese
- **Images**: Place in `assets/img/` and reference with `/assets/img/filename.png`

## Workflow

1. Determine the post title from user input.
2. Generate or confirm the filename slug (English, lowercase, hyphenated).
3. Get current date and time in `Asia/Shanghai` timezone.
4. Ask user for `categories` and `tags` if not provided; use sensible defaults if they skip.
5. Ask user for `description` if not provided; leave empty if they skip.
6. Create the file with the correct front matter and save to `_posts/`.
7. Report the created file path to the user.
