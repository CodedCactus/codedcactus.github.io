# CodedCactus

![Image title](assets/logo.png){ align=right }
Welcome! This site is a collection of my work and experiments at the intersection of hardware, software, and curiosity. I spend a lot of time building and modifying things — from smart home systems powered by Home Assistant and ESPHome, to custom 3D-printed parts and electronics projects.

You'll find projects where I reverse-engineer devices, design and print components, and create practical solutions for everyday problems. Many of these builds combine automation, embedded systems, and hands-on prototyping.

This space serves as a project log — a place to document ideas, share what I learn, and explore new challenges along the way.


## Latest Posts

<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

<style>
.article-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
    margin: 20px 0;
}

.article-card {
    border: 1px solid rgba(142, 142, 142, 0.2);
    border-radius: 8px;
    padding: 16px;
    font-family: inherit;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    background: var(--md-default-bg-color);
    transition: box-shadow 0.2s, transform 0.2s;
}

.article-card:hover {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transform: translateY(-2px);
}

.card-header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    text-decoration: none;
}

.card-title {
    font-weight: 700;
    line-height: 1.4;
    color: var(--md-typeset-color, inherit);
    margin-right: 12px;
    font-size: 1.05rem;
}

.card-date {
    font-size: 0.84em;
    color: rgba(100, 100, 100, 0.6);
    white-space: nowrap;
    margin-right: 10px;
    transform: translateY(-1px);
}

.card-header:hover .card-title,
.card-header:focus .card-title {
    color: var(--md-accent-fg-color, var(--md-primary-fg-color));
    text-decoration: underline;
}

.card-body {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-top: 12px;
    gap: 10px;
}

.card-summary {
    flex: 1;
    min-width: 0;
    color: rgba(142, 142, 142, 1);
    font-size: 0.8rem;
    line-height: 1.6;
    letter-spacing: 0.02rem;
    word-break: break-word;
    
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 4;
    overflow: hidden;
}

.card-cover {
    width: 120px;
    height: 80px;
    border-radius: 8px;
    flex-shrink: 0;
    overflow: hidden;
    transition: filter 0.3s ease;
    background: var(--md-default-fg-color--lightest);
}

.card-cover:hover {
    filter: brightness(0.8);
}

.card-cover img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transform: scale(1.05);
    transition: transform 0.4s ease-out;
}

.card-cover:hover img {
    transform: scale(1.1) translateX(4px);
}

[data-md-color-scheme="slate"] .article-card {
    border-color: rgba(142, 142, 142, 0.3);
}

[data-md-color-scheme="slate"] .card-date {
    color: rgba(200, 200, 200, 0.6);
}

[data-md-color-scheme="slate"] .card-summary {
    color: rgba(200, 200, 200, 0.8);
}

[data-md-color-scheme="slate"] .card-cover {
    background: rgba(142, 142, 142, 0.15);
}
</style>

<div id="latest-posts-container" class="article-grid"></div>

<script>
fetch('search.json')
  .then(response => response.json())
  .then(data => {
    const blogPosts = data.items.filter(item => 
      item.location.startsWith('blog/') && 
      item.location.match(/^blog\/\d{4}\/\d{2}\/\d{2}\/[^/]+\/$/)
    );
    
    if (blogPosts.length > 0) {
      const latestPosts = blogPosts.slice(0, 3);
      const container = document.getElementById('latest-posts-container');
      
      const cardsHtml = latestPosts.map(post => {
        const dateMatch = post.text.match(/<time[^>]*>([^<]+)<\/time>/);
        const date = dateMatch ? dateMatch[1] : '';
        
        // Extract first image as cover
        const imgMatch = post.text.match(/<img[^>]+src=["']([^"']+)["']/);
        const cover = imgMatch ? imgMatch[1] : '';
        
        // Extract summary (first paragraph, stripped of tags)
        const textContent = post.text.replace(/<[^>]*>/g, ' ');
        const summary = textContent.trim().substring(0, 200) + '...';
        
        // Parse date for datetime attribute
        let datetime = '';
        if (date) {
          const parsed = new Date(date.replace(/(\w+) (\d+), (\d+)/, '$1 $2, $3'));
          if (!isNaN(parsed.getTime())) {
            datetime = parsed.toISOString().split('T')[0];
          }
        }
        
        return `
          <article class="article-card">
            <a class="card-header" href="${post.location}">
              <div class="card-title">${post.title}</div>
              <time class="card-date"${datetime ? ` datetime="${datetime}"` : ''}>${date}</time>
            </a>
            <div class="card-body">
              <div class="card-summary">${summary}</div>
              ${cover ? `<div class="card-cover"><img src="${cover}" alt="${post.title}" loading="lazy"></div>` : ''}
            </div>
          </article>
        `;
      }).join('');
      
      container.innerHTML = cardsHtml;
    }
  })
  .catch(console.error);
</script>

<!-- ---
## Start here

- Read the blog: [Blog home](blog/index.md)
- Browse all posts: [Archives](blog/archive/2026)

--- -->