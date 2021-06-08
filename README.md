### README

This is a simple test that uses https://github.com/pure-data/helloworld.git with Github Actions to automatically cross-compile a 64bits Windows binary of the lib against Pd-0.51-4, and automatically release it if the push command is made with any tag.

To do that, I use the following git commands (after congiguring the yaml file):

```
git add .
git commit -m "commit text"
git tag -a v0.0.1 -m "text of the version tag"
git push origin v0.0.1
```

When I got errors, I had to delete the tags using
```
git tag -d v0.0.1
git push origin --delete v0.0.1
```

Not sure if it is a good practice, but maybe it can be usefull...
