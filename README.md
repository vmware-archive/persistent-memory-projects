# Persistent Memory Projects at VMware

## Overview
This repository holds the GitHub pages for sharing information about various
persistent memory projects happening in VMware.

## Dependencies
If you are running a build on Ubuntu you will need the following packages 
* ruby
* ruby-dev
* ruby-bundler
* build-essential
* zlib1g-dev
* nginx (or apache2)

For other operating systems such as MacOS you will need equivalent packages or
install xcode

## Local Development
1. Install Jekyll and plug-ins in one fell swoop. `gem install github-pages` 
This mirrors the plug-ins used by GitHub Pages on your local machine including
Jekyll, Sass, etc.
2. Clone down your fork `git clone https://github.com/vmware/pmem.git`
3. Serve the site and watch for markup/sass changes `bundle exec jekyll serve --livereload`
4. View your website at http://127.0.0.1:4000/
5. Commit any changes and push everything to the master branch of your GitHub
user repository. GitHub Pages will then rebuild and serve your website.

## Contributing

The Persistent Memory Projects at VMware project team welcomes contributions
from the community. Before you start working with pmem, please read our
[Developer Certificate of Origin](https://cla.vmware.com/dco). All contributions
to this repository must be signed as described on that page. Your signature
certifies that you wrote the patch or have the right to pass it on as an
open-source patch. For more detailed information, refer to
[CONTRIBUTING.md](CONTRIBUTING.md).