# Palm City Funnel

A 2-step landing page funnel for Palm City Agro-Real Estate.
Built for Vercel. Static HTML, CSS, and vanilla JavaScript.

---

## 📁 File Structure

```
palm-city-funnel/
│
├── css/
│   └── style.css          ← Shared stylesheet (edit once, applies to all pages)
│
├── images/                ← Site imagery (logo, estate photos, OG share image)
│
├── landing-a.html         ← Variant A: Income-focused
├── landing-b.html         ← Variant B: Legacy-focused
├── landing-c.html         ← Variant C: Scarcity / social proof
├── step2.html             ← Second step: Video + WhatsApp (shared by all variants)
│
└── README.md              ← You are here
```

---

## 🚀 Quick Start (Deploy to Vercel)

### Option 1: Drag & Drop (Easiest)
1. Go to [vercel.com](https://vercel.com) and sign in
2. Click **"Add New..." → "Project"**
3. Select **"Import Git Repository"** or use the **Vercel CLI**
4. Or simply drag this entire `palm-city-funnel` folder into the Vercel dashboard
5. Your site will be live in seconds at `https://your-project.vercel.app`

### Option 2: Vercel CLI
```bash
# Install Vercel CLI if you haven't
npm i -g vercel

# Navigate to this folder
cd palm-city-funnel

# Deploy
vercel --prod
```

### Option 3: GitHub + Vercel (Recommended for teams)
1. Push this folder to a GitHub repository
2. Connect the repo to Vercel
3. Every push auto-deploys

---

## ✏️ What to Edit

### 1. Prices
Find and replace in **all three landing pages**:
- `landing-a.html`
- `landing-b.html`
- `landing-c.html`

Look for these lines (marked with `EDIT THIS` comments):
```html
<div class="price">₦9,000,000</div>   ← Hectare price
<div class="price">₦3,600,000</div>   ← Acre price
```

### 2. WhatsApp Number
In `step2.html`, find:
```html
<a class="btn btn-whatsapp" href="https://wa.me/234XXXXXXXXXX?text=...">
```
Replace `234XXXXXXXXXX` with your actual WhatsApp number.
- Include country code (234 for Nigeria)
- No spaces, no + sign
- Example: `2348031234567`

You can also customize the pre-filled message after `text=`.

### 3. Video Embed
In `step2.html`, find the `loadVideo()` function and replace:
```javascript
container.innerHTML = `
  <iframe class="video-embed" src="about:blank" ...>
```
Replace `about:blank` with your actual video URL:
- **Vimeo:** `https://player.vimeo.com/video/YOUR_VIDEO_ID`
- **YouTube:** `https://www.youtube.com/embed/YOUR_VIDEO_ID`
- **Wistia:** `https://fast.wistia.net/embed/iframe/YOUR_VIDEO_ID`

Or replace the entire `video-wrap` div with your platform's embed code.

### 4. Social Proof Numbers
In all landing pages, find and update:
- Investor count: `1,000+`
- Acreage: `4,000`
- Seedling capacity: `73,000`
- Allocation percentage: `75%`

### 5. Testimonials
In `landing-c.html`, find the `.testimonial` blocks and replace with real quotes.

### 6. Open Graph / Social Sharing
In the `<head>` of each page, update:
```html
<meta property="og:url" content="https://your-domain.vercel.app/landing-a.html">
<meta property="og:image" content="https://your-domain.vercel.app/images/og-image.jpg">
```

### 7. Colors & Branding
Edit `css/style.css` at the top:
```css
:root {
  --color-primary:   #111111;   ← Main buttons, headings
  --color-positive:  #2e7d32;   ← Success badges, checkmarks
  --color-warning:   #f9a825;   ← Warning boxes, urgency
  --color-whatsapp:  #25d366;   ← WhatsApp button
  /* ... etc */
}
```

---

## 🔗 Page Links

| Page | URL | Use Case |
|------|-----|----------|
| Landing A | `/landing-a.html` | Facebook Ad Set A — Income-focused |
| Landing B | `/landing-b.html` | Facebook Ad Set B — Legacy-focused |
| Landing C | `/landing-c.html` | Facebook Ad Set C — Scarcity/social proof |
| Step 2 | `/step2.html` | Video + WhatsApp (shared by all) |

**Facebook Ad URLs:**
```
https://your-domain.vercel.app/landing-a.html
https://your-domain.vercel.app/landing-b.html
https://your-domain.vercel.app/landing-c.html
```

---

## 📊 Connecting the Form (Lead Capture)

The form currently shows a success message but **does not send data anywhere**.
You need to connect it to one of these:

### Option A: Google Forms (Free, easiest)
1. Create a Google Form with 3 fields: Name, Email, Phone
2. Get the pre-filled link
3. Replace the `submitForm()` function with a Google Form submit via fetch or iframe

### Option B: Typeform / Tally (No-code)
1. Create a form on Typeform or Tally
2. Embed it in place of the current form HTML
3. Set the redirect URL to `step2.html`

### Option C: Webhook to your CRM
Replace the `submitForm()` function in each landing page:

```javascript
function submitForm(suffix) {
  const name  = document.getElementById('name' + suffix).value.trim();
  const email = document.getElementById('email' + suffix).value.trim();
  const phone = document.getElementById('phone' + suffix).value.trim();

  if (!name || !email || !phone) {
    alert('Please fill in all fields.');
    return;
  }

  // EDIT THIS: Replace with your webhook URL
  fetch('https://your-crm.com/webhook/endpoint', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      name,
      email,
      phone,
      page: 'landing-' + suffix.toLowerCase(),
      timestamp: new Date().toISOString()
    })
  })
  .then(() => {
    document.getElementById('form' + suffix).style.display = 'none';
    document.getElementById('success' + suffix).style.display = 'block';
    document.getElementById('success' + suffix).scrollIntoView({ behavior: 'smooth', block: 'center' });
  })
  .catch(err => {
    alert('Something went wrong. Please try again.');
    console.error(err);
  });
}
```

### Option D: Email Service (Mailchimp, ConvertKit, etc.)
Use their HTML embed form and replace the form section in each landing page.

---

## 🧪 A/B Testing Setup

1. Create **3 ad sets** in Facebook Ads Manager
2. Each ad set uses a different landing page URL:
   - Ad Set A → `landing-a.html`
   - Ad Set B → `landing-b.html`
   - Ad Set C → `landing-c.html`
3. Track these metrics in Facebook Pixel / Google Analytics:
   - **PageView** on each landing page
   - **Lead** event on form submission
   - **Contact** event on WhatsApp click (add to step2.html)

### Adding Facebook Pixel
Add this to the `<head>` of every page:
```html
<!-- Facebook Pixel Code -->
<script>
!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};
if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];
s.parentNode.insertBefore(t,s)}(window,document,'script',
'https://connect.facebook.net/en_US/fbevents.js');
fbq('init', 'YOUR_PIXEL_ID');
fbq('track', 'PageView');
</script>
```

Then track the lead event in `submitForm()`:
```javascript
fbq('track', 'Lead', {
  content_name: 'palm-city-presentation-request',
  content_category: 'landing-' + suffix.toLowerCase()
});
```

---

## 🖼️ Adding Images

1. Create an `images/` folder in this directory
2. Add your images:
   - `images/og-image.jpg` — Social sharing preview (1200×630px)
   - `images/plantation-aerial.jpg` — Video thumbnail background
   - `images/logo.png` — Your company logo
3. Reference them in HTML:
   ```html
   <img src="images/plantation-aerial.jpg" alt="Palm City Estate">
   ```

---

## 📱 Responsive Design

All pages are fully responsive and work on:
- Desktop (720px max-width centered)
- Tablet
- Mobile (stacked layouts, larger tap targets)

Test by resizing your browser or using Chrome DevTools.

---

## ⚡ Performance Tips

1. **Compress images** before uploading (use TinyPNG)
2. **Lazy-load the video** — already implemented (loads on click)
3. **Use a CDN for video** — Vimeo, Wistia, or Bunny.net
4. **Enable Vercel Analytics** in your project dashboard

---

## 🛠️ Troubleshooting

| Issue | Fix |
|-------|-----|
| Styles not loading | Check that `css/style.css` path is correct (case-sensitive) |
| WhatsApp link not working | Make sure number has country code, no spaces |
| Form not submitting | Connect to a real form backend (see "Connecting the Form") |
| Video not playing | Replace `about:blank` with real video URL |
| Pages look different | All pages share `css/style.css` — edit there |

---

## 📄 License

This is a private funnel for Palm City / Xymbolic Development Ltd.
Not for resale or redistribution.

---

## 📞 Need Help?

- **Vercel docs:** [vercel.com/docs](https://vercel.com/docs)
- **Facebook Pixel:** [developers.facebook.com/docs/meta-pixel](https://developers.facebook.com/docs/meta-pixel)
- **WhatsApp API:** [business.whatsapp.com/products/business-platform](https://business.whatsapp.com/products/business-platform)

---

*Built for Palm City by Xymbolic Development Ltd & Yieldrise Africa Ltd.*
