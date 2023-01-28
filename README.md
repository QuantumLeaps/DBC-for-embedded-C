## Brought to you by:
[![Quantum Leaps](https://www.state-machine.com/attachments/logo_ql_400.png)](https://www.state-machine.com)
<hr>

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/QuantumLeaps/DBC-for-embedded-C)](https://github.com/QuantumLeaps/DBC-for-embedded-C/releases/latest)
[![GitHub](https://img.shields.io/github/license/QuantumLeaps/DBC-for-embedded-C)](https://github.com/QuantumLeaps/DBC-for-embedded-C/blob/master/LICENSE)

# Design By Contract (DBC)
Design By Contract (DBC), pioneered by Bertrand Meyer, views a software system
as a set of components whose collaboration is based on precisely defined
specifications of mutual obligations—the contracts.1 The central idea of this
method is to inherently embed the contracts in the code and validate them
automatically at run time. Doing so consistently has two major benefits:

1. It automatically helps detect bugs (as opposed to "handling" them), and
2. It is one of the best ways to document code.

# DBC for Embedded C and C++
You can implement the most important aspects of DBC (the contracts) in C or
C++ with **assertions**. The Standard C Library macro `assert()` is rarely
applicable to embedded systems, however, because its default behavior (when
the integer expression passed to the macro evaluates to `false`) is to print
an error message and exit. Neither of these actions makes much sense for most
embedded systems, which rarely have a screen to print to and cannot really
exit either (at least not in the same sense that a desktop application can).
Therefore, in an embedded environment, you usually have to define your own
assertions that suit your tools and allow you to customize the error response.
However, you should think twice before you go about "enhancing" assertions
because a large part of their power derives from their relative *simplicity*.

# This DBC implementation
The DBC implementation in this repository consists of just one header file
[dbc_assert.h](./dbc_assert.h), which has been specifically tailored
for embedded systems and has the following properties:

- allows customizing the error response (by implementing the DBC
fault handler `DBC_fault_handler()`);

- conserves memory by avoiding proliferation of multiple copies
of the filename string (macro `DBC_MODULE_NAME()`);

- provides additional macros for checking and documenting preconditions
(`DBC_REQUIRE()`), postconditions (`DBC_ENSURE()`), and invariants
(`DBC_INVARIANT()`).

> **NOTE**<br>
The names of the three last macros are a direct loan from Eiffel,
the programming language that natively supports DBC.


# Example of Use
```
#include "dbc_assert.h" /* Design By Contract (DBC) assertions */

DBC_MODULE_NAME("sst")  /* for DBC assertions in this module */

/*..........................................................................*/
void SST_Task_post(SST_Task * const me, SST_Evt const * const e) {
    /*! @pre the queue must be sized adequately and cannot overflow */
    DBC_REQUIRE(300, me->nUsed <= me->end);
    ...
}
```

## More DBC Resources
To learn more about DBC for embedded systems, please check out the following
resources:

[1] [Key Concept: Design by Contract](https://www.state-machine.com/dbc)<br>
[2] ["Design by Contract for Embedded C"](https://barrgroup.com/embedded-systems/how-to/design-by-contract-for-embedded-software)<br>
[3] ["A Nail for a Fuse"](https://www.state-machine.com/a-nail-for-a-fuse)<br>
[4] ["An Exception or a Bug"](https://www.state-machine.com/doc/Samek0308.pdf)


# How to Help this Project?
Please feel free to clone, fork, and make pull requests to improve DBC
for Embedded C. If you like this project, please give it a star (in the
upper-right corner of your browser window):

<p align="center"><img src="img/github-star.jpg"/></p>
