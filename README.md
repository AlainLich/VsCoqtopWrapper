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

1.  To use it, parametrize `vscoq`  (example for `ssh`)

    ```
    "vscoq.args": [  "--wd", <the working dir on remote> [ , "--log"] ],
    "vscoq.path": "ssh <your parms> <your realpath>/vscoqtopWrapper"
    ```

### Easily configurable version vscoqtopEZWrap

1. Adds parametrization via initialization.ini file, thereby simplifying
`vscoq` parametrization when supporting multiple `_CoqProjects` or `vscoqtop` versions.

1. Very much tuned to my own use cases !

1. Parametrization via `.ini` file:
	1. The format supported is Python's `configparser.ExtendedInterpolation`,
see <https://docs.python.org/3/library/configparser.html#supported-ini-file-structure>. This is inspired by Microsoft's Window `.ini` file format.

	1. The parameter names recognized in the `.ini` file are (they correspond but are not identical to the CLI flags):

     <table>
      <tr><th> .ini flag </th><th>.ini keyword</th></tr>
      <tr> 
        <td> --working_dir </td><td>workingdir</td>  </tr>
        <td> --vscoqtop    </td><td> vscoqtop_path </td>  </tr>
        <td> --log         </td><td> dolog         </td>  </tr>
        <td> --dry         </td><td> dodryrun      </td>  </tr>
     </table>

    1. Note that the CLI tags which permit to locate the .ini file are not included.

    1. An example of such is:

    ```
    [DEFAULT]
    workingdir : /mount/user_etc
    vscoqtop_path: /mount/built/opam/CP.~8.20~2025.01/bin/vscoqtop

    #-----------------------------------------------------------------------
    # Example sections, of course vscoqtop path can be varied
    #-----------------------------------------------------------------------

    [base]
    log        : True
    workingdir : ${DEFAULT:workingdir}
    vscoqtop_path : ${DEFAULT:vscoqtop_path}

    [project1]:
    workingdir : /mount/user_etc
    vscoqtop_path : ${DEFAULT:vscoqtop_path}
    ```

1. **`VsCoq`** parametrization:
	1. 2 methods are available:
		- CLI parametrization without `.ini` file
		- parametrization with `.ini` file

	1. Without `.ini` file: 
       - The CLI tags `--working_dir` and `--vscoqtop` are
		  required, the tag `--log` is optional.
	
       - The VsCoq settings look like:
         ```
          "vscoq.args": 
                  [  "--working_dir", <the working dir on remote> ,
		              "--vscoqtop", <absolute path to vscoqtop>
                     "--log" ],
          "vscoq.path": "ssh <your parms> <your realpath>/vscoqtopEZWrap"
         ```


	1. With the `.ìni`file, 
        - The tag --ini-file is required, --ini-path has the home dir
	      for default, --ini-section has "base" for default
        - the VsCoq setting look like:
         ```
          "vscoq.args": [
                        "--log", 
                        "--ini-file", "vscoqtop.ini", 
                        "--ini-section", "project1", 
                        "--ini-path", "/mount/user_etc:/home/me/usr:"
			] ,
          "vscoq.path": "ssh <your parms> <your realpath>/vscoqtopEZWrap"
         ```

		the tags `--working_dir`, `--vscoqtop`, `--log` in `vscoq.args`would override those in the `.ini` file.






