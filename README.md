# palanx-blog
HUGO project to create my personal Blog and stop using third party services with subscription.

## Commands

## Init Dependencies
```
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.14.1
echo '. "$HOME/.asdf/asdf.sh"' >> ~/.zshrc
source ~/.zshrc
asdf plugin add golang
asdf plugin add hugo
asdf intall
git submodule update --init -- themes/blowfish
```

### Start Server
``` bash
hugo server -D
```
Then manually go to http://localhost:1313.
Hugo doesn’t auto-redirect to specific pages.

### Create Content

#### Create default content
Use:
``` bash
hugo new content/my-new-post.md
```
If `archetypes/default.md` exists, it will apply the template.

#### Create cooking recipe content
For ES, use:
``` bash
hugo new --kind cooking-recipe recipe/chicken-masala.md
```
For EN, use:
``` bash
hugo new --kind cooking-recipe.en recipe/chicken-masala.en.md
```

#### Create book recommendation content
For ES, use:
``` bash
hugo new --kind book-recommendation books/how-to-clean-your-cheeks.md
```
For EN, use:
``` bash
hugo new --kind book-recommendation.en books/how-to-clean-your-cheeks.en.md
```

### Dependency
* Blowfish Theme: v2.89.1
* Hugo: v0.148.1-extended
