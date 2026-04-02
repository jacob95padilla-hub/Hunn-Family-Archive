# Hunn-Family-Archive
An ongoing project to collect an audio-visual history of our family

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Hunn Family Archive</title>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;1,400&family=Lato:wght@300;400;700&display=swap" rel="stylesheet" />
  <style>
    /* ============================================================
       HUNN FAMILY ARCHIVE — index.html
       Hosted on GitHub Pages + YouTube (unlisted) + Google Photos

       HOW TO ADD A NEW VIDEO:
       1. Upload your video to YouTube
       2. Set visibility to "Unlisted"
       3. Copy the video ID from the URL:
          https://youtu.be/XXXXXXXXXXX  ← just the XXXXXXXXXXX part
       4. Find the ADD VIDEOS HERE comment below
       5. Copy one of the existing <div class="video-card"> blocks
       6. Paste it and fill in: the YouTube ID, year, type tag,
          title, format (e.g. "Video interview"), and duration

       AVAILABLE TYPE TAGS (use one per card):
         Oral History · Home Video · Photos · Documents

       HOW TO ADD A PHOTO ALBUM:
       1. Create a Google Photos shared album
       2. Set sharing to "Anyone with the link"
       3. Copy the link
       4. Find the <!-- ADD ALBUMS HERE --> comment below
       5. Copy an existing <div class="album-card"> block
       6. Paste it and fill in the link, decade label, and title
    ============================================================ */

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --cream: #F5F0E8;
      --card-bg: #FAF7F2;
      --warm-brown: #5C3D2E;
      --mid-brown: #7A5242;
      --light-brown: #A07858;
      --gold: #B8892E;
      --text-dark: #241208;
      --text-body: #3D2416;
      --text-muted: #7A6052;
      --border: rgba(92,61,46,0.14);
      --border-strong: rgba(92,61,46,0.28);
    }

    html { scroll-behavior: smooth; }

    body {
      background-color: var(--cream);
      color: var(--text-body);
      font-family: 'Lato', sans-serif;
      font-weight: 400;
      line-height: 1.6;
      min-height: 100vh;
    }

    /* ── Page wrapper ── */
    .page {
      max-width: 960px;
      margin: 0 auto;
      padding: 0 1.5rem 5rem;
    }

    /* ── Masthead ── */
    .masthead {
      text-align: center;
      padding: 3.5rem 1rem 2.5rem;
      border-bottom: 1px solid var(--border-strong);
      margin-bottom: 2.5rem;
    }
    .masthead-eyebrow {
      font-size: 10px;
      font-weight: 700;
      letter-spacing: 4px;
      text-transform: uppercase;
      color: var(--gold);
      margin-bottom: 1rem;
    }
    .masthead-title {
      font-family: 'Playfair Display', serif;
      font-size: clamp(36px, 6vw, 56px);
      font-weight: 400;
      color: var(--text-dark);
      line-height: 1.15;
      margin-bottom: 0.6rem;
    }
    .masthead-title em {
      font-style: italic;
      color: var(--warm-brown);
    }
    .masthead-sub {
      font-size: 15px;
      font-weight: 300;
      color: var(--text-muted);
      letter-spacing: 0.5px;
      margin-bottom: 1.75rem;
    }
    .masthead-ornament {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 12px;
      color: var(--gold);
    }
    .masthead-ornament::before,
    .masthead-ornament::after {
      content: '';
      width: 48px;
      height: 1px;
      background: var(--gold);
    }
    .masthead-ornament span {
      font-size: 18px;
      line-height: 1;
    }

    /* ── Section label ── */
    .section-label {
      font-size: 10px;
      font-weight: 700;
      letter-spacing: 3.5px;
      text-transform: uppercase;
      color: var(--gold);
      padding-bottom: 0.6rem;
      border-bottom: 1px solid var(--border);
      margin-bottom: 1.25rem;
    }

    /* ── Filter bar ── */
    .filter-bar {
      display: flex;
      gap: 6px;
      flex-wrap: wrap;
      margin-bottom: 2rem;
    }
    .filter-btn {
      font-family: 'Lato', sans-serif;
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 1.5px;
      text-transform: uppercase;
      background: transparent;
      border: 1px solid var(--border-strong);
      color: var(--text-muted);
      padding: 6px 14px;
      border-radius: 2px;
      cursor: pointer;
      transition: all 0.18s ease;
      white-space: nowrap;
    }
    .filter-btn:hover {
      border-color: var(--warm-brown);
      color: var(--warm-brown);
    }
    .filter-btn.active {
      background: var(--warm-brown);
      color: #FAF7F2;
      border-color: var(--warm-brown);
    }

    /* ── Video grid ── */
    .video-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(270px, 1fr));
      gap: 1.25rem;
      margin-bottom: 3rem;
    }

    .video-card {
      background: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 2px;
      overflow: hidden;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }
    .video-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 10px 28px rgba(92,61,46,0.13);
    }
    .video-card.hidden { display: none; }

    /* Thumbnail / embed wrapper */
    .thumb-wrap {
      position: relative;
      aspect-ratio: 16 / 9;
      background: #CEC3B5;
      overflow: hidden;
      cursor: pointer;
    }
    /* YouTube iframe (hidden until clicked) */
    .thumb-wrap iframe {
      position: absolute;
      inset: 0;
      width: 100%;
      height: 100%;
      border: none;
      display: none;
    }
    .thumb-wrap.playing iframe { display: block; }
    .thumb-wrap.playing .thumb-overlay { display: none; }

    .thumb-overlay {
      position: absolute;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(160deg, rgba(92,61,46,0.08) 0%, rgba(36,18,8,0.22) 100%);
    }
    .play-btn {
      width: 52px;
      height: 52px;
      background: rgba(92,61,46,0.82);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: transform 0.15s ease, background 0.15s ease;
    }
    .thumb-wrap:hover .play-btn {
      background: var(--warm-brown);
      transform: scale(1.08);
    }
    .play-btn::after {
      content: '';
      border-left: 18px solid #FAF7F2;
      border-top: 10px solid transparent;
      border-bottom: 10px solid transparent;
      margin-left: 5px;
    }

    /* Badges */
    .badge {
      position: absolute;
      font-size: 9px;
      font-weight: 700;
      letter-spacing: 1.5px;
      text-transform: uppercase;
      padding: 3px 8px;
      border-radius: 1px;
    }
    .badge-year {
      top: 8px;
      left: 8px;
      background: var(--warm-brown);
      color: #FAF7F2;
    }
    .badge-tag {
      top: 8px;
      right: 8px;
      background: rgba(184,137,46,0.88);
      color: #241208;
    }

    .card-body {
      padding: 0.875rem 1rem 1rem;
    }
    .card-title {
      font-family: 'Playfair Display', serif;
      font-size: 15px;
      font-weight: 600;
      color: var(--text-dark);
      line-height: 1.35;
      margin-bottom: 5px;
    }
    .card-meta {
      font-size: 12px;
      color: var(--text-muted);
      font-weight: 300;
    }

    /* ── No results message ── */
    .no-results {
      display: none;
      grid-column: 1 / -1;
      text-align: center;
      padding: 2.5rem;
      color: var(--text-muted);
      font-style: italic;
      font-size: 15px;
    }

    /* ── Photo albums grid ── */
    .album-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 1rem;
      margin-bottom: 3rem;
    }
    .album-card {
      background: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 2px;
      overflow: hidden;
      text-decoration: none;
      display: block;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }
    .album-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 22px rgba(92,61,46,0.12);
    }
    .album-thumb {
      aspect-ratio: 4 / 3;
      background: #D5C9BB;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      overflow: hidden;
    }
    .album-thumb-inner {
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-template-rows: 1fr 1fr;
      width: 100%;
      height: 100%;
    }
    .album-thumb-cell {
      background: #C9BBAA;
      border: 1px solid var(--cream);
    }
    .album-thumb-cell:nth-child(2) { background: #BFB09E; }
    .album-thumb-cell:nth-child(3) { background: #C4B5A3; }
    .album-thumb-cell:nth-child(4) { background: #B8A891; }
    .album-decade {
      position: absolute;
      bottom: 8px;
      left: 8px;
      background: rgba(36,18,8,0.65);
      color: #FAF7F2;
      font-size: 10px;
      font-weight: 700;
      letter-spacing: 2px;
      padding: 3px 9px;
      border-radius: 1px;
    }
    .album-body {
      padding: 0.75rem 0.875rem;
    }
    .album-title {
      font-family: 'Playfair Display', serif;
      font-size: 14px;
      font-weight: 600;
      color: var(--text-dark);
      margin-bottom: 3px;
    }
    .album-count {
      font-size: 11px;
      color: var(--text-muted);
      font-weight: 300;
    }

    /* ── Footer ── */
    .site-footer {
      text-align: center;
      padding-top: 2.5rem;
      border-top: 1px solid var(--border);
      margin-top: 1rem;
    }
    .site-footer p {
      font-size: 12px;
      color: var(--text-muted);
      font-style: italic;
      line-height: 1.8;
    }

    /* ── Responsive ── */
    @media (max-width: 600px) {
      .masthead { padding: 2rem 0.5rem 1.75rem; }
      .video-grid { grid-template-columns: 1fr; }
      .album-grid { grid-template-columns: repeat(2, 1fr); }
    }
    .banner {
  width: 100%;
  max-height: 320px;
  object-fit: cover;
  object-position: center;
  display: block;
}
  </style>
</head>
<body>
<div class="page">
  <img src="banner.jpg" alt="Hunn Family Archive" class="banner" />

  <!-- ═══════════════════════════════
       MASTHEAD
       Change the subtitle to whatever
       date range suits your archive.
  ═══════════════════════════════ -->
  <header class="masthead">
    <p class="masthead-eyebrow">Family Archive</p>
    <h1 class="masthead-title">The <em>Hunn</em> Family</h1>
    <p class="masthead-sub">A living record &nbsp;·&nbsp; 1930s to present</p>
    <div class="masthead-ornament"><span>❧</span></div>
  </header>


  <!-- ═══════════════════════════════
       VIDEO SECTION
  ═══════════════════════════════ -->
  <section id="videos">
    <p class="section-label">Home Movies &amp; Video</p>

    <!-- Filter by type -->
    <div class="filter-bar" id="tag-filters">
      <button class="filter-btn active" data-tag="all">All</button>
      <button class="filter-btn" data-tag="Oral History">Oral History</button>
      <button class="filter-btn" data-tag="Home Video">Home Video</button>
      <button class="filter-btn" data-tag="Photos">Photos</button>
      <button class="filter-btn" data-tag="Documents">Documents</button>
    </div>

    <!-- ═══════════════════════════════════════════════════
         ADD VIDEOS HERE
         Copy one <div class="video-card"> block,
         paste it below, and fill in these things:

         1. data-tag="Oral History"   ← one of: Oral History,
                                         Home Video, Photos, Documents
         2. data-youtube-id="XXXXX"  ← the ID from your
            unlisted YouTube URL:
            https://youtu.be/XXXXXXXXXXX
         3. The year badge text
         4. The tag badge text (same as data-tag)
         5. Card title and meta line (format · duration)
    ════════════════════════════════════════════════════ -->
    <div class="video-grid" id="video-grid">

      <!-- Grandma & Grandpa — Oral History -->
<div class="video-card" data-tag="Oral History">
  <div class="thumb-wrap" data-youtube-id="Y4FzQP7gJs8">
    <img src="https://img.youtube.com/vi/Y4FzQP7gJs8/hqdefault.jpg" alt="Grandma &amp; Grandpa Hunn" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;" />
    <div class="thumb-overlay">
      <div class="play-btn"></div>
    </div>
    <iframe allowfullscreen allow="autoplay; encrypted-media"></iframe>
    <span class="badge badge-year">2021</span>
    <span class="badge badge-tag">Oral History</span>
  </div>
  <div class="card-body">
    <p class="card-title">How Grandma & Grandpa Met</p>
    <p class="card-meta">Video interview &nbsp;·&nbsp; 3 min &nbsp;·&nbsp; Recalls 1930s–40s</p>
  </div>
</div>

    <!-- Grandma — Home Video -->
<div class="video-card" data-tag="Home Video">
  <div class="thumb-wrap" data-youtube-id="XVysuppELw8">
    <img src="https://img.youtube.com/vi/XVysuppELw8/hqdefault.jpg" alt="Grandma reads A Cup of Christmas Tea" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;" />
    <div class="thumb-overlay">
      <div class="play-btn"></div>
    </div>
    <iframe allowfullscreen allow="autoplay; encrypted-media"></iframe>
    <span class="badge badge-year">2023</span>
    <span class="badge badge-tag">Home Video</span>
  </div>
  <div class="card-body">
    <p class="card-title">Grandma reads &#8220;A Cup of Christmas Tea&#8221;</p>
    <p class="card-meta">Home video &nbsp;·&nbsp; 13 min &nbsp;·&nbsp; Dec 26, 2023</p>
  </div>
</div>

<!-- Grandma & Grandpa — Oral History -->
<div class="video-card" data-tag="Oral History">
  <div class="thumb-wrap" data-youtube-id="4n-tfQJ0_jE">
    <img src="https://img.youtube.com/vi/4n-tfQJ0_jE/hqdefault.jpg" alt="Grandma &amp; Grandpa's First Apartment" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;" />
    <div class="thumb-overlay">
      <div class="play-btn"></div>
    </div>
    <iframe allowfullscreen allow="autoplay; encrypted-media"></iframe>
    <span class="badge badge-year">2021</span>
    <span class="badge badge-tag">Oral History</span>
  </div>
  <div class="card-body">
    <p class="card-title">Grandma &amp; Grandpa&#8217;s First Apartment</p>
    <p class="card-meta">Video interview &nbsp;·&nbsp; 2 min &nbsp;·&nbsp; Recalls 1950s</p>
  </div>
</div>

<!-- July 4, 2022 — Home Video -->
<div class="video-card" data-tag="Home Video">
  <div class="thumb-wrap" data-youtube-id="pBB-gwtBe3E">
    <img src="https://img.youtube.com/vi/pBB-gwtBe3E/hqdefault.jpg" alt="July 4, 2022" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;" />
    <div class="thumb-overlay">
      <div class="play-btn"></div>
    </div>
    <iframe allowfullscreen allow="autoplay; encrypted-media"></iframe>
    <span class="badge badge-year">2022</span>
    <span class="badge badge-tag">Home Video</span>
  </div>
  <div class="card-body">
    <p class="card-title">July 4, 2022</p>
    <p class="card-meta">Home video &nbsp;·&nbsp; 2 min</p>
  </div>
</div>

<!-- Grandma & Grandpa — Oral History -->
<div class="video-card" data-tag="Oral History">
  <div class="thumb-wrap" data-youtube-id="4JOO0Te8XQg">
    <img src="https://img.youtube.com/vi/4JOO0Te8XQg/hqdefault.jpg" alt="26 Lennox Ave." style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;" />
    <div class="thumb-overlay">
      <div class="play-btn"></div>
    </div>
    <iframe allowfullscreen allow="autoplay; encrypted-media"></iframe>
    <span class="badge badge-year">2021</span>
    <span class="badge badge-tag">Oral History</span>
  </div>
  <div class="card-body">
    <p class="card-title">26 Lennox Ave.</p>
    <p class="card-meta">Video interview &nbsp;·&nbsp; 1 min &nbsp;·&nbsp; Recalls 1940s</p>
  </div>
</div>

      <!-- No results message (shown automatically by JS) -->
      <p class="no-results" id="no-results">No videos match that filter.</p>

    </div><!-- /video-grid -->
  </section>


  <!-- ═══════════════════════════════
       PHOTO ALBUMS SECTION
  ═══════════════════════════════ -->
  <section id="photos">
    <p class="section-label">Photo Albums</p>

    <!-- ═══════════════════════════════════════════════════
         ADD ALBUMS HERE
         Copy one <a class="album-card"> block,
         paste it below, and fill in:

         1. href="https://photos.google.com/..."
            ← your Google Photos shared album link
         2. The decade label inside .album-decade
         3. The album title inside .album-title
         4. The photo count inside .album-count
    ════════════════════════════════════════════════════ -->
    <div class="album-grid">

      <!-- EXAMPLE ALBUM 1 — replace href with your Google Photos link -->
      <a class="album-card" href="YOUR_GOOGLE_PHOTOS_LINK_HERE" target="_blank" rel="noopener">
        <div class="album-thumb">
          <div class="album-thumb-inner">
            <div class="album-thumb-cell"></div>
            <div class="album-thumb-cell"></div>
            <div class="album-thumb-cell"></div>
            <div class="album-thumb-cell"></div>
          </div>
          <span class="album-decade">1950s</span>
        </div>
        <div class="album-body">
          <p class="album-title">The Early Years</p>
          <p class="album-count">— photos</p>
        </div>
      </a>

    </div><!-- /album-grid -->
  </section>


  <!-- ═══════════════════════════════
       FOOTER
  ═══════════════════════════════ -->
  <footer class="site-footer">
    <p>This archive is for the Hunn family.<br>
    Please don't share this link publicly.<br>
    <em>Built with love &amp; careful preservation.</em></p>
  </footer>

</div><!-- /page -->


<script>
  // ── Play videos inline on click ──────────────────────────────
  document.querySelectorAll('.thumb-wrap').forEach(wrap => {
    wrap.addEventListener('click', () => {
      const id = wrap.dataset.youtubeId;
      if (!id || id === 'YOUR_YOUTUBE_ID_HERE') return;
      const iframe = wrap.querySelector('iframe');
      iframe.src = 'https://www.youtube.com/embed/' + id + '?autoplay=1&rel=0';
      wrap.classList.add('playing');
    });
  });

  // ── Filter logic ──────────────────────────────────────────────
  document.getElementById('tag-filters').addEventListener('click', e => {
    if (!e.target.matches('.filter-btn')) return;
    document.querySelectorAll('#tag-filters .filter-btn').forEach(b => b.classList.remove('active'));
    e.target.classList.add('active');
    const activeTag = e.target.dataset.tag;
    let visible = 0;
    document.querySelectorAll('.video-card').forEach(card => {
      const match = activeTag === 'all' || card.dataset.tag === activeTag;
      card.classList.toggle('hidden', !match);
      if (match) visible++;
    });
    document.getElementById('no-results').style.display = visible === 0 ? 'block' : 'none';
  });
</script>

</body>
</html>
