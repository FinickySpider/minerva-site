---
{"dg-publish":true,"permalink":"/vault/04-projects/eisen-darknet-market/final-prompt/"}
---


# **Eisen Darknet Marketplace – Design Doc v1.0**

_A cyberpunk “Silk Road”-inspired, immersive, modular web marketplace for Metro City, Eisenhorn._

---

## **1. Purpose & Scope**

Design an interactive, living “darknet” market inspired by Silk Road and early-2010s underground sites, with a cyberpunk/fantasy overlay. The marketplace is a storefront for illegal goods/services (drugs, weapons, etc.), written in an in-universe, roleplay style. The focus is on immersion, modular content, and extensibility—NOT real purchasing or live transactions.

---

## **2. Visual & UX Design**

- **Core Theme:**
    - Early 2010s darknet/Silk Road nostalgia meets modern cyberpunk.
    - Always darkmode: black/charcoal backgrounds, neon accent colors (green, blue, magenta, orange), glitch/pixel artifacts.
    - Terminal elements: monospaced fonts, ANSI-style banners, flickering overlays, loading ticks.
    - Subtle CRT/glass blur, slight noise, or scanlines to mimic underground web browsers.
- **Navigation:**
    - **Left Sidebar:** Categories (Drugs, Weapons, Services, etc.), collapsible.
    - **Top Bar:** Search, filter, credits balance, profile (user alias), darkweb “time” (syncs to RL time, see Mechanics).
    - **Main Area:** Storefront grid/list of items for selected category. Infinite scroll/paged.
    - **Listing Cards:** Each item displays image (placeholder pool), product name, price, short seller-written description, and “seller” avatar/tag.

---

## **3. Content & Extensibility**

- **Dynamic Content System:**
    - All content—images, listing text, seller names/styles—is pulled via modular content pools.
    - **Endpoints:** Pre-built for OpenAI API usage (text and, optionally, images), ready to plug in for dynamic generation (descriptions, product names, seller bios).
    - **Placeholder Content:** If API not configured, fallback to static pools or “lorem ipsum” darkwebified snippets.
- **Image Management:**
    - All images (item, seller, banners, etc.) are referenced via configurable pools/paths.
    - Add new art by dragging into `public/img/items/`, `public/img/sellers/`, etc. Update the manifest or JSON file to include.
- **Seller System:**
    - Randomly generate sellers (drawn from a list + “notable” frequent sellers weighted to appear more often).
    - Each seller: Alias, rating, avatar (img), and style/personality tag (which is used for text generation).
    - Seller bios and text flavor can be dynamically written with the OpenAI API (endpoint ready).
- **Listing System:**
    - Each category contains item listings; each listing references:
        - Product name (generated/pooled)
        - Seller (see above)
        - Price (in credits, dynamically adjusted)
        - Image (from pool)
        - Short description (seller style)
        - Quantity/availability
    - **Expanded Listing (Ebay-Style):**
        - Clicking opens a detailed view with:
            - Full writeup (API-generated or template)
            - Seller profile, stats
            - Shipping/transaction “terms” (RP flavor)
            - Buy/add-to-cart controls

---

## **4. Marketplace Simulation & Mechanics**

- **Live Market Fluctuations:**
    - **Time of Day:** Syncs to player’s RL time, altering:
        - Prices (e.g. night surge, daytime lull)
        - Supply (remove/add items based on “demand” curves or RNG)
        - Listing themes (holiday/special event content can be injected)
    - **Event Hooks:** Periodically trigger “news” or “police crackdown” events that change listings, remove sellers, or alter prices temporarily.
- **Prominent Sellers:**
    - Certain sellers have a higher chance to appear (“trusted” or “notorious” figures).
    - Others cycle in and out randomly; flavor text should hint at reputation, last seen, etc.

---

## **5. Purchasing Framework (Basic)**

- **Cart & Credits System:**
    - Each listing has “Select amount” (dropdown/input, bounded by available qty).
    - “Add to Cart” button (with feedback animation).
    - Cart view: Lists items, quantities, total cost in credits.
    - “Pay with Credits” button (triggers success/fail based on current balance, with flavor popups).
    - Credits tracked per session or player profile (basic logic; backend endpoint ready for future expansion).
    - No real payments—immersion only. The intent is extensibility for future in-game economy integration.

