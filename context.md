⏺ Here's a detailed, self-contained prompt you can hand to a coding agent to reproduce this skeleton exactly as it stands.

  ---

  Build the SKELETON (unfinished scaffold) of a Flask personal expense tracker called
  "Spendly". This is a teaching scaffold — the backend logic is intentionally NOT
  implemented yet; only routes that render static templates work, and the rest are
  string placeholders marking future build steps. Do NOT implement auth, database
  logic, or expense CRUD. Reproduce exactly what's described below.

  ═══════════════════════════════════════════════════════════════════
  TECH STACK
  ═══════════════════════════════════════════════════════════════════
  - Python + Flask (server-rendered Jinja2 templates, no JS framework)
  - SQLite is the intended DB (not implemented yet)
  - Plain CSS, no build tooling
  - Dev server runs on port 5001 with debug=True

  requirements.txt (pin these exact versions):
      flask==3.1.3
      werkzeug==3.1.6
      pytest==8.3.5
      pytest-flask==1.3.0

  ═══════════════════════════════════════════════════════════════════
  DIRECTORY STRUCTURE
  ═══════════════════════════════════════════════════════════════════
  expense-tracker/
  ├── app.py
  ├── requirements.txt
  ├── .gitignore
  ├── database/
  │   ├── __init__.py          (empty file — package marker)
  │   └── db.py                (spec comments only, NO code)
  ├── templates/
  │   ├── base.html
  │   ├── landing.html
  │   ├── login.html
  │   └── register.html
  └── static/
      ├── css/style.css
      └── js/main.js            (one comment line only)

  ═══════════════════════════════════════════════════════════════════
  app.py
  ═══════════════════════════════════════════════════════════════════
  - Create `app = Flask(__name__)`.
  - IMPLEMENTED routes (GET only, just render templates):
      GET "/"          -> landing()  -> render landing.html
      GET "/register"  -> register() -> render register.html
      GET "/login"     -> login()    -> render login.html
  - PLACEHOLDER routes that just `return` a plain string (these encode the future
    build roadmap — keep the step labels exactly):
      "/logout"                       -> return "Logout — coming in Step 3"
      "/profile"                      -> return "Profile page — coming in Step 4"
      "/expenses/add"                 -> return "Add expense — coming in Step 7"
      "/expenses/<int:id>/edit"       -> return "Edit expense — coming in Step 8"
      "/expenses/<int:id>/delete"     -> return "Delete expense — coming in Step 9"
  - Use section-divider comment banners separating "Routes" from "Placeholder
    routes — students will implement these".
  - At the bottom: `if __name__ == "__main__": app.run(debug=True, port=5001)`

  ═══════════════════════════════════════════════════════════════════
  database/db.py  (DO NOT write real code — only this spec as comments)
  ═══════════════════════════════════════════════════════════════════
  A comment block stating students will write this in "Step 1 — Database Setup",
  and that the file should contain:
      get_db()   — returns a SQLite connection with row_factory and foreign keys enabled
      init_db()  — creates all tables using CREATE TABLE IF NOT EXISTS
      seed_db()  — inserts sample data for development

  ═══════════════════════════════════════════════════════════════════
  static/js/main.js
  ═══════════════════════════════════════════════════════════════════
  Single comment line:
      // main.js — students will add JavaScript here as features are built

  ═══════════════════════════════════════════════════════════════════
  .gitignore
  ═══════════════════════════════════════════════════════════════════
  venv/
  expense_tracker.db
  __pycache__/
  *.pyc
  *.pyo
  .env
  .DS_Store
  .claude/plans/

  ═══════════════════════════════════════════════════════════════════
  TEMPLATES (Jinja2)
  ═══════════════════════════════════════════════════════════════════
  base.html — shared layout that all pages extend:
    - HTML5 doc, responsive viewport meta.
    - <title>{% block title %}Spendly{% endblock %}</title>
    - Preconnect + load Google Fonts: "DM Serif Display" (ital 0;1) and
      "DM Sans" (weights 300;400;500;600).
    - Link static css via url_for('static', filename='css/style.css').
    - {% block head %} in <head>.
    - Sticky navbar (.navbar > .nav-inner): left = brand link to landing
      (a "◈" diamond icon + "Spendly"); right = "Sign in" link (to login) and
      "Get started" link to register styled as .nav-cta.
    - <main class="main-content">{% block content %}{% endblock %}</main>
    - Footer (.footer): "◈" icon, "Spendly" name, tagline
      "Track every rupee. Own your finances."
    - Load static js via url_for('static', filename='js/main.js'), then
      {% block scripts %}.
    - Use {{ url_for(...) }} for all internal links (landing, login, register).

  landing.html — extends base.html, title "Spendly — Track Every Rupee":
    - <section class="hero"> with .hero-inner: a .hero-badge "Personal Finance
      Tracker", an <h1 class="hero-title"> reading "Know where your <em>money
      goes</em>" (the em is the accent color), a .hero-subtitle paragraph, and
      .hero-actions with a .btn-primary "Start tracking free" (-> register) and
      a .btn-ghost "Sign in" (-> login).
    - .hero-visual containing a .mock-card: header with .mock-label "March 2026"
      and .mock-total "₹12,450"; then .mock-bars with 4 .mock-bar-row entries
      (category label, a .mock-bar-track holding a colored .mock-bar at a set
      width %, and an amount):
          Bills      72%  ₹4,500   (default accent bar)
          Food       52%  ₹3,200   (.mock-bar-2)
          Health     33%  ₹2,050   (.mock-bar-3)
          Transport  28%  ₹1,800   (.mock-bar-4)
    - <section class="features"> with .features-inner holding 3 .feature-card
      items (icon, title, body):
          "₹"  "Log expenses instantly" — Add any expense in seconds...
          "◎"  "Understand your patterns" — See exactly where your money goes...
          "◷"  "Filter by time period" — View your spending for any date range...
    - <section class="cta-section"> with .cta-title "Ready to take control?",
      a body line, and a .btn-primary "Create free account" (-> register).

  login.html — extends base.html, title "Sign in — Spendly":
    - .auth-section > .auth-container.
    - .auth-header: .auth-title "Welcome back", .auth-subtitle
      "Sign in to your Spendly account".
    - .auth-card: {% if error %}<div class="auth-error">{{ error }}</div>{% endif %},
      then <form method="POST" action="/login"> with two .form-group blocks:
        email (type=email, name="email", placeholder "nitish@example.com",
               required, autofocus)
        password (type=password, name="password", required)
      and a .btn-submit "Sign in".
    - .auth-switch line: "Don't have an account? <a -> register>Create one free</a>".

  register.html — extends base.html, title "Create account — Spendly":
    - Same auth layout. .auth-title "Create your account", subtitle
      "Start tracking your expenses today".
    - <form method="POST" action="/register"> with three .form-group blocks:
        name (type=text, name="name", placeholder "Nitish Kumar", required, autofocus)
        email (type=email, name="email", placeholder "nitish@example.com", required)
        password (type=password, name="password", placeholder "Min. 8 characters",
                  required)
      and a .btn-submit "Create account".
    - .auth-switch: "Already have an account? <a -> login>Sign in</a>".

  NOTE: The auth forms POST to /login and /register, but those routes are GET-only
  for now — leave that gap intentionally (it's a later build step).

  ═══════════════════════════════════════════════════════════════════
  static/css/style.css — full design system
  ═══════════════════════════════════════════════════════════════════
  Define :root CSS variables and an "editorial / paper" aesthetic:
    Colors:
      --ink #0f0f0f, --ink-soft #2d2d2d, --ink-muted #6b6b6b, --ink-faint #a0a0a0
      --paper #f7f6f3, --paper-warm #f0ede6, --paper-card #ffffff
      --accent #1a472a (deep green), --accent-light #e8f0eb
      --accent-2 #c17f24 (gold), --accent-2-light #fdf3e3
      --danger #c0392b, --danger-light #fdecea
      --border #e4e1da, --border-soft #eeebe4
    Fonts:
      --font-display: 'DM Serif Display', Georgia, serif
      --font-body: 'DM Sans', system-ui, sans-serif
    Layout vars: --max-width 1200px, --auth-width 440px
    Radii: --radius-sm 6px, --radius-md 12px, --radius-lg 20px

    Then full component styles (use these exact class names):
    - Reset (*,::before,::after box-sizing/margin/padding 0), html 16px base +
      smooth scroll, body uses --paper bg, --ink text, body font, line-height 1.6,
      antialiased. Links inherit color, no underline.
    - .navbar (sticky, top 0, z 100, paper bg, bottom border), .nav-inner
      .brand-icon (accent), .brand-name, .nav-links (flex gap),
      .nav-cta (ink bg, paper text, hover -> accent).
    - .main-content { min-height: calc(100vh - 60px - 100px); }
    - .hero (grid 1fr 1fr, gap 4rem, max-width centered, 5rem top padding),
      .hero-badge (pill, accent-light bg, uppercase), .hero-title (display font,
      clamp(2.5rem,5vw,4rem)), .hero-title em (italic accent), .hero-subtitle
      (muted, max 460px), .hero-actions (flex gap).
    - .hero-visual, .mock-card (white card, lg radius, soft shadow, max 380px),
      .mock-card-header (flex space-between, bottom border), .mock-label
      (uppercase muted), .mock-total (display font), .mock-bars (column gap),
      .mock-bar-row (grid 80px 1fr 60px), .mock-cat, .mock-bar-track (6px,
      border-soft, rounded), .mock-bar (accent, width transition 0.6s),
      .mock-bar-2 (accent-2/gold), .mock-bar-3 (#5b7fa6 blue),
      .mock-bar-4 (#8b5e83 mauve), .mock-amt (right-aligned muted).
    - .btn-primary (ink bg, paper text, hover -> accent),
      .btn-ghost (transparent, border, hover -> darkens border+text).
    - .features (paper-warm bg, top+bottom borders, 5rem padding),
      .features-inner (grid 3 cols, gap 2rem), .feature-card (white, md radius),
      .feature-icon (1.5rem accent bold), .feature-title (display font),
      .feature-body (muted).
    - .cta-section (centered, 6rem padding), .cta-inner (max 560px),
      .cta-title (display font, clamp), .cta-body (muted).
    - Auth: .auth-section (min-height calc, flex centered), .auth-container
      (max --auth-width), .auth-header (centered), .auth-title (display 2rem),
      .auth-subtitle (muted), .auth-card (white, md radius, padding),
      .auth-error (danger-light bg, danger text, border), .form-group,
      .form-group label, .form-input (full width, border, focus -> accent border,
      placeholder --ink-faint), .btn-submit (full width ink button, hover accent),
      .auth-switch (centered muted, accent link, hover underline).
    - .footer (ink bg, paper text, centered, 2.5rem padding), .footer-inner
      (column centered), .footer .brand-icon (gold accent-2), .footer-name,
      .footer-copy (faint).
    - Responsive:
        @media (max-width:900px): .hero -> single column + centered,
          .hero-subtitle full width, .hero-actions centered, .hero-visual hidden,
          .features-inner -> 1 column.
        @media (max-width:600px): hide non-CTA nav links, reduce .hero padding.

  ═══════════════════════════════════════════════════════════════════
  DELIVERABLE
  ═══════════════════════════════════════════════════════════════════
  A runnable skeleton: `pip install -r requirements.txt` then `python app.py`
  serves the landing, login, and register pages styled correctly on port 5001.
  The placeholder routes return their plain-string step messages. db.py stays
  a comment-only spec. Keep code clean with section-comment banners; use
  neutral, readable formatting throughout.

  ---
  This prompt is faithful to every concrete detail in the current repo (exact versions, colors, copy, class names, route step-labels, the
  deliberate GET-only/POST mismatch, and the unimplemented db.py). A couple of notes if you want to tune it:

  - It tells the agent not to implement auth/DB/CRUD on purpose. If you'd rather the agent stub those out differently, adjust the
  "DELIVERABLE" and app.py sections.
  - I left the placeholder strings verbatim ("Logout — coming in Step 3", etc.) since those encode your build roadmap — drop them if you
  don't want the step scaffolding visible.