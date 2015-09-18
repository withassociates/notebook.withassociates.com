---
layout: post
title: Lunch with Phoenix
---

A couple of Ruby developers talk about [Phoenix][4].


**Jamie**:

> So we took a lunch break to play around with [Phoenix][4].

**Calum**:

> That, we did.

**Jamie**:

> Why did we do it?

**Calum**:

> It's an interesting one. It’s very much the new hotness, isn't it?
> So we thought we’d give it a bash, get a rudimentary grasp of how it works.

**Jamie**:

> In the studio, we have this [API which is kind of a bridge to Spotify][0]
> running on a Mac Mini. We'd already built a server for it using [Grape][1], a
> DSL-thing for [Rack][2]. A bit like [Sinatra][3] but purely for generating
> APIs.

> So our API was written with Grape, in Ruby. It wasn’t super slow… but it
> wasn’t fast by any means. It did fall over quite a lot.

**Calum**:

> Yeah, there were times when I couldn't change a song fast enough.

**Jamie**:

> So we thought we'd have a look at replicating the API in Phoenix.
> It seemed like a good place to start because we knew
> exactly how the API should look.

**Calum**:

> It could potentially be bashed out in an hour.

**Jamie**:

> Yeah. So… what were your first impressions?

**Calum**:

> It’s nice. I like it a lot.

**Jamie**:

> Nice website.

**Calum**:

> The install process was painless. It's just a case of installing
> [Elixir][5], [Erlang][6], then Phoenix itself. The website explains it
> super clearly, and has pretty in-depth documentation.

**Jamie**:

> The website’s made with [readme.io][7].

**Calum**:

> What’s that?

**Jamie**:

> It’s like a service for building these sorts of websites. It’s quite
> neat, maybe we should use it for [Slices][8]. Oh, it’s quite
> expensive: $14 a month.

**Calum**:

> That’s not too bad!

**Jamie**:

> Well if it’s an open source project though, who’s paying? That’s
> the funny thing about it.

**Calum**:

> You are. Literally you, Jamie.

**Jamie**:

> Darn. So yeah, you can use [Homebrew][9] to install Erlang and
> Elixir and then… how do you install Phoenix itself?

**Calum**:

> You use Hex, the package manager.

**Jamie**:

> Oh yeah.

```sh
mix archive.install https://github.com/phoenixframework/phoenix/releases/download/v0.16.1/phoenix_new-0.16.1.ez
```

> You point it at the URL of the most recent Phoenix release.

**Calum**:

> But that’s it.

**Jamie**:

> It doesn’t live on a repository like [RubyGems][11]. Although I think
> there is one for Hex, but you can also point this thing directly at
> any URL where a Hex archive lives.
> So Phoenix is well documented, and it’s a lot like [Rails][12] - that’s
> kind of the key to its burgeoning popularity.

**Calum**:

> The first big Rails-esque framework. Plus it’s fast and you can deploy
> straight to Heroku.

**Jamie**:

> Yep. Phoenix has a command line interface just like Rails…

**Calum**:

> Simple generators.

**Jamie**:

> To make a new project:

```sh
mix phoenix.new dj54b_api_phoenix
```

> In our case, we tell it not to include [Brunch][13] because we’re building
> an API and don’t want all the included Javascript.

```sh
mix phoenix.new dj54b_api_phoenix --no-brunch
```

**Calum**:

> I guess Brunch is the Phoenix solution to the Rails asset pipeline.

**Jamie**:

> Running the generated app has that familiar static “hello” page.

**Calum**:

> “Welcome to Phoenix, babes.”

**Jamie**:

> And then [Better Errors][14], is a version of that built into it?

**Calum**:

> Yeah, and it live reloads as well.

**Jamie**:

> So it injects its own bit of JavaScript into any HTML pages.

**Calum**:

> Which is cute as heck.

**Jamie**:

> So straight off the bat it feels just like Rails, except… it takes
> all the stuff the community’s learned in Rails and adds the little bits of
> finesse.

> I felt like it took us a little while to figure out how to render something
> *other* than HTML.

**Calum**:

> Yeah, how did we get caught up with that?

**Jamie**:

> So looking at the controller, something interesting is that the directory
> that contains what would be called your 'app' in rails is called *web*
> in Phoenix.

```
.
├── README.md
├── config
├── lib
├── mix.exs
├── test
└── web
```

> …on the basis that your app may have other parts?

**Calum**:

> And that’s the only folder we touched.

**Jamie**:

> App, sorry *web*, it’s got a router, it looks just like
> a Rails router. It’s got controllers, they look just like Rails controllers.

