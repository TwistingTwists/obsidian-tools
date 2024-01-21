---
tags:
    - elixir
    - hacks
    - TIL
---


## Polymorphism in ecto 
https://gist.github.com/tmattio/4a9ce29b208c5c35f0b48179de3db681#file-schemas-ex
https://github.com/mathieuprog/polymorphic_embed
https://hexdocs.pm/ecto/polymorphic-associations-with-many-to-many.html
https://honesw.com/blog/polymorphic-associations-in-elixir
https://hashrocket.com/blog/posts/modeling-polymorphic-associations-in-a-relational-database

https://elixirforum.com/t/abstract-model-polymorphism/13236/4

## Fun reading 

- [ ] Elixir bucket moving average -  https://lucassifoni.info/blog/tipping-buckets/

## dependencies in elixir 

https://gist.github.com/marcandre/fd999ff3d9e5b9137380f15bab49f417
### libraries -- in elixir 

- [ ] https://github.com/mwhitworth/phoenix_better_table
- [ ] [[time]]
- [ ] https://elixirforum.com/t/qry-query-your-domain/60428
- [ ] https://github.com/spawnfest/fluffytrain
- [ ] https://hexdocs.pm/envio/backends.html

## Streaming Blogs

- [ ] https://benreinhart.com/blog/openai-streaming-elixir-phoenix/

## Becoming Elixir Specialist 

