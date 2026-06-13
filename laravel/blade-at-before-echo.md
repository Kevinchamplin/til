# Blade: `@` directly before `{{ }}` breaks interpolation

Blade treats a leading `@` as the "leave this for a JS framework" escape. So when you write a `@` immediately before an echo — common in email templates or anything that mixes Blade with a `{{ }}`-using frontend framework — Blade renders the braces *literally* instead of evaluating them:

```blade
{{-- renders the literal text "@{{ $name }}" --}}
@{{ $name }}

{{-- @@ doesn't save you either --}}
@@{{ $name }}
```

## Fix

Move the `@` *inside* the echo so it's just a string being concatenated, and Blade evaluates the variable normally:

```blade
{{ '@'.$name }}
```

I hit this in a transactional email where a handle was prefixed with `@` — recipients saw the raw `{{ $var }}` instead of their name.

## Takeaway

A `@` touching `{{` is the directive-escape syntax, not text. Put the `@` inside the braces (`{{ '@'.$x }}`) when you actually want a literal at-sign next to an interpolated value.
