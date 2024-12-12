heroku-buildpack-imagemagick
=================================

- This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for vendoring the ImageMagick binaries into your project.
- It supports WEBP image format. 
- The current commit on master branch supports:
  * Heroku 20 & ImageMagick 7.1.1-30
  * Heroku 22 & ImageMagick 7.1.1-30
  * Heroku 24 & ImageMagick 7.1.1-30

### Install

In your project root:

`heroku buildpacks:add https://github.com/flexspace/heroku-buildpack-imagemagick  --index 1 --app HEROKU_APP_NAME`

"index 1" means that imagemagick will be installed first.

### Clear cache
Since the installation is cached you might want to clean it out due to config changes.

1. `heroku plugins:install heroku-repo`
2. `heroku repo:purge_cache -app HEROKU_APP_NAME`

### Reference to generate a tar file:

* <https://elements.heroku.com/buildpacks/drnic/heroku-buildpack-imagemagick-webp>