Termit
==============

Library for serializing Erlang terms to signed encrypted binaries and reliably deserializing them back.

Usage
--------------

A typical use case is to provide means to keep secrets put in public domain, e.g. secure cookies.

```erlang
Term = {this, is, an, [erlang, <<"term">>]}.
% time-to-live is 10 seconds =:= secret valid no more than 10 seconds
Cookie = termit:encode_base64(Term, <<"cekpet">>, 10).

% secret is alive
{ok, Term} = termit:decode_base64(Cookie, <<"cekpet">>, 10).

% secret's time-to-live must fit
{error, forged} = termit:decode_base64(Cookie, <<"cekpet">>, 11).

% after 10 seconds elapsed
{error, expired} = termit:decode_base64(Cookie, <<"cekpet">>, 10).

% check whether secret was not forged
{error, forged} = termit:decode_base64(<<Cookie/binary, "1">>, <<"cekpet">>, 10).
{error, forged} = termit:decode_base64(Cookie, <<"secret">>, 10).
{error, forged} = termit:decode_base64(undefined, <<"cekpet">>, 10).
```

[License](termit/blob/master/LICENSE.txt)
-------

Copyright (c) 2013 Vladimir Dronnikov <dronnikov@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
