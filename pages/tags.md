---
layout: page
title: "Browse by Tag"
permalink: /tags/
---

<div id="tag-bar" style="display:flex; gap:.5rem; flex-wrap:wrap; margin-bottom:1rem"></div>
<div id="tag-results"></div>

<script>
const items = [
  {% for doc in site.items %}
  {
    url: "{{ doc.url | relative_url }}",
    title: {{ doc.title | jsonify }},
    source: {{ doc.source | jsonify }},
    date: {{ doc.date | jsonify }},
    img: {{ (doc.thumb | default: doc.image) | jsonify }},
    tags: {{ doc.tags | jsonify }}
  }{% unless forloop.last %},{% endunless %}
  {% endfor %}
];

const allTags = Array.from(new Set(items.flatMap(i => i.tags || []))).sort();

const bar = document.getElementById('tag-bar');
const results = document.getElementById('tag-results');

function renderList(list) {
  results.innerHTML = list.map(i => `
    <article style="display:flex; gap:1rem; align-items:center; margin:.5rem 0">
      ${i.img ? `<img src="${i.img}" alt="${i.title}" style="height:60px">` : ''}
      <div>
        <h3 style="margin:0"><a href="${i.url}">${i.title}</a></h3>
        <p style="margin:.25rem 0; opacity:.8">${i.source || ''} ${i.date ? '— ' + i.date : ''}</p>
      </div>
    </article>
  `).join('');
}

function selectTag(tag) {
  const filtered = items.filter(i => (i.tags || []).includes(tag));
  renderList(filtered);
}

bar.innerHTML = allTags.map(t => `<button type="button" data-tag="${t}">${t}</button>`).join('');
bar.addEventListener('click', e => {
  const t = e.target.getAttribute('data-tag');
  if (t) selectTag(t);
});

// default: show all
renderList(items);
</script>
