# Chapter 7 - Consistency and Replication

## Introduction

### Reasons for Replication

1. Increase reliability
    a. Possible to continue if one replica crashes
    b. Better protection against write corruptions
2. Performance
    a. Handle increasing number of processes accessing the data
    b. Price could be paid in network keeping replicas up to date

Cost is consistency problems.

### Replication as a scaling technique

- Scalability appears in the form of performance issues.
- Tight consistency:
    - Where all read operations on any copy return the same result and when updates occur, it is propagated to all copies before the next operation.
    - Update is performed as a single atomic operation or transaction.
    - This is inherently costly in terms of performance.
- Only real solution may be to relax consistency constraints.
    - Highly dependent on the access constraints.

## Data-centric consistency models

- Sequential consistency: the result of any execution is the same as if the operations by all processes on the data store were executed in some sequential order and the operations of each individual process appear in this sequence in the order specified by its program.
- Casual consistency: a weakening of sequential consistency in that it makes a distinction between events that are potentiall causally related and those that are not.
-


