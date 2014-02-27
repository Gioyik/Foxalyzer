Foxalyzer
===

Goal: provide developers and reviewers with a browser-based security toolset they can use to scan Firefox OS apps for security issues.

Based on our [tool evaluation](https://wiki.mozilla.org/Security/B2G/JavaScript_code_analysis) we chose [JSPrime](https://github.com/dpnishant/jsprime), [scanjs](https://github.com/freddyb/scanjs) ([online demo](http://freddyb.github.io/scanjs/client/)) and [retire.js](https://github.com/bekk/retire.js) for the first run of tools. The concept of our add-on is to run each scanner on any relevant file within a Firefox OS app and to integrate the required controls and the tools' output into the app-manager and developer tools framework in a meaningful way.

We need to evaluate two promising concepts:

#### Add a "Analyze" button to app-manager's app view (close to the debug button)

This approach might be the quickest and most pain-free for prototyping, as well as future developments. However, the lack of integration into existing developer tools might be inconvenient for developers. Convenience link between the tools' reports and the code they refer to might be hard to implement.

#### Make the tools output separately in an additional "Analysis" tab in the developer tools.

Having controls and output integrated into the developer tools will enable a natural workflow with code-highlighting, convenience links, and other goodies. But since the debugger's views only targets single files, it's so far unclear how we can provide a meaningful way to combine scan results that draw from several files in the app.

## Steps

This is a rough outline of the project.

### Fork scanjs, jsprime, retire.js into repo

We need to find a good mechanism to fork our target scanners into this repository. There is plenty of discussions of git submodules over git trees. cr's leaning towards trees. Any views on this?

We also need to devise an update strategy that will allow us to easily upgrade the scanners.

### Patch out node dependencies from the scanners

We should no further explore the option of bundling node binaries with the add-on, but to focus on removing node dependencies from the scanners. They only use node for opening files and creating templated views of their results.

### Design a modular scanner wrapper

The project requires a central manager class that abstracts scanners, their calling conventions, and their output into a common format. Its main task is to run the scanner modules across all significant files in the app and then pull the results together.

### Write callers and report wrappers for each scanner

The goal is not just extensibility, but to provide a unified look&feel instead of a potpurri of report styles. It is unclear as of now if this can be achieved by a greatest-denominator report format, but so far all important scanner results boil down to (code reference, information block).

### Integrate the wrapper into about:app-manager and/or developer tools

It might be a good strategy to start out with a simple scan button integration in about:app-manager's app view that generates a single combined report. Moving it into a developer tools tab is definitely desireable, but feels like an additional step that can be taken once we have the app-manager integration up and running.


## Add-on template information

Simple template project for developing restartless Firefox Developer Tools addons.

![Screenshot](https://dl.dropboxusercontent.com/u/2388316/screenshots/firefox-restartless-addon.png)
