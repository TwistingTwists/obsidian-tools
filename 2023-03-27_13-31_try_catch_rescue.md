---
tags:
  - blog draft
---


![Exception.format](/Users/abhishektripathi/Downloads/dev_work/learnings/assets/imgs/2023-03-27-13-34-01.png)


```elixir
defp do_tasks([fun | rest], resource, state, acc) do
    taskstate = Map.get(state.state, resource, nil)

    task =
      Task.Supervisor.async_nolink(Tnest.Scheduler.TaskSupervisor, fn ->
        try do
          fun.(resource, taskstate)
        catch
          a, b ->
            Logger.error(Exception.format(a, b, __STACKTRACE__))
            {:error, {a, b}}
        end
      end)

    {:ok, ms_to_next_occurrence} = Model.Schedule.next(state.schedule)
```
