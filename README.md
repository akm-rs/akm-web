# akm-web

Website for [AKM — Agent Kit Manager](https://github.com/akm-rs/akm), hosted on GitHub Pages at [akm.raphaelsimon.fr](https://akm.raphaelsimon.fr).

## Local Preview

```bash
bundle exec jekyll serve
```

Opens at `http://localhost:4000`. Rebuilds automatically on file changes.

## Setup on a New Machine

```bash
# Install Ruby (Ubuntu/Debian)
sudo apt install ruby-full build-essential zlib1g-dev

# Avoid installing gems as root
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Install Bundler and project dependencies
gem install bundler
bundle install
```

## Repository Map

| Path | What's there |
|---|---|
| `index.html` | Homepage (hero, features, install instructions) |
| `docs/` | All documentation pages (getting started, CLI reference, domains, config, skill format) |
| `_layouts/` | Page templates — `default.html`, `docs.html`, `home.html` |
| `_includes/` | Reusable components — header, footer, sidebar, head |
| `_data/navigation.yml` | Sidebar navigation structure |
| `assets/css/style.css` | Stylesheet (dark theme) |
| `assets/images/` | Logo and images |
| `_config.yml` | Jekyll configuration |
| `Gemfile` | Ruby dependencies (`github-pages` gem) |
| `CNAME` | Custom domain for GitHub Pages |

## Deployment

GitHub Pages deploys automatically from `main`. All changes go through PRs — preview locally before merging.
