# SnapShot

This is a really small experimental package for saving code associated with
an experiment, and reloading it at a later time. There are two main functions:

1. `snapshot.capture`: Captures the code associated with a module into a snapshot at a given path.
2. `snapshot.load`: Loads the code from the snapshot into a module of a specified name. If no name is given, then the module name is whatever the folder
name is for the snapshot.

## Usage

Let's say I'm working on the code in `examples/hello_world.py`. Let's save the current version 
(working in the `examples/` directory):

```python
import snapshot
import hello_world

# First let's take a snapshot of the 
# hello_world package.

path = snapshot.capture(hello_world, '/tmp/hello_world_snap/', overwrite=True)
hello_world_snap = snapshot.load(path)
print(hello_world_snap.__file__)

# The snapshot is loaded from the specified directory. The code was all copied to
# and is under the snapshot:

print("From original package code", hello_world.func1())
print("From snapshot", hello_world_snap.hello_world.func1())

# You can import from the snapshot as if it were a regular package

from hello_world_snap import hello_world
print("From snapshot, overwriting the original package code", hello_world.func1())
```

This has the output:

```bash
❯ cd examples/
❯ python demo.py
/private/tmp/hello_world_snap/__init__.py
/Users/prem/research/snapshot/examples/hello_world/hello.py TEST
From original package code TEST
/private/tmp/hello_world_snap/hello_world/hello.py TEST
From snapshot TEST
/private/tmp/hello_world_snap/hello_world/hello.py TEST
From snapshot, overwriting the original package code TEST
```