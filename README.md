# yuummmer.github.io
# Jennifer Slotnick's Personal Website

This is my personal website and blog, built with [Jekyll](https://jekyllrb.com/) using the [Moonwalk theme](https://github.com/abhinavs/moonwalk) and hosted on **GitHub Pages**. It serves as a portfolio to showcase my work in bioinformatics.

## Features
- Blog with posts on bioinformatics and related topics
- Project showcase section
- Research page
- Custom styling and layouts
- Comments via Utterances

## Setup Instructions

### Run locally (recommended: WSL / Ubuntu)
If you're using WSL, clone and run this repo from the Linux filesystem (e.g. `~/sites/...`), not from `/mnt/c/...` (OneDrive/Windows paths can be slow and may cause Jekyll to hang).

```sh
mkdir -p ~/sites
cd ~/sites
git clone https://github.com/yuummmer/yuummmer.github.io.git
cd yuummmer.github.io

# Install gems locally (avoids permission issues)
bundle config set --local path 'vendor/bundle'
bundle install

bundle exec jekyll serve
# optional:
# bundle exec jekyll serve --livereload
```
The website should now be accessible at `http://localhost:4000`.

## Deployment
The site is hosted on **GitHub Pages**. Any changes pushed to the `main` branch will trigger a rebuild and deploy automatically.

## Contributing
If you'd like to suggest improvements, feel free to open an issue or submit a pull request.

## License
This project is licensed under the MIT License.
```
