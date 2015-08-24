---
layout: post
title: Lunch with Phoenix
---

**Jamie**:

> So we took a lunch break to play with [Phoenix][4].

**Calum**:

> Yep

**Jamie**:

> Why did we do it?

**Calum**:

> Don’t know, it just seemed interesting. It was very much… it’s like
> the new hotness innit? So we thought we’d give it a bash.

**Jamie**:

> So we have this [API which is kind of a bridge to Spotify][0] running
> on a Mac Mini, and we already built a server for it using [Grape][1]
> which is like a DSL-thing for [Rack][2]. A bit like [Sinatra][3] but
> just for generating APIs.

> So we built it with Grape, in Ruby. It wasn’t super slow… but it
> wasn’t fast by any means. It did fall over quite a lot.

**Calum**:

> Yep

**Jamie**:

> So we had a look at Phoenix. So… it was good because we knew
> exactly the API should look like.

**Calum**:

> It could be bashed out in an hour.

**Jamie**:

> Yeah. So… what were your first impressions?

**Calum**:

> It’s nice.

**Jamie**:

> Nice website.

**Calum**:

> Installing Phoenix seems pretty painless. What do you need to do:
> Just get [Elixir][5], [Erlang][6], Phoenix. Yeah, it was pretty
> painless, good website, pretty in-depth docs.

**Jamie**:

> The website’s made with [readme.io][7].

**Calum**:

> What’s that?

**Jamie**:

> It’s like a service for building these sorts of websites. It’s quite
> neat, maybe we should use it for [Slices][8]. PAUSE. Oh, it’s quite
> expensive: $14 a month.

**Calum**:

> That’s alright. It’s better than what we’ve currently got.

**Jamie**:

> Well if it’s an open source project though, who’s paying? That’s
> the funny thing about it.

**Calum**:

> You are.

**Jamie**:

> So yeah, you have to, what is it, like use [Homebrew][9] to install
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

**Calum**:

> What version is it on? 0.16. I imagine… they’re looking at 1.0 soon
> isn’t it, that’s the next one.

**Jamie**:

> So it’s well documented and it’s a lot like [Rails][12] and that’s
> kind of the key to its burgeoning popularity.

**Calum**:

> And it’s fast and you can deploy to Heroku. Is that right?

**Jamie**:

> Yeah, you can deploy to Heroku. So you do… the command line is just
> like Rails…

**Calum**:

> Yeah, good generators.

**Jamie**:

> Yeah, so you do

```sh
mix phoenix.new dj54b_api_phoenix
```

> and that’s making a new Phoenix project, and you have to remember to
> remember to tell it not to include [Brunch][13] because we’re building
> an API and we don’t want all that JavaScript

```sh
mix phoenix.new dj54b_api_phoenix --no-brunch
```

**Calum**:

> Right yeah, that’s the asset pipeline isn’t it, or sort of asset
> manager.

**Jamie**:

> And then we ran it and it had that familiar like static “hello” page

**Calum**:

> Yeah, “Welcome to Phoenix”

**Jamie**:

> And then, like, what’s it called, [Better Errors][14], is that built
> into it?

**Calum**:

> Yeah, and it live reloads as well.

**Jamie**:

> Oh yeah, live reload.

**Calum**:

> That’s amazing.

**Jamie**:

> So it injects its own bit of JavaScript into any HTML pages.

**Calum**:

> Which is cute as heck.

**Jamie**:

> So straight off the bat it feels just like Rails, except… refined
> like taking all the stuff the community’s learned in Rails and
> adding the little bits of finesse.

> And then, I felt like it took us a little while to figure out how
> to render something *other* than HTML.

**Calum**:

> Yeah, how did we get caught up with that?

**Jamie**:

> So looking at the controller, well I suppose something interesting
> as well is that it’s uh… the directory that contains your… what
> would be called app in rails is called *web* in Phoenix.

```
.
├── README.md
├── config
├── lib
├── mix.exs
├── test
└── web
```

> …on the basis that your app might have other parts?

**Calum**:

> And that’s the only folder we touched in the whole project.

**Jamie**:

> Yeah, so app, sorry *web*, it’s got a router, it looks just like
> Rails, it’s got controller, they look just like Rails.

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

> It’s got… a lot less stuff is inferred. It’s not got that Rails-esque
> sort of… opinionated feel.

**Jamie**:

> Fewer magic strings is I think how I’d put that.

**Calum**:

> Yeah. You can put that I said that.

**Jamie**:

> In Rails, it’d be like here’s a magic string and we will magically
> pluralise it for you and all that stuff and here it’s like I’m going
> to give you the literal module constant to use and a symbol and the
> action to use on that module.

**Calum**:

> Yup

**Jamie**:

> Which is… sometimes you do end up on a yak shave with Rails’ magic.

> Yeah, you’ve got controllers and they’ve got actions and by default
> they will render a template just like Rails does and they come with
> a view… module? as well. So you… I think you put your helpers in
> there, I get the impression that’s how it works.

**Calum**:

> Right yeah, we didn’t touch on views at all.

**Jamie**:

> No. But to render JSON out, you just need to use the `json`
> method which comes as part of the err… Phoenix controller
> sort of base module.

```elixir
def info(conn, _params) do
  json(conn, spotify_info)
end
```

> I guess something that’s slightly confusing about it is that every
> action takes two arguments.

**Calum**:

> The connection itself

**Jamie**:

> And the params object, which we never actually touched.

**Calum**:

> No

**Jamie**:

> And then you… you can’t mutate the connection because it’s erlang
> and you can’t mutate objects so you create a *new* connection and
> you return that from the action.

