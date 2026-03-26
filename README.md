# Blablabla
A place where I spin the wheel

# Blablabla
A place where I spin the wheel

---

## 1. Cursor — global HTML
Because nothing says "I spent three weeks on this" like a circle that follows your mouse

> [!WARNING]
> This targets Cargo-specific shadow DOM structure and may not work on other platforms.
```html
<style>
  * { cursor: none; }
  #custom-cursor {
    background: rgba(255, 255, 255, 0.25);
    border: 0.5px solid #000000;
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 99999;
    left: 0;
    top: 0;
    backdrop-filter: blur(6px);
    -webkit-backdrop-filter: blur(6px);
  }
</style>
<div id="custom-cursor"></div>
<script>
  const myCursor = document.getElementById('custom-cursor');
  let posX = 0, posY = 0, mouseX = 0, mouseY = 0;
  let currentSize = 16, targetSize = 16;
  document.addEventListener('mousemove', e => {
    mouseX = e.clientX; mouseY = e.clientY;
    myCursor.style.display = 'none';
    const el = document.elementFromPoint(e.clientX, e.clientY);
    myCursor.style.display = 'block';
    const isLink = el && (el.tagName === 'A' || el.closest('a') || (el.closest('media-item') && el.closest('media-item').classList.contains('linked')));
    targetSize = isLink ? 32 : 16;
  });
  function animateCursor() {
    posX += (mouseX - posX) * 0.10;
    posY += (mouseY - posY) * 0.10;
    currentSize += (targetSize - currentSize) * 0.15;
    myCursor.style.width = currentSize + 'px';
    myCursor.style.height = currentSize + 'px';
    myCursor.style.transform = `translate(${posX - currentSize / 2}px, ${posY - currentSize / 2}px)`;
    requestAnimationFrame(animateCursor);
  }
  animateCursor();
</script>
```

---

## 2. Scroll Blur — global HTML
A frosted gradient that appears when you scroll, making your site look like it costs more than it does

> [!TIP]
> Adjust `height: 40vh` and `blur(12px)` to control the size and intensity of the effect.
```html
<style>
  #blur-overlay {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 40vh;
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    pointer-events: none;
    z-index: 9998;
    -webkit-mask-image: linear-gradient(to top, black 0%, black 40%, transparent 100%);
    mask-image: linear-gradient(to top, black 0%, black 40%, transparent 100%);
    opacity: 0;
    transition: opacity 0.4s ease;
  }
  #blur-overlay.visible { opacity: 1; }
</style>
<div id="blur-overlay"></div>
<script>
  const overlay = document.getElementById('blur-overlay');
  function checkScroll() {
    const scrolled = window.scrollY || document.documentElement.scrollTop || document.body.scrollTop;
    overlay.classList.toggle('visible', scrolled > 0);
  }
  window.addEventListener('scroll', checkScroll);
  document.addEventListener('scroll', checkScroll);
  document.documentElement.addEventListener('scroll', checkScroll);
</script>
```

---

## 3. Image Zoom + Edge Blur — global HTML
Forty-something iterations to make images slightly bigger when you hover over them. Senior UX, that's right.

> [!WARNING]
> This targets Cargo-specific shadow DOM structure and may not work on other platforms.

> [!TIP]
> Adjust `scale(1.04)` and `blur(12px)` to taste.
```html
<script>
  (function() {
    function applyZoom() {
      document.querySelectorAll('media-item.linked').forEach(function(item) {
        if (item.dataset.zoomReady) return;
        item.dataset.zoomReady = 'true';
        if (item.shadowRoot) {
          var style = document.createElement('style');
          style.textContent = [
            'a.sizing-frame { overflow: hidden !important; display: block; position: relative; }',
            'img { transition: transform 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94) !important; transform-origin: center center; }',
            'a.sizing-frame:hover img { transform: scale(1.04) !important; }',
            'a.sizing-frame::after {',
            '  content: "";',
            '  position: absolute;',
            '  inset: 0;',
            '  opacity: 0;',
            '  transition: opacity 0.6s ease;',
            '  -webkit-mask-image: radial-gradient(ellipse at center, transparent 40%, black 100%);',
            '  mask-image: radial-gradient(ellipse at center, transparent 40%, black 100%);',
            '  backdrop-filter: blur(12px);',
            '  -webkit-backdrop-filter: blur(12px);',
            '  pointer-events: none;',
            '}',
            'a.sizing-frame:hover::after { opacity: 1; }',
          ].join(' ');
          item.shadowRoot.appendChild(style);
        }
      });
    }
    setTimeout(applyZoom, 500);
    setTimeout(applyZoom, 1000);
    setTimeout(applyZoom, 2000);
    setTimeout(applyZoom, 4000);
    var observer = new MutationObserver(applyZoom);
    window.addEventListener('load', function() {
      applyZoom();
      observer.observe(document.body, { childList: true, subtree: true });
    });
  })();
</script>
```

---

## 4. Typewriter — global HTML
Spent an afternoon debugging shadow DOM timing issues to make text appear one letter at a time. Design thinking.

> [!NOTE]
> Replace the segments array with your own text content before using.

> [!WARNING]
> This targets Cargo-specific shadow DOM structure and may not work on other platforms.
```html
<script>
  (function() {
    var segments = [
      { type: 'text', content: ' YOUR TEXT ' },
      { type: 'html',  content: '<span style="filter: blur(4px);">BLURRED WORD</span>' },
      { type: 'text', content: ' more text ' },
      { type: 'html',  content: '<br>' },
      { type: 'text', content: 'and ' },
      { type: 'html',  content: '<i>italic word</i>' },
      { type: 'text', content: '.' },
    ];
    var speed = 38;
    function typeSegments(el, segs) {
      el.innerHTML = '';
      var segIndex = 0;
      function nextSegment() {
        if (segIndex >= segs.length) return;
        var seg = segs[segIndex];
        segIndex++;
        if (seg.type === 'html') {
          el.innerHTML += seg.content;
          nextSegment();
        } else {
          var charIndex = 0;
          function typeChar() {
            if (charIndex < seg.content.length) {
              el.innerHTML += seg.content[charIndex];
              charIndex++;
              setTimeout(typeChar, speed);
            } else {
              nextSegment();
            }
          }
          typeChar();
        }
      }
      nextSegment();
    }
    function initTypewriter() {
      var headline = document.querySelector('h1[uses="fit-text"]');
      if (headline) {
        typeSegments(headline, segments);
      } else {
        setTimeout(initTypewriter, 100);
      }
    }
    initTypewriter();
  })();
</script>
```
