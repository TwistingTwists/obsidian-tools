---
tags:
  - draft
  - blogs
---
[Outside Elixir = Sasa Juric ](https://www.theerlangelist.com/article/outside_elixir)

* Ports , Monitors, Spawns 
	* Using Task, GenServer, etc 
* Rustler 
* Zigler


[Source ](https://gist.github.com/runlevel5/eb8bd745d0af7508ccbc5a20574a4383)
```elixir

defmodule Uber.Task do
  # this module takes advantage of `Task` to wrap around
  # the Port process, it does extra processing in messages
  # aggregation and waiting for exit_status message.
  #
  # example:
  #    cmd = "ls /"
  #    Uber.Task.run_async(cmd) |> Uber.Task.get_result()
  #
  def run_async(cmd) do
    Task.async(fn ->
      port = Port.open({:spawn, cmd}, [:stderr_to_stdout, :binary, :exit_status])
      loop(port, %{})
    end)
  end

  # get the results of a task pid
  def get_result(task) do
    Task.await(task)
  end

  defp loop(port, results) do
    receive do
      {port, {:data, data}} ->
        # aggregate all messages into %{data: data}
        data = data <> Map.get(results, :data, "")
        loop(port, Map.put(results, :data, data))
      {^port, {:exit_status, 0}} ->
        # when exit_status 0 is detected, terminate the loop
        # and return success tuple
        {:ok, results}
      {^port, {:exit_status, 1}} ->
        # when exit_status 1 is detected, terminate the loop
        # and return error tuple
        {:error, results}
    after 10000 ->
      # if the cmd hangs, we want to ensure this task process
      # is gracefully die out
      {:error, :timeout}
    end
  end

end

```
