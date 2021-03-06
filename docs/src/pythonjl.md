# The Julia module `Python`

To get started, just do `using Python`. There are two main ways to use this module:

**Way 1:** There is a [collection of macros](#Execute-Python-code) for directly executing Python code, interpolating Julia values in and extracting Julia values out. For example ```@pyv `$x+1`::Int``` adds `x` to `1` in Python and converts the result to an `Int`.

**Way 2:** There is a [collection of functions](#Python-functions) which typically produce and consume Python objects. The previous example can be implemented as `pyadd(Int, x, 1)` or `pyconvert(Int, PyObject(x)+1)`.

In all cases, when a Julia value needs to be passed to Python, it will be converted according to [this table](@ref jl2py).

When a Python value is returned to Julia, by default it will be as a [`PyObject`](@ref). Most functions provide an optional way to specify the return type, in which case it will be converted according to [this table](@ref py2jl).

You can also specify one of the [wrapper types](@ref python-wrappers) as a return type.

## `PyObject`

```@docs
PyObject
```

## Execute Python code

These macros are used to execute or evaluate Python code. The main differences between them are in whether/how any values are extracted out again.

**Note to package writers.** These all expect there to be a variable `pyglobals` in scope, which is a Python dictionary giving the global scope. For convenience, this module exports such a variable so that these macros work in the REPL. However other packages should define their own global scope by defining `const pyglobals = PyDict()`. You can alternatively define `const pyglobals = Python.pylazyobject(()->pyimport("some_module").__dict__)` to use the global scope of an existing Python module.

```@docs
@py
@pyv
@pyg
@pya
@pyr
```

## Python functions

### Construct Python objects

These functions convert Julia values into Python objects of standard types.

```@docs
pynone
pybool
pyint
pyfloat
pystr
pybytes
pytuple
pylist
pycollist
pyrowlist
pyset
pyfrozenset
pydict
pyslice
pyellipsis
pynotimplemented
pymethod
pytype
```

### Wrap Julia values

These functions wrap Julia values into Python objects, documented [here](@ref julia-wrappers).

```@docs
pyjl
pyjlraw
pyisjl
pyjlgetvalue
pytextio
pyrawio
pybufferedio
```

### Python builtins

```@docs
pyconvert
pyimport
pywith
pyis
pyrepr
pyhasattr
pygetattr
pysetattr
pydir
pycall
pylen
pycontains
pyin
pygetitem
pysetitem
pydelitem
pytruth
pyissubclass
pyisinstance
pyhash
pyiter
```

### Numbers

```@docs
pyeq
pyne
pyle
pylt
pyge
pygt
pyadd
pyiadd
pysub
pyisub
pymul
pyimul
pymatmul
pyimatmul
pyfloordiv
pyifloordiv
pytruediv
pyitruediv
pymod
pyimod
pydivmod
pylshift
pyilshift
pyrshift
pyirshift
pyand
pyiand
pyor
pyior
pyxor
pyixor
pypow
pyipow
pyneg
pypos
pyabs
pyinv
```

## [Wrapper types](@id python-wrappers)

```@docs
PyList
PySet
PyDict
PyIterable
PyArray
PyBuffer
PyIO
PyPandasDataFrame
PyCode
@py_cmd
@pyv_cmd
PyInternedString
@pystr_str
PyException
```
