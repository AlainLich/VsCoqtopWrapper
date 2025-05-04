# VsCoqtopWrapper

## Summary

Wrapper code to help using extension Vscoq of VScode when connecting remotely (ssh, containers,...). Sets working dir in accordance with `_CoqProject`and library tree.

## Note

  For details about VsCoq top see
  <https://marketplace.visualstudio.com/items/?itemName=maximedenes.vscoq>
  or search for VsCoq extension in Vscode.

 VsCoq is an extension for Visual Studio Code (VS Code) and VSCodium which provides
 support for the Coq Proof Assistant. It is built around a language server
 which natively speaks the LSP protocol.

## Features

### Simple version: vscoqtopWrapper

1. Simple minded wrapper to change the working dir and call vscoqtop.
    Intended to work when connecting to Coq in a remote (tested with
    Docker containers and ssh)

1. Flags
    - `--wd <working_dir_path>` preferably absolute !
    - `--log`: logging via system's logger

1. The idea is that `vscoqtop` relies on its working directory,
    where it expects to find `_CoqProject` and the top of the
    libraries directory tree.    Therefore when vscoq connects
    remotenly, (e.g. with `ssh`), there are little  means to
    change this in a flexible way.

    This wrapper permits to establish a working directory before
    calling `vscoqtop` on the remote.

    To use it, parametrize `vscoq`  (example for `ssh`)

    ~~~ xxx
       "vscoq.args": [  "--wd", <the working dir on remote> [ , "--log"] ],
       "vscoq.path": "ssh <your parms> <your realpath>/vscoqtopWrapper"
    ~~~~

### Easily configurable version vscoqtopEZWrap

1. Yet TBD, features parametrization via initialization.ini file, thereby simplifying
vscoq parametrization when supporting multiple _CoqProjects or vscoqtop versions.

1. Very much tuned to my own use case !
