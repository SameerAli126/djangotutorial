# Part 3 deep dive: templates, URLs, and views

Part 3 is where your Polls app becomes a real web app:

- URLs become meaningful and dynamic (`/polls/5/`)
- views stop returning placeholder strings and start returning rendered HTML
- templates become the “UI layer” (server-rendered)

This file explains what you implemented and why it matters.

## Step A — You introduced templates (UI layer)

In earlier tutorial parts, views often return:

- `HttpResponse("Hello, world")`

That’s great for learning routing, but not scalable for real pages.

### Where templates live

Django finds templates in installed apps when:

- `APP_DIRS = True` (in `mysite/mysite/settings.py`)

The conventional location is:

- `polls/templates/polls/...`

You followed this convention exactly:

- `mysite/polls/templates/polls/index.html`
- `mysite/polls/templates/polls/detail.html`

This “double polls” folder name is intentional:

- the first `polls` = app name
- the second `polls` = template namespace

It avoids template name collisions if multiple apps have `index.html`.

## Step B — You started passing data into templates

Your `index` view does:

1. Query:
   - `Question.objects.order_by("-pub_date")[:5]`
2. Put result into `context` dict:
   - `{"latest_question_list": latest_question_list}`
3. Render:
   - `render(request, "polls/index.html", context)`

Key idea:

- The template gets variables from the context dict.
- The template never queries the database directly.

If you think in React terms:

- `context` is like the props passed into a component.

## Step C — You used Django Template Language (DTL)

In `index.html`, you use:

- `{% if latest_question_list %}` — conditional
- `{% for question in latest_question_list %}` — loop
- `{{ question.question_text }}` — variable output

### URL reversing (why `{% url %}` is important)

Instead of hardcoding:

- `<a href="/polls/{{ question.id }}/">...</a>`

You use:

- `{% url 'polls:detail' question.id %}`

Benefits:

- If you ever change your URL pattern (e.g., from `/<id>/` to `/questions/<id>/`), you update it in one place (`urls.py`) and templates keep working.

This is similar to:

- using named routes in a router, rather than concatenating strings.

## Step D — You used `get_object_or_404` (a “fail fast” pattern)

In `detail`, you do:

- `get_object_or_404(Question, pk=question_id)`

Why this is good:

- If someone hits `/polls/999999/` you return a real 404 page, not a server error.
- You avoid manual error-checking boilerplate.

Express analogy:

- In Express you might do `const doc = await Model.findById(id); if (!doc) return res.status(404).send(...)`.
 Django gives you a helper for this common pattern.

## Step E — You leaned on ORM relationships in the template

In `detail.html` you iterate:

- `question.choice_set.all`

This works because:

- `Choice.question` is a `ForeignKey` to `Question`
- Django automatically creates a reverse relation called `<childmodel>_set` unless you rename it with `related_name=...`

This is a huge productivity feature:

- you model the relationship once
- queries become intuitive in both Python and templates

## Step F — What’s still placeholder (and why that’s okay)

You already have URLs for:

- `results`
- `vote`

But the views return placeholder strings.

That is exactly where Part 4 starts:

- `detail` becomes a form
- `vote` becomes a POST handler that increments the chosen option’s `votes`
- `results` becomes a real template showing the current vote counts

