# A gomobile fork compatible with Go-Module

Gomobile generates code in a temporary directory(/tmp/gomobile-work-xxxx) outside current project, so the generated code can't import go packages from project not under GOPATH.

To add support for Go Module, a simple and temporary solution is to generate wrapper code inside current project, let the generated code be a part of the project, so packages can be referenced correctly.

## Modifications

1. Add flag '-outdir' in gomobile and pass it to godind
1. Gobind generates wrapper code in 'outdir', not in 'outdir/src/gobind'
1. Invoke go build for code generated by gobind without modified GOPATH env
1. Use pkg.Types as type package, remove imp.Import, because imp.Import failed with Go Module, and also it is extremely slow on my MacBook Pro.
1. Reserve contents in 'outdir', for ease of diagnoisis

## RISK

This program is not tested extensively, and only 'gomobile bind' is tested, other commands may not work, you should use it with care. 
