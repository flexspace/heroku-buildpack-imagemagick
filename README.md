heroku-buildpack-imagemagick
=================================

- This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for vendoring the ImageMagick binaries into your project.
- It supports the WEBP and HEIC image formats.
- The current commit on master branch supports:
  * Heroku 20 & ImageMagick 7.1.1-30
  * Heroku 22 & ImageMagick 7.1.1-30
  * Heroku 24 & ImageMagick 7.1.1-41

### Install

In your project root:

`heroku buildpacks:add https://github.com/flexspace/heroku-buildpack-imagemagick  --index 1 --app HEROKU_APP_NAME`

"index 1" means that imagemagick will be installed first.

### Clear cache
Since the installation is cached you might want to clean it out due to config changes.

1. `heroku plugins:install heroku-repo`
2. `heroku repo:purge_cache -app HEROKU_APP_NAME`

### Regenerating a vendored tarball

The tarballs under `build/` are self-contained ImageMagick install prefixes
(`bin/`, `etc/`, `include/`, `lib/`, `share/`) with the webp + heif delegates
bundled. They are produced from the Dockerfiles in
[`drnic/heroku-buildpack-imagemagick-webp`](https://github.com/drnic/heroku-buildpack-imagemagick-webp).

> **Important — build for x86-64, not the host arch.** Heroku Cedar dynos run
> **x86-64**. When building on an Apple Silicon / ARM64 machine you MUST pass
> `--platform linux/amd64`, otherwise Docker produces an `aarch64` binary that
> extracts fine but fails at runtime with `Exec format error` on the first
> `magick`/`convert` call. This is exactly why the earlier heroku-24 tarball was
> marked unsupported. Acceptance gate: `file bin/magick` must report `x86-64`
> before committing.

```bash
# from a checkout of drnic/heroku-buildpack-imagemagick-webp
docker build --platform linux/amd64 -t im-webp-amd64:h24 -f Dockerfile.24 .
docker run --rm --platform linux/amd64 -v "$PWD/build":/data im-webp-amd64:h24 \
  sh -c 'cp -f /usr/src/imagemagick/build/imagemagick-heroku-24.tar.gz /data/'
# verify the vendored bin/magick is x86-64 before committing into build/
tar -xzOf build/imagemagick-heroku-24.tar.gz bin/magick | file -   # must report: x86-64
```

Reference: <https://elements.heroku.com/buildpacks/drnic/heroku-buildpack-imagemagick-webp>