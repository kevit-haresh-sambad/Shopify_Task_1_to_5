# Shopify Video Product Carousel

## Project summary

This work explores how Shopify Online Store 2.0 sections and blocks are used in **Dawn** and **Horizon**, then applies those concepts to a video-led product carousel.

The project demonstrates the same storefront goal through two theme approaches:

- **Dawn:** a custom Liquid section with a product/video block, section schema, and companion CSS.
- **Horizon:** a carousel assembled in the theme editor from Horizon's native section and nested blocks, including video and product-content blocks.

The implementations share the same outcome, but are not the same code. Horizon's screenshot demonstrates native editor composition; the standalone Liquid file is written against Dawn's markup and theme assets.

## Work completed

- Compared the themes' block composition and rendering models: section-defined blocks in the Dawn implementation versus Horizon's composable, nested theme-editor blocks.
- Built a Dawn section with configurable heading, color scheme, video controls, and repeatable product-slide blocks.
- Connected each Dawn slide to a catalog product and a Shopify-hosted video. Product title, formatted price, and product URL are read dynamically from the selected product rather than hardcoded.
- Configured the Horizon carousel with native editor blocks. The screenshots show the card structure containing video, product inventory, and product-card content, with product title, price, and purchase actions supplied by the selected catalog items.
- Reviewed the carousel at desktop and narrow/mobile preview widths.

## Common Shopify concepts

Both implementations use the Online Store 2.0 theme editor model:

- A **section** is a configurable page-level composition.
- **Blocks** represent configurable content or components within a section.
- Theme editor settings let a merchant choose or arrange content without changing Liquid source.
- Products remain catalog records: title, price, availability, and destination come from Shopify product data rather than duplicated text in the carousel.
- The presentation layer can change between themes while preserving the same data relationship and customer-facing purpose.

## Dawn and Horizon block structure

| Area | Dawn implementation in this project | Horizon implementation shown |
| --- | --- | --- |
| Composition | The custom section defines a `product_item` block in its `{% schema %}`. | A native Carousel section contains a Header and Carousel content; Carousel content contains Card blocks, with nested Video and product-related blocks. |
| Block rendering | The section loops over `section.blocks` and reads each block's settings. | The Horizon theme/editor manages the nested composition and each native block's settings. |
| Product data | A product picker on each slide supplies `product.title`, `product.price`, and `product.url`. | Product card/content blocks are configured against catalog products in the theme editor. |
| Theme dependencies | Uses Dawn slider markup, CSS classes, icon assets, and translation keys. | Uses Horizon's native section/block system and its own layout, media, and product blocks. |
| Portability | The file can be added to a compatible Dawn theme, subject to the theme's assets and slider implementation. | The Horizon editor configuration is theme-native and is not created merely by copying the Dawn Liquid file. |

Horizon supports reusable and nested theme blocks, but not every section in every theme is composed identically. The exact structure depends on the section and theme version. Likewise, a Dawn section can use section-defined blocks without using Horizon's reusable theme-block architecture. The key distinction in this project is the implementation path: custom section schema and Liquid rendering in Dawn, versus native nested block composition in Horizon.

## Files

| File | Role |
| --- | --- |
| `custom_carousel_video_section.liquid` | Dawn custom section markup, settings, product-slide schema, and product/video rendering |
| `component-custom-carousel.css` | Carousel video and product-content styling referenced by the Dawn section |
| `Screenshot From 2026-10-19 20-00-34.png` | Dawn theme-editor desktop preview |
| `Screenshot From 2026-10-19 20-00-11.png` | Dawn narrow/mobile preview |
| `Screenshot From 2026-10-19 20-11-33.png` | Horizon theme-editor narrow/mobile preview and nested Card settings |
| `Screenshot From 2026-10-19 20-12-29.png` | Horizon theme-editor desktop preview and Video block settings |

The project files are at the workspace root for review. In a Dawn theme, place the Liquid file in `sections/` and the CSS file in `assets/`. The Dawn section loads the stylesheet by its asset filename.

## Data flow and merchant customization

In the Dawn section, a merchant adds a **Product Slide**, chooses a product, and selects a video. The section assigns the chosen product to `product` and renders:

- `product.title` for the product name.
- `product.price | money` for the formatted price.
- `product.url` for the product-page link.

When the merchant selects a different product, these values update from that product. If no product is selected, the product details and link are omitted. If no video is selected, the section renders a placeholder.

In the Horizon composition, product information is supplied through the selected product and native product-related blocks rather than manually duplicating product details in carousel text. This keeps catalog content and merchandising presentation separate.

The Dawn section exposes heading text, heading size, color scheme, video-controls visibility, and a repeatable slide block in the theme editor. Horizon exposes controls through the settings of its native section and nested blocks, as visible in the screenshots.

## Responsive review and implementation notes

The Dawn markup uses Dawn's slider/grid conventions and a one-column grid setting for tablet-and-smaller layouts. Its video CSS applies a 400px height and `object-fit: cover`. The Horizon screenshots show wide desktop cards and a narrow/mobile preview. Video aspect ratio, card width, text wrapping, and navigation should be verified at the target store's actual mobile, tablet, and desktop breakpoints before publishing.

Known details in the current Dawn files:

- The CSS contains `.custom_carousel_section` rules, but the current Liquid markup does not apply that class; those rules do not currently style the section.
- The schema exposes `enable_desktop_slider`, but the current Liquid markup does not use this setting to change slider behavior.
- The section relies on Dawn's `<slider-component>`, slider assets, and theme translation keys. Other themes require their own compatible rendering and assets.

These details are recorded to distinguish the implemented behavior from settings or styles that are present but not currently wired into the markup.

## Previews

### Dawn

![Dawn custom video product carousel in the theme editor](./Screenshot%20From%202026-10-19%2020-00-34.png)

![Dawn custom video product carousel in a narrow mobile preview](./Screenshot%20From%202026-10-19%2020-00-11.png)

### Horizon

![Horizon carousel nested Card configuration in a narrow mobile preview](./Screenshot%20From%202026-10-19%2020-11-33.png)

![Horizon video product carousel in the desktop theme editor](./Screenshot%20From%202026-10-19%2020-12-29.png)
