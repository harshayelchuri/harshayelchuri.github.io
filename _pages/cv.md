---
layout: page
permalink: /cv/
title: cv
nav: true
nav_order: 5
description: My latest curriculum vitae.
---

<div class="mb-3">
  <a class="btn btn-primary" href="{{ '/assets/pdf/CV.pdf' | relative_url }}" target="_blank" rel="noopener noreferrer">
    Open CV in a new tab
  </a>
  <a class="btn btn-outline-primary" href="{{ '/assets/pdf/CV.pdf' | relative_url }}" download>
    Download CV
  </a>
</div>

<object
  data="{{ '/assets/pdf/CV.pdf' | relative_url }}"
  type="application/pdf"
  width="100%"
  height="1100"
  aria-label="Harsha Yelchuri's curriculum vitae"
>
  <p>
    Your browser cannot display the embedded PDF.
    <a href="{{ '/assets/pdf/CV.pdf' | relative_url }}">Open the CV directly</a>.
  </p>
</object>
