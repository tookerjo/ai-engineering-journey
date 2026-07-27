## Why Supabase Auth over NextAuth
The sharper reason isn't just "less complexity" — Supabase Auth's
session integrates directly with the same Postgres instance RLS
policies run on. auth.uid() is usable in a policy with zero extra
plumbing. NextAuth would need a bridge to get that.

## Why the redirect URI check happens before the redirect fires
Google's involvement ends the moment it redirects the browser — there
is no "after" where Google could claw back a misdirected code. The
check has to happen before the redirect, since the redirect is a
one-way, unrecoverable action. If checked downstream instead, the
code would already be delivered to whatever URI was requested,
attacker-controlled or not, before anyone could object.
