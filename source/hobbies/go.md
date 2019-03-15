# go

## commands
  - 実行
    - 基本的には、`go run hoge.go`
    - もしくは `go build hoge hoge.go` して `./hoge` 


  - build(compile)
    - `go build <out_file> <src_file>` で実行バイナリができる．

## go install battle on MacOS

```
% brew install goenv
==> Downloading https://github.com/syndbg/goenv/archive/1.23.3.tar.gz
==> Downloading from https://codeload.github.com/syndbg/goenv/tar.gz/1.23.3
######################################################################## 100.0%
🍺  /usr/local/Cellar/goenv/1.23.3: 158 files, 264.5KB, built in 12 seconds
%
% goenv version
system (set by /Users/yudaihashimoto/.goenv/version)
% goenv -v
goenv 1.23.3
% goenv --help
Usage: goenv <command> [<args>]

Some useful goenv commands are:
   commands    List all available commands of goenv
   local       Set or show the local application-specific Go version
   global      Set or show the global Go version
   shell       Set or show the shell-specific Go version
   install     Install a Go version using go-build
   uninstall   Uninstall a specific Go version
   rehash      Rehash goenv shims (run this after installing executables)
   version     Show the current Go version and its origin
   versions    List all Go versions available to goenv
   which       Display the full path to an executable
   whence      List all Go versions that contain the given executable

See `goenv help <command>' for information on a specific command.
For full documentation, see: https://github.com/syndbg/goenv#readme
% goenv install -help
Usage: goenv install [-f] [-kvp] <version>
       goenv install [-f] [-kvp] <definition-file>
       goenv install -l|--list
       goenv install --version

  -l/--list          List all available versions
  -f/--force         Install even if the version appears to be installed already
  -s/--skip-existing Skip if the version appears to be installed already

  go-build options:

  -k/--keep          Keep source tree in $GOENV_BUILD_ROOT after installation
                     (defaults to $GOENV_ROOT/sources)
  -p/--patch         Apply a patch from stdin before building
  -v/--verbose       Verbose mode: print compilation status to stdout
  --version          Show version of go-build
  -g/--debug         Build a debug version

For detailed information on installing Go versions with
go-build, including a list of environment variables for adjusting
compilation, see: https://github.com/yyuu/goenv#readme

% goenv install -l
Available versions:
  1.2.2
  1.3.0
  1.3.1
  1.3.2
  1.3.3
  1.4.0
  1.4.1
  1.4.2
  1.4.3
  1.5.0
  1.5.1
  1.5.2
  1.5.3
  1.5.4
  1.6.0
  1.6.1
  1.6.2
  1.6.3
  1.6.4
  1.7.0
  1.7.1
  1.7.3
  1.7.4
  1.7.5
  1.8.0
  1.8.1
  1.8.3
  1.8.4
  1.8.5
  1.8.7
  1.9.0
  1.9.1
  1.9.2
  1.9.3
  1.9.4
  1.9.5
  1.9.6
  1.9.7
  1.10.0
  1.10beta2
  1.10rc1
  1.10rc2
  1.10.1
  1.10.2
  1.10.3
  1.10.4
  1.10.5
  1.10.6
  1.10.7
  1.11.0
  1.11beta2
  1.11beta3
  1.11rc1
  1.11rc2
  1.11.1
  1.11.2
  1.11.3
  1.11.4
  1.12beta1
%
% goenv install 1.11.4
Downloading go1.11.4.darwin-amd64.tar.gz...
-> https://dl.google.com/go/go1.11.4.darwin-amd64.tar.gz
Installing Go Darwin 64bit 1.11.4...
Installed Go Darwin 64bit 1.11.4 to /Users/yudaihashimoto/.goenv/versions/1.11.4

%
% goenv global 1.11.4
% goenv rehash
% vi .zshrc

# add follows to .zshrc to apply PATH to env
# export GOENV_ROOT="$HOME/.goenv"
# export PATH="$GOENV_ROOT/bin:$PATH"
# eval "$(goenv init -)"

% source .zshrc
%
% go -v
flag provided but not defined: -v
Go is a tool for managing Go source code.

Usage:

  go <command> [arguments]

The commands are:

  bug         start a bug report
  build       compile packages and dependencies
  clean       remove object files and cached files
  doc         show documentation for package or symbol
  env         print Go environment information
  fix         update packages to use new APIs
  fmt         gofmt (reformat) package sources
  generate    generate Go files by processing source
  get         download and install packages and dependencies
  install     compile and install packages and dependencies
  list        list packages or modules
  mod         module maintenance
  run         compile and run Go program
  test        test packages
  tool        run specified go tool
  version     print Go version
  vet         report likely mistakes in packages

Use "go help <command>" for more information about a command.

Additional help topics:

  buildmode   build modes
  c           calling between Go and C
  cache       build and test caching
  environment environment variables
  filetype    file types
  go.mod      the go.mod file
  gopath      GOPATH environment variable
  gopath-get  legacy GOPATH go get
  goproxy     module proxy protocol
  importpath  import path syntax
  modules     modules, module versions, and more
  module-get  module-aware go get
  packages    package lists and patterns
  testflag    testing flags
  testfunc    testing functions

Use "go help <topic>" for more information about that topic.
```