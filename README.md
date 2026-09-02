# NAME

Perl::Critic::Policy::ProhibitNoWarningsRedefine - Mock with Test::MockModule, don't clobber the symbol table by hand.

# VERSION

version 1.000

# Perl::Critic::Policy::ProhibitNoWarningsRedefine

Switching off the `redefine` warning has exactly one purpose: assigning over
somebody else's subroutine, usually in a test.

```perl
no warnings 'redefine';
local *Some::Module::thing = sub { 1 };
```

The warning is the only thing standing between you and a typo.  Misspell the
package or the sub and that line silently installs a brand new symbol nobody
calls, the test passes, and it goes on passing after the real code is deleted.

[Test::MockModule](https://metacpan.org/pod/Test%3A%3AMockModule) in strict mode does the same job and checks the sub exists
first, unmocks itself at scope exit, and does not need the warning switched off:

```perl
my $mock = Test::MockModule->new('Some::Module');
$mock->redefine( thing => sub { 1 } );
```

For a package with no `.pm` of its own -- a modulino loaded out of `bin/` --
pass `no_auto => 1` so it doesn't go looking for the file.

## PROHIBITED

```
no warnings 'redefine';
no warnings qw{redefine once};
no warnings;                    # this switches off redefine too
```

## ALLOWED

```
no warnings 'once';
no warnings qw{uninitialized numeric};
```

## CONFIGURATION

- `categories`

    Space separated list of warning categories that may not be switched off.
    Defaults to `redefine`.

    ```
    [ProhibitNoWarningsRedefine]
    categories = redefine prototype
    ```

## CAVEATS

A bare `no warnings;` is flagged because it takes the guarded category with it,
which may not be why it was written.  That is the point: say which categories
you mean.

## METHODS

### supported\_parameters

`categories`, the warning categories that may not be switched off.

### default\_severity

SEVERITY\_HIGH

### default\_themes

maintenance, tests

### applies\_to

PPI::Statement::Include

### violates

Standard [Perl::Critic::Policy](https://metacpan.org/pod/Perl%3A%3ACritic%3A%3APolicy) interface.  Returns a violation for a `no
warnings` that switches off one of the guarded categories, whether it names it
or takes it along with everything else.

# BUGS

Please report any bugs or feature requests on the bugtracker website
[https://github.com/teodesian/perl-critic-policy-prohibitnowarningsredefine/issues](https://github.com/teodesian/perl-critic-policy-prohibitnowarningsredefine/issues)

When submitting a bug or request, please include a test-file or a
patch to an existing test-file that illustrates the bug or desired
feature.

# AUTHORS

Current Maintainers:

- George S. Baugh <teodesian@gmail.com>

# COPYRIGHT AND LICENSE

Copyright (c) 2026 Troglodyne LLC

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
