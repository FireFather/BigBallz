# Big Ballz

![alt tag](https://raw.githubusercontent.com/FireFather/BigBallz/master/bitmaps/bigballz.png)

**Many thanks to Tomass Sobczyk (Sopel97) for his huge NNUE!, and the inspiration to create this engine**

This is a **FREE** UCI-compatibile chess engine. It is a fork of [FatTitz](https://github.com/Sopel97/FatTitz), which is a fork of [Cfish](https://github.com/syzygy1/Cfish), which is a fork of [Stockfish](https://github.com/official-stockfish/Stockfish), which is a fork of [Glaurung](https://github.com/phenri/glaurung).

This engine uses a HalfKAv2-2048x2-64-64-1 evaluation network, which contains whopping **4 times** the knowledge of Stockfish 14. It was trained partially on Lc0 data, which gives it a unique positional style, while still preserving the tactical sharpness of Stockfish. The network was trained using a modification of the [state-of-the-art NNUE trainer](https://github.com/glinscott/nnue-pytorch), utilizing publicly available datasets [1](https://drive.google.com/file/d/1VlhnHL8f-20AXhGkILujnNXHwy9T-MQw/view?usp=sharing), [2](https://drive.google.com/file/d/1seGNOqcVdvK_vPNq98j-zV3XPE5zWAeq/view?usp=sharing), [3](https://drive.google.com/file/d/1RFkQES3DpsiJqsOtUshENtzPfFgUmEff/view?usp=sharing).


## Changes/Improvements
- nuue net embedded
- search changes
- clang-tidy checks, clang diagnostics, etc.
- resharper c optimizations
- const added to vars & parameters
- vars moved to innerscope
- compile warnings resolved
- lowplyhistory & other search changes added
- compile info printed at startup
- obfuscated node count/nps corrected
- anarchy (forced enpassant) mode removed
- persistent transposition table removed
- unused includes removed
- for details, see: [changes.md](docs/changes.md)

Due to international interest in this project, Fat Titz included a readme translated to Chinese & Hindi...
so to help that effort, I've added documents in Japanese & Russian (and will add more later as time permits).
  
## Compiling Big Ballz
Compiling Big Ballz requires a working gcc or clang environment. The [MSYS2](https://www.msys2.org/) environment is recommended for compiling Big Ballz on Windows (see below on how to set up MSYS2).

To compile, type:

    make target [ARCH=arch] [COMP=compiler] [COMPCC=gcc-4.8] [further options]

from the `src` directory. Lists of supported targets, archs and compilers can be viewed by typing `make` or `make help`.

If the `ARCH` variable is not set or is set to `auto`, the Makefile will attempt to determine and use the optimal settings for your system. If this fails with an error or gives unsatisfactory results, you should set the desired architecture manually. The following `ARCH` values are supported: `x86-86-modern`, `x86-64-avx2`, `x86-64-bmi2`, `x86-64-avx512`, `x86-64-avx2-vnni`, `x86-64-bmi2-vnni`, `x86-64-avx512-vnni`. SSSE3 support is required, if your cpu does not support SSSE3 then see [here](https://www.timeanddate.com/).

Be aware that a Big Ballz binary compiled specifically for your machine may not work on other (older) machines. If the binary has to work on multiple machines, set `ARCH` to the architecture that corresponds to the oldest/least capable machine.

Further options:

<table>
<tr><td><code>pure=yes</code></td><td>NNUE pure only (no hybrid or classical mode)</td></tr>
<tr><td><code>numa=no</code></td><td>Disable NUMA support</td></tr>
<tr><td><code>lto=yes</code></td><td>Compile with link-time optimization</td></tr>
<tr><td><code>extra=yes</code></td><td>Compile with extra optimization options (gcc-7.x and higher)</td></tr>
</table>

Add `numa=no` if compilation fails with`numa.h: No such file or directory` or `cannot find -lnuma`.

The optimization options currently enabled with `extra=yes` appear to be less effective now that the NNUE code has been added.

## Big Ballz UCI settings

#### Analysis Contempt
By default, contempt is set to zero during analysis to ensure unbiased analysis. Set this option to White or Black to analyse with contempt for that side.

#### Threads
The number of CPU threads used for searching a position.

#### Hash
The size of the hash table in MB.

#### Clear Hash
Clear the hash table.

#### Ponder
Let Big Ballz ponder its next move while the opponent is thinking.

#### MultiPV
Output the N best lines when searching. Leave at 1 for best performance.

#### Move Overhead
Compensation for network and GUI delay (in ms).

#### Slow Mover
Increase to make Big Ballz use more time, decrease to make Big Ballz use less time.

#### SyzygyPath
Path to the folders/directories storing the Syzygy tablebase files. Multiple directories are to be separated by ";" on Windows and by ":" on Unix-based operating systems. Do not use spaces around the ";" or ":".

Example: `C:\tablebases\wdl345;C:\tablebases\wdl6;D:\tablebases\dtz345;D:\tablebases\dtz6`

#### SyzygyProbeDepth
Minimum remaining search depth for which a position is probed. Increase this value to probe less aggressively.

#### Syzygy50MoveRule
Disable to let fifty-move rule draws detected by Syzygy tablebase probes count as wins or losses. This is useful for ICCF correspondence games.

#### SyzygyProbeLimit
Limit Syzygy tablebase probing to positions with at most this many pieces left (including kings and pawns).

#### SyzygyUseDTM
Use Syzygy DTM tablebases (not yet released).

#### BookFile/BestBookMove/BookDepth
Control PolyGlot book usage.

#### EvalFile
Name of NNUE network file.

#### Use NNUE
By default, Big Ballz uses NNUE in Stockfish's Hybrid mode, where certain positions are evaluated with the old handcrafted evaluation. Other modes are Pure (NNUE only) and Classical (handcrafted evaluation only).

#### LargePages
Control allocation of the hash table as Large Pages (LP). On Windows this option does not appear if the operating system lacks LP support or if LP has not properly been set up.

#### NUMA
This option only appears on NUMA machines, i.e. machines with two or more CPUs. If this option is set to "on" or "all", Big Ballz will spread its search threads over all nodes. If the option is set to "off", Big Ballz will ignore the NUMA architecture of the machine. On Linux, a subset of nodes may be specified on which to run the search threads (e.g. "0-1" or "0,1" to limit the search threads to nodes 0 and 1 out of nodes 0-3).

## How to set up MSYS2
1. Download and install MSYS2 from the [MSYS2](https://www.msys2.org/) website.
2. Open an MSYS2 MinGW 64-bit terminal (e.g. via the Windows Start menu).
3. Install the MinGW 64-bit toolchain by entering `pacman -S mingw-w64-x86_64-toolchain`.
4. Close the MSYS2 MinGW 64-bit terminal and open another.
