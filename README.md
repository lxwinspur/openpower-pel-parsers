# SRC and user data parsers for OpenPOWER PELs

The parsers are made up of python modules which are packaged together with
`setuptools` via the `setup.py` script. The modules are kept in a subdirectory
in order to separate them from potential other tools that may be needed to build
the packages.

For a reference on the requirements of these python modules see the
[OpenPOWER PEL README.md](https://github.ibm.com/openbmc/phosphor-logging/blob/master/extensions/openpower-pels/README.md#adding-python3-modules-for-pel-userdata-and-src-parsing).

## SRC parsers

Each subsystem requiring an SRC parser will create a single module in the format
of:

```
modules/srcparsers/<subsystem>src/<subsystem>src.py
```

Where `<subsystem>` is a single character:

* 'o' => BMC
* 'b' => Hostboot

For example, the BMC subsystem would create:

```
modules/srcparsers/osrc/osrc.py
```

All SRC modules must define the `parseSRCToJson` function as shown in the
OpenPOWER PEL README.md (see link above).

**Important Note:** Each SRC module will emcompass the parsing for all
components of the target subsystem. This is unlike the user data parsers.
Fortunately, as with all python, we can create submodules for each component if
needed. So the parsing code does not need to be contained with the SRC module.
Only the top level `parseSRCToJson` function is required.

**Important Note:** A wrapper has been created for the `BMC` subsystem SRC
module. This wrapper mimics how the modules work for user data sections. See
[modules/srcparsers/osrc/osrc.py](modules/srcparsers/osrc/osrc.py) for details.

## User data parsers

Each subsystem requiring user data parsers will create a module for each
component in the format of:

```
modules/udparsers/<subsystem><component>/<subsystem><component>.py
```

Where `<subsystem>` is a single character:

* 'o' => BMC
* 'b' => Hostboot

And `<component>` is the four character component ID in the form `xx00` (all
lowercase).

For example, the PRD component in the Hostboot subsystem would create:

```
modules/udparsers/be500/be500.py
```

All user data modules must define the `parseUDToJson` function as shown in the
OpenPOWER PEL README.md (see link above).

## Testing

It is highly encouraged to build and maintain automated test cases using the
standard [unittest module](https://docs.python.org/3/library/unittest.html). All
test modules should be stored in the `test/` directory (or subdirectory).
Remember that all of the test files must be `modules` or `packages` importable
from the top-level `test/` directory. To run all test cases in `test/`, issue
the following command (at project root):

```sh
python3 -m unittest discover -s 'test/' -v
```

