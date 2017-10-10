```bash
# list of all run scripts
npm run

# reduce the ouput with less
npm run | less

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
