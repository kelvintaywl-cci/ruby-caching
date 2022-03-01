# ruby-caching
Exploring Ruby gems caching on CircleCI

Gems:

 - httparty
 - cowsay

Bundler: v2.3.4

## Things I found out

- Ensure `x86_64-linux` is included in PLATFORMS section of _Gemfile.lock_

> When setting up project locally (macOS), this reflected just `universal-darwin-20` in my _Gemfile.lock_.

> This is critical when we use Docker executors, like cimg/ruby (Linux), and **if using _Gemfile.lock_ as part of the cache key**.

> If `x86_64-linux` was not declared, the _Gemfile.lock_ will be updated to include it during our CI builds, since the dependencies are now installed in a different OS platform (Linux). This behavior seems to be default and cannot be disabled.

> When this happens, our _Gemfile.lock_ contents is now updated.
> This also means the checksum value for _Gemfile.lock_ is now different.

> If your cache key uses `{{ checksum "Gemfile.lock" }}` for instance, the next run will see a cache miss, until you update the _Gemfile.lock_ to also include the `x86_64-linux` already.
