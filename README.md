During the Summer of 2023, I worked for OpenPrinting which comes under The Linux Foundation Umbrella for Google Summer of Code
# Project: Continous Integration: Designing Testing Programs and CI for Libcupsfilters

What is triggered on each commit is usually some static analysis of the code using common, specialized tools and build and execution tests, usually doing `./configure; make; make test` on different system platforms. This naturally requires test scripts/programs compiled and run by the `make test` step. For CUPS, for example, the daemon is started (on an unprivileged port so that it does not need root), queues created and listed, jobs sent, and the logs checked whether everything went OK. For this project we wanted to go over and beyond than just common tests, essentially adding test programs for all kinds of functionality implemented in the code base.

The task here was to write test programs for [**Libcupsfilters**](https://github.com/openprinting/libcupsfilters) so that `make test` utilizes some core functionality of the project and essentially could test against the any failure in the working/output caused by any wrong snippet of code or bugs. It should exercise important functionality of the software with different parameters and analyze logs and output files to check whether the program did the expected work. Ultimately, `make check` was then to be run using Github Actions for the sanity check of Pull Requests and commits.
 


### Test Suite for Libcupsfilters
![Flowchart](https://github.com/pranjanpr/Delfi/assets/57591230/c371a43c-8781-4a00-912f-a7b5ca70e269)

#### Final Work Product: [CI23](https://github.com/pranjanpr/libcupsfilters/blob/ci23/)
Each of the test cases of the test suite (`cupsfilters/test-filter-cases.txt`) takes these parameters as inputs:\
`Input_File` `Input_Type` `Output_File` `Output_Type` `Make` `Model` `Color` `Duplex` `Formats` `Job-Id` `User` `Title` `Copies`\
The set of filter functions which would be utlized for this print job (test case) would depend on the Input MIME types and output MIME types

#### Test Program
The test program emulates an IPP printer with the properties defined in the test cases and attempts to test it by calling `cfFilterUniversal()`. The logs of the test case is then redirected to `Test_summary.txt`. These test cases are summarised based on bug reports on OpenPrinitng. Failure of any of the test cases returns a non-zero exit status from the tester executable, which ultimately fails the `make check`.

#### Github Actions
`.github/workflows/build.yml` builds ubuntu-latest and installs all the necessary libraries needed for libcupsfilters. Future contributers could also use it as a heads-up on the libraries needed for libcupsfilters and their sources. The build essentially runs `make check` on ubuntu-latest. Libcupsfilters complete test suite is done.

### Test Suite for cpdb-libs
The Common Print Dialog Backends (CPDB) project of OpenPrinting is about separating the print dialogs of different GUI toolkits and applications (GTK, Qt, LibreOffice, Firefox, Chromium, ...) from the different print technologies (CUPS/IPP, cloud printing services, ...) so that they can get developed independently and so always from all applications one can print with all print technologies and changes in the print technologies get supported quickly.

I attempted to combine the autopkgtest test for Debian which Till uses, with already present `make check` for cpdb-libs. The autopkgtest in Debian takes use of system files, which needs sudo permissions when used in Ubuntu. Hence for security reasons we needed to configure these tests to work without root permissions and think of possibe workaround for necessary tests. Build of these tests is still in progress.

Latest commit for [cpdb-libs tests](https://github.com/pranjanpr/cpdb-libs/tree/ci23) 

#### Misc
During my work, I faced few petty issues which I think could be helpful for new contributers of openPrinting. I kept a compilation of these issues and their solutions in this [Common Issues](https://docs.google.com/document/d/1BiPURRAOFpUP2EnR-Uk8JYL2L30MflEZiFIOMRWnHL8/edit?usp=sharing)

###### Acknowledgements
I would like to express my sincere vote of thanks to my mentors, Till (`till [dot] kamppeter [at] gmail [dot] com`) and Deepak (`patankardeepak04 [at] gmail [dot] com`), for being extremely supportive throughout the project. I would like to express my gratitude to Aveek Basu (`basu [dot] aveek [at] gmail [dot] com`).

###### Footnotes
CUPS-Filters got split out of CUPS for CUPS version 1.6.x, containing the filters and backends which Apple does not need for Mac OS X and therefore did not want to maintain anymore. Till Kamppeter had overtaken this part as an OpenPrinting project named CUPS-Filters. He added CUPS-browsed as CUPS itself did not automatically make remote CUPS queues available locally anymore. He also took maintainership on all CUPS features which Apple has given up. With the time, cups-filters got improved cups-filters, especially switched to a PDF-based printing workflow, added legacy CUPS broadcasting/browsing, sophisticated filtering of remote printers, auto setup of remote IPP printers, driverless printing, etc., and all the time kept it compatible with new CUPS features.
