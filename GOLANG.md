# Golang Cheat Sheet

## Common
### Initialize Module
    $ go mod init example/todo


## Tests
### mockgen

    $ mockgen -source=./<source file> \
        -destination <destination> \
        -aux_files=rest=<referenced package files>


