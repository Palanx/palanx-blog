# palanx-blog
HUGO project to create my personal Blog and stop using third party services with subscription.

## Commands

### Start Server
``` bash
hugo server -D
```
Then manually go to http://localhost:1313.
Hugo doesn’t auto-redirect to specific pages.

### Create Content
Use:
``` bash
hugo new posts/my-new-post.md
```
If `archetypes/default.md` exists, it will apply the template.
Use:
``` bash
hugo new --kind recipe recipes/chicken-masala.md
```
> This will use archetypes/recipe.md.

### Dependency
* Blowfish Theme: v2.89.1
* Hugo: v0.148.1-extended
