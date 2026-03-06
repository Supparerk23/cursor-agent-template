# Frontend — rules

- Use functional components; avoid class components unless the codebase already uses them.
- Extract reusable logic into custom hooks; keep components focused on rendering.
- Prefer composition over prop drilling; use context sparingly for true cross-tree state.
- Colocate styles with components (CSS modules or styled-components) unless a global design system says otherwise.
- Ensure interactive elements are keyboard-accessible and have sensible focus order.
- Use semantic HTML (nav, main, section, button, etc.); avoid div soup.
- Do not change API contracts or server routes; only consume them.
