# fact check

[Midje](https://github.com/marick/Midje)-like testing for Julia.

![Example output](http://img594.imageshack.us/img594/8189/screenshot20130329at222.png)

### Why?

[Base.Test](https://github.com/JuliaLang/julia/blob/master/base/test.jl)
seemed to be the only option in the way of Julia testing, and I wasn't a
huge fan. Midje is a simple and powerful testing framework written for
Clojure, and so I sought to (at least partially) recreate it. This is a
work in progress.

### Installation

```jl
julia> Pkg.add("FactCheck")
```

### Usage

```jl
using FactCheck
```

The top-level function `facts` describes the scope of your tests and
does the setup required by the test runner. It can be called with or
without a description:

```jl
facts("With a description") do
    # ...
end

facts() do
    # ...
end
```

Inside of the function passed to `facts`, a fact can be asserted using
the `@fact` macro, or `@fact_throws` if you're asserting a thrown
exception.

```jl
facts("Simple facts") do

    # expression => assertion
    @fact 1 => 1

    @fact_throws error()

end
```

Related facts can also be grouped inside of a `context`:

```jl
facts("Simple facts") do

    context("numbers are themselves") do
        @fact 1 => 1
        @fact 2 => 2
        @fact 3 => 3
    end

end
```

The symbol `=>` is used as an assertion more general than `==`. Each
fact will be transformed into a test, the type of which depends on the
value to the right of the `=>`. (We'll call that value the assertion.)

An assertion can take two forms:

```jl
# If the assertion is a function, it will be called on the expression
# to determine whether or not the fact holds.
@fact 2 => iseven

# Otherwise, the fact holds if the expression is `==` to the assertion.
@fact [1,2,3] => [1,2,3]
```

As the function-assertion form is rather convenient and reads nicely,
a number of helper assertion functions are provided.

```jl
facts("Using helper assertions") do

    context("some helpers operate on values") do
        @fact false => anything
        @fact 1 => not(2)
    end

    context("... or on functions") do
        @fact 2 => not(isodd)
    end

end
```

These can be found at the bottom of [FactCheck.jl](https://github.com/zachallaun/FactCheck.jl/blob/master/src/FactCheck.jl).

### Contributing

I'm incredibly open to contributions. The code base is quite small and
(I think) well documented. I'm also happy to explain any decisions
I've made, with the understanding that they may have been uninformed.
