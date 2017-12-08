## r2 Quickstart Methodology
* This is a live document, and I'll update it as I learn more RE.
* The order presented in is a good place to start; if you decide you need to do extensive static analysis, or any dynamic analysis in a debugger, you'll need to jump around a lot.
* I'm no expert, so please make pull requests if you have changes or recommendations, or message me on Twitter @caffeine_eric

### Initial File Assessment
Note: in a malware analysis scenario there's a lot to do before opening a file in r2, but for the purposes of this intro, we'll assume that has either already been performed, or that the needed information can be found in r2.

```
$> r2 <filename>
[0x00000000]> aaaa                # full analysis including experimental functions
[0x00000000]> ie                  # information on entrypoints
[0x00000000]> fs                  # display flagspaces
[0x00000000]> fs imports; f       # select flagspace 'imports'; list flags
[0x00000000]> fs functions; f     # select flagspace 'functions'; list flags
[0x00000000]> fs strings; f       # select flagspace 'strings'; list flags
[0x00000000]> iz                  # strings in .data section
[0x00000000]> i                   # display file information (equiv. to 'rabin <filename>')
```

* Note that when you run ```ie``` you're likely to see an entrypoint of ```type=program```. This first entrypoint will have a corresponding entry in the 'symbol' flagspace (i.e. ```fs symbols```). If you print the symbol list, you . should see the virtual addresses (```vaddr```) in the entrypoints list all appear as addresses of symbols. The first entrypoint, ```sym.entry0``` actually loads to a section before ```sym.main```, where registers are initialized and the address for sym.main is pushed to the stack in preparation for the syscall which 'starts' the program.

At this point, you should have enough information to make some initial assessments of the file, based on imports, functions, strings, and the file information. Next you can move onto static analysis and program-flow analysis.

### Static Analysis of Disassembled File
```
[0x00000000]> s main              # seek to sym.main
[0x00000000]> pdf                 # print disassemble(d) function
```
example output:
![sym.main pdf](/images/cm3_main-pdf.png)

Look for ```call``` and jump commands ```jmp je jne```, etc.