---

## **6. Content/Integration Endpoints**

- **OpenAI API Endpoints:**
    - All listing, seller, and flavor text generation endpoints are pre-integrated for easy swap/plug of OpenAI keys (gpt-4, or equivalent).
    - Images: Optional support for OpenAI/DALLE or local pool, fallback to placeholder if missing.
    - API endpoints can be called on page load, refresh, or on-demand for “regenerate listing” or “randomize seller” functions.

---

## **7. Modularity/Ease of Use**

- **Content Pools:**
    - `/content/items.json` for items
    - `/content/sellers.json` for sellers
    - `/content/descriptions.json` for templates
    - `/img/items/`, `/img/sellers/` for images
    - Easily add or remove entries by editing JSON/CSV; backend automatically updates available pools.
- **Custom Hooks:**
    - Any content area (listing, seller, banner) can be swapped to an API call or custom script for dynamic behavior.

---

## **8. File/Folder Structure Example**

```txt
eisen-darknet/
├── public/
│   ├── img/
│   │   ├── items/
│   │   └── sellers/
├── src/
│   ├── components/
│   │   ├── Sidebar.vue
│   │   ├── Storefront.vue
│   │   ├── ListingCard.vue
│   │   ├── ListingDetail.vue
│   │   ├── Cart.vue
│   │   └── SellerProfile.vue
│   ├── api/
│   │   ├── openai.js
│   │   └── contentPools.js
│   ├── content/
│   │   ├── items.json
│   │   ├── sellers.json
│   │   ├── descriptions.json
│   │   └── images.json
│   ├── App.vue
│   └── main.js
├── tailwind.config.js
├── vite.config.js
└── server/
    └── index.js
```

---

## **9. Example User Flow**

1. User loads the darknet page (auto darkmode, early darknet/cyberpunk theme).
2. Sidebar shows categories (e.g. Stimulants, Weapons, Services).
3. Main window: Listings from selected category; each with seller, image, price, and brief description (all dynamically generated or pooled).
4. Clicking a listing opens a detailed view (ebay-style) with full flavor, buy/add-to-cart, and seller info.
5. “Add to cart,” select amount, pay with credits if balance allows.
6. Marketplace auto-updates prices and items based on RL time, event hooks, and internal “market” logic.

---

## **10. Notes & Extensibility**

- All descriptions, prices, and seller content are **placeholders** or **AI-generated** (never hardcoded).
- Content system ready for ChatGPT/OpenAI integration—simply swap in keys or use mock endpoints for testing.
- Immersive, RP-heavy, modular by default; easy to reskin or expand for future events or world tie-ins.

---

## **11. Things to Make Sure Are Left Prepped to Expand With Later**

