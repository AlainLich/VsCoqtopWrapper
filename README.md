# VsCoqtopWrapper

## Summary

This addresses the issue "*Cannot find a physical path...*" when `Require Import`, when connected to a remote (eg. with `ssh`).

Wrapper code help with using extension **Vscoq** of **VScode** when connecting remotely (ssh, containers,...), by setting working directory in accordance with `_CoqProject`and library tree.

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
    libraries directory tree. Looking at the
code <https://github.com/rocq-prover/vscoq/blob/main/language-server/vscoqtop/vscoqtop.ml>, this seems rather wired in (at least for the time being:

    ```
    let cwd = Unix.getcwd () in
      let opts = Args.get_local_args cwd in
        let () = Coqinit.init_runtime ....
    ```

   Therefore when vscoq connects
    remotenly, (e.g. with `ssh`), there are little  means to
    change this in a flexible way. This wrapper permits to establish a working directory before
    calling `vscoqtop` on the remote.

1.    To use it, parametrize `vscoq`  (example for `ssh`)

    ```
    "vscoq.args": [  "--wd", <the working dir on remote> [ , "--log"] ],
    "vscoq.path": "ssh <your parms> <your realpath>/vscoqtopWrapper"
    ```

### Easily configurable version vscoqtopEZWrap

1. Yet TBD, features parametrization via initialization.ini file, thereby simplifying
vscoq parametrization when supporting multiple _CoqProjects or vscoqtop versions.

1. Very much tuned to my own use case !
