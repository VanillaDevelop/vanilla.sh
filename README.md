## vanilla.sh

This is the project for my blog- and portfolio page, [vanilla.sh](https://vanilla.sh). It is a simple, low-functionality 
page that is generated using [Hugo](https://gohugo.io/), a static site generator that converts markdown to 
HTML pages. 

[vanilla.sh](https://vanilla.sh) uses a custom theme called "vanillash", which can be found in the `themes` directory. 
The theme defines layouts, page partials like footers and headers, and shortcodes like grids and cards that are 
used to build some of the static sites, such as the home page.

Blog posts can be found in the `content/posts` directory. The overview page for the blog posts is automatically 
generated at `/posts` and makes use of the heavily stylized `listitem.html` partial.


### Local Development
For local development, install [Hugo](https://gohugo.io/), then run `hugo server --disableFastRender` (or other desired 
development arguments) in the project directory.

### Deployment
The project is deployed via GitHub actions. The deployment pipeline generates the static sites, then deploys them 
to my webserver. The deployment pipeline may be altered in `.github/workflows/deploy.yml`.