# Golang Cheat Sheet

## The "go" command
[Reference](https://pkg.go.dev/cmd/go)
### Initialize Module
    $ go mod init example/todo

### Download and add dependencies to the go.mod file
    $ go mod tidy



## Tests
### mockgen

    $ mockgen -source=./<source file> \
        -destination <destination> \
        -aux_files=rest=<referenced package files>


