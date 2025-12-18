# Django mental model for a MERN/Next.js developer

This is the “translation guide” between Django’s core concepts and the patterns you already know from React/Next.js + Node/Express.

## Django’s shape: Project vs App (monorepo-ish thinking)

In Django, you typically have:

- A **project**: global configuration and composition root
  - Example in your repo: `mysite/mysite/settings.py`, `mysite/mysite/urls.py`
- One or more **apps**: modular feature packages you can plug into projects
  - Example in your repo: `mysite/polls/`

If you come from MERN:

- A Django **project** feels like your “server entrypoint + global config” (Express `app.js` / `server.ts` plus environment config).
- A Django **app** feels like a feature module: routes + controllers + models + templates for one domain (like an Express router folder plus its Mongoose models and controllers).

## Request/response lifecycle (very Express-like, but opinionated)

Django is fundamentally an HTTP request → response system:

1. A request comes in (e.g. `GET /polls/5/`).
2. URL routing selects a view function/class to handle it.
3. The view:
   - reads data (ORM query),
   - chooses a template (for HTML), or builds a response directly,
   - returns an `HttpResponse`.

Like Express:

- URL config (`urls.py`) ≈ Express routing table
- View function (`views.py`) ≈ controller handler `(req, res) => { ... }`
- `HttpResponse` ≈ `res.send(...)`

But Django also gives you:

- ORM + migrations out of the box
- Template rendering out of the box
- Admin CRUD UI out of the box
- Security defaults (CSRF, escaping, etc.)

## MTV vs MVC (why Django calls it MTV)

Django is often described as **MTV**:

- **Model**: your database schema + query API (`models.py`)
- **Template**: the presentation layer (HTML with Django template language)
- **View**: the business logic that selects data and returns a response

If you think “MVC”:

- Django “view” is closer to a **controller**
- Django “template” is closer to a **view** in MVC

So the naming is slightly inverted compared to some MVC explanations.

## ORM comparison: Django ORM vs Mongoose/Prisma

When you do this in your app:

- `Question.objects.order_by("-pub_date")[:5]`

You are doing the Django equivalent of:

- Mongoose: `Question.find().sort({ pub_date: -1 }).limit(5)`
- Prisma: `prisma.question.findMany({ orderBy: { pubDate: 'desc' }, take: 5 })`

Relational relationships:

- Your `Choice.question = ForeignKey(Question)` creates a one-to-many relationship.
- Django automatically creates a reverse relation called `choice_set` on `Question`.

That’s why your template can do:

- `question.choice_set.all`

Comparable idea:

- Mongoose `populate` gives you children, but Django can query them via reverse relations without an explicit field on `Question`.

## Templates vs React/Next.js rendering

Django templates (DTL) are not React:

- DTL runs server-side only (HTML is rendered before being sent).
- You use tags like `{% for %}` and variables like `{{ question.question_text }}`.
- Django auto-escapes output by default (a security win).

Think of Django templates as closer to:

- Next.js SSR generating HTML
- Handlebars/EJS-style server templates

If you later build APIs with Django REST Framework, you might switch to JSON responses and use React separately. But this tutorial is intentionally teaching server-rendered HTML first.

## Routing: `include()` and namespacing

Your project routes:

- `mysite/mysite/urls.py` includes `polls.urls` under the `/polls/` prefix

Your app routes:

- `mysite/polls/urls.py` defines patterns like `<int:question_id>/`

The app declares:

- `app_name = "polls"`

This enables namespaced URL reversing in templates:

- `{% url 'polls:detail' question.id %}`

That’s similar to named routes in many router systems, and it prevents collisions if multiple apps have a `detail` route.

## Security basics you’ll hit in Part 4

Coming from Node/Express, a few things will feel familiar:

- **CSRF protection**: Django will require CSRF tokens for POST forms (similar to CSRF middleware in Express, but Django makes it default).
- **Redirect after POST**: Django encourages “POST → redirect → GET” (PRG pattern) to avoid duplicate submissions on refresh.

Part 4 will make these concepts concrete by implementing voting (a POST that updates the `votes` column).

