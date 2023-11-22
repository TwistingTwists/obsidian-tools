---
tags:
  - elixir
  - hacks
  - TIL
  - peer-learning
---

###### You can use functions in mix.exs + reenable task if run once

	file: mix.exs

```elixir
fn _ -> Mix.Task.reenable("npm") end
```



###### don't encode elixir to json again n again

	file: apps/shared/lib/conditional_json_encoder.ex

```elixir
def encode_to_iodata!({:skip_json_encoding, data}) when is_binary(data) do
    Logger.info("Skipping JSON encode step (payload is already JSON encoded)")
    data
end
```
