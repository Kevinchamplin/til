# How `Co-authored-by:` trailers actually work

Git and GitHub credit multiple authors on a single commit via a trailer in the commit *message* — not via any special flag.

```
Implement rate limiter

Co-authored-by: Ada Lovelace <ada@example.com>
Co-authored-by: Alan Turing <alan@example.com>
```

Rules that trip people up:

- The trailer must be in the **last paragraph** of the message, each on its own line, with a blank line separating it from the body.
- The format is exact: `Co-authored-by: Name <email>`.
- For GitHub to link the co-author to a profile (and count it on their contribution graph), the email must be one GitHub knows for that account — typically their `ID+username@users.noreply.github.com` no-reply address, which you can read from their profile's commits.

## Why it matters

Pairing, mob programming, and "I wrote it, you reviewed and reshaped it" all deserve shared credit. The trailer is also what GitHub keys its *Pair Extraordinaire* achievement off of.

## Takeaway

Co-authorship lives in the commit message, the email has to resolve to a real account, and the trailer belongs in the final block of the message.
