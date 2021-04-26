# Website

This repo migrated to the new version: https://github.com/WorkplaceX/ApplicationDoc

Source code for Website: https://workplacex.org/. It's hosted as GitHub page (https://pages.github.com/). GitHub pages are generated with static site generator jekyll (https://jekyllrb.com/). 

Clone Website https://workplacex.org/ source code and serve it locally to modify like this:

## Git Clone
```cmd
git clone https://github.com/WorkplaceX/workplacex.github.io.git
```

## Install
For Ubuntu 18.04 see: https://jekyllrb.com/docs/installation/ubuntu/
For Windows install : https://rubyinstaller.org/downloads/ Ruby+Devkit 2.5.5-1 (x64)
```cmd
gem install bundler jekyll
bundle install
```

## Jekyll Serve
```cmd
bundle exec jekyll serve
```
Open local website http://127.0.0.1:4000

## Jekyll Build
```cmd
bundle exec jekyll build
```
Creates folder "_site"

## Add a new Site
Register it in _data/pageList.yml collection

## Sitemap
Open local website http://127.0.0.1:4000/sitemap.xml
