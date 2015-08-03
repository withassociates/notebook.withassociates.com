---
layout: post
title: "Phoenix: What’s Cool? - With Associates Notebook"
---

# Elixir & Phoenix: What’s Cool?

On July 1 2015 we spent lunch hour porting our in-house Spotify controller
API, DJ54B, from [Ruby](https://github.com/withassociates/dj54b-api) to
[Elixir](https://github.com/withassociates/dj54b-api-phoenix) using the
[Phoenix framework](http://www.phoenixframework.org/). Here’s what we learned…

## [Jamie](https://twitter.com/jgwhite):

Phoenix makes no bones about taking Rails’ best ideas as-is. The tooling,
conventions, routing, guides, all feel super familiar right away.

The router is a good example. It looks very much like Rails’ router but
improves upon it in subtle ways.

For example, rather than middleware stacks declared in separate config files,
in Phoenix we declare *pipelines* right in the router alongside the routes that
use them.

```elixir
pipeline :api do
  plug :accepts, ["json"]
  plug :cors
end

scope "/", Dj54bApiPhoenix do
  pipe_through :api

  get "/play", SpotifyController, :play
  # ...
end
```

This being a functional language, the pieces that make up a pipeline are
“just functions”. They take two structs, `connection` and `params`, and return a new
`connection` struct. The connection contains everything you need to know about
the current request/response. It’s pretty much like Rack’s `env` argument.

Here’s the plug we use to add whitelist CORS headers:

```elixir
def cors(conn, _params) do
  conn
  |> put_resp_header("access-control-allow-origin", "*")
  |> put_resp_header("access-control-allow-methods", "GET, PUT, HEAD, OPTIONS")
end
```

The `|>` operator probably looks a little weird at first blush. Think of just
like `|` in a shell script, or like this:

```elixir
# This:
put_resp_header(conn, "access-control-allow-origin", "*")
# Is the same as:
conn |> put_resp_header("accepts-controller-allow-origin", "*")
```

The thing on the left hand side of the pipe operator gets inserted as the
first argument or the function call on the right. A simple rearrangment,
but makes it much easier to read chains of transformations.
