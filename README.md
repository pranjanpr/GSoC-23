During the Summer of 2023 I worked for OpenPrinting which comes under The Linux Foundation Umbrella for Google Summer of Code
# Project: Continous Integration: Designing Testing Programs and CI for Libcupsfilters

What is triggered on each commit is usually some static analysis of the code using common, specialized tools and build and execution tests, usually doing `./configure; make; make test` on different system platforms. This naturally requires test scripts/programs compiled and run by the `make test` step. For CUPS, for example, the daemon is started (on an unprivileged port so that it does not need root), queues created and listed, jobs sent, and the logs checked whether everything went OK. For this project we wanted to go over and beyond than just common tests, essentially adding test programs for all kinds of functionality implemented in the code base.

The task here was to write test programs for [**Libcupsfilters**](https://github.com/openprinting/libcupsfilters) so that `make test` utilizes some core functionality of the project and essentially could test against the any failure in the working/output caused by any wrong snippet of code or bugs. It should exercise important functionality of the software with different parameters and analyze logs and output files to check whether the program did the expected work. Ultimately, `make check` was then to be run using Github Actions for the sanity check of Pull Requests and commits.

Here is a FlowChart overview: 
![Flowchart](https://github.com/pranjanpr/Delfi/assets/57591230/c371a43c-8781-4a00-912f-a7b5ca70e269)
