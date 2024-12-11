heroku-buildpack-imagemagick
=================================

- This is a custom [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for vendoring the ImageMagick binaries into your project.
- Go to https://www.imagemagick.org/download/releases and find a version you want (*.tar.gz)

### Install

In your project root:

**Heroku 20:**

`heroku buildpacks:add https://github.com/flexspace/heroku-buildpack-imagemagick  --index 1 --app HEROKU_APP_NAME`

"index 1" means that imagemagick will be installed first.


**Heroku 22:**

`heroku buildpacks:add https://github.com/flexspace/heroku-buildpack-imagemagick.git#v22 --index 1 --app HEROKU_APP_NAME`

### Clear cache
Since the installation is cached you might want to clean it out due to config changes.

1. `heroku plugins:install heroku-repo`
2. `heroku repo:purge_cache -app HEROKU_APP_NAME`

### Reference:

* <https://elements.heroku.com/buildpacks/drnic/heroku-buildpack-imagemagick-webp>