---
layout: page
title: "Search"
permalink: /search/
---

<input id="q" type="search" placeholder="Search title, source, tags…" style="width:100%;padding:.5rem;margin:.5rem 0"/>
<div id="results"></div>

<script src="{{ '/assets/elasticlunr.min.js' | relative_url }}"></script>

<script>
// Build a small index from front matter (client-side)
const idx = elasticlunr(function () {
  this.setRef('url');
  this.addField('title');
  this.addField('source');
  this.addField('tags');
  this.addField('date');
});

const docs = [
  {% for doc in site.items %}
  {
    url: "{{ doc.url | relative_url }}",
    title: {{ doc.title | jsonify }},
    source: {{ doc.source | jsonify }},
    tags: {{ doc.tags | jsonify }},
    date: {{ doc.date | jsonify }}
  }{% unless forloop.last %},{% endunless %}
  {% endfor %}
];

docs.forEach(d => idx.addDoc(d));

const q = document.getElementById('q');
const out = document.getElementById('results');

function render(matches) {
  if (!matches.length) { out.innerHTML = "<p>No results.</p>"; return; }
  out.innerHTML = matches.map(m => {
    const d = m.doc;
    return `
      <article style="margin:0 0 1rem 0">
        <h3 style="margin:.25rem 0"><a href="${d.url}">${d.title}</a></h3>
        <p style="margin:.25rem 0; opacity:.8">${d.source || ''} ${d.date ? '— ' + d.date : ''}</p>
      </article>`;
  }).join('');
}

q.addEventListener('input', () => {
  const text = q.value.trim();
  if (!text) { out.innerHTML = ""; return; }
  const hits = idx.search(text, { expand: true });
  render(hits);
});
</script>
