# Part 4 preview: forms, POST, redirects, and generic views

Part 4 is where the Polls app stops being “read-only” and becomes interactive.

You’ll implement a real “vote” flow and then refactor to Django’s generic class-based views (CBVs).

## 1) The voting flow you’re about to build

You’ll end up with this user journey:

1. User visits a question detail page: `GET /polls/<id>/`
2. They select a choice and submit a form: `POST /polls/<id>/vote/`
3. The server updates the database (increments votes)
4. The server redirects them to results: `302 → GET /polls/<id>/results/`

Why the redirect matters:

- It prevents double-submitting votes if the user refreshes the page (PRG pattern).

## 2) What will change in your templates

### `detail.html` will become a form

Instead of listing choices as plain `<li>...`, you’ll:

- render radio buttons (one per choice)
- include `{% csrf_token %}` inside the `<form>`
- submit to the vote URL

### A new `results.html` template will appear

You’ll create `mysite/polls/templates/polls/results.html` to display:

- each choice
- its vote count
- a link back to vote again

## 3) What will change in your views (function-based first)

Your `vote` view will:

- read the posted form value (selected choice)
- handle the “no choice selected” error case
- update the selected `Choice.votes`
- `return HttpResponseRedirect(...)`

You’ll also likely use:

- `reverse("polls:results", args=(question.id,))`

This is Python-side URL reversing (like `{% url %}` but in a view).

## 4) CSRF: Django’s default protection on POST

Django enables CSRF protection by default in middleware.

That means:

- Any POST HTML form must send a valid CSRF token
- The template tag `{% csrf_token %}` injects it

This is a big difference from many Express apps, where CSRF is often optional or manually configured.

## 5) Then you refactor to generic views (CBVs)

The tutorial’s second half of Part 4 replaces your `index` and `detail` functions with:

- `generic.ListView` for the index page
- `generic.DetailView` for the detail page

Why Django pushes this:

- common “fetch object(s) then render template” patterns become declarative
- it reduces repetition and encourages consistent behavior

But it’s not “magic”; under the hood it still:

- picks a queryset
- loads objects
- renders a template

## 6) Practical checklist before you start Part 4

- Confirm you can load `/polls/` and `/polls/<id>/` successfully.
- Ensure you have at least one `Question` and a few `Choice` rows in the database (use admin UI).
- Decide whether you want to register `Choice` in admin now or wait (either is fine).

