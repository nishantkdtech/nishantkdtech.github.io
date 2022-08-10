# Nishant Kompella's School Site

This is the site in which I will do my stuff, like upload code and stuff. I'll add more links here later, but for now, there is nothing here.
a
{% for post in site.posts %}
  <article>
    <h2>
      <a href="{{ post.url }}">
        {{ post.title }}
      </a>
    </h2>
    <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_long_string }}</time>
    {{ post.content }}
  </article>
{% endfor %}

