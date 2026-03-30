## Dev

1. clone repository
2. Create .env based on .env.template
3. Run the command `git submodule update --init --recursive` to rebuild the submodules
4. Run the command `docker compose up --build`


### Steps to create Git Submodules

1. Create a new repository on GitHub
2. Clone the repository on your local machine
3. Add the submodule, where `repository_url` is the url of the repository and `directory_name` is the name of the folder where you want to save the submodule (it must not exist in the project)
```
git submodule add <repository_url> <directory_name>
```
4. Add the changes to the repository (git add, git commit, git push)
Ej:
```
git add .
git commit -m "Add submodule"
git push
```
5. Initialize and update Sub-modules, when someone clones the repository for the first time, they must run the following command to initialize and update the sub-modules
```
git submodule update --init --recursive
```
6. To update the references of the sub-modules
```
git submodule update --remote
```


## Important
If you are working on the repository that has the sub-modules, **first update and push** in the sub-module and **then** in the main repository. 

If you do it the other way around, you will lose the references of the sub-modules in the main repository and we will have to resolve conflicts.
