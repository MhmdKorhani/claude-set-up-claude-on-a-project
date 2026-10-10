### CLAUDE.md
I have added CLAUDE.md by using the /init command in claude.
I removed the introduction used by claude and replaced it with the project name and description.
I removed single file testing because it's not needed from CLAUDE.md
Replaced the error handling from code specific to have general on the level of the whole project.
Removed reading or commiting .env file since it is denied access in permissions.

### Permissions
I have added .claude/settings.json manually, where I enabled linting and testing, and I prohibited reading from envirnoment files.
Claude is granted to access to run the project, and run tests
Claude is denied access to read environoment variables as it contains secrets - Not doing so, Claude could read and expose API keys or database credentials from .env files.
Claude is denied pushing to main and production branches. - Not doing so, Claude may shift code to production that may not be reviewed properly causing major problems.

### Memory and Permissions
When running /memory I was able to access CLAUDE.md content
When runnning /permissions I was able to see the rules I have added in .claude/settings.json