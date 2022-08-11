~~~~~~~~~~~~~~~~

//***FILE 182 is the TSO "PDS" command processor and ISPF dialog    *   FILE 182
//*           program package.  There are possibly 1000 separate    *   FILE 182
//*           dataset manipulation functions built into this        *   FILE 182
//*           package, and it is something that no systems          *   FILE 182
//*           programmer can afford to be without  .......          *   FILE 182
//*                                                                 *   FILE 182
//* Current:  PDS -- VERSION 8.6.18.11  AUGUST 22, 2021             *   FILE 182
//* -------                                                         *   FILE 182
//*       ->  Pre-built load libraries are now included:            *   FILE 182
//*           These can be used for a VERY QUICK INSTALL of PDS.    *   FILE 182
//*                                                                 *   FILE 182
//*                 Members are:                                    *   FILE 182
//*                                                                 *   FILE 182
//*     >>>>  Z035XMIT (PDS load modules - avoids reassembly,       *   FILE 182
//*                     but you can't set options so precisely.)    *   FILE 182
//*     >>>>  UTILXMIT (needed utility load modules from CBT Tape)  *   FILE 182
//*     >>>>  COMXMIT  (load modules to invoke COMPARE programs)    *   FILE 182
//*                                                                 *   FILE 182
//*           - - - - - - - -  H I S T O R Y  - - - - - - - -       *   FILE 182
//*                                                                 *   FILE 182
//*           PDS was written in 1972 by Tom Springer, William      *   FILE 182
//*           Finkelstein, and Steve Smith at Security Pacific      *   FILE 182
//*           National Bank.                                        *   FILE 182
//*                                                                 *   FILE 182
//*           Bruce Leland and Steve Smith extensively modified     *   FILE 182
//*           PDS in the 1970's, 1980's, and early 1990's adding    *   FILE 182
//*           many new subcommands and ISPF Dialog mode support.    *   FILE 182
//*                                                                 *   FILE 182
//*           John Kalinich added Y2K support in 1997 and is now    *   FILE 182
//*           supporting and enhancing this package.                *   FILE 182
//*                                                                 *   FILE 182
//*           Greg Price and John Kalinich added PDSE support in    *   FILE 182
//*           2005.                                                 *   FILE 182
//*                                                                 *   FILE 182
//*           John Kalinich                                         *   FILE 182
//*           St Louis, MO                                          *   FILE 182
//*           the.pds.command@gmail.com                             *   FILE 182
//*                                                                 *   FILE 182
//*           Greg Price                                            *   FILE 182
//*           Prycroft Six Pty. Ltd.                                *   FILE 182
//*           service2@prycroft6.com.au                             *   FILE 182
//*                                                                 *   FILE 182
//*                                                                 *   FILE 182
//* >>>>      Please notify John or Greg if you have any fixes or   *   FILE 182
//* >>>>      enhancements to PDS, so that they may incorporate     *   FILE 182
//* >>>>      and/or test them.                                     *   FILE 182
//*                                                                 *   FILE 182
//*  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  *   FILE 182
//*                                                                 *   FILE 182
//*           This is the highest current version of the free PDS   *   FILE 182
//*           command, which is Version 8.6, from John Kalinich     *   FILE 182
//*           and Greg Price.                                       *   FILE 182
//*                                                                 *   FILE 182
//*           PDS 8.6 incorporates the following changes:           *   FILE 182
//*                                                                 *   FILE 182
//*      o    PDSE (partitioned data set extended) support.         *   FILE 182
//*      o    MXI interface.                                        *   FILE 182
//*      o    SuperC Search-For interface.                          *   FILE 182
//*      o    DISASM interface.                                     *   FILE 182
//*      o    DELINK interface.                                     *   FILE 182
//*      o    TSO TRANSMIT interface.                               *   FILE 182
//*      o    OFFLOAD interface.                                    *   FILE 182
//*      o    SMPGEN support.                                       *   FILE 182
//*      o    IBM Debug Tool Load Module Analysis interface.        *   FILE 182
//*      o    FDR Compaktor/DISKMAP interface.                      *   FILE 182
//*      o    COBANAL load module analysis interface.               *   FILE 182
//*      o    CONDEND subcommand for batch execution.               *   FILE 182
//*      o    PDSLOAD interface.                                    *   FILE 182
//*      o    AMBLIST interface.                                    *   FILE 182
//*      o    Extended ISPF statistics (z/OS 1.11) support.         *   FILE 182
//*      o    GIMCPTS interface.                                    *   FILE 182
//*      o    PDSMATCH interface.                                   *   FILE 182
//*      o    IEBPDSE interface (z/OS 1.13).                        *   FILE 182
//*      o    EAV support for PDSE data sets in CYL managed space.  *   FILE 182
//*      o    8-Character TSO userid (z/OS 2.3) support.            *   FILE 182
//*      o    RMODE64 reporting (z/OS 2.3).                         *   FILE 182
//*      o    PDSE V2 member generations support (PGLITE exec).     *   FILE 182
//*                                                                 *   FILE 182
//*           PDS 8.5 incorporated the following changes:           *   FILE 182
//*                                                                 *   FILE 182
//*      o    Year 2000 date support.                               *   FILE 182
//*      o    Dynamic UCB and 4-digit device number support.        *   FILE 182
//*      o    31-bit UCB support.                                   *   FILE 182
//*      o    9345 and Fat DASD (3390-9 and above) support.         *   FILE 182
//*      o    Point-and-shoot sort columns in dialog panels.        *   FILE 182
//*      o    SuperC and Comparex compare interface.                *   FILE 182
//*      o    Reset ISPF creation/last modification date and time.  *   FILE 182
//*      o    ISPF View support.                                    *   FILE 182
//*      o    ISO alternative date support (yy/mm/dd).              *   FILE 182
//*      o    AMODE64 query and reporting.                          *   FILE 182
//*      o    NRETRIEV/REFLIST panel logic support.                 *   FILE 182
//*      o    PDS-determined block size.                            *   FILE 182
//*                                                                 *   FILE 182
//*           See member #PDSMODS for more details, or issue        *   FILE 182
//*           CONTROL MODS to list the most recent modifications.   *   FILE 182
//*                                                                 *   FILE 182
//*           It is possible to assemble PDS 8.6, so that it will   *   FILE 182
//*           run in Line Mode under MVS 3.8, under Hercules.       *   FILE 182
//*           This is documented in member $$$HERC.  The PDS8638    *   FILE 182
//*           load module in File 035 on this tape has been         *   FILE 182
//*           assembled to run under MVS 3.8, under Hercules.       *   FILE 182
//*                                                                 *   FILE 182
//*           It would be "GROSS NEGLECT" to have a copy of the     *   FILE 182
//*           CBT tape and not investigate this product.            *   FILE 182
//*                                                                 *   FILE 182
//*           This file is best combined with utilities on          *   FILE 182
//*           Files 296, 112, and 134 of this tape.  For optimal    *   FILE 182
//*           value, programs:  DSAT, DVOL, VTOC, REVIEW, HEL,      *   FILE 182
//*           BLKDISK with all its aliases, COMPARE, AND COMPAREB   *   FILE 182
//*           should be available to your TSO session, in an ISPF   *   FILE 182
//*           tasklib (STEPLIB, TSOLIB, or in some other way).      *   FILE 182
//*                                                                 *   FILE 182
//*           UTILXMIT was added to the installation file for       *   FILE 182
//*           a quick install of these PDS related utilities:       *   FILE 182
//*                                                                 *   FILE 182
//*           BLKDISK, COBANAL, COMPARE*, DISASM, DISKMAP,          *   FILE 182
//*           DELINKI, DSAT, DVOL, HEL, MXI, OFFLOAD, PDSLOAD,      *   FILE 182
//*           PDSMATCH, RELEASE, REVIEW, AND VTOC.                  *   FILE 182
//*                                                                 *   FILE 182
//*           Bruce Leland has donated to PDS 8.6, SuperC and       *   FILE 182
//*           Comparex interfaces for the COMPARE subcommand of     *   FILE 182
//*           PDS.                                                  *   FILE 182
//*                                                                 *   FILE 182
//*           See member COMXMIT, which has load modules for        *   FILE 182
//*           these interfaces:  COMPAREC is for SuperC, COMPAREW   *   FILE 182
//*           is for Comparex (R).                                  *   FILE 182
//*                                                                 *   FILE 182
//*           The PDS command allows the TSO user to access and     *   FILE 182
//*           manipulate the directory and selected members of a    *   FILE 182
//*           partitioned data set.  The PDS command contains       *   FILE 182
//*           hundreds of separate functions, and can be operated   *   FILE 182
//*           either in TSO Line Mode (with PUTLINE interfacing)    *   FILE 182
//*           or in ISPF Fullscreen mode.  ISPF mode has all of     *   FILE 182
//*           the line mode functions, and also, many additional    *   FILE 182
//*           capabilities.  PDS, in Line Mode, can be run from     *   FILE 182
//*           a system console under TSSO (from File 404).  TSSO    *   FILE 182
//*           is a subsystem, which can be brought up under         *   FILE 182
//*           SUB=MSTR without JES.  In that case, the Line Mode    *   FILE 182
//*           functions of PDS still work.  Therefore, you can      *   FILE 182
//*           expand the directory of a pds, copy members from      *   FILE 182
//*           one pds to another, etc etc, without JES2 or JES3     *   FILE 182
//*           and without TSO being up.  This makes for a great     *   FILE 182
//*           recovery tool.  Please explore this while your        *   FILE 182
//*           system is healthy, and have the mechanisms in place,  *   FILE 182
//*           just in case.                                         *   FILE 182
//*                                                                 *   FILE 182
//*           With its directory options, the PDS command can       *   FILE 182
//*           produce statistics on directory and data set usage,   *   FILE 182
//*           display portions of the directory, and scratch,       *   FILE 182
//*           rename or create aliases for selected members.  For   *   FILE 182
//*           all of a pds's members that have previously been      *   FILE 182
//*           deleted and before the library has been compressed    *   FILE 182
//*           PDS will allow you to go in and restore those         *   FILE 182
//*           members.  For load data sets, options are available   *   FILE 182
//*           to list load module history data, display and         *   FILE 182
//*           modify load module linkage attributes, and produce    *   FILE 182
//*           load module CSECT maps in two different lengths.      *   FILE 182
//*           For other partitioned data sets, options are          *   FILE 182
//*           available to SUBMIT a member (JCL) for background     *   FILE 182
//*           processing, list a member, edit a member or list      *   FILE 182
//*           lines from a member containing a specified search     *   FILE 182
//*           string.  This file is in IEBUPDTE SYSIN format and    *   FILE 182
//*           contains the source and help member for this command. *   FILE 182
//*           The RESTORE option will also allow the recovery of    *   FILE 182
//*           deleted load module members.                          *   FILE 182
//*                                                                 *   FILE 182
//*           The PDS product at version 8.6 provides an ISPF       *   FILE 182
//*           interface and utility value of awesome proportion.    *   FILE 182
//*           You are advised NOT to pass over this file without    *   FILE 182
//*           looking at it ..........                              *   FILE 182
//*                                                                 *   FILE 182
//*  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  -  *   FILE 182
//*                                                                 *   FILE 182
~~~~~~~~~~~~~~~~

