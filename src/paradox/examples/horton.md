# Responsibility Tracking (Horton)

One of the issues that is commonly raised with capabilities is being able to track the usage of a capability, and ensure it is correlated with an identity or "security principal" during operation.

The [Horton](https://www.usenix.org/legacy/event/hotsec07/tech/full_papers/miller/miller.pdf) protocol is a way to ensure capabilities can be used with attribution.

Horton is used for responsibility tracking.  Using Horton, stubs and proxies are used in conjunction with dynamic sealing so that a proxy can always establish the provenance of a request as coming from a particular principal.

To the end user, most of this is transparent.  Scala is statically typed, instances of proxies must be created at compile time, by the trait's singleton object.  After that, proxies are generated by establishing the `from` and `to` principals associated with the objects.

@@snip [Main.scala]($examples$/horton/Main.scala) { #main }

The output from running `Main` is the following text:

```
Alice said:
> I ask Bob to:
> > foo/1
Carol said:
> Alice asks me to:
> > meet Bob
Bob said:
> Alice asks me to:
> > foo/1
Bob said:
> I ask Carol to:
> > hi/1
Carol said:
> Bob asks me to:
> > meet Carol
Carol said:
> Bob asks me to:
> > hi/1
hi
```

Because Horton uses stubs and proxies internally, the process of establishing a relationship and communicating messages involves several steps.  These steps are internal to the operation, but are vital to ensure that the relationship between principals cannot be faked.

The [slides](http://www.erights.org/elib/capability/horton/horton-talk.pdf) for Horton are a useful guide for going through the steps.

The implementation is as follows:

@@snip [package.scala]($examples$/horton/package.scala) 

@@snip [Horton.scala]($examples$/horton/Horton.scala) { #horton }