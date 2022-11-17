Add as a first buildpack in the chain. Set `SWAP_SUBDIR_NAME` environment variable to point to subdirectory that will become root. It will be promoted to slug's root, everything else will be moved into subdirectory within new root. Name of subdirectory for moved root can be defined in `SWAP_ROOT_NAME` optional environment variable, if not specified it will be "_root_". Following buildpack (e.g. nodejs) will finish slug compilation.

# How to use:
1. `heroku buildpacks:clear` if necessary
2. `heroku buildpacks:set https://github.com/timanovsky/subdir-heroku-buildpack`
3. `heroku buildpacks:add heroku/nodejs` or whatever buildpack you need for your application
4. `heroku config:set PROJECT_PATH=projects/nodejs/frontend` pointing to what you want to be a project root.
5. Deploy your project to Heroku.

# How it works
The buildpack moves all slug content into a subfolder(provided in `SWAP_ROOT_NAME`) and copies content from subfolder(provided in `SWAP_SUBDIR_NAME`) into slug root. Then normal Heroku slug compilation proceeds.  

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
SWAP_SUBDIR_NAME=folder2
SWAP_ROOT_NAME=app
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