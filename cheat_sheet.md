# Markdown Cheat Sheet

This Markdown cheat sheet provides a quick overview of all the Markdown syntax elements. It can’t cover every edge case, so if you need more information about any of these elements, refer to the reference guides for [basic syntax](https://www.markdownguide.org/basic-syntax) and [extended syntax](https://www.markdownguide.org/extended-syntax).

## Basics

### Headings

```markdown
# H1
## H2
### H3
```

### Bold

```markdown
**bold text**
```

### Italic

```markdown
*italicicized text*
```

### Blockquote

```markdown
> blockquote
```

### Ordered List

```markdown
1. First Item
2. Second Item
3. Third Item
```

### Unordered List

```markdown
- First Item
- Second Item
- Third Item
```

### Code

```markdown
`code`
```

### Horizontal Rule

```markdown
---
```

### Link

```markdown
[title](https://www.example.com)
```

### Image

```markdown
![alt text](image.jpeg)
```

## Extended

### Table

```markdown
| Syntax | Description |
| ---------- | ---------- |
| Header | Title |
| Paragraph | Text |
```

### Fenced Code Block

```markdown
​```
{
	"firstName": "John",
	"lastName": "Smith"
}
​```
```

### Footnote

```markdown
Here's a sentence with a footnote. [^1]

[^1]: This is the footnote
```

### Heading ID

```markdown
### My Heading {#custom-id}
```

### Definition List

```markdown
term
: definition
```

### Strikethrough[^1]

```
~~The world is flat.~~
```

### Task List

```markdown
- [x] Write press release
- [ ] Update website
- [ ] Contact Media
```

[^1]: Notice these are tildes (~), not dashes