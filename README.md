# heroku-buildpack-stack

Easily run [stack](https://www.stackage.org/)-based Haskell projects on Heroku.

```
heroku buildpacks:set https://github.com/nejstastnejsistene/heroku-buildpack-stack
```

You can [read more about using custom buildpacks here](https://devcenter.heroku.com/articles/third-party-buildpacks#using-a-custom-buildpack).

### How does it work?

This installs `libgmp` and `stack`, calls `stack setup` to install GHC as specified in `stack.yaml`, and then runs `stack build`. This will probably take a long time the first time a slug is compiled, but subsequent compilations will use cached data to speed up the process.

The default behavior without a `Procfile` is to run the first executable listed in the `.cabal` file. This should work most of the time, kind of like `cabal run` used to.
```bash
#!/bin/bash
$(grep ^executable *.cabal | head -n1 | awk '{print $2}')
```

### TODO

* Setup the `PATH` so it doesn't have to be explicitly defined (see [`bin/release`](bin/release))
* Automatically `stack upgrade`