- [ ] Write a [compiler](#Compilers). It is just a mix Task 
- [ ] Learn [Beam Internals](#Beam_Internals)
- [ ] Learn [ERTS](https://blog.stenmans.org/theBeamBook/) -- [ERTS_Notes](#ERTS_Notes)
- [ ] Erlang in Anger
- [ ] Contribute to your favourite library 
- [ ] Be a Mentor 
- [ ] [[IO Lists]]

> It turns out that, coupled with my natural talent, a few hours of investment have made these “always wanted to be able to do it” abilities attainable. 

## Compilers
- [ ] Dispatchex - Protocols - implement pattern matching in protocols? 


## Beam_Internals
- [ ] What would crash the BEAM vm ?
- [ ] 


## ERTS_Notes
- [ ] 
## Protocols - implement for a boolean value?
https://github.com/elixir-nx/nx/blob/f90bb61138921183f3455ecfac5e8fbe14423b23/nx/lib/nx/lazy_container.ex#L80

## Json format logging - for datadog and newrelic
https://github.com/dscout/medea

## Unquote splicing and Macro 
```
defmacro foobar(name, things) do
  quote do
    def unquote(name)(*things) do
      ...
    end
  end
end
```

-- need to expand `*things`

https://github.com/ygunayer/craftgate-elixir-client/blob/cd0b3a93782b97f45d1bdd9efa8400e6f36ae834/lib/craftgate/adapter.ex#L143

### To check if :value is a key in fields passed!
```elixir
 def new(fields) when is_map(fields) and :erlang.is_map_key(:value, fields) do
      struct!(__MODULE__, fields)
    end
```

### To check what function is currently executing in elixir ?
    `__ENV__.function`
    `https://hexdocs.pm/elixir/1.12/Macro.Env.html`


### Writing Shell Scripts in Elixir
1. https://akoutmos.com/post/elixir-shell-scripts/
2. https://thoughtbot.com/blog/is-elixir-a-scripting-language


### for iterating over the string as binary (`buf`)

 `Enum.join(for(<<b <- buf>>, do: "#{b}"), ".")`

### SVG Charting in Elixir

```elixir
       <svg height={@height} width={@width}>
        <polyline fill="none" stroke="#0074d9" stroke-width="1" points={points} />
      </svg>
```

### Ecto shorts and composing ecto
https://learn-elixir.dev/blogs/creating-reusable-ecto-code


### using tags in database -- using postgres array column type

Source: https://fly.io/phoenix-files/tag-all-the-things/

```elixir
execute("create index books_genres_index on books using gin (genres);")
```

> Notice that the field type used is {:array, :string}. Also note, the last line executes an SQL statement to create a special GIN index on our array field. The GIN index will keep our look ups fast!


### building dynamic queries in ecto -
#### way 01 - use query functions like, by_id , by_author, by_published_date
#### way 02 - build query via `dynamic` in ecto

docs: https://hexdocs.pm/ecto/Ecto.Query.API.html#field/2
example:
https://github.com/fly-apps/phoenix-recipe-sync-url-with-form/blob/a81caa04e6382bd0b6b6c5bb545b6dfc030f428a/lib/form_url_recipe/blog.ex#L43

```elixir
 @doc """
  Returns the list of posts based on a search map.
  ## Examples
      iex> search_posts(%{"title" => "foo"})
      [%Post{}, ...]
  """
  def search_posts(params) do
    conditions =
      params
      |> Map.take(["title", "author"])
      |> Enum.reduce(true, fn
        {_key, value}, acc when value in ["", nil] ->
          acc

        {key, value}, acc ->
          atomized_key = String.to_atom(key)
          dynamic([p], field(p, ^atomized_key) == ^value and ^acc)
      end)

    query = from p in Post, where: ^conditions

    Repo.all(query)
  end
```

Advantage: it removed the need to write all those `by_user` , `by_...` functions manually.


### todo

1. https://github.com/elixirsydney/elixirsydney/issues/12
2. https://github.com/elixir-lang/elixir/blob/main/lib/elixir/lib/kernel/utils.ex#L209
   1. implementing something like this -- also look at dispatch.ex


>  writing your own functions is definitely better, a protocol wouldn't really buy you anything. However, you could do this in Elixir:

  ```elixir
  defmodule T do
    defstruct [:a]
    defoverridable [__struct__: 1, __struct__: 0]

    def __struct__() do
      ....
    end
    def __struct__(_attrs) do
      ...
    end
  end
  ```

  Essentially both `%T{}` and `%T{a: 1}` and `struct`/2desugar to calling` __struct__` fns, which means you can use them as constructors and implement type checks or anything you want, eg:

  ```elixir
  defmodule T do
    defstruct [:a]
    defoverridable [__struct__: 1, __struct__: 0]

    def __struct__() do
      %{__struct__: T}
    end
    def __struct__(%A{}) do
      ...
    end
    def __struct__(_) do
      ...
    end
  end
  ```
  I can't say i would recommend it though


```elixir
 defmacro defstruct(fields) do
    builder =
      case bootstrapped?(Enum) do
        true ->
          quote do
            case @enforce_keys do
              [] ->
                def __struct__(kv) do
                  Enum.reduce(kv, @__struct__, fn {key, val}, map ->
                    Map.replace!(map, key, val)
                  end)
                end

              _ ->
                def __struct__(kv) do
                  {map, keys} =
                    Enum.reduce(kv, {@__struct__, @enforce_keys}, fn {key, val}, {map, keys} ->
                      {Map.replace!(map, key, val), List.delete(keys, key)}
                    end)

                  case keys do
                    [] ->
                      map

                    _ ->
                      raise ArgumentError,
                            "the following keys must also be given when building " <>
                              "struct #{inspect(__MODULE__)}: #{inspect(keys)}"
                  end
                end
            end
          end

        false ->
          quote do
            _ = @enforce_keys

            def __struct__(kv) do
              :lists.foldl(fn {key, val}, acc -> Map.replace!(acc, key, val) end, @__struct__, kv)
            end
          end
      end
```





### graphql optimisation with elixir

https://www.erlang-solutions.com/blog/optimizing-graphql-with-dataloader/



## Metaprogramming Elixir

### Module attributes and their compilation efficiently
https://elixir-lang.org/getting-started/module-attributes.html



## Strings

### binary and bitstring
 if `is_binary(x)` is `true`, then `is_bitstring(x)` will be `true` as well


### BitStrings in Elixir

https://dev.to/aaronc81/elixirs-bitstrings-the-data-type-i-didnt-know-i-wanted-2842


### string interpolation

https://www.erlang.org/doc/man/io.html#fwrite-3
https://docs.python.org/3/library/string.html#format-specification-mini-language
https://www.erlang.org/doc/man/io.html#fwrite-1
https://elixirforum.com/t/how-to-convert-decimal-value-to-binary-value/27749/5
https://elixirforum.com/t/any-chance-we-get-format-for-string-interpolation/47512/6


## Deployment Elixir

### bare metal deployment
https://0x7f.dev/post/deploying-phoenix/
### nginx elixir config
https://pdgonzalez872.github.io/articles/012_20210311_deploy_phoenix_bare_metal.html



## BUGS 
1. When esbuild emits phoenix_html not found error 
https://github.com/phoenixframework/phoenix/issues/5491#issuecomment-1609269436