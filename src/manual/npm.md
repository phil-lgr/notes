# npm

```bash
# list of all run scripts
npm run

# reduce the ouput with less
npm run | less

# prefer offline (avoid checking the repository for changed in cached version)
npm config -g set prefer-offline true

# access package.json as variables in npm scripts
cross-var http-server -g -a localhost -c-1 -p $npm_package_config_ports_prod

# change the default save prefix for all projects
npm config set save-prefix="~"

# lock down dependencies versions
npm shrinkwrap

# check for packages not declared in package.json and remove them
npm prune

# check a package for outdated dependencies
npm outdated

# autocomplete npm run command with bash
npm completion >> ~/.bashrc
source ~/.bashrc

# run scripts in parallel
npm run task:js & npm run task:css

# run scripts in series
npm run task:js && npm run task:css

# pre and post scripts
"script": {
    "pretask": "clean"
    "task": "tasks"
    "posttask": "deploy"
 }

# list npm environment variables
npm run env | grep "npm_package"

# list npm environment variables for the config entry
npm run env | grep "config_"

# run multiple tasks with a npm-run-all package
npm-run-all task:*

# set log level output (-q quiet, -d INFO, -dd VERBOSE, -s SILENT)
npm run task -q
npm run task -d
npm run task -dd
npm run task -s

# ntl package list all the npm script with interactive commands
ntl

```
