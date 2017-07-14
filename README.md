bcrypt
======

[![Build Status](https://travis-ci.org/erlangpack/bcrypt.svg?branch=master)](https://travis-ci.org/erlangpack/bcrypt)
[![Hex pm](http://img.shields.io/hexpm/v/bcrypt.svg?style=flat)](https://hex.pm/packages/bcrypt)

erlang-bcrypt is a wrapper around the OpenBSD Blowfish password hashing
algorithm, as described in 
[A Future-Adaptable Password Scheme](http://www.openbsd.org/papers/bcrypt-paper.ps) 
by Niels Provos and David Mazieres.

Basic build instructions
------------------------

1. Build it (project uses rebar, but I’ve included a Makefile):

    ```shell
    make
    ```

2. Run it (simple way, starting sasl, crypto and bcrypt):

    ```shell
    erl -pa ebin -boot start_sasl -s crypto -s bcrypt
    ```

Basic usage instructions
------------------------

Hash a password using a salt with the default number of rounds:

```erlang
1> {ok, Salt} = bcrypt:gen_salt().
{ok,"$2a$12$sSS8Eg.ovVzaHzi1nUHYK."}
2> {ok, Hash} = bcrypt:hashpw("foo", Salt).
{ok,"$2a$12$sSS8Eg.ovVzaHzi1nUHYK.HbUIOdlQI0iS22Q5rd5z.JVVYH6sfm6"}
```

Verify the password:

```erlang
3> {ok, Hash} =:= bcrypt:hashpw("foo", Hash).
true
4> {ok, Hash} =:= bcrypt:hashpw("bar", Hash).
false
````

Configuration
-------------

The bcrypt application is configured by changing values in the
application's environment:

`default_log_rounds`
  Sets the default number of rounds which define the complexity of the
  hash function. Defaults to ``12``.

`mechanism`
  Specifies whether to use the NIF implementation (`'nif'`) or a
  pool of port programs (`'port'`). Defaults to `'nif'`.

  `Note: the NIF implementation no longer blocks the Erlang VM
  scheduler threads`

`pool_size`
  Specifies the size of the port program pool. Defaults to ``4``.

Original authors
----------------

Hunter Morris & [Mrinal Wadhwa](https://github.com/mrinalwadhwa).
