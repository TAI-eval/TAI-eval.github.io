---
layout: page
use-site-title: true
css:
  - "/assets/css/custom.css"
---
{% assign paper_count = site.data.accepted_papers | size %}
<div class="paper-toolbar">
  <label class="paper-search">
    <i class="fa fa-search" aria-hidden="true"></i>
    <span class="sr-only">Search papers</span>
    <input type="search" placeholder="Search by title or author" autocomplete="off">
  </label>
  <span class="paper-status" aria-live="polite">{{ paper_count }} papers</span>
  <div class="paper-sort" role="group" aria-label="Sort papers">
    <button type="button" data-sort="id" aria-pressed="true">ID</button>
    <button type="button" data-sort="title" aria-pressed="false">A-Z</button>
  </div>
</div>

<ol class="accepted-paper-list">
{% for paper in site.data.accepted_papers %}
  {% assign author_list = paper.authors | join: ', ' %}
  <li data-number="{{ paper.number }}" data-title="{{ paper.title | escape }}" data-search="{{ paper.title | append: ' ' | append: author_list | downcase | escape }}">
    <span class="paper-number">#{{ paper.number | prepend: '0000' | slice: -4, 4 }}</span>
    <div class="paper-details">
      <a class="paper-title" href="{{ paper.url | escape }}" target="_blank" rel="noopener">{{ paper.title | escape }}</a>
      <span class="paper-authors"><i class="fa fa-users" aria-hidden="true"></i>{{ author_list | escape }}</span>
    </div>
    <a class="paper-link" href="{{ paper.url | escape }}" target="_blank" rel="noopener" aria-label="Open paper #{{ paper.number }} on OpenReview">
      OpenReview <i class="fa fa-external-link" aria-hidden="true"></i>
    </a>
  </li>
{% endfor %}
</ol>
<p class="paper-empty" hidden>No papers match your search.</p>

<script>
  const paperList = document.querySelector('.accepted-paper-list');
  const paperItems = [...paperList.children];
  const searchInput = document.querySelector('.paper-search input');
  const status = document.querySelector('.paper-status');
  const empty = document.querySelector('.paper-empty');

  searchInput.addEventListener('input', () => {
    const terms = searchInput.value.toLowerCase().split(/\s+/).filter(Boolean);
    let shown = 0;
    paperItems.forEach((item) => {
      const match = terms.every((term) => item.dataset.search.includes(term));
      item.hidden = !match;
      if (match) shown += 1;
    });
    status.textContent = terms.length
      ? `${shown} of ${paperItems.length} papers`
      : `${paperItems.length} papers`;
    empty.hidden = shown > 0;
  });

  document.querySelectorAll('.paper-sort button').forEach((button) => {
    button.addEventListener('click', () => {
      const byTitle = button.dataset.sort === 'title';
      const titleKey = (item) => item.dataset.title.replace(/^[^\p{L}\p{N}]+/u, '');
      const sorted = [...paperItems].sort((a, b) => byTitle
        ? titleKey(a).localeCompare(titleKey(b), undefined, { sensitivity: 'base' })
        : Number(a.dataset.number) - Number(b.dataset.number));
      paperList.replaceChildren(...sorted);
      document.querySelectorAll('.paper-sort button').forEach((option) => {
        option.setAttribute('aria-pressed', String(option === button));
      });
    });
  });
</script>
