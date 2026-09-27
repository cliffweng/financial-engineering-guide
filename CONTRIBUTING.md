# Contributing

Thanks for helping improve the guide. A few ground rules:

- **Scope**: one topic per file under `topics/`. Keep each topic readable in ~10 minutes.
- **Template**: follow the structure already used in existing topic files (Why it matters, Core concepts, Mental model, Interview questions, Watch, Further reading).
- **Links must be real**: only link to YouTube videos and articles you have personally verified exist (open the URL, confirm the title/content). Never guess a video ID or URL. Prefer Khan Academy, MIT OpenCourseWare, Bionic Turtle, Yale Open Courses, and other reputable educational sources — but any verified source is fine. If you are unsure of an exact URL, omit the video rather than invent one.
- **No invented product direction**: this guide covers financial-engineering fundamentals for quant/FE interview prep and self-study. It is not a live pricing app, not a paid-data product, and not a quiz/auth site. If you want to propose a new topic or reorganize the curriculum, open an issue first.
- **Keep topics tight**: prefer accurate intuition and interview-ready answer keys over encyclopedic coverage. Don't invent extra scope (no live market data, no interactive pricing widgets, no login).
- **Local preview**:
  ```bash
  bundle install
  bundle exec jekyll serve
  ```
  Then open `http://localhost:4000`.
- **Pull requests**: keep them focused (one topic or fix per PR) and describe what changed and why.
