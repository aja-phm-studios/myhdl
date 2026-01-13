MyHDL 0.11 
==========

[![Documentation Status](https://readthedocs.org/projects/myhdl/badge/?version=stable)](http://docs.myhdl.org/en/stable/manual/)
[![Documentation Status](https://readthedocs.org/projects/myhdl/badge/?version=latest)](http://docs.myhdl.org/en/latest/manual)

[![Join the chat at https://gitter.im/myhdl/myhdl](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/myhdl/myhdl?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[<img src="https://d11a6trkgmumsb.cloudfront.net/original/3X/a/d/ad2526334012b2ad0c3421f9b6dd94bae2ede52f.svg" alt="Visit MyHDL at Discourse" align="left" height="36" width="134" >](https://discourse.myhdl.org)
<br><br><br>


What is MyHDL?
--------------
MyHDL is a free, open-source package for using Python as a hardware
description and verification language.

To find out whether MyHDL can be useful to you, please read:

   - http://www.myhdl.org/start/why.html

License
-------
MyHDL is available under the LGPL license.  See ``LICENSE.txt``.

Website
-------
The project website is located at http://www.myhdl.org

Documentation
-------------
The manual is available on-line:

   - http://docs.myhdl.org/en/stable/manual

What's new
----------
To find out what's new in this release, please read:

   - http://docs.myhdl.org/en/stable/whatsnew/0.11.html

Installation
------------
It is recommended to install MyHDL (and your project's other dependencies) in
a virtualenv.

Installing the latest stable release:

```
pip install myhdl
```
Unfortunately the version on PyPI is quite behind the current development status, so you are better off installing the *stable* master branch directly from this GitHub repository:

```
pip install git+https://github.com/myhdl/myhdl.git@master
```

To install a local clone of the repository:

```
pip install -e path/to/dir
```

To install a specific commit hash, tag or branch from git:

```
pip install git+https://github.com/myhdl/myhdl@f696b8
```


You can test the proper installation as follows:

```
cd myhdl/test/core
py.test
```

To install co-simulation support:

Go to the directory ``cosimulation/<platform>`` for your target platform
and following the instructions in the ``README.txt`` file.

Repository Organization
-----------------------
This repository is organized to support both the core MyHDL framework and derived projects:

- **Core Library** (`/myhdl/`) - The main MyHDL package
- **Examples** (`/example/`) - Reference implementations and tutorials
- **Documentation** (`/doc/`) - Comprehensive project documentation
- **Projects** (`/projects/`) - Independent projects built on MyHDL

For detailed information about the repository structure, see:
- **QUICK_START.md** - Quick reference guide
- **PROJECT_STRUCTURE.md** - Detailed structure documentation
- **projects/README.md** - Projects directory guide

### Projects Directory

The `/projects/` directory contains independent projects that leverage the MyHDL framework. Each project maintains its own structure, tests, and documentation while using the core MyHDL library.

**Example Project**: `projects/Electroplate-Store-v1/` demonstrates a complete project template with:
- Organized source code structure (`src/core/`, `src/modules/`, `src/utils/`)
- Comprehensive testing (`tests/unit/`, `tests/integration/`)
- Usage examples (`examples/`)
- Documentation (`docs/`)
- Configuration management (`config/`)

To learn more, see the project README at `projects/Electroplate-Store-v1/README.md`.
