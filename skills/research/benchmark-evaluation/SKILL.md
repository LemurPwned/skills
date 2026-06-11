---
name: benchmark-evaluation
description: Run a strict research benchmark for a model that will 
---

# Benchmark evaluation 

## When to use 

Use this skill for evaluating the benchmarks for any model. 


## Core prompt 

Start from this baseline

> Establish adequate metrics, if those have not been already indicated by the user or used in previous iterations
> Run benchmakrs 
> Prepare a report that clearly indicates the results and any failures across analyzed axes. For example, if the number of FPs has increased, indicate this clearly in the report and perhaps give some examples.
> Keep a score of benchmark results in SCORES.md. Each dataset should have a separate table in that file.
> Table columns must include run_name, metrics that are evaluated and a date. You can also add a column called `comments` to add any additional context or notes about the run -- for example, critical changes that were made for this run (ex. `fixed parsing city formats`)



## Non-negotiable standards

> Make sure that you always provide a delta relative to the baseline
> Clearly indicate the failures while running the benchmarks that might've skewed the score one way or the other
> Prefer to fail the benchmark if the code fails overly rather than doing try/except
