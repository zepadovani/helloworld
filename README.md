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

### Cross-compilation/release steps performed by the workflow:

#### 0. env variable setting
sets tmp folder to work with downloaded files...
  ```
TMPPD: /tmp/pd/
```

#### 1. installation of cross-compilation tools:
  ```
sudo apt-get install build-essential automake autoconf libtool gettext
sudo apt-get install make gcc-mingw-w64-x86-64 gcc-mingw-w64-i686 mingw-w64 mingw-w64-tools
sudo apt-get install nsis
```

#### 2. download of Pd-0.51-4 Windows/64 binary

Note: Simpler than compile, but not sure if it is a good practice

  ```
mkdir $TMPPD
cd $TMPPD
wget http://msp.ucsd.edu/Software/pd-0.51-4.msw.zip
unzip pd-0.51-4.msw.zip
```

#### 3. cross-compiling the library
  ```
make PLATFORM=x86_64-w64-mingw32 PDDIR=$TMPPD/pd-0.51-4/
```

#### 4. make install to dir
  ```
sudo make install PLATFORM=x86_64-w64-mingw32 \
  PDDIR=$TMPPD/pd-0.51-4/ DESTDIR=$GITHUB_WORKSPACE
```
#### 5. creation of zip file with compiled lib
  ```
  cd $GITHUB_WORKSPACE/Pd
  ls -la
  sudo zip -r helloworld_win64_release.zip ./helloworld
  sudo mv helloworld_win64_release.zip $GITHUB_WORKSPACE/
  ```

#### 6. release using ncipollo workflow
  see `.github/workflows/win64_build.yml` (last lines)
