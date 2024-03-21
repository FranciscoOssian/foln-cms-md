# foln-cms-md
CMS stored on github for my website, all .md file is a post, website transform in html on build time and in revalidate

# settings.yml example
```yaml
# Default values are provided for the following settings:
# If a setting is not provided, the default value will be used.

title: "Post Title"  # Default: Folder Name
categories:
  - "Technology"
  - "Web Development"
  - "etc"
description: "This is a post about X, Y, and Z."
keywords: "markdown, CMS, GitHub, YAML, Front Matter"

OG:
  title: "Open Graph Title"  # Default: Folder Name
  description: "Open Graph Description"
  image: "/path/to/image.jpg"
  url: "https://www.example.com/page"  # Default: "https://www.foln/blog/[folder-name]"
  type: "article"  # Default: "article"
  keywords: "SEO, Open Graph, Social Media"
```
