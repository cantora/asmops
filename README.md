#asmops

print assembly opcodes


##usage
```
> asmops -h
usage: asmops [-h] [-v] [-a ASSEMBLY] [--bc ARCH] [--raw] [-d [0xBASE_ADDR]]
              [-- ...]
              [file]

print assembly op codes

positional arguments:
  file                  file from which to get assembly instructions. if no
                        file is given, stdin will be used

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         print debug output. repeat to increase verbosity.
  -a ASSEMBLY, --asm ASSEMBLY
                        use instructions passed to this flag instead of
                        reading from file. if --bc ARCH is used, ASSEMBLY will
                        be interpreted as byte codes, not mnemonics.
  --bc ARCH             dont assemble anything. just disassemble ASSEMBLY
                        (from -a) or disassemble the input file if -a is not
                        given. the data will be disassembled using the base
                        address given at -d.
  --raw                 print raw bytes instead of an escaped string
  -d [0xBASE_ADDR], --disassemble [0xBASE_ADDR]
                        also disassemble output opcodes (printed to stderr).
                        assume a base address of BASE_ADDR (in hex) for
                        disassembly. default: 0x08040000
  -- ...                all flags following this will be passed to gas.
```

##examples

```
$> asmops -a 'push $0x1; push $0xbadbad; push $0x8048784; push $0x0; ret'
\x6a\x01\x68\xad\xdb\xba\x00\x68\x84\x87\x04\x08\x6a\x00\xc3
$> asmops -a 'push $0x1; push $0xbadbad; push $0x8048784; push $0x0; ret' --raw | asmops --bc x86 -d 0x33330000
using stdin as input
  33330000:          6a01              PUSH 0x1
  33330002:          68addbba00        PUSH DWORD 0xbadbad
  33330007:          6884870408        PUSH DWORD 0x8048784
  3333000c:          6a00              PUSH 0x0
  3333000e:          c3                RET
  3333000f:          0a                DB 0xa
```