```
.
├── README.md
├── config
│   ├── config.exs
│   ├── dev.exs
│   ├── prod.exs
│   ├── prod.secret.exs
│   └── test.exs
├── lib
│   ├── dj54b_api_phoenix
│   └── dj54b_api_phoenix.ex
├── mix.exs
├── test
│   ├── channels
│   ├── controllers
│   ├── models
│   ├── support
│   ├── test_helper.exs
│   └── views
└── web
    ├── channels
    ├── controllers
    ├── models
    ├── router.ex
    ├── templates
    ├── views
    └── web.ex
```

**Calum**:

> It removes a lot of Rails' opinionated feel. A lot of what Rails assumes
> about your app is not here.

**Jamie**:

> Fewer magic strings is how I’d put that.
> In Rails, it’s like "here’s a magic string and we will magically
> pluralise it for you", and here it’s like "I’m going to give you the literal
> module constant to use and a symbol and the action to use on that module."

**Calum**:

> Yup. A bit less hand-holdy.

**Jamie**:

> Sometimes you do end up on a yak shave with Rails’ magic.

> Phoenix has controllers with actions that, by default, render a template -
> plus like Rails does - but they come with a view module as well. I think
> you put your helpers in there.

**Calum**:

> Right, we didn’t touch on views at all. No need to with just an API.

**Jamie**:

> No. But to render JSON out, you just use the `json` method which comes
> as part of the Phoenix controller base module.

```elixir
def info(conn, _params) do
  json(conn, spotify_info)
end
```

> I guess something that’s slightly confusing about it is that every
> action takes two arguments.

**Calum**:

> The connection itself…

**Jamie**:

> …And the params object, which we never actually touched.

**Calum**:

> No.

**Jamie**:

> You can’t mutate the connection because it’s Erlang, which doesn't allow
> you to mutate objects. So you create a *new* connection and return that
> from the action.

**Calum**:

> What’s that weird Erlang operator? `|>`?

**Jamie**:

> Ah, the pipeline thing. In Rails, you receive a request, set a
> load of instance variables and then something happens afterwards but you
> don’t return anything from the action. In Phoenix, you receive the
> connection, but produce a *new* connection with the changes you want
> and return it.

**Calum**:

> Right.

**Jamie**:

> So the `json` method will copy the connection and produce a new
> connection with the `Content-Type: application/json` header set on
> it and the body set to whatever data you pass into it.

**Calum**:

> Sure.

**Jamie**:

> I think it’s quite nice that there’s nothing outside of this function.
> It’s a bit like… what’s that book? [Thinking Fast and Slow][15]. One of
> the things in that book is “What You See is All There Is”, referring to
> one of the theories in that but here, the arguments you receive and what
> you return - that’s all you need to know about. There’s nothing coming from
> anywhere else apart from the functions, available to the module.

> I think we struggled to find that `json` function, initially.

**Calum**:

> It seems fairly self explanatory. Perhaps it seemed too easy.

**Jamie**:

> Yeah, it’s just a matter of finding it in the docs. It lives in the
> [Phoenix.Controller][16] module.

> So, what did you think of the pipeline operators then?
> These things: `|>`

```elixir
conn
|> halt
|> put_status(200)
|> json(peace_info)
```

**Calum**:

> I like that syntax. It forces you to keep things clear.

**Jamie**:

> Yeah. It’s almost exactly like the [Shell `|` operator][17]
> but rather than it directing data somewhere, it takes the result of the
> expression on the left and uses it as the first argument to the
> expression on the right.

> So `conn` is going to be the first argument to `halt`…

```elixir
conn
|> halt # halt(conn)
```

`halt(conn)` is going to the be first argument to `put_status`,
before the `200`…

```elixir
conn
|> halt
|> put_status(200) # put_status(halt(conn), 200)
```

> And `put_status(halt(conn), 200)` is going the be the first argument
> to `json` in front of `peace_info`.

```elixir
conn
|> halt
|> put_status(200)
|> json(peace_info) # json(put_status(halt(conn), 200), peace_info)
```

> It’s a good way to break up what would be a big long nested
> function invocation.

> There’s only really one bit of the funky destructuring assignment
> (pattern matching) we used and that’s here:

```elixir
case File.stat(peacefile_path) do
  {:ok,stat} ->
    # ...
  _ ->
    # ...
end
```

> It’s a switch or a case statement. There are no statements in
> this language but it’s a case expression. `File.stat...` gets
> fed into it and you can both match the result and pull out variables
> simultaneously.

