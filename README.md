# TrackShop – GA4 Practice Store

A static, frontend-only practice store. The seven HTML pages share style.css and script.js. Product art and downloads live in assets/. No framework, package install, server, payment gateway, or Node.js is required to run the website.

## Run and host

Open index.html to explore locally. For consistent localStorage across pages, serve the folder with any static web server or publish it to GitHub Pages. The ZIP linked in the site's footer contains the ready-to-upload source.

For GitHub Pages, extract the ZIP, put the seven HTML files, style.css, script.js, and assets/ at your repository root, then enable Pages from that branch/root. All internal paths are relative, including product sharing, so project subpaths work. The Sites copy is separate from any GitHub deployment you create. See https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site for GitHub's hosting instructions.

## Install GTM

1. Create a Web container in your own Google Tag Manager account.
2. Copy its two real installation snippets. Do not invent a container ID.
3. In EVERY HTML page, paste the script at the marked head comment after window.dataLayer initialization; paste the noscript immediately after the opening body tag.
4. Create your Google tag using your own GA4 web-stream measurement ID.
5. For a first exercise, create a GTM Custom Event trigger named add_to_cart. Create a GA4 event tag named add_to_cart, using your Google tag.
6. Send ecommerce data from the Data Layer. Alternatively, create Version 2 Data Layer Variables for ecommerce.currency, ecommerce.value, and ecommerce.items and map them to currency, value, and items. For purchase, also map ecommerce.transaction_id; optional fields include ecommerce.coupon, ecommerce.shipping, and ecommerce.tax.
7. Connect GTM Preview to your hosted site. Perform an action, inspect the Custom Event, verify the tag, and compare parameters with GA4 DebugView. Publish your container when your test configuration is ready.

The site intentionally has no installed analytics service. The local debug panel shows dataLayer pushes; it cannot confirm delivery to GA4. Use made-up form values. Do not enter personal data in the search box because search_term is an analytics parameter.

## Event reference

| Interaction | dataLayer event | Main parameters |
| --- | --- | --- |
| Navigation | navigation_click | link_text, link_url |
| Hero CTA | cta_click and custom_cta_click | cta_name, element_id |
| Product lists | view_item_list | ecommerce.items, item_list_name |
| Product opened | select_item, view_item | ecommerce.items |
| Cart changes | add_to_cart, remove_from_cart | changed quantity, value, items |
| Cart page | view_cart | ecommerce.currency, value, items |
| Checkout entered | begin_checkout | ecommerce.currency, value, items |
| Shipping confirmed | add_shipping_info | ecommerce.shipping_tier, items |
| Fake method selected | add_payment_info | ecommerce.payment_type, items |
| Valid TRACK10 applied | coupon_applied | coupon, discount_value, currency |
| Confirmation | purchase | ecommerce.transaction_id, value, currency, items |
| Contact | generate_lead | form_id, lead_source |
| Newsletter | sign_up | method |
| Forms | form_start, form_interaction, form_submit | form_id, field_id where relevant |
| Search submission | search | search_term |
| Brochure | download_click and file_download | file_name, file_extension, link_id |
| Instagram link | outbound_click | link_url, link_domain, outbound |
| Playground | custom_cta_click | cta_name, element_id |
| Video simulator | video_start, video_progress, video_pause, video_resume, video_complete | video_title, video_provider, video_percent, video_current_time |
| About-page scroll | scroll_depth | percent_scrolled, page_name |

Canonical names are used directly, without a _demo suffix. Do not create duplicate GA4 tags for click-based triggers and data-layer triggers targeting the same action. The paired CTA/download event names are intentional alternative exercises. Likewise check enhanced measurement to avoid duplicate collection.

The add/remove cart events represent the changed units only. TRACK10 reduces unit price by 10%; purchase.value is the sum of discounted price × quantity. Original unit discount is provided as discount on each item. No shipping or tax is added. Confirmation reloads do not push purchase again for that transaction in this browser.

## Local state

- localStorage: cart, most recent anonymous order, purchase transaction guards.
- sessionStorage: latest 80 debug events and debug-panel open state.
- Form values remain in the form and are never included in order storage or our dataLayer events.
- Clearing browser storage resets the cart, order, and duplicate-purchase guards.
- Browser storage must be enabled for the complete multi-page checkout.

## Learning references

Google's ecommerce guide: https://developers.google.com/analytics/devguides/collection/ga4/ecommerce
Google's recommended event reference: https://developers.google.com/analytics/devguides/collection/ga4/reference/events

Product images are fictional AI-generated demo placeholders. The video interface is a simulator, not a real media player. Forms, bookings, enquiries, payments, and orders are simulations only.
