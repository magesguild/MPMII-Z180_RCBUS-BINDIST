**MP/M II RCBus Z180 Binary Distribution**

This is a collection of binary files used to spin up a working MP/M II
installation based on Tadeusz Pycio's contributions to the platform. It includes
both necessary and optional files for the platform from multiple projects. This
build is tested on the SC126 and the SC792. It does not work on z50bus
implementations of Z180 within the SCC ecosystem.

Two versions of the kernel are provided KRNZ180.LBR has DMA support for the CF
card enabled, and will require the user to pull the DREQ1 line low. KRNZ180N.LBR
disables this support, and requires no special settings to the hardare lines.

Note that the SC126 provides a jumper to pull this line low, while the SC792
will need an added pulldown resistor.

Both kernels are build with the following settings:

    MP/M II V2.1 System Generation
    Copyright (C) 1981, Digital Research


    Default entries are shown in (parens).
    Default base is Hex, precede entry with # for decimal

    Use SYSTEM.DAT for defaults (Y) ?
    Top page of operating system (FF) ?
    Number of TMPs (system consoles) (#2) ?
    Number of Printers (#1) ?
    Breakpoint RST (06) ?
    Enable Compatibility Attributes (Y) ?
    Add system call user stacks (Y) ?
    Z80 CPU (Y) ?
    Number of ticks/second (#50) ?
    System Drive (A:) ?
    Temporary file drive (P:) ?
    Maximum locked records/process (#16) ?
    Total locked records/system (#32) ?
    Maximum open files/process (#153) ?
    Total open files/system (#153) ?
    Bank switched memory (Y) ?
    Number of user memory segments (#7) ?
    Common memory base page (C0) ?
    Dayfile logging at console (N) ?

     SYSTEM  DAT  FF00H  0100H
     TMPD    DAT  FE00H  0100H
     USERSYS STK  FC00H  0200H
     XIOSJMP TBL  FB00H  0100H

    Accept new system data page entries (Y) ?

     RESBDOS SPR  EF00H  0C00H
     XDOS    SPR  CD00H  2200H
     BNKXIOS SPR  B300H  1A00H
     BNKBDOS SPR  9000H  2300H
     BNKXDOS SPR  8E00H  0200H
     TMP     SPR  8A00H  0400H

     LCKLSTS DAT  8200H  0800H
     CONSOLE DAT  8000H  0200H

    Enter memory segment table:
     Base,size,attrib,bank (80,80,80,00) ?
     Base,size,attrib,bank (00,C0,00,01) ?
     Base,size,attrib,bank (00,C0,00,02) ?
     Base,size,attrib,bank (00,C0,00,03) ?
     Base,size,attrib,bank (00,C0,00,04) ?
     Base,size,attrib,bank (00,C0,00,05) ?
     Base,size,attrib,bank (00,C0,00,06) ?
     Base,size,attrib,bank (00,C0,00,07) ?

     MP/M II Sys  8000H  8000H  Bank 00
     Memseg  Usr  0000H  C000H  Bank 01
     Memseg  Usr  0000H  C000H  Bank 02
     Memseg  Usr  0000H  C000H  Bank 03
     Memseg  Usr  0000H  C000H  Bank 04
     Memseg  Usr  0000H  C000H  Bank 05
     Memseg  Usr  0000H  C000H  Bank 06
     Memseg  Usr  0000H  C000H  Bank 07

    Accept new memory segment table entries (Y) ?

    ** GENSYS DONE **

**Sources**
- MPMII-RCBus: https://github.com/tpycio/MPMII-RCBus
- CPM-D: https://smallcomputercentral.com/articles/installing-cp-m-2-2-with-scm/
- SCM-S9: https://smallcomputercentral.com/firmware/firmware-scm-s9/
- SCM-S6: https://smallcomputercentral.com/firmware/firmware-scm-s6/
- Extras: Various tools from Kevin Boone: https://github.com/kevinboone

**Systems**
- SC126: https://smallcomputercentral.com/rcbus/sc100-series/sc126-z180-motherboard-rc2014/
- SC792: https://smallcomputercentral.com/rcbus/sc700-series/sc792-modular-z180-computer/
_Note: The systems do not include the required CF Card Adapters, pick one of these:_
- SC715: https://smallcomputercentral.com/rcbus/sc700-series/sc715-rcbus-compact-flash-module/
- SC729: https://smallcomputercentral.com/rcbus/sc700-series/sc729-rcbus-compact-flash-module/

**Installation**

You probably already have the correct revision of SCM on your system, as it
arrived with the system, but in case you need to update it, the files are
included in the scm directory. S9 is for the SC792 and the S6 build is for the
SC126.

Use minicom and your chip programmer to flash this to your memory module.

From here you can install CP/M 2 using the contents of the scm\_apps directory.
You will need to reboot between installing DOWNLOAD.COM and XM.COM. Simply send
the files as ascii files over serial. This is easily done from within minicom
and similar tools.

DOWNLOAD.COM will receive the files in the pkg directory. You will need to
enable Hardware Flow Control in minicom for these downloads to succeed. MBasic
is optional but provided, the CPM files and the NULU application are required.

After receiving these files, you will want to disable Hardware Flow Control and
begin using XM to pull the files from LBR. The following LBR files are required
and they must be extracted into the default A: drive for user 0:

- KRNZ180.LBR or KRNZ180N.LBR (only one)
- DISTRIB.LBR

The other files are optional, but you probably want to install Y2KTOOLS.LBR
_after_ you install DISTRIB.LBR. This will enable Y2K compliance on your system.
This package was kept separate for those who want historical accuracy, including
Y2K non-compliance.

You may also want ADDON180.LBR for extended operating system functions.

CPMTOOLS.LBR contains a wide range of CP/M 2 command, including multiple
assemblers and the Aztec C compile toolchain from Manx Software. These are
optional but highly recommended. You may want to install them in specific places
or just install them directly to the default A: drive.

EXTRAS.LBR contains various tools created by Kevin Boone for CP/M 2. I recommend
scalc and kcalc, in particular. Some tools are unixy, which is fun.

Use NULU.COM to extract LBR files. You can extract files selectively if you only
want certain tools from CPMTOOLS.LBR or EXTRAS.LBR.

**Documentation**

The doc dir contains a range of documentation and, while it is far from
complete, it should be enough to get any user developing in assembly and C with
some confidence. Some of the assemblers provided in the CPMTOOLS.LBR have been
difficult to find documentation for, but I've provided as many as I have been
able to scrape together (so far, a deeper search may yield more documentation in
the future.)

**System Administration**

The SET.PRL command is extremely important for administering the system. While
it may be a bit silly to be worried about admin versus non-admin users and their
permission sets on these particular systems, where any user with physical access
can simply boot into CP/M and gain full admin access, MP/M II gives us tools for
managing file access, and there are some considerations to be made if you do
want commands and files protected.

SET.PRL itself can disable all password protection on any given drive, so it
must, itself, be password protected for this to work well. Please read through
the SET command documentation provided in the MP/M II User Guide closely before
spending too much time attempting to password protect files.

If you password protect set, you need to pass it the password manually like this
to use it (to set your default password, which can then unlock things
automatically):

set;s3kr1t [default = s3kr1t]

Replace s3kr1t with your own password.

Also note that a password protected file must reside in a password enabled drive
to function as expected.

A:set [protect = on]

Again, see the manual for more details, but this is a potential stumbling block,
so practice it if you want to use passwords.

This does mean that non administrative users can not password protect their own
files, though an administrators can use different passwords to protect files
from each other. It means all admins who need to use the set command must share
a password for that command. Bare these things in mind as you build out your
user design.
