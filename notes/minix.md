# Lessons Learned from 30 Years of MINIX

Andrew S. Tanenbaum, Communications of the ACM 2016

## OS-specific lessons

### Lesson. Each device driver should run as an independent, user-mode process.

Updating critical system components without rebooting the computer is a super appealing idea. This also prevents the system from crashing when there is a driver crash. I agree with Andrew's thought that performance issue is less important today than decades ago. On the other hand, we are more reliant on life-critical devices used in medicine and transportation, reliability is more important now.

## General technical lessons

### Meta-lesson. be sceptical, anything could have bugs, anything could go down.

The interrupt-15 story is illuminating. Documents can have things that left uncovered. Hardware can have bugs as well. Never trust anything blindly.

### Lesson. When standard exists, stick to them.

This one is interesting. The Linux kernel cannot be compiled with clang/LLVM as it uses many GCC C extensions that are not part of the standard.

### Lesson. Doing Ph.D. research and developing a software product at the same time are very difficult to combine.

There are only a handful of software systems with an academic origin yet widely deployed in production that can come to my mind: MXNet, Spark, Scala, LLVM, and that's it. Also, LLVM is kind of special as Apple played an important role in developing LLVM after they hired Chris Lattner. In the academic setting, one is expected to do innovations, while developing a production system often involves tons of dirty work without much innovative elements. There is an inherent conflict in it. If you can succeed in either part of the world, you are outstanding, but if you can combine the two, you are just excellent.

## Non-technical lessons

### Meta-lesson. You need to find a place in the market where your product fits in. If market changes, reexamine your plan.

MINIX came into the play after UNIX was prohibited from being taught in class. It targeted the vacuum in the market well and as a result, it received enormous popularity in the education market. However, later when MINIX tried to enter the production market in the early 2000s, it was already too late as Windows and Linux have dominated the market. If MINIX was not exceptionally better than its competitors, there was no chance MINIX could win. Andrew later shifted the focus of the MINIX project targeting towards the research of highly reliable system, which I think is wise.

### Lesson. No matter how desirable your product is, you need a way to market or distribute it.

I didn't thought of it too much before until I read Andrew's story on persuading the publisher to distribute the books together with floppy disks. Nowadays we have the Internet, and things become much easier. However, we still need to let people know our products, so marketing is still important.