### 1. **User Customization**
- **Profile Customization:** Allow users to customize their alias/avatar for deeper immersion. (Login and credit balance with API/hook to add credits to a user's profile.)

### 2. **Future Expansion**
- **Plugin System:** Design with a plugin/hook system for adding new event types, categories, or RP mechanics, enabling true extensibility.

---

## **12. Additional Implementation Notes**

- **Accessibility:** Ensure all UI components are accessible (contrast, keyboard navigation, ARIA labels).
- **Testing:** Scaffold basic unit and integration tests for major flows and components.
- **Performance:** Use lazy loading and image optimization for listings and assets.
- **Dev/Prod Modes:** Support mock data/dev mode for local development and demos.
- **API Handling:** Gracefully handle API errors/rate limits with fallback content and immersive error messages.
- **Database:** Store at all times the listings, sellers, prices, descriptions, quantities, etc.

---


## **13. Tech Stack:**

- Frontend: Vue 3 (Vite), Tailwind CSS, modern CSS/JS, dynamic imports for content pools
- Backend: Node.js (Express, for future API endpoints)
- AI: OpenAI API integration (text generation, ready for image gen)
- Dev tools: Vite, ES modules, .env for secrets, hot reload, modular structure

---


# **DialogFlow UI Flows:**


## 1. Main UI Navigation Flow

```txt
NODE Start TYPE: Start DESC: "User lands on /eisen-darknet/"
NODE Sidebar TYPE: State DESC: "Sidebar: Categories & Filters"
NODE TopBar TYPE: State DESC: "Top bar: Search, credits, profile, RL time"
NODE Storefront TYPE: State DESC: "Main Panel: Listings Feed"
NODE ListingModal TYPE: State DESC: "Listing Detail Modal/Page"
NODE Cart TYPE: State DESC: "Cart view (session credits, items)"
NODE ProfileSim TYPE: State DESC: "Login/Profile Simulation (supports alias/avatar customization, credit balance, extensible via API/hook and plugins)"
NODE NewsTicker TYPE: State DESC: "Live News Ticker"
NODE IncidentPopup TYPE: Event DESC: "Random Incident (e.g., Raid, Crackdown, Downtime)"

Start -> Sidebar
Start -> TopBar
Sidebar -> Storefront
TopBar -> Storefront [onSearch/filter]
Storefront -> ListingModal [onClick: Listing]
Sidebar -> Storefront [onChange: Category/Filter]
Storefront -> Cart [onClick: Cart]
TopBar -> ProfileSim [onClick: Profile]
Storefront -> NewsTicker
Storefront ^> IncidentPopup {randomEvent}
ListingModal -> Storefront [onClose]
Cart -> Storefront [onClose]
```

---

## 2. Listing Browsing & Filtering

```txt
NODE Sidebar TYPE: Input DESC: "User selects category/filter"
NODE TopBarSearch TYPE: Input DESC: "User enters search/filter"
NODE FetchListings TYPE: Process DESC: "Fetch listings for selection"
NODE ListingsGrid TYPE: State DESC: "Display listing cards"
NODE InfiniteScroll TYPE: Logic DESC: "Load more on scroll"
NODE ListingCard TYPE: State DESC: "Single listing card"
NODE SellerBadge TYPE: State DESC: "Seller prominence, avatar, tags"

Sidebar -> FetchListings
TopBarSearch -> FetchListings
FetchListings -> ListingsGrid
ListingsGrid -> InfiniteScroll [onScroll: End]
InfiniteScroll -> FetchListings
ListingsGrid => [ListingCard, SellerBadge]
ListingCard -> ListingModal [onClick]
```

---

## 3. Listing Detail Modal/Page

```txt
NODE ListingCard TYPE: Input DESC: "User clicks a listing"
NODE OpenModal TYPE: Process DESC: "Open detail modal"
NODE ListingDetail TYPE: State DESC: "Expanded listing info"
NODE SellerProfile TYPE: State DESC: "Seller info, badges, stats"
NODE PriceGraph TYPE: State DESC: "Historical price graph"
NODE PurchaseSim TYPE: Input DESC: "Fake purchase button"
NODE AddToCart TYPE: Input DESC: "Add to cart (select amount)"
NODE ContactSeller TYPE: Input DESC: "Fake contact button"
NODE UserReviews TYPE: State DESC: "Autogen user reviews"
NODE OtherListings TYPE: State DESC: "Other listings by seller"
NODE CloseModal TYPE: Input DESC: "User closes modal"

ListingCard -> OpenModal
OpenModal -> ListingDetail
ListingDetail => [SellerProfile, PriceGraph, UserReviews, OtherListings]
ListingDetail -> PurchaseSim [onClick: BuyNow]
ListingDetail -> AddToCart [onClick: AddToCart]
ListingDetail -> ContactSeller [onClick: Contact]
ListingDetail -> CloseModal [onClick: Close]
CloseModal -> Storefront
```

---

## 4. Simulated Purchase & Cart Flow

```txt
NODE AddToCart TYPE: Input DESC: "User adds item to cart"
NODE Cart TYPE: State DESC: "Cart view"
NODE PayWithCredits TYPE: Input DESC: "User clicks Pay with Credits"
NODE ConfirmPopup TYPE: State DESC: "Show confirmation popup"
NODE OrderPlaced TYPE: State DESC: "Fake order placed notification"
NODE InsufficientCredits TYPE: State DESC: "Not enough credits popup"
NODE ClosePopup TYPE: Input DESC: "User closes popup"

AddToCart -> Cart
Cart -> PayWithCredits [onClick: Pay]
PayWithCredits [enough credits] -> ConfirmPopup
PayWithCredits [not enough credits] -> InsufficientCredits
ConfirmPopup -> OrderPlaced [onConfirm]
OrderPlaced -> ClosePopup [onAcknowledge]
InsufficientCredits -> ClosePopup [onAcknowledge]
ClosePopup -> Cart
```

---

## 5. Dynamic Content & Event Hooks (Simulation)

```txt
NODE RLTimeSync TYPE: Timer DESC: "Sync to RL time"
NODE MarketUpdate TYPE: Process DESC: "Update prices, supply, listings"
NODE EventTrigger TYPE: Event DESC: "Special event/news (e.g., crackdown, surge)"
NODE NewsTicker TYPE: State DESC: "Display news/event"
NODE Storefront TYPE: State

RLTimeSync -> MarketUpdate
MarketUpdate -> Storefront
MarketUpdate ^> EventTrigger {randomEvent}
EventTrigger -> NewsTicker
```





# DiagramFlow v2 Cheatsheet & Key

## 1. Node Types

| Node Type   | Typical Label      | Purpose/Example                        |
|-------------|-------------------|----------------------------------------|
| Start       | Start             | Entry point of flow                    |
| End         | End, EndSuccess   | Exit/termination                       |
| Process     | AuthUser          | Deterministic work step                |
| Decision    | IsValid?          | Branching, Boolean or multi-way        |
| Logic       | CalcTax           | Calculation, rules, single outcome     |
| State       | Dashboard         | Passive UI or system state             |
| Input       | LoginForm         | Human/external data entry              |
| Output      | ExportCSV         | Data exit                              |
| External    | StripeAPI         | 3rd-party/API/service call             |
| Fork        | F, Fork1          | Split flow into branches               |
| Merge       | J, Join           | Join branches                          |
| Parallel    | SyncAll           | Wait for all branches                  |
| Timer       | Delay, RetryTimer | Wait/delay                             |
| Guard       | RateLimiter       | Conditional gate                       |
| SubFlow     | SubFlow2FA        | Reference to another flow              |
| Event       | OnPay             | Triggered action                       |
| Data        | Counter           | Store/counter                          |
| SideEffect  | Log, Notify       | External effect (email, log, etc)      |
| Error       | ErrorPage         | Error handler                          |

---

## 2. Edge Operators (Flow Arrows)

| Operator | Visual | Meaning/Usage                  |
|----------|--------|-------------------------------|
| `->`     | A -> B | Sequential step               |
| `<->`    | A <-> B| Bidirectional/handshake       |
| `-/>`    | A -/> B| Error/exception path          |
| `~>`     | A ~> B | Async, non-blocking           |
| `=>`     | A => [B, C]| Fork/fan-out (parallel start) |
| `==`     | [B, C] == D| Merge/fan-in (wait-all)   |
| `^>`     | A ^> B | Event-triggered edge          |
| `*->`    | A *-> B| Loop/repeat                   |

---

## 3. Attributes (Metadata)

| Attribute     | Example                        | Meaning                        |
|---------------|-------------------------------|--------------------------------|
| `TYPE:`       | `TYPE: Process`               | Node category                  |
| `DESC:`       | `DESC: "User logs in"`        | Human description              |
| `CONDITION:`  | `CONDITION: "x > 0"`          | Branch/test logic              |
| `ROLE:`       | `ROLE: "API-Gateway"`         | Responsible actor/service      |
| `DURATION:`   | `DURATION: "30s"`             | Timer length                   |
| `INIT:`       | `INIT: 0`                     | Initial value (Data/Loop)      |
| `REF:`        | `REF: "OtherFlow"`            | SubFlow reference              |
| `{...}`       | `{onFail}`                    | Side-effect/annotation         |

---

## 4. Visual Key: Mini Diagrams

### Sequential Flow
```txt
Start -> Input -> Auth
```
Start ---→ Input ---→ Auth

---

### Decision Branch
```txt
Auth -> IsValid? [yes] -> Dashboard
Auth -> IsValid? [no]  -> ErrorPage
```
Auth ---→ IsValid? ---[yes]---→ Dashboard  


