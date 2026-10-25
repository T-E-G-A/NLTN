# Nigerian LegalTech Network (NLTN) website

The website of the Nigerian LegalTech Network, *the centre of Nigeria's legal tech ecosystem*.

The whole site is **one file, `index.html`**. Its styles, scripts, content, policies and the LTIC 2.0 poster are all inside it. There is no build step, framework or package to install. Open it in a browser, or upload it to any static host.

---

## Contents

- [What's on the site](#whats-on-the-site)
- [Hosting](#hosting)
- [Editing content](#editing-content)
  - [Add a resource](#add-a-resource)
  - [Search](#search)
  - [Add an event](#add-an-event)
  - [Change the homepage report preview](#change-the-homepage-report-preview)
  - [Partner logos](#partner-organisations)
  - [Policies](#policies)
- [Shareable links](#shareable-links)
- [How previews work](#how-previews-work)
- [Cookies](#cookies)
- [Brand rules](#brand-rules)
- [Before you publish a change](#before-you-publish-a-change)
- [Known limits](#known-limits)

---

## What's on the site

| Section | What it shows |
|---|---|
| **Hero** | Tagline, intro and a live preview of the latest report |
| **Partner organisations** | Partner logos |
| **About us** | 01 From the Founder's Desk · 02 Our mission · 03 The Five Ps · 04 Recognition and milestones |
| **Events** | Upcoming and past events as cards, each opening a full write-up |
| **Resources** | Reports, articles, guides, events and news with live previews, search, filters and "Show more" |
| **Footer** | LinkedIn, Instagram, email, Privacy policy, Terms of use, Cookie policy, Cookie settings |

Everything that opens in the pop-up viewer stays on the site. That covers documents, event write-ups, the registration form and the policies. Only LinkedIn and Instagram open in a new tab, because those sites refuse to be shown inside another page.

---

## Hosting

Any static host works: GitHub Pages, Netlify, Cloudflare Pages or your web host. Upload `index.html` and nothing else.

**GitHub Pages:**
1. Create a repository and upload `index.html` to it.
2. Go to **Settings → Pages** and set the source to the `main` branch, root folder.
3. To use your own domain, add `nigerianlegaltech.com` under **Custom domain** and follow GitHub's DNS instructions.

**Share preview (open graph image):** social platforms need the share image at a public web address, so it can't live inside the HTML file.
1. Upload `og-image.png` (1200 × 630) to your image host, such as ibb.co.
2. Near the top of `index.html`, replace the image link in **both** `og:image` and `twitter:image` with the new link.
3. Check the result with LinkedIn's Post Inspector or opengraph.xyz.

The `canonical` and `og:url` tags assume the site lives at `https://nigerianlegaltech.com/`. Change them if it lives somewhere else.

---

## Editing content

All content lives in the `<script>` near the bottom of `index.html`. Each kind of content is a list you add a line to. Search the file for the name shown in each heading below.

### Add a resource

Search for `const RESOURCES`.

1. Upload the file to Google Drive.
2. Set its sharing to **Anyone with the link → Viewer**. If you skip this, the preview won't show.
3. Copy the file ID from its link: `https://drive.google.com/file/d/`**`THIS_PART`**`/view`.
4. Add a line to the list:

```js
{id:"PASTE_FILE_ID", tag:"article", date:"2026-11-03", title:"Your Title Here", summary:"One sentence on what it covers.", keywords:"law or regulation, organisations, people, topics"},
```

| Field | Notes |
|---|---|
| `id` | The Drive file ID |
| `tag` | `report`, `article`, `guide`, `event` or `news`. This sets the filter it appears under |
| `date` | `YYYY-MM-DD`. Resources are sorted newest first. Shown as "Nov 2026"; add `day:true` to show "3 Nov 2026" |
| `title` | Also becomes the resource's shareable link (see [Shareable links](#shareable-links)) |
| `summary` | One short sentence shown under the title |
| `keywords` | Hidden search terms: the laws, organisations, people and topics the document covers, separated by commas. Visitors never see them, but search uses them (see [Search](#search)) |

Filter counts, numbering and "Show more" update automatically. Nine resources show at first; to change that, edit `const PAGE = 9;`.

### Search

The search box next to the filters looks at each resource's **title, summary, type, date and hidden `keywords`**. It cannot read the text inside the Drive documents, which is why the keywords matter. If a document discusses the NDPA, Section 84 or a named firm, put those terms in its keywords so people can find it.

How it behaves:
- Results update as you type. Case and accents are ignored, and "AI-generated" matches "AI generated".
- If several words are typed, a resource must match **all** of them.
- Title matches rank first; ties stay newest first.
- Matching words are underlined in Greenstone in titles and summaries.
- Search and filters combine (e.g. "AI" within Guides), and each filter's count shows how many matches it has.
- When nothing matches, a message offers a "Clear search" button.
- Searches can be shared: `/#resources?q=arbitration` opens the page at Resources with that search filled in. The address updates as you search.

### Add an event

Search for `const EVENTS`. Each event shows as a card in **Upcoming** or **Past events**.

```js
{id:"my-event", status:"upcoming", badge:"Registration open", title:"Event name",
 location:"Lagos, Nigeria", when:"12 Dec 2026", image:"https://i.ibb.co/.../photo.jpg",
 summary:"One or two sentences for the card.",
 register:"https://docs.google.com/forms/d/e/FORM_ID/viewform?embedded=true"},
```

| Field | Notes |
|---|---|
| `id` | Short, lowercase, hyphenated. Becomes the link `/#my-event` |
| `status` | `upcoming` or `past`. Move an event to `past` once it has happened |
| `badge` | Label on the image, e.g. "Registration open", "Fireside chat" |
| `image` | Square-friendly image. Optional `imgPos:"center 40%"` adjusts the crop |
| `register` | Optional, upcoming events only. Use a Google Form link ending in `viewform?embedded=true` so it opens on the site. Creates the link `/#my-event-register` |

Each event also needs a write-up in `const POSTS` under the same `id`. Copy an existing one, such as `"spotdraft"`, and edit the text. Write-ups use plain HTML: `<p>`, `<h3>`, a `<dl>` for details, and `<blockquote class="pq">` for quotes.

To list the event under Resources as well, add a line to `RESOURCES` with `kind:"post"`, `tag:"event"`, the same `id`, and `photo` set to the image.

When fewer than three upcoming events are listed, a "More events coming soon" card fills the space automatically.

### Change the homepage report preview

Search for `const HERO_DOC` and replace the ID with the Drive file ID of the report you want to feature. That file must also be in `RESOURCES`. Update the caption under the preview (search for `hero-caption`).

### Partner organisations

Search for `partner-row`. Each logo is one line:

```html
<li><img src="https://i.ibb.co/.../logo.png" alt="Organisation name" loading="lazy"></li>
```

Always put the organisation's name in `alt`, so screen readers can announce it. Three logos currently use the placeholder "Partner organisation logo" until their names are confirmed.

### Policies

Search for `const POLICIES`. The Privacy policy, Terms of use and Cookie policy are each a list of `["Section title", "HTML content"]` pairs. The Contents list and section numbers are generated automatically.

- **Last updated date:** `const UPDATED`
- **Contact emails:** `const PRIVACY` (privacy@nigerianlegaltech.com, used by the privacy and cookie policies) and `const HELLO` (hello@nigerianlegaltech.com, used by the terms)
- **Linking to another policy from inside one:** `<a href="#cookies" data-policy="cookies">Cookie policy</a>`. Use `privacy`, `terms` or `cookies`

> ⚠️ The policies were drafted against the Nigeria Data Protection Act 2023 and GAID 2025, and describe what the site actually does. Have them reviewed by a qualified lawyer before launch, and again if the site starts collecting new data, such as adding analytics or a newsletter signup.

---

## Shareable links

Everything that opens in the viewer has its own address. Opening the link lands with that item already open. The **Copy link** button in the viewer copies it, and the browser's back button closes the viewer.

| Link | Opens |
|---|---|
| `/#privacy` · `/#terms` · `/#cookies` | The policies |
| `/#ltic` | LTIC 2.0 write-up |
| `/#ltic-register` | LTIC 2.0 registration form |
| `/#legal-geek` · `/#spotdraft` | Past event recaps |
| `/#contract-automation-for-nigerian-smes` | Any resource: its title in lowercase, with hyphens |
| `/#resources?q=ransomware` | The Resources section with that search filled in |

Section links (`#about`, `#events`, `#resources`, `#partners`, `#join`) scroll the page as normal.

**Changing a resource's title changes its link,** so links shared earlier will stop working. Fix typos before sharing a link widely.

---

## How previews work

The Resources cards and the homepage report use Google Drive's preview page (`https://drive.google.com/file/d/ID/preview`) inside an iframe. That is the same live viewer that opens when you click a card.

- Each card's preview is created when the card comes near the screen, so the page doesn't load every document at once.
- A transparent button sits over each preview, so clicking anywhere on it opens the full document in the viewer.
- Until a preview loads, the card shows a patterned cover in the brand colours.

If a preview stays blank, the file is almost certainly not shared as **Anyone with the link → Viewer**.

---

## Cookies

- New visitors see a pop-up with **Accept all**, **Decline all** and **Read the full cookie policy**.
- Reading the policy hides the pop-up while the policy is open. If no choice has been made when the policy is closed, the pop-up returns. The pop-up and the viewer never show at the same time.
- The choice is saved in the visitor's browser (`localStorage`, key `nltn-consent`) for 12 months. "Cookie settings" in the footer reopens the pop-up.
- NLTN sets no tracking, analytics or advertising cookies. Google may set cookies through the document previews and forms. Previews load whatever the visitor chooses, and the cookie policy says so.

**If you add analytics or any other optional cookies,** only load them when the visitor has accepted (`consent === "accepted"`), and update the cookie policy.

---

## Brand rules

From the NLTN Brand Identity System v1.0. Keep to these when editing.

| Token | Value | Use |
|---|---|---|
| Carbon | `#010101` | Text, logomark, principal interface elements |
| Limestone | `#FCFCFC` | Main background |
| Greystone | `#979799` | Secondary text, hover states. Use sparingly |
| Greenstone | `#2AACA3` | Highlights, underlines, small accents only. Never large fills or headlines |

- **Type:** Zalando Sans for everything, Geist Mono for small labels.
- **Icons:** Material Symbols Sharp, outline style.
- **Buttons:** white with a Greystone border and Carbon text, turning light grey (`#E9E9EA`) on hover. Use the solid Carbon button only for the main action.
- **Logos:** use only the approved logo files. Don't stretch, recolour, add effects or rebuild them.

---

## Before you publish a change

- [ ] New Drive files are shared as **Anyone with the link → Viewer**
- [ ] Previews appear for new resources
- [ ] New resources have keywords, and searching a key term finds them
- [ ] Filters and "Show more" show the right counts
- [ ] Event cards and write-ups open, and registration opens the form on the site
- [ ] Each policy opens, and its Contents links jump within the policy
- [ ] The page fits on a phone with no sideways scrolling
- [ ] Shared links (`/#privacy`, `/#ltic` and so on) open the right item

---

## Known limits

- **Google Drive's toolbar:** the viewer shows Google's own small "open in Drive" button, which can't be removed from an embedded preview.
- **Dates:** most resource dates come from the Drive file dates, not the original publish dates. Correct them in `RESOURCES` where you know the real date.
- **SpotDraft session date:** listed as May 2026, taken from the photo's upload timestamp. Confirm and correct it.
- **Files not listed:** "The Evolution of Claude for Legal Professionals" isn't listed, because the document only contains a LinkedIn link.
- **Remote images:** logos and photos load from ibb.co. If that host goes down, they won't show. The LTIC poster is built into the file, so it is unaffected.

---

© 2026 Nigerian LegalTech Network. All rights reserved.
