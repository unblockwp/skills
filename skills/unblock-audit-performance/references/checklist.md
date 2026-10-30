# Performance checklist

Core Web Vitals at the 75th percentile: LCP 2.5 s or less, INP 200 ms or less, CLS 0.1 or less.
**[code]** from the markup and the CSS, **[browser]** from a trace and the network, **[human]** by a
person.

## Largest Contentful Paint

- [ ] **[browser]** The LCP element and its time come from a trace, with the breakdown: server
  response, load delay, load duration, render delay.
- [ ] **[code]** The LCP image is an `<img>` in the HTML, not a CSS background.
- [ ] **[code]** The LCP image has `fetchpriority="high"` and no `loading="lazy"`.
- [ ] **[code]** It has `srcset` and `sizes`, so a phone gets a small file.
- [ ] **[browser]** Its file weight fits its displayed size.
- [ ] **[browser]** Server response under 0.8 s, with the page cache on.

## Layout shifts

- [ ] **[code]** Every image and embed has `width` and `height`, or an `aspect-ratio`.
- [ ] **[browser]** No shift when the fonts load.
- [ ] **[browser]** Nothing is inserted above the content after it shows: banners, notices, embeds.

## Interaction

- [ ] **[browser]** No task over 50 ms while the page loads, from block scripts or others.
- [ ] **[browser]** Opening a menu, a tab or an accordion answers at once.
- [ ] **[code]** Block scripts run only where their component is, and use native elements when HTML
  can do the job.

## Images

- [ ] **[code]** Images below the fold have `loading="lazy"`.
- [ ] **[code]** Images come from the media library (`data-unbk-attachment`), in WebP or AVIF where
  the site produces them.
- [ ] **[browser]** No image is much larger than its displayed size.

## Fonts

- [ ] **[code]** Two families at most, and only the weights used.
- [ ] **[browser]** The font files come from the site itself.
- [ ] **[browser]** Text shows while fonts load.

## CSS and scripts

- [ ] **[code]** Rules marked `scope:global` are only the ones that need it (base styles, classes
  added by scripts or only inside conditions): they load on every page.
- [ ] **[browser]** No render-blocking script in the `head` that could be deferred.
- [ ] **[browser]** The scripts and styles of the theme, other plugins and third parties, listed with
  their weight.

## Network

- [ ] **[browser]** No 404 and no failed request.
- [ ] **[browser]** The total weight and the number of requests of the page, mobile first.

## Real visits

- [ ] **[human]** Field data (PageSpeed Insights, Search Console) once the site has traffic.
- [ ] **[human]** Hosting, page cache and CDN set up by whoever runs the server.
