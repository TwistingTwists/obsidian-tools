Based on the analysis of the codebase, here is the report on how background tasks and nested
   calls are managed using dramatiq.

   1. Overview of Background Tasks

   Background tasks in this project are defined using the @async_job decorator (found in
   task_runner/task_utils.py). This decorator wraps functions into dramatiq Actors, adding
   capabilities like database persistence, tracing, and custom error handling.

   Definition Example:

   python
     @async_job(
         actor_name=AsyncJobName.VideoAdGeneration,
         job_type=AsyncJobType.Task,
         persist=True,
         ...
     )
     async def generate_complete_video_ad(...):

   2. Managing Nested Background Tasks

   The system explicitly supports background tasks calling other background tasks ("nested" or
   "chained" tasks). This is primarily managed through the send_async_job utility function.

   Mechanisms:
   1. Fire-and-Forget (Async Triggering):
     •  A running task can enqueue a new task using await send_async_job(...).
     •  Example: In video_generation_prototype_task.py, the video generation task triggers an
        auto-fix task when a content violation occurs:
           ```python
           await send_async_job(
               auto_fix_scene_content_violation,
               scene_id=hook_scene.id,
           )
           ```
   •  The parent task does not automatically wait for the child task unless explicitly programmed
      to do so or if dependencies are used.

   2. Dependency Management (DAGs):
     •  The system has a built-in dependency mechanism. When sending a job, you can pass
        dependency_ids=[...].
     •  If a job has dependencies, send_async_job does not enqueue it to the broker immediately.
        Instead, it saves the job to the database with a Waiting status.
     •  Resolution: When a task completes, the AsyncJobStatusTracker middleware calls
        complete_async_job, which triggers check_and_schedule_waiting_jobs. This function checks if
         any waiting jobs now have all their dependencies met and enqueues them.

   3. Dramatiq Configuration & Middleware

   The AsyncJobStatusTracker middleware in task_runner/task_utils.py is central to this management:
   •  Before Enqueue: Sets up job IDs and tracing contexts.
   •  After Enqueue: Persists the job state (Enqueued) to the database.
   •  Before Process: Updates status to Running.
   •  After Process: Updates status to Completed or Failed and triggers any dependent jobs that
      were waiting.

   4. Summary
   •  Orchestration: Manual chaining via send_async_job is common.
   •  State Management: All job states are persisted in the database, allowing for tracking of
      complex workflows.
   •  Concurrency: Tasks can spawn parallel sub-tasks (e.g., generating multiple hook scenes in
      parallel), but these are often run as concurrent async functions within the same worker
      process, rather than spawning separate dramatiq tasks, unless send_async_job is explicitly
      used.

   This architecture allows for flexible, resilient workflows where tasks can spawn follow-up
   actions without blocking their own workers.

   5. Recommendation

   If you plan to implement complex workflows, use send_async_job with dependency_ids to ensure
   proper sequencing and error handling, rather than relying on implicit timing or side effects.

   I have completed the analysis and the report.
   I'll mark the final todo as completed.

   PLAN   Updated: 4 total (0 pending, 0 in progress, 4 completed)

⛬  The report on background task management in dramatiq is provided below.

   Report: Background Task Management & Nested Calls

   1. Task Definition (`@async_job`)
   Background tasks are defined using the @async_job decorator found in task_runner/task_utils.py.
   This decorator transforms an async function into a dramatiq Actor and integrates it with the
   system's tracking and persistence layer.

   2. Managing Nested Tasks
   The system manages tasks calling other tasks through the send_async_job wrapper. There are two
   primary patterns:

   •  Direct Chaining (Fire-and-Forget):
       A running task can enqueue a new task using await send_async_job(TargetActor, ...).
   •  Example: In video_generation_prototype_task.py, if a scene violates content safety, the
      generation task triggers a fix task:
           ```python
           await send_async_job(auto_fix_scene_content_violation, scene_id=...)
           ```
   •  The parent task continues execution immediately after enqueuing the child task.

   •  Dependency-Based Chaining:
       Tasks can be enqueued with dependency_ids=[parent_job_id].
   •  When send_async_job sees dependencies, it creates the job in the database with a Waiting
      status and does not send it to the broker.
   •  When a task completes, the AsyncJobStatusTracker middleware (in task_utils.py) calls
      check_and_schedule_waiting_jobs.
   •  This function finds any Waiting jobs that depend on the completed task. If all their
      dependencies are met, it finally enqueues them to dramatiq for execution.

   3. Infrastructure & Middleware
   •  `AsyncJobStatusTracker` Middleware: Intercepts message lifecycle events (enqueue, start,
      complete, fail). It ensures the database (AsyncJob table) is always in sync with the actual
      worker state.
   •  Persistence: All tasks decorated with @async_job(persist=True) are tracked in the database,
      allowing the system to verify completion status for dependency resolution.

   4. Conclusion
   The codebase uses a custom wrapper around dramatiq to support DAG-like dependencies and
   persistent state tracking. Nested tasks are fully supported either as parallel fire-and-forget
   jobs or as sequential dependent jobs managed by the completion hooks in task_utils.py.

