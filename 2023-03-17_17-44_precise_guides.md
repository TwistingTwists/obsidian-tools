---
tags:
  - guides
  -
---



## [elixir] Uploading to S3 via presigned url.

1. https://elixirforum.com/t/presigned-url-with-post-method-not-allowed/30350/2


## neat-pattern


```elixir

  def keys(keys) do
    keys = List.wrap(keys)

    if Enum.all?(keys, &is_atom/1) do
      {:ok, keys}
    else
      {:error, "Expected a list of atoms for the identity keys"}
    end
  end
  ```
  don't need to write two clauses to pattern match against nil keys!
