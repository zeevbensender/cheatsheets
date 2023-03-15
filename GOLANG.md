# Golang Cheat Sheet


## Tests
### mockgen

    $ mockgen -source=./<source file> \
        -destination <destination> \
        -aux_files=rest=<referenced package files>

