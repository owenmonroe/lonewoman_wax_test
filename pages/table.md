---
layout: page
title: "Table View"
permalink: /table/
---

<table id="items-table">
  <thead>
    <tr>
      <th>Title</th>
      <th>Source</th>
      <th>Date</th>
      <th>Preview</th>
    </tr>
  </thead>
  <tbody>
    {% assign rows = site.items | sort: 'date' %}
    {% for doc in rows %}
      {% assign img = doc.thumb | default: doc.image %}
      <tr>
        <td><a href="{{ doc.url | relative_url }}">{{ doc.title }}</a></td>
        <td>{{ doc.source }}</td>
        <td>{{ doc.date }}</td>
        <td>
          {% if img %}
            <img src="{{ img | relative_url }}" alt="{{ doc.title }}" style="max-height:60px">
          {% endif %}
        </td>
      </tr>
    {% endfor %}
  </tbody>
</table>

<script src="{{ '/assets/datatables/datatables.min.js' | relative_url }}"></script>
<link rel="stylesheet" href="{{ '/assets/datatables/datatables.min.css' | relative_url }}"/>

<script>
document.addEventListener('DOMContentLoaded', function () {
  new DataTable('#items-table', {
    pageLength: 10,
    order: [[2, 'asc']]
  });
});
</script>
