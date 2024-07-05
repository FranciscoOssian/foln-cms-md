# [Foln.dev](https://www.foln.dev) CMS - MD+YAML
Data stored for my website.
An attempt to create a different CMS that is easy to create and edit, without having to deal with other services.
The annoying part is creating the functions to access the content of the files, but this can be solved if the idea becomes a project with its own lib to manipulate the data.

# settings.yml example.

Noting here the wording of the special settings.yaml file, the webserver converts it to json and uses the data. Any values ​​relating to the project or more focused data such as SEO or openGraph can be used

```yaml
title: "Post Title"
categories:
  - "Technology"
  - "Web Development"
  - "etc"
description: "This is a post about X, Y, and Z."
keywords: "markdown, CMS, GitHub, YAML, Front Matter"

OG:
  title: "Open Graph Title"
  description: "Open Graph Description"
  image: "/path/to/image.jpg"
  url: "https://www.example.com/page"
  type: "article"
  keywords: "SEO, Open Graph, Social Media"
```

The idea of storing on GitHub is not only due to the ease and cost-effective data allocation, but also due to the public visibility, the collaboration that this repository can have, and the API that GitHub provides in addition to working with git and allowing for high version control if necessary. For example, in a scenario where blog posts are stored here, collaboration on the blog content, both editing and creation, can be public and accessible to anyone. However, it is clear that this is only a proof of concept and applies only to a small and non-constant post-based blog. In my case, I cache at build time with Next.js.
