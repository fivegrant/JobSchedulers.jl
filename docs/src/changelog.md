# Changelog

v0.8.1

- Fix: scheduler() handles errors and InteruptExceptions more wisely. (Thanks to @fivegrant, #7)

v0.8.0

- Feat: `ncpu == 0` can set to a `Job`, but a warning message shows.
- Feat: `dependency = DONE => job_A`: to be simplified to `dependency = job_A` or `dependency = job_A.id`.
- Feat: Simplify `Job()` methods.
- Feat: `submit!(Job(...))`: to be simplified to `submit!(...)`.
- Feat: schedule repetitive jobs using `Cron` until a specific date and time: `Job(cron = Cron(0,0,*,*,*,*), until = Year(1))`. It is  inspired by Linux-based crontab.
- Change: `Job()`: default wall time value increase to `Year(1)` from `Week(1)`.
- Change: `SCHEDULER_TASK` is now a `Base.RefValue{Task}` rather than undefined or `Task`.

v0.7.12

- Compat: Pipelines v0.9, 0.10 (new), 1 (not published).
- Docs: Use Documenter.jl.

v0.7.11

- Update: Term to v2.
- Feat: Set a lower loop interval of nthreads > 2.
- Feat: Move `scheduler_start()` in `__init__()`.

v0.7.10

- Feature: Better progress bar for visualization.

v0.7.9

- Fix: `solve_optimized_ncpu()`: devision by 0 if njob == 0.

v0.7.8

- Feature: `solve_optimized_ncpu()`: Find the optimized number of CPU for a job.

v0.7.7

- Fix: `style_line()`: index error for special UTF characters.

v0.7.6

- Fix: if original stdout is a file, not contaminating stdout using `wait_queue(show_progress = true)`.

v0.7.5

- Change: remove extra blank lines after `wait_queue(show_progress = true)`.

- Fix a benign error (task switch error for `sleep()`).

v0.7.4

- Feature: Progress meter: `wait_queue(show_progress = true)`.

v0.7.3

- Compat: Pipelines v0.9: significant improvement on decision of re-run: considering file change.
- Fix: pretty print of Job and Vector{Job}.

v0.7.2

- Fix: unexpected output of `scheduler_status()` when SCHEDULER_TASK is not defined.

v0.7.1

- Compat: PrettyTables = "0.12 - 2" to satisfy DataFrames v1.3.5 which needs PrettyTables v1 but not v2.

v0.7.0

- Remove dependency DataFrames and change to PrettyTables. The loading time of DataFrames is high.

- Feature: now a Job is sticky to one thread (>1). JobSchedulers allocates and manuages it. The SCHEDULER_TASK is sticky to thread 1.

- Feature: `queue(...)` is rewritten.

- Feature: Better pretty print of Job and queue().

- Feature: New function: `wait_queue()` waits for all jobs in `queue()` become finished.

- Feature: New function: `set_scheduler()`

- Fix: `set_scheduler_max_cpu(percent::Float64)`: use default_ncpu() if error.

- Change: SCHEDULER_UPDATE_SECOND to 0.05 from 0.6

v0.6.12

- Feature: Enchance compatibility with Pipelines v0.8.5: Program has a new field called arg_forward that is used to forward user-defined inputs/outputs to specific keyword arguments of JobSchedulers.Job(::Program, ...), including name::String, user::String, ncpu::Int, mem::Int.

v0.6.11

- Fix: running `queue()` when updating queue: use lock within `DataFrames.DataFrame(job_queue::Vector{Job})`.

v0.6.10

- Update documents.

v0.6.9

- Support Pipelines.jl v0.8.

v0.6.8

- Feature: Replace `@Job` with `Job` to run `program` without creating `inputs::Dict` and `outputs::Dict`. Remove `@Job`.

v0.6.7

- Feature: Run `program` without creating `inputs::Dict` and `outputs::Dict`: `@Job program::Program key_value_args... Job_args...`. See also `@run` in Pipelines.jl.

v0.6.6

- Optimize: `job.dependency` now accepts `DONE => job`, `[DONE => job1.id; PAST => job2]`.

- Optimize: `is_dependency_ok(job::Job)::Bool` is rewritten: for loop when found a dep not ok, and delete previous ok deps. If dep is provided as Int, query Int for job and then replace Int with the job.

v0.6.5

- Fix: If an app is built, SCHEDULER_MAX_CPU and SCHEDULER_MAX_MEM will be fixed to the building computer: fix by re-defining `SCHEDULER_MAX_CPU` and `SCHEDULER_MAX_MEM` in `__init__()`.

- Debug: add debug outputs.

v0.6.4

- Fix: `scheduler_stop()` cannot stop because v0.6.1 update. Now `scheduler_stop` does not send ^C to `SCHEDULER_TASK`, but a new global variable `SCHEDULER_WHILE_LOOP::Bool` is added to control the while loop in `scheduler()`.

- Optimize: the package now can be precompiled: global Task cannot be precompiled, so we do not define `SCHEDULER_TASK::Task` when loading the package. Define it only when needed.

v0.6.3

- Fix: `scheduler_start()` now wait until `SCHEDULER_TASK` is actually started. Previously, it returns after `schedule(SCHEDULER_TASK)`.

v0.6.2

- Compat Pipelines v0.7.0.

v0.6.1

- Robustness: scheduler() and wait_for_lock(): wrap sleep() within a try-catch block. If someone sends ctrl + C to sleep, scheduler wont stop.

v0.6.0

- Compatibility: Pipelines v0.5.0: Job(...; dir=dir).

v0.5.1

- Fix: program_close_io: If the current stdout/stderr is IO, restore to default stdout/stderr.

v0.5.0

- Compatibility: Pipelines v0.5.0: fixed redirection error and optimized stack trace display. Extend `Base.istaskfailed` to fit Pipelines and JobSchedulers packages, which will return a `StackTraceVector` in `t.result`, while Base considered it as `:done`. The fix checks the situation and modifies the real task status and other properties.

v0.4.1

- Export `PAST`. PAST is the super set of DONE, FAILED, CANCELLED, which means the job will not run in the future.

v0.4.0

- If running with multi-threads Julia, `SCHEDULER_TASK` runs in thread 1, and other jobs spawn at other threads. Thread assignment was achieved by JobScheduler. Besides, `SCHEDULER_MAX_CPU = nthreads() > 1 ? nthreads()-1 : Sys.CPU_THREADS`.

- New feature: `queue(job_state::Symbol)`.

- Use try-finally for all locks.

v0.3.0

- Tasks run on different threads, if Julia version supports and `nthreads() > 1`.

- Use `SpinLock`.

- Fix typo "queuing" from "queueing".

- Notify when a job is failed.
