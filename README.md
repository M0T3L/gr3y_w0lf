NAME         top

       file — determine file type

SYNOPSIS         top

       file [-bcdEhiklLNnprsSvzZ0] [--apple] [--exclude-quiet]
       [--extension] [--mime-encoding] [--mime-type] [-e testname]
       [-F separator] [-f namefile] [-m magicfiles] [-P name=value]
       file ... file -C [-m magicfiles] file [--help]

DESCRIPTION         top

       This manual page documents version 5.46 of the file command.

       file tests each argument in an attempt to classify it.  There are
       three sets of tests, performed in this order: filesystem tests,
       magic tests, and language tests.  The first test that succeeds
       causes the file type to be printed.

       The type printed will usually contain one of the words text (the
       file contains only printing characters and a few common control
       characters and is probably safe to read on an ASCII terminal),
       executable (the file contains the result of compiling a program in
       a form understandable to some UNIX kernel or another), or data
       meaning anything else (data is usually “binary” or non-printable).
       Exceptions are well-known file formats (core files, tar archives)
       that are known to contain binary data.  When modifying magic files
       or the program itself, make sure to preserve these keywords.
       Users depend on knowing that all the readable files in a directory
       have the word “text” printed.  Don't do as Berkeley did and change
       “shell commands text” to “shell script”.

       The filesystem tests are based on examining the return from a
       stat(2) system call.  The program checks to see if the file is
       empty, or if it's some sort of special file.  Any known file types
       appropriate to the system you are running on (sockets, symbolic
       links, or named pipes (FIFOs) on those systems that implement
       them) are intuited if they are defined in the system header file
       <sys/stat.h>.

       The magic tests are used to check for files with data in
       particular fixed formats.  The canonical example of this is a
       binary executable (compiled program) a.out file, whose format is
       defined in <elf.h>, <a.out.h> and possibly <exec.h> in the
       standard include directory.  These files have a “magic number”
       stored in a particular place near the beginning of the file that
       tells the UNIX operating system that the file is a binary
       executable, and which of several types thereof.  The concept of a
       “magic number” has been applied by extension to data files.  Any
       file with some invariant identifier at a small fixed offset into
       the file can usually be described in this way.  The information
       identifying these files is read from the compiled magic file
       /usr/local/share/misc/magic.mgc, or the files in the directory
       /usr/local/share/misc/magic if the compiled file does not exist.
       In addition, if $HOME/.magic.mgc or $HOME/.magic exists, it will
       be used in preference to the system magic files.

       If a file does not match any of the entries in the magic file, it
       is examined to see if it seems to be a text file.  ASCII,
       ISO-8859-x, non-ISO 8-bit extended-ASCII character sets (such as
       those used on Macintosh and IBM PC systems), UTF-8-encoded
       Unicode, UTF-16-encoded Unicode, and EBCDIC character sets can be
       distinguished by the different ranges and sequences of bytes that
       constitute printable text in each set.  If a file passes any of
       these tests, its character set is reported.  ASCII, ISO-8859-x,
       UTF-8, and extended-ASCII files are identified as “text” because
       they will be mostly readable on nearly any terminal; UTF-16 and
       EBCDIC are only “character data” because, while they contain text,
       it is text that will require translation before it can be read.
       In addition, file will attempt to determine other characteristics
       of text-type files.  If the lines of a file are terminated by CR,
       CRLF, or NEL, instead of the Unix-standard LF, this will be
       reported.  Files that contain embedded escape sequences or
       overstriking will also be identified.

       Once file has determined the character set used in a text-type
       file, it will attempt to determine in what language the file is
       written.  The language tests look for particular strings (cf.
       <names.h>) that can appear anywhere in the first few blocks of a
       file.  For example, the keyword .br indicates that the file is
       most likely a troff(1) input file, just as the keyword struct
       indicates a C program.  These tests are less reliable than the
       previous two groups, so they are performed last.  The language
       test routines also test for some miscellany (such as tar(1)
       archives, JSON files).

       Any file that cannot be identified as having been written in any
       of the character sets listed above is simply said to be “data”.
