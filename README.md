### Benchmarks for our submission "Improving SMT Solver Integrations for the Validation of B and Event-B Models" to [FMICS2021](https://qonfest2021.lacl.fr/fmics21.php).

We decided to use constraints from bounded model checking (BMC).
In particular, we use the monolithic bounded model checking implementation [[6]](https://link.springer.com/chapter/10.1007/978-3-319-33600-8_8) of ProB which sends a single formula to a selected constraint solving backend.
Hereby, we solve 26 constraints for each model and compare the amount of constraints that can be solved by a specific solver.
We use a maximum solver timeout of 30 seconds for each constraint, and compare the ProB constraint solver, its integration of Z3 using the former translation, as well as the new parallel integration of Z3.

We use a subset of TLA+ benchmarks compiled by Igor Konnov, Jure Kukovec, and Thanh-Hai Tran which they used to test their symbolic model checker [Apalache](https://dl.acm.org/doi/10.1145/3360549).
The benchmarks are publicly available [here](https://zenodo.org/record/3370071#.YFMd2y1Q1Zc).
We use the translation from TLA+ to B by Dominik Hansen and Michael Leuschel [[1]](https://dl.acm.org/doi/10.1007/978-3-642-30729-4_3) to load TLA+ models in ProB.

The benchmarks named LargeBranching and SearchEvents were compiled by Sebastian Krings to evaluate the performance of symbolic model checking for B and Event-B [[2]](https://docserv.uni-duesseldorf.de/servlets/DocumentServlet?id=43261).
The remaining B machines were compiled by Sebastian Krings and Michael Leuschel [[3]](https://dl.acm.org/doi/abs/10.1007/978-3-319-33693-0_23) and are taken from a submission to the ABZ 2016 case study by Hoang et al. [[4]](https://dl.acm.org/doi/10.1007/978-3-319-33600-8_31),
as well as from a submission to the ABZ 2014 landing gear case study by Hansen et al. [[5]](https://dl.acm.org/doi/10.1007/s10009-015-0395-9). The models were provided by the authors and are publicly available in ProB's [public examples repository](https://github.com/hhu-stups/specifications).

### Run Benchmarks

The benchmarks can be run on Linux by executing `./probcli -fmics21_benchmarks`.

For each benchmark, a text file presenting the runtimes of all solvers will be created in the root directory.

The Prolog fact `solved_constraints(SolverName, Amount)` indicates that a solver was able to solve `Amount` contraints of a total amount of 26 constraints.

### Results

The benchmarks were run on a system with an Intel Core I7-8750H CPU (2.2GHz) and 16GB of RAM using ProB version 1.10.2, and SICStus Prolog version 4.6.0.
All presented times are measured in milliseconds.

|  Nr | Name         | ProB | ProB-Z3 [[7]](https://link.springer.com/chapter/10.1007/978-3-319-33693-0_23) | ProB-Z3-Par |
| --- | ------- | ------- | ------- | ------- |
|  1 | Prisoners-4  | 13/303424 ms | 0/425392 ms | 0/408259 ms |
|  2 | Bakery        | 3/68024 ms | 0/101908 ms | 1/90582 ms |
|  3 | SimpleTwoPhase  | 26/160 ms | 26/1211 ms | 26/1604 ms |
|  4 | Lightbot  | 2/720850 ms | 0/46615 ms | 10/520642 ms |
|  5 | LargeBranching | 26/127 ms | 26/1963 ms | 26/2272 ms |
|  6 | SearchEvents | 3/120182 ms | 5/60241 ms | 5/60315 ms |
|  7 | ABZ16_m4  | 26/1256 ms  | 26/2865 ms | 26/2372 ms |
|  8 | ABZ16_m5 | 0/1412 ms\*  | 26/3715 ms | 26/2791 ms |
|  9 | ABZ16_m7 | 0/2235 ms\*  | 11/102221 ms | 12/160056 ms |
|  10 | R3_Sensors | 12/458056 ms | 2/511739 ms| 2/505207 ms |
|  11 | R4_Handle | 4/13139 ms | 0/39399 ms | 0/51316 ms |
|  12 | R5_Switch | 8/550449 ms | 3/25564 ms | 3/73136 ms |
|  13 | R6_Lights | 6/426062 ms | 3/20252 ms | 3/49772 ms |

\*unknown due to the use of deferred sets