# Coding standards

Apply these when implementation begins. They intentionally avoid selecting a framework; see 02_ARCHITECTURE.md.

- Keep game rules, persistence, timing, rendering, and user input separable enough to reason about independently.
- Use descriptive names, small focused functions, consistent formatting, and comments for non-obvious rules rather than narrating obvious code.
- Prefer semantic HTML and accessible names for controls. Preserve visible focus and avoid making color the only signal for a game state.
- Keep visual tokens and theme-specific values centralized. Keep board cells square; do not round tile corners.
- Treat saved and imported data as untrusted input: validate shape/version before using it and handle storage failures visibly.
- Avoid dependencies unless they materially reduce complexity. Record added dependencies and their purpose in 02_ARCHITECTURE.md.
- Do not silently change approved requirements. Update documentation when the user approves a behavior change.