> So the thing `File.stat` will return will be a tuple. If the
> first item in the tuple is the symbol `:ok`, it’ll pull out
> `stat` for you. In Erlang that’s an old and deeply optimised part of the
> language - it’s super fast to do it that way, so it’s always preferable.

**Calum**:

> And the `_` means “anything else”.

**Jamie**:

> Yeah, an ignored variable.

> The project genuinely came together over a lunch time.

**Calum**:

> Yeah, start to finish.

**Jamie**:

> We even got it running on the machine. I think it’s more dependable
> than the Ruby version. Talking to Spotify over AppleScript, if Spotify
> stopped talking to it, that would chew up a process, we’d run out of
> processes and the whole thing would just stop working.
> And that’s not really a knock against Ruby or Rack or Grape, it’s
> just that…

**Calum**:

> There are more suitable languages for that sort of thing.

**Jamie**:

> Yeah. Especially when you know you’re going to have to talk to
> other services and they might fall apart on you. This just seems
> to handle it very well, it takes it in its stride.

**Calum**:

> I’d be interested in seeing the sort of things that people are making
> with Phoenix. If there’s any high traffic sites using it
> already or…

**Jamie**:

> WhatsApp is built on Erlang, but not Elixir. But then, the idea with Elixir,
> because Elixir compiles to Erlang (or [BEAM][18], the Erlang virtual
> machine) bytecode there should be no slowdown. There’s no interpretation
> step, it’s all compiled code.

> [Rik Lomas][19] is building [SuperHi][20] using Phoenix, swapping
> over from Rails to Phoenix and getting good results there. I’m not sure
> what else there really is to say about it apart from that it’s clearly
> been designed to look friendly to Rails developers, to feel familiar.

**Calum**:

> Which I reckon is a good idea. Rails made full-stack web development a lot
> more accessible to newcomers, but it's been a little lonely at the top
> there.

**Jamie**:

> There’s always been [Django][21], and PHP has Laravel, but none of
> those really solve this massive concurrency issue and stay friendly,
> whereas this is quite a promising way to do that.

**Calum**:

> I'm looking forward to having a deeper dive into it. We didn’t even touch
> the model stuff, [Ecto][22].

**Jamie**:

> We didn’t write any tests…

**Calum**:

> This is going on the internet, Jamie. We could be killed for saying that.

**Jamie**:

> I got a bit confused by the config, setting up the port for the
> production environment, but I got into the IRC channel and got
> an answer from Phoenix's author within five minutes.

**Calum**:

> Was it the correct answer?

**Jamie**:

> Yeah, I was being an idiot. I was just misunderstanding how the thing
> worked. It was because the config has exactly the same config
> layout as Rails, where it has a base config and then specific
> configs for each environment. So you load config first
> and then development, or production, or test.

> I guess it’s also interesting that it’s got the equivalent of a
> Gemfile in `mix.exs`.

**Calum**:

> And `mix.lock`.

**Jamie**:

> I think what’s nice about these files is that they are also
> written in Elixir, they’re not a YAML or a DSL or something like that.

**Calum**:

> The syntax is fairly close to Ruby as well. There’s nothing done
> too differently.

**Jamie**:

> Yeah, I tripped on needing the `do`…

**Calum**:

> Ah yeah, on a method.

**Jamie**:

> On a function definition, yeah, a few times. But that’s more
> consistent than Ruby because you always need `do`.

**Calum**:

> Yeah, on everything else there’s a `do` and `end`.

**Jamie**:

> You need `do` on `if` expressions as well.

[0]: github.com/withassociates/dj54b-api-phoenix
[1]: https://github.com/ruby-grape/grape
[2]: http://rack.github.io/
[3]: http://www.sinatrarb.com/
[4]: http://www.phoenixframework.org/
[5]: http://elixir-lang.org/
[6]: http://www.erlang.org/
[7]: http://readme.io/
[8]: http://slices.withassociates.com/
[9]: http://brew.sh/
[10]: https://hex.pm/
[11]: https://rubygems.org/
[12]: http://rubyonrails.org/
[13]: http://brunch.io/
[14]: https://github.com/charliesome/better_errors
[15]: https://en.wikipedia.org/wiki/Thinking,_Fast_and_Slow
[16]: http://hexdocs.pm/phoenix/Phoenix.Controller.html
[17]: http://linuxcommand.org/lts0060.php#pipes
[18]: https://beamcommunity.github.io/
[19]: https://twitter.com/riklomas
[20]: http://www.superhi.com/
[21]: https://www.djangoproject.com/
[22]: https://github.com/elixir-lang/ecto
