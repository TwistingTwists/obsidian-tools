
## Try out 

```elixir
defmodule MyApp.Query do

@moduledoc false

  

@doc """

Make JSON paths more accessible

  

## Example

  

from(r in Resource, where: path(r, meter.manufacturer_id) == ^id)

"""

defmacro path(column, fields) do

# Make json paths more easily accessible.

compacted =

Macro.postwalk(fields, fn

{r, _, nil} -> r

{:., _, [a, b]} -> [a, b]

{r, _, []} -> r

r -> r

end)

  

[root | fields] = List.flatten(compacted)

  

key =

quote do

unquote(column).unquote(root)

end

  

quote do

fragment(unquote("?->>'#{Enum.join(fields, "'->>'")}'"), unquote(key))

end

end

end
```

- [ ] Macro.postwalk 

- [ ] rabbitmq has supervisor2 -- which restarts a child with a delay - an intrinsic mode of restart


```elixir

# https://github.com/lawik/fedex/blob/368be04b26ab068b8e66161204c42d5852a52ba9/lib/fedex/store/ets.ex

Sample ets usage.

```

### Networks in erlang 

- [ ] It's about guarantees fred herbert
- [ ] persistent connections with gen state m - andrea leopardi 
- [ ] thousand island elixir libray 
- [ ] computer networks by A Tanenbaum 
- [ ] protohackers in elixir - Youtube series 


## Libraries to read 
https://hexdocs.pm/envio/backends.html
tus implementations


### {lib} - Castle 

###### hot code reload
* appup scripts -- dev's responsibility to test it for hot code upgrade
* relup scripts 
