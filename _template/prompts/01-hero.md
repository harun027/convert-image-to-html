Design a sticky site header plus a Hero section as plain HTML with Tailwind utility classes only. Every class below is exact: use it verbatim, never substitute, never add one not listed.

FOCUS RING = focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-neutral-400

LAYOUT — Sticky header with logo, desktop nav and mobile menu, then a two-column hero: copy left, placeholder right.

SPACING (exactly these, no ranges):
- Header row px-6, py-4, lg:px-8. Section py-16, md:py-24, lg:py-32
- Container mx-auto, max-w-7xl, px-6, lg:px-8. Hero gap-10, md:gap-12, lg:gap-16
- Left column: badge→h1 mt-6, h1→paragraph mt-6, paragraph→buttons mt-8, buttons→logos mt-12
- Button row gap-3. Logo row gap-x-8, gap-y-4

HEADER:
- header: sticky, top-0, z-40, border-b, border-neutral-200, bg-white/80, backdrop-blur, text-neutral-900
- Inner row: mx-auto, max-w-7xl, flex, items-center, justify-between, gap-6
- Logo link (flex, items-center, gap-2, text-base font-semibold tracking-tight): a square mark (grid, size-8, place-items-center, rounded-lg, bg-neutral-900, text-white) then "Acme"
- Desktop nav (hidden, lg:block): a ul (flex, items-center, gap-8, text-sm, text-neutral-500) with three links "Produk", "Harga", "Dokumentasi", each transition-colors hover:text-neutral-900
- Desktop actions (hidden, lg:flex, items-center, gap-3): ghost link "Masuk" (inline-flex, min-h-11, items-center, px-3, text-sm font-medium, text-neutral-500, transition-colors, hover:text-neutral-900, FOCUS RING) and solid "Mulai gratis" (inline-flex, min-h-11, items-center, rounded-lg, bg-neutral-900, px-5, text-sm font-medium, text-white, transition-opacity, hover:opacity-90, FOCUS RING)
- Mobile menu, CSS-only: a details (lg:hidden). Summary is the trigger (grid, size-11, cursor-pointer, list-none, place-items-center, rounded-lg, marker:content-none, hover:bg-neutral-100, FOCUS RING) holding the Lucide "menu" icon at size-6, aria-label "Buka menu". The panel is a nav (absolute, inset-x-0, top-full, border-b, border-neutral-200, bg-white, px-6, py-4) with a ul (flex, flex-col, gap-1, text-sm) repeating the three links (block, rounded-lg, px-3, py-3, hover:bg-neutral-100), then a "Mulai gratis" button (mt-4, inline-flex, min-h-11, w-full, items-center, justify-center, rounded-lg, bg-neutral-900, px-5, text-sm font-medium, text-white)

HERO SECTION:
- Root section carries its own colors: bg-white, text-neutral-900
- Container also gets grid, items-center, lg:grid-cols-12
- Left column spans lg:col-span-6, holding in order:
- Pill "Versi 2.0": inline-flex, items-center, rounded-full, border, border-neutral-200, px-3, py-1, text-xs font-medium, uppercase, tracking-widest, text-neutral-500
- H1 split over two lines with a br, "Ubah gambar desain" / "jadi web statis": text-4xl, md:text-5xl, lg:text-6xl, font-semibold, tracking-tight, leading-[1.1]
- Paragraph "Tailwind v4, tanpa satu baris JavaScript perilaku. Satu halaman penuh plus file terpisah per section.": max-w-lg, text-base, md:text-lg, leading-relaxed, text-neutral-500
- Button row (flex, flex-wrap, items-center), two CTAs sharing (w-full, sm:w-auto, inline-flex, min-h-11, items-center, justify-center, rounded-lg, px-6, text-sm font-medium, FOCUS RING): primary "Mulai sekarang" (bg-neutral-900, text-white, transition-opacity, hover:opacity-90), secondary "Lihat contoh" (gap-2, border, border-neutral-200, transition-colors, hover:bg-neutral-100) with the Lucide "arrow-right" icon at size-4 after its label
- Logo row (flex, flex-wrap, items-center, text-sm, text-neutral-500): lead-in "Dipakai oleh" then "Northwind", "Contoso", "Fabrikam", each font-medium text-neutral-900
- Right column spans lg:col-span-6 with one empty div: aspect-[4/3], w-full, rounded-xl, border, border-neutral-200, bg-neutral-100

STYLE REQUIREMENTS:
- Palette: white, neutral-100/200/500/900 only. No custom tokens or CSS variables
- Radius: rounded-lg buttons and menu items, rounded-xl placeholder, rounded-full pill
- Icons inline SVG from Lucide: viewBox "0 0 24 24", fill none, stroke currentColor, stroke-width 2, linecap and linejoin round, aria-hidden
- Every interactive element is at least 44px tall (min-h-11 or size-11)
- Do not add md: or lg: variants not listed, never swap a value for a similar one
- Both CTAs share identical padding and height so they line up side by side

CONSTRAINTS:
- Output the header and section markup only — no doctype, html, head, meta, title, body
- No custom CSS, style attributes, or style blocks
- No JavaScript — the mobile menu works only with details and summary
- Icons are inline SVG only, no emoji or icon fonts
- Use only Tailwind class names for styling, layout, responsiveness
