---
tags:
  - elixir
  - hacks
  - TIL
  - peer-learning
---

###### debounce events in genserver

	file: https://github.com/zachdaniel/aoc/blob/8847f3b7513f13bafcc646a0966e107cd78f3f0f/lib/day/watcher.ex

```elixir
  defp wait_for_file_to_stop_changing(path) do
    # this is only kind of a debounce
    receive do
      {:file_event, _, {^path, _events}} ->
        wait_for_file_to_stop_changing(path)
    after
      @file_change_debounce_ms ->
        :ok
    end
  end


  def handle_info({:file_event, _, {path, _events}}, state) do
    wait_for_file_to_stop_changing(path)
	... 
end
```



