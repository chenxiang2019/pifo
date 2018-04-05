# Push-In First-Out Queue (PIFO)

An attempt of running Push-In First-Out Queue (PIFO) following SIGCOMM'16 "Programmable Packet Scheduling at Line Rate".

## Hands-on Steps

### Install Dependencies

```
sudo pip install graphviz
sudo pip install pydot2
```

### Install pifo-compiler and pifo-machine

```
git clone https://github.com/programmable-scheduling/pifo-compiler.git

git clone https://github.com/programmable-scheduling/pifo-machine.git
cd pifo-machine/
./autogen.sh && ./configure && make && make check
```

### Try PIFO

```
cd pifo-compiler/
python pifo_compiler.py HPFQ.dot
```

This command will produce both a `HPFQcompilation.cc` and a `HPFQEnqueues.gv.pdf` on current directory.

```
cp -r HPFQcompilation.cc ../pifo-machine/
cd ../pifo-machine/
```

Replace the `pifo_machine.cc` with `HPFQcompilation.cc`:

```
mv pifo_machine.cc pifo_machine_bak.cc
mv HPFQcompilation.cc pifo_machine.cc
make clean
make
```

Execute the executable file `pifo_machine`:

```
./pifo_machine
```

The output could be found at [HPFQ_results.txt](HPFQ_results.txt) in this repository.

## Known Issues:

1.As executing `python pifo_compiler.py HPFQ.dot`:

```
Couldn't import dot_parser, loading of dot files will not be possible.
Traceback (most recent call last):
  File "pifo_compiler.py", line 153, in <module>
    g= pydot.graph_from_dot_data(x)
  File "/usr/local/lib/python2.7/dist-packages/pydot.py", line 220, in graph_from_dot_data
    return dot_parser.parse_dot_data(data)
NameError: global name 'dot_parser' is not defined
```

Executing the following statements to fix this bug:

```
pip install pyparsing==1.5.7
pip install pydot==1.0.28
```

2.As executing `python pifo_compiler.py HPFQ.dot`:

```
Traceback (most recent call last):
  File "pifo_compiler.py", line 237, in <module>
    outVars.write(pifoArgs)
NameError: name 'outVars' is not defined
```

Commenting the following lines in [pifo_compiler.py](https://github.com/programmable-scheduling/pifo-compiler/blob/master/pifo_compiler.py): 221,237,246.

## Author 

Wasdns (Xiang Chen) - wasdnsxchen@gmail.com
