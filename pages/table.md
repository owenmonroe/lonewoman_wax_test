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

<!-- jQuery first (needed for the jQuery DataTables plugin) -->
<script src="{{ '/assets/jquery-3.2.1.min.js' | relative_url }}"></script>

<!-- DataTables CSS + JS -->
<link rel="stylesheet" href="{{ '/assets/datatables/datatables.css' | relative_url }}"/>
<script src="{{ '/assets/datatables/datatables.min.js' | relative_url }}"></script>

<script>
document.addEventListener('DOMContentLoaded', function () {
  try {
    if (window.jQuery && jQuery.fn && jQuery.fn.DataTable) {
      jQuery('#items-table').DataTable({
        pageLength: 10,
        order: [[2, 'asc']]
      });
    } else if (window.DataTable && typeof window.DataTable === 'function') {
      new DataTable('#items-table', {
        pageLength: 10,
        order: [[2, 'asc']]
      });
    } else {
      console.warn('DataTables not found. Check JS/CSS filenames and paths in assets/datatables/.');
    }
  } catch (e) {
    console.error('DataTables init error:', e);
  }
});
</script>
