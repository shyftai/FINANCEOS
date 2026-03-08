# Collaboration Mode

## Mode: solo

Options:
- `solo` — all state is file-based, single operator
- `team` — shared state via Supabase, multi-user

## Solo mode (default)
- All files stored locally in workspace folders
- No external dependencies
- Full functionality, single operator

## Team mode
- Requires Supabase project (free tier works)
- Shared budgets, reports, and approvals
- Multi-user coordination (CFO, controller, bookkeeper)
- Required keys in .env: SUPABASE_URL, SUPABASE_ANON_KEY
