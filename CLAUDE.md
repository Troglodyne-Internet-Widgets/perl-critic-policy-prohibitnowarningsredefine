# Perl::Critic::Policy::ProhibitNoWarningsRedefine

This distribution holds one Perl::Critic policy. The policy flags `no warnings 'redefine'` and a bare `no warnings;`. It tells the author to use Test::MockModule instead.

## Layout

- `lib/Perl/Critic/Policy/ProhibitNoWarningsRedefine.pm` holds the policy and its POD.
- `t/main.t` holds the tests. Run them with `prove -l t`.
- `dist.ini`, `weaver.ini`, and `prereqs.yml` control the Dist::Zilla build. `[AutoPrereqs]` finds most prerequisites. `prereqs.yml` lists the rest.

## Standards

- Run `perlcritic --profile .perlcriticrc lib t` before you commit.
- To turn a policy off, add `[-Name]` to `.perlcriticrc`. Write the reason in a comment beside it.
- In new code, use `use warnings FATAL => 'all';` and `no autovivification;`.
- The pre-commit hook runs `perltidy` with `.perltidyrc`. Do not skip the hooks.
- Write `Changes`, POD, and Markdown in plain English.
- In tests, call `Perl::Critic->new` with `-profile => q{}`. The profile of this repository then does not run against the test snippets.
