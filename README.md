Add as a first buildpack in the chain. Set `SWAP_ROOT_APP_DIR` environment variable to point to subdirectory that will become root. It will be promoted to slug's root, everything else will be moved into subdirectory within new root. Name of subdirectory for moved root can be defined in `SWAP_ROOT_APP_DIR` optional environment variable, if not specified will be "_root_". Following buildpack (e.g. nodejs) will finish slug compilation.

# Example
If your file structure is 
```
folder1
  - file1
  - file2
folder2
  - file3
  - file4
folder3
  - file5
  - file6
 file7
 file8
```
And heroku environment variables set to
```
SWAP_ROOT_APP_DIR=folder2
SWAP_ROOT_APP_DIR=app
```
Resulting file structure will be
```
app
  - folder1
    - file1
    - file2
  - folder2
    - file3
    - file4
  - folder3
    - file5
    - file6
  - file7
  - file8
file3
file4
```
As you can see, all content was moved into "app" folder and "folder2" content became a root content

# How to use:
1. `heroku buildpacks:clear` if necessary
2. `heroku buildpacks:set https://github.com/timanovsky/subdir-heroku-buildpack`
3. `heroku buildpacks:add heroku/nodejs` or whatever buildpack you need for your application
4. `heroku config:set PROJECT_PATH=projects/nodejs/frontend` pointing to what you want to be a project root.
5. Deploy your project to Heroku.

# How it works
The buildpack takes subdirectory you configured, erases everything else, and copies that subdirectory to project root. Then normal Heroku slug compilation proceeds.