**Calum**:

> So you sort of… what’s that weird Erlang mutation operator? Is that
> what that is?

**Jamie**:

> Ah no that’s the pipeline thing. Yeah so whereas in Rails you receive
> a request, set a load of instance variables and then, like, something
> happens afterwards but you don’t return anything from the action,
> in Phoenix you receive the connection, you produce a *new* connection
> with the changes you want and you return it.

**Calum**:

> Right.

**Jamie**:

> So when do `json` that, like, copies the connection, produces a new
> connection with the `Content-Type: application/json` header set on
> it and the body set to whatever data you pass into it, serialised
> I assume. Or maybe it’s actually that it will… maybe it’s just like
> setting up a load of config for whatever happens which actually goes
> “okay, now I’m going to construct the response”

**Calum**:

> Sure.

**Jamie**:

> But that’s like… I think it’s quite nice that there’s nothing outside
> of this function. Like, this is all that comes in and what you return
> is all there is. It’s a bit like… what’s that book? [Thinking Fast
> and Slow][15]. One of the things in that book is “What You See is
> All There Is”, referring to one of the theories in that but here
> it’s like, the arguments you receive and the thing you return,
> that’s all you need to know about. There’s nothing coming from
> anywhere else apart from the methods, sorry functions, available
> to the module.

> I think we struggled to find that `json` function but looking at
> it now.

**Calum**:

> It seems fairly self explanatory.

**Jamie**:

> Yeah, it’s just a matter of finding it in the docs. It lives in
> [Phoenix.Controller][16] the module. So if you look in there you
> know everything you can do, so that’s pretty cool.

> So, what did you think of the pipeline operators then?
> These things: `|>`

```elixir
conn
|> halt
|> put_status(200)
|> json(peace_info)
```

**Calum**:

> It’s quite nice, I like the syntax. It’s sort of like… I don’t know,
> it almost forces you to keep things very simple.

**Jamie**:

> Yeah. It’s almost exactly like the [Shell `|` operator][17]
> except that rather than it directing standard in somewhere
> it takes the result of the expression on the left and puts
> it as the first argument to that expression on the right.

> So `conn` is going to the first argument to `halt`…

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
> method, sorry, function invocation.

> There’s only really one bit of the funky destructuring assignment
> (pattern matching) we used in this and that’s here:

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
> fed into it and what’s cool is that you can both match the result
> and pull out variables at the same time.

> So the thing `File.stat` will return will be a tuple and if the
> first item in the tuple is the symbol `:ok` then it’ll pull out
> `stat` for you. Apparently in Erlang that’s like, an old and
> deeply optimised part of the language. So apparently it’s super
> fast to do it that way, so it’s always preferable.

**Calum**:

> And the `_` means “anything else”.

**Jamie**:

> Yeah, an ignored variable.

> It genuinely did come together over a lunch time.

**Calum**:

> Yeah.

**Jamie**:

> Got it running on the machine. And, I think it’s more dependable
> than the Ruby version. Cos the Ruby version would, if when it was
> talking to Spotify over AppleScript, if Spotify stopped talking to
> it, that would chew up a process, we’d run out of processes and the
> whole thing would just stop working, and hang.
> And that’s not really a knock against Ruby or Rack or Grape, it’s
> just that…

**Calum**:

> There are more suitable languages for that sort of thing…

**Jamie**:

> Yeah. Especially when you know you’re going to have to talk to
> other services and they might fall apart on you. This just seems
> to handle it very well, it takes it in its stride.

**Calum**:

> Fast as you like that, innit?

**Jamie**:

> It’s freaky fast.

**Calum**:

> I’d be interested in seeing the sort of things that people are making
> with the web. Like if there’s any, like, high traffic sites using it
> already or…

**Jamie**:

> Yeah, well I mean WhatsApp is built on Erlang, but not Elixir. But then,
> the idea with Elixir, because Elixir compiles to Erlang (or [BEAM][18],
> the Erlang virtual machine) bytecode there should be no slowdown.
> There’s no interpretation step, it’s all compiled code.

> So [Rik Lomas][19] is building [SuperHi][20] using Phoenix, swapping
> out Rails for Phoenix and getting good results there. I’m not sure
> what else there really is to say about it apart from that it’s clearly
> been designed to look friendly to Rails developers, to feel familiar.

**Calum**:

> Which is a good idea, because it’s about time there was an alternative
> really.

**Jamie**:

> Yeah, I mean there’s always been like [Django][21] and stuff like that
> and like the PHP space has Laravel and stuff like that, but none of
> those really solve this massive concurrency issue and stay friendly,
> whereas this is quite a promising way to do that.

**Calum**:

> I need to dive deep into it, we didn’t touch the model stuff,
> [Ecto][22].

**Jamie**:

> Didn’t write any tests…

**Calum**:

> No we did, we wrote loads of tests. More tests than software ;)

**Jamie**:

> I got a bit confused by the config, setting up the port for the
> production environment, but I got into the IRC channel and got
> an answer from the author within about five minutes.

**Calum**:

> Oh cool.

**Jamie**:

> It was wicked.

**Calum**:

> Was it the correct answer?

**Jamie**:

> Yeah, I was being an idiot. I was just misunderstanding how the thing
> worked. It was cos the config, it’s got exactly the same config
> layout as Rails where it’s kind of a base config and then additive
> config for each subsequent environment. So you load config first
> and then dev, or prod, or test.

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

> On a function definition, yeah, a few times. But that’s actually
> more consistent than Ruby because you always need `do`.

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
