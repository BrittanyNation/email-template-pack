# Responsive HTML Email Template Pack

Hand-coded, production-style HTML email templates built with table-based layout and inline CSS. No framework output, no drag-and-drop builder exports. Every template is written to survive the way real email clients actually render HTML, including Outlook on Windows, Gmail (web and mobile), Apple Mail, and mobile clients with dark mode enabled.

## Templates

| File | Purpose |
| --- | --- |
| `templates/welcome.html` | Welcome / onboarding email with a getting-started steps block and a single clear call to action |
| `templates/newsletter.html` | Newsletter with a featured story, two-column article blocks, and a quick-tips section |
| `templates/promotional.html` | Promotional email with an announcement bar, hero image, offer block, and a two-product grid |

## Techniques used

**Table-based layout.** All structure is built on nested tables with `role="presentation"`, `cellpadding="0"`, and `cellspacing="0"`. This is still the only layout method that renders consistently across every major email client.

**Inline CSS.** All critical styling is inlined on the elements themselves. The `<style>` block in the head only carries what cannot be inlined: media queries, dark-mode rules, and client reset styles.

**Outlook handling.**
- MSO conditional comments (`<!--[if mso]>`) wrap fixed-width "ghost tables" so Outlook on Windows holds the 600px container instead of stretching it full width.
- Buttons are built as padded table cells with an MSO conditional spacer, so they keep their padding and rounded look in Outlook instead of collapsing.
- An OfficeDocumentSettings block sets 96 DPI so images are not scaled unexpectedly.

**Responsive behavior.** A 620px breakpoint stacks multi-column blocks full width, scales images fluidly (`width:100%; height:auto` with a max-width), and increases tap target sizes on small screens. Columns use `dir` attributes and `font-size:0` wrappers where needed to avoid whitespace gaps.

**Dark mode.**
- `color-scheme` and `supported-color-schemes` meta tags declare support so clients do not force their own inversion.
- `@media (prefers-color-scheme: dark)` rules restyle backgrounds and text for Apple Mail and other clients that honor them.
- Outlook.com / Outlook app overrides are included via `[data-ogsc]` and `[data-ogsb]` selectors.
- Images that must stay unchanged in dark mode can be locked with a transparent outline trick where noted in comments.

**Accessibility.** Semantic heading order, real text instead of image-only content, descriptive `alt` text on every image, sufficient color contrast in both modes, and a hidden preheader that doubles as an inbox preview and a screen-reader-friendly summary.

## Customizing

1. Replace the placeholder brand name, logo text, and footer address with your own.
2. Swap placeholder images (placehold.co URLs) for hosted images on your CDN or ESP media library. Always keep the `width`, `height`, and `alt` attributes accurate.
3. Replace `#` links with real URLs. Footer links for unsubscribe, preferences, and view-in-browser should use your ESP's merge tags, for example `*|UNSUB|*` (Mailchimp), `{{unsubscribe_url}}` (Klaviyo / Customer.io style), or the SFMC personalisation string for the unsubscribe link in Salesforce Marketing Cloud.
4. Keep total file size under roughly 100KB of HTML so Gmail does not clip the message.

## Using with Salesforce Marketing Cloud

These templates drop into SFMC Content Builder as HTML content blocks or full paste-in templates. To convert one into a reusable SFMC template:

- Move shared sections (header, footer) into their own content blocks and reference them with `%%=ContentBlockByName("...")=%%` or the block reference AMPscript your account uses.
- Replace static headlines and body copy with named content areas if the template will be edited by non-developers.
- Keep the inline CSS as-is. Content Builder preserves it, and inlining is what keeps rendering consistent once the email is assembled.

## Testing checklist

Before sending anything built from these templates:

- Preview in Litmus or Email on Acid across Outlook (Windows), Gmail app (iOS/Android), Apple Mail, and Yahoo.
- Send real test emails to physical devices, not just previews.
- Check dark mode on at least one Apple Mail and one Outlook client.
- Confirm the email reads clearly with images blocked (alt text carries the message).
- Click every link, including footer and unsubscribe links, in the test send.

## Notes

Placeholder copy and the fictional brand "Lumen & Co." are samples only, written so the templates can be evaluated as layout and code rather than as finished campaigns.
