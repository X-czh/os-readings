# Ad Hoc Synchronization Considered Harmful

W. Xiong, S. Park, J. Zhang, Y. Zhou, and Z. Ma, OSDI 2010

## Summary

This paper does a comprehensive characteristic study of ad hoc (aka non-modularized) synchronizations in concurrent programs and builds a tool to automatically identify and annotate ad hoc synchronizations in concurrent C/C++ programs.

## Motivations

Many synchronizations in existing concurrent programs are implemented in an ad hoc (aka non-modularized) way. These ad hoc synchronizations are error-prone and have poor readability and maintainability. It is therefore essential to understand the characteristics of these ad hoc synchronizations and further develop methods to identify and annotate them.

## Solution

### Characteristic study of ad hoc synchronizations

The authors firstly conduct an empirical characteristic study of ad hoc synchronizations in 12 representative concurrent programs of different types. They found that:

* Every studied concurrent program uses ad hoc synchronization.
* Ad hoc synchronizations are diverse, making it hard to manually identify them.
* Ad hoc synchronizations are error-prone.
* Ad hoc synchronizations significantly impact the effectiveness and accuracy of various bug detection and performance tuning tools.

### Tool to automatically identify and annotate ad hoc synchronizations in concurrent C/C++ programs

They further builds a builds a tool to automatically identify and annotate ad hoc synchronizations in concurrent C/C++ programs based on the following found commonality among ad hoc synchronizations:

* Ad hoc synchronizations are all implemented using loops.
* A loop is identified as a sync loop if its exit conditions are (1) loop invariant, (2) directly or indirectly depend on a shared variable, (3) can be satisfied by a remote thread’s update to this variable.

Automatic annotations are then generated using LLVM static instrumentation framework.

### Evaluation

The tool was evaluated on 25 concurrent programs to identify sync loops. Statistics are reported, among which accuracy and false positive rates are emphasized. However, the ground truth of sync loop identification seems to be done manually, and it is mentioned that the tool identifies some sync loops which are missed by the authors initially. This indicates that there might be some sync loops that are identified by neither the authors nor the tool, which weakens the validity of the statistics.

### Comments

It is interesting to note that some programmers are aware of the potential harm of using ad hoc synchronizations (revealed by their comments) but they still use it. Reasons includes performance issues or flexibility concerns. It'll be valuable to dig the underneath psychological/technical reasons behind that. Is that because the programmers lacks understanding of modularized synchronization approaches? Or is that because the current synchronization methods lacking some key features for usability?
