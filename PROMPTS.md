## Prompt 1

I'm working on the MeroxIO Developer Assessment for the Mistvale Tea
Co. store.

For now, don't change anything. I first want you to understand the
project and give me a proper baseline audit.

Please read:

- README.md

- BRAND.md

- index.html

- the existing images/assets

Then inspect the current store and tell me what is wrong.

Please focus on:

1. Bugs in the current functionality.

2. Which business rules R1--R8 are currently broken, and why.

3. Problems with the cart, quantities, stock, prices, coupons,
shipping, and checkout.

4. Problems with search, category filters, and sorting, especially when
they are used together.

5. Problems with the pincode/delivery check, including loading and
error handling.

6. Desktop and mobile UX issues.

7. Accessibility issues.

8. Missing or incorrect SEO and structured data.

9. Problems with the current images/assets.

10. Any violations of the constraints mentioned in the README.

11. Anything that looks contradictory or unclear and needs a decision
instead of a guess.

12. Any code that we need to be careful not to change because the
README treats it as a contract.

Don't modify, create, delete, or rewrite any files yet. This is only an
audit.

Please organize the result into:

- Critical bugs

- Business-rule issues

- UX/mobile issues

- Accessibility issues

- SEO/schema issues

- Image/asset issues

- Constraint risks

- Suggested order for fixing things

For each issue, mention the relevant function, section, or code area if
you can.

Please base everything on the actual project files and the README/BRAND
requirements, not assumptions.

## Prompt 2

Let's fix the first issue from the audit: the project currently depends
on external libraries, but the assignment requires plain HTML, CSS and
JavaScript.

Please remove the jQuery, animate.css, and Font Awesome dependencies
from the project.

Before changing anything, make sure you understand where each of them is
currently being used.

Replace their usage with equivalent vanilla JavaScript/CSS where needed.
In particular, preserve the current functionality that depends on jQuery
such as the wishlist toggle.

Do not redesign the page yet and do not change the PRODUCTS data, API
object, checkout form contract, or approved legal footer text.

After making the changes:

1. Make sure there are no remaining jQuery, animate.css, or Font
Awesome imports or usages.

2. Search the project for `$`, `jQuery`, Font Awesome classes, and
animate.css classes that may now be broken.

3. Make sure the page still loads without JavaScript errors.

4. Tell me exactly what you changed and any functionality that needs to
be tested manually.

Keep this change focused only on removing the external libraries and
replacing their necessary functionality with vanilla code.

## Prompt 3

Let's fix the next issue from the audit: the cart crashes for a
first-time visitor when there is no saved cart in localStorage.

Please fix only the cart initialization problem.

When localStorage does not contain "mv_cart", the cart should safely
start as an empty array instead of null.

Make sure the existing saved cart still loads normally when valid cart
data exists.

Also handle invalid or corrupted localStorage data safely so it does not
crash the page.

Do not change the cart pricing, quantity rules, coupon logic, shipping
logic, filtering, UI design, or any other behavior yet.

Do not modify the PRODUCTS array, API object, checkout form contract, or
approved legal footer text.

After the change:

1. Verify that a fresh browser/incognito visit does not throw a
cart-related JavaScript error.

2. Verify that an existing valid cart in localStorage still loads.

3. Verify that invalid/corrupted "mv_cart" data does not crash the
page.

4. Tell me exactly what you changed and where.

## Prompt 4

Let's fix the next issue from the audit: the cart currently reads
product prices from the formatted price text in the DOM.

This violates business rule R1 and also causes prices such as ₹1,299 to
be parsed incorrectly as 1 because of the comma.

Please fix only the product price source used when adding an item to the
cart.

The price must always come directly from the matching product in the
PRODUCTS array using the product id. Do not read or parse the displayed
price text from the DOM.

Keep the stored cart price numeric and accurate.

Please also make sure:

- Products with prices like 349, 799, 1299, and 1899 are stored with
their exact PRODUCTS prices.

- The displayed formatted price on the page can remain as it is.

- No product id, name, or price inside PRODUCTS is changed.

Do not change quantity limits, coupon logic, shipping, filtering,
sorting, checkout, UI design, or any other behavior yet.

Do not modify the API object, checkout form contract, or approved legal
footer text.

After the change, verify that adding a product stores the exact price
from PRODUCTS in the cart and tell me what you changed.

## Prompt 5

Let's fix the next cart bug from the audit: adding the same product more
than once creates duplicate cart entries because the product id from the
HTML is a string while the id in PRODUCTS/cart is numeric.

Please fix only the duplicate detection in addToCart().

The desired behavior is:

- If the product is not already in the cart, add it normally with qty
1.

- If the product is already in the cart, increase its existing qty
instead of creating another cart entry.

- Compare product ids reliably regardless of whether the incoming id is
a string or number.

- Do not change the quantity limits yet; we will handle those
separately.

- Keep the cart data structure and product price from PRODUCTS intact.

Do not change coupon, shipping, filtering, sorting, checkout, UI design,
or any other behavior.

Do not modify the PRODUCTS array, API object, checkout form contract, or
approved legal footer text.

After the change, test adding the same product multiple times and
confirm that only one cart entry exists with an increasing qty.

## Prompt 6

Let's fix the remaining cart and quantity issues from the audit in one
focused pass.

Some cart fixes are already done, so please don't undo them:

- cart initialization is already safe

- product price now comes from PRODUCTS

- duplicate cart items are already handled even if the id type differs

Now fix only the remaining cart/quantity problems:

1. Quantity controls:

   - The + button must increase quantity numerically, not concatenate
strings.

   - The - button must never reduce quantity below 1.

   - Manual quantity input must actually update the cart.

   - Manual quantity must be limited to the valid range.

2. Quantity limits:

   - A product can never have more than 5 units in the cart.

   - A product can never exceed its stock.

   - Apply these limits consistently when adding, using +, or entering a
quantity manually.

   - Sold-out products must never be added to the cart.

3. Remove:

   - Fix removeItem so it removes only the selected item, not everything
after it.

4. Cart count:

   - The cart badge should show total units in the cart, not just the
number of different products.

5. Cart rendering:

   - Fix any event-listener issue caused by renderCart being called
repeatedly. Re-rendering the cart should not make buttons fire multiple
times.

6. Empty cart:

   - Show a proper empty-cart state when there are no items.

   - Don't change the shipping/coupon business rules yet; we'll handle
pricing separately.

Please keep this pass focused only on cart and quantity/state behavior.
Don't redesign the UI and don't touch search, filters, sorting, coupons,
shipping, pincode, checkout, SEO, images, PRODUCTS, API, or the legal
footer.

After making the changes, test these cases:

- add the same product multiple times

- + and - quantity

- manually enter 0, 1, 5, 6, and a value greater than stock

- try adding a sold-out product

- remove one item when multiple products are in the cart

- verify cart badge counts total units

- render/update the cart multiple times and verify each click fires
only once

- verify empty cart state

Finally, tell me exactly what you changed and what you tested.

## Prompt 7

Let's handle the pricing, coupon, and shipping rules in one focused
pass.

R1 is already fixed: product prices must come from PRODUCTS, not from
DOM text. Please keep that fix and don't change PRODUCTS.

Now fix only these remaining business rules:

1. WELCOME10 coupon:

   - Coupon code must be case-insensitive, so welcome10 / WELCOME10 /
Welcome10 should work the same.

   - Give 10% off eligible items.

   - Maximum discount is ₹150.

   - Coupon can only be applied when the subtotal is at least ₹399.

   - Products in the Gifts category must not receive the discount.

   - The coupon can only be applied once per order.

   - Applying it again should not stack another discount.

   - If the coupon is invalid or the subtotal is below ₹399, show a
clear message and don't apply a discount.

2. Shipping:

   - Shipping is ₹49 normally.

   - Free shipping should apply when the amount after discount reaches
the configured free-shipping threshold.

   - Please use the existing threshold from the project instead of
inventing a new one.

   - Don't charge ₹49 when the cart is empty.

3. Money handling:

   - Keep monetary calculations numeric internally.

   - Don't use DOM text as the source for prices.

   - Format displayed amounts consistently as Indian rupees with ₹ and
Indian comma grouping, for example ₹1,299.

   - Do not round intermediate calculations.

   - Only the final total should be rounded to the nearest rupee.

4. Cart summary:

   - Make sure subtotal, discount, shipping, and final total are
recalculated correctly whenever cart quantity or coupon state changes.

   - Don't allow stale discount or shipping values after the cart
changes.

Keep this pass focused. Don't redesign the UI and don't touch
search/filter/sort, pincode, checkout, SEO, images, PRODUCTS, API, or
the legal footer.

Before changing anything, inspect the existing coupon and shipping
implementation and preserve its existing structure where possible.

After making the changes, test at least:

- WELCOME10 with subtotal below ₹399

- WELCOME10 at exactly ₹399

- discount above the ₹150 cap

- a cart containing Gifts products

- lowercase and mixed-case coupon codes

- applying the coupon twice

- normal ₹49 shipping

- free shipping after discount

- empty cart

- quantities changing after a coupon is applied

- prices such as ₹1,299 and ₹1,899

- a case where the calculated final total needs rounding

Also check that only the final total is rounded, not the intermediate
discount/subtotal calculations.

Finally, tell me exactly what you changed and what you tested.

## Prompt 8

Let's fix the search, category filter, and sorting behavior in one
focused pass.

Right now these controls don't work reliably together. Please fix them
without changing the cart, pricing, coupon, shipping, checkout, or UI
design.

Please make sure there is one consistent filtering/sorting flow:

1. Search:

   - Search results must actually match what the user typed.

   - Search should work against the relevant product information already
available in PRODUCTS.

   - Make the search case-insensitive.

   - Handle leading/trailing spaces properly.

   - An empty search should show all products in the selected category.

   - If there are no matches, show a clear empty-results message.

   - Remove/fix any inverted indexOf/filter logic.

   - Avoid async/race-condition behavior if the current search
implementation uses asynchronous filtering.

2. Category:

   - All category options must work correctly.

   - Selecting a category should filter the products without resetting
the current search text.

   - Selecting "All" should remove only the category restriction.

   - Keep the current search and sort state when changing category.

3. Sorting:

   - Fix the sort comparator so it actually sorts correctly.

   - Keep the current search and category filters when changing sort.

   - Sold-out products must ALWAYS appear after in-stock products,
regardless of the selected sort.

   - Among products with the same stock status, apply the selected sort
normally.

   - Make sure changing sort doesn't reset the other controls.

4. Combined state:

   - Search + category + sort must work together.

   - Changing one control must not reset the other active controls.

   - Every render should be based on the current search, category, and
sort state.

   - Don't mutate PRODUCTS itself while filtering or sorting; work from
a derived array/copy.

5. Empty states:

   - Show a useful message when the current combination of
search/category filters has no results.

   - Don't show unrelated products just because a filter returned no
matches.

Please keep this pass focused only on search, category filtering,
sorting, and their state.

Do NOT touch:

- cart logic

- quantities/stock limits

- pricing

- WELCOME10

- shipping

- pincode

- checkout

- SEO

- images

- PRODUCTS data

- API object

- legal footer

- overall UI redesign

After making the changes, test these combinations:

- search for an existing product

- search using different letter casing

- search with leading/trailing spaces

- search for something that doesn't exist

- search with category selected

- change category while search is active

- change sort while search and category are active

- test every available sort option

- verify sold-out products always appear last

- select All and verify search still works

- clear search and verify the selected category remains

- repeatedly change search, category, and sort in different orders

Please tell me exactly what you changed and what you tested.

## Prompt 9

Let's fix the delivery pincode check and checkout flow in one focused
pass.

Please keep the existing cart, pricing, coupon, search/filter/sort logic
unchanged unless a small integration change is required.

## 1. Delivery / Pincode Check

Fix the existing delivery check so it correctly uses the provided
API.checkPincode() function.

Requirements:

- Use API.checkPincode() instead of inventing a new API.

- Show a clear loading state while the request is running, such as
"Checking...".

- Always resolve the loading state, whether the API succeeds or fails.

- If the pincode is serviceable, show the delivery days returned by the
API.

- If it is not serviceable, show a clear not-serviceable message.

- Invalid pincode input should show a helpful validation message.

- Handle API errors/rejected promises so the UI never gets stuck on
"Checking...".

- Trim the input before checking.

- The check should also work when the user presses Enter.

- Prevent unnecessary duplicate requests while one check is already
running.

Please do not change the API object between API START and API END.

## 2. Checkout Form Contract

Fix the Checkout button so it uses the existing checkout form instead of
bypassing it with an alert or custom flow.

The existing form contract MUST remain exactly:

<form id="checkout-form"
action="https://mistvale.example/cart/checkout" method="POST">

Keep the existing `items` and `coupon` fields.

When the user clicks Checkout:

- Build the cart items JSON from the actual cart state.

- The `items` field must contain JSON in this format:

  [{"id":101,"qty":2}]

- Include every cart item with its actual id and quantity.

- The `coupon` field should contain the currently applied coupon
code, or an empty value if no coupon is applied.

- Submit the existing form normally.

- Do not replace the form submission with an alert, fetch request, or
fake checkout page.

- Do not change the checkout URL or HTTP method.

If the cart is empty, Checkout should not submit an empty order. Show an
appropriate message instead.

## 3. Keep This Pass Focused

Do NOT redesign the UI.

Do NOT change:

- PRODUCTS

- API object

- cart quantity rules

- pricing calculations

- WELCOME10 rules

- shipping rules

- search/filter/sort

- SEO

- images

- legal footer

After making the changes, test:

### Pincode

- valid serviceable pincode

- valid non-serviceable pincode

- invalid/empty pincode

- API rejection/error

- pressing Enter

- repeated checks

- verify "Checking..." never remains after completion/error

### Checkout

- cart with one product

- cart with multiple products and quantities

- checkout with WELCOME10 applied

- checkout without coupon

- verify `items` contains the correct JSON

- verify `coupon` contains WELCOME10 when applied

- verify coupon field is empty when no coupon is applied

- verify the exact form action and method are preserved

- verify empty cart does not submit

Finally, tell me exactly what you changed and what you tested.

## Prompt 10

Let's handle the accessibility, SEO, FAQ, and structured-data work in
one focused pass.

The existing functionality and responsive UI are working, so please
don't rewrite the business logic or redesign the page.

## 1. Accessibility

Please inspect the whole page and fix accessibility issues that are
currently present.

- Use semantic HTML where appropriate.

- Interactive actions should use real buttons/links instead of
clickable non-semantic elements where possible.

- Make sure every form control has a visible, associated label or an
appropriate accessible name.

- Add useful aria-labels/aria-expanded/aria-controls where they are
actually needed.

- Make the FAQ accordion keyboard accessible.

- Make sure keyboard users can reach all interactive controls.

- Preserve visible  styles.

- Don't remove focus outlines.

- Make sure focus isn't trapped or lost unexpectedly when
opening/closing the cart drawer or Quick View.

- Add appropriate dialog semantics to Quick View/cart where useful.

- Use aria-live for dynamic messages such as coupon, pincode,
newsletter, and cart feedback where appropriate.

- Make sure images have meaningful alt text. Decorative images can use
empty alt text.

- Check heading hierarchy: exactly one main H1 and logical H2/H3
structure.

- Check text/background contrast against the BRAND.md colors and fix
any failing combinations without introducing unrelated colors.

- Keep touch targets at least 44px.

- Respect prefers-reduced-motion.

Do not add an accessibility library.

## 2. SEO Metadata

Make the page SEO-ready according to README.md and BRAND.md.

Please add/fix:

- A useful title around 50--60 characters.

- A meta description around 120--160 characters.

- One canonical URL using the website already specified in BRAND.md.

- Open Graph metadata.

- Twitter/X card metadata.

- Correct html lang attribute.

- One clear H1.

- Logical headings throughout the page.

Use only facts provided by README.md and BRAND.md. Do not invent awards,
reviews, ratings, statistics, locations, claims, or product facts.

## 3. Organization / OnlineStore Structured Data

Add valid JSON-LD for the Mistvale business using only the company facts
from BRAND.md.

Include the appropriate organization/online-store information such as:

- Mistvale Tea Co.

- tagline

- website

- logo if an appropriate existing logo asset is available

- address

- phone

- email

- Instagram/social profile where appropriate

Do not invent any additional company information.

## 4. Product Structured Data

Add Product JSON-LD for each tea/product using the existing PRODUCTS
data.

Use:

- product name

- description/details available in PRODUCTS

- price in INR

- stock-based availability

- relevant product URL only if it can be derived safely from the
existing site structure

Only include ratings/review information where it actually exists in the
provided data. Do not invent ratings or review counts.

Make sure prices come from PRODUCTS rather than DOM text.

## 5. FAQ Section

Add the FAQ section required by BRAND.md.

Use the approved FAQ information from BRAND.md only.

Include these topics:

- delivery time

- returns

- tea storage

- caffeine-free Chamomile & Tulsi

- India-only delivery

- Tea Lover's Sampler gift box / brewing guide

Make the FAQ an accessible accordion:

- keyboard accessible

- clear expanded/collapsed state

- visible focus state

- usable on mobile

Also add valid FAQPage JSON-LD based on the same FAQ answers.

Do not invent additional FAQ answers.

## 6. Important Constraints

Do NOT change:

- PRODUCTS data

- API object

- cart logic

- quantity/stock rules

- WELCOME10

- shipping rules

- pincode logic

- checkout form contract

- product images

- legal footer text

Do not add Bootstrap, Tailwind, jQuery, Font Awesome, animate.css,
React, or other libraries.

Keep the existing Mistvale visual style and responsive layout.

## 7. Verification

After making the changes, verify:

- exactly one H1

- logical H2/H3 hierarchy

- all important form controls have labels/names

- keyboard can reach the main controls

- FAQ works with keyboard

- Quick View/cart can be opened and closed accessibly

- dynamic feedback uses appropriate accessible announcements

- images have appropriate alt text

- title length is reasonable

- meta description length is reasonable

- canonical is present

- OG/Twitter metadata is present

- Organization/OnlineStore JSON-LD is valid

- Product JSON-LD exists for all products

- FAQPage JSON-LD matches the visible FAQ

- no invented company/product/review facts were added

- existing business-logic test suites still pass

Please tell me exactly what you changed and what you tested.

## Prompt 11

Let's do the final image and asset pass for the Mistvale Tea Co. store.

The functionality, responsive UI, accessibility, SEO, schema, cart,
pricing, search/filter/sort, pincode, and checkout work is already
completed and tested. Please don't rewrite any of that.

Focus only on images/assets and small visual polish.

## 1. Product images

Inspect all 8 product images and make sure:

- All 8 use a consistent visual style.

- They have the same/similar aspect ratio and framing.

- Product cards don't jump in height because of different image
dimensions.

- Images look appropriate for the calm, earthy, premium Mistvale brand.

- No image contains fake badges, ratings, claims, or unnecessary
baked-in text.

- Keep the actual product identity intact.

- Use appropriate loading/decoding attributes where useful, without
hurting the first visible hero image.

Target:

- Each product image should ideally be under 150KB.

- Prefer WebP or AVIF when practical.

- Don't make images visibly blurry just to hit the size target.

## 2. Hero image

Optimize the hero image:

- Target under 250KB.

- Keep it visually sharp at the displayed size.

- Use an appropriate modern format where practical.

- Make sure it doesn't cause unnecessary layout shift.

- Keep the existing Mistvale visual direction.

## 3. Logo

Inspect the current logo.

- Prefer a clean SVG if a suitable existing logo can be created from
the existing brand direction.

- Do not invent a completely different brand mark.

- Make sure it renders sharply on desktop and mobile.

- Make sure its alt/accessibility treatment is correct.

## 4. Image references

Check every image reference in index.html.

- No broken image paths.

- No unused image references where they can be safely removed.

- Make sure all product images map to the correct PRODUCTS entries.

- Make sure structured-data image references still point to valid
assets.

## 5. Performance

Please check the total image/asset size after optimization.

Aim to keep the store lightweight while preserving visual quality.

Use native browser features only. Do not add image libraries or frontend
frameworks.

## 6. Small visual polish

Only fix obvious visual inconsistencies that you discover during this
pass, such as:

- image aspect-ratio mismatch

- awkward image cropping

- inconsistent object positioning

- unexpected spacing caused by images

- broken/missing assets

Do NOT redesign the page.

## Important constraints

Do NOT change:

- PRODUCTS data

- API object

- cart logic

- quantity/stock rules

- WELCOME10

- shipping

- search/filter/sort

- pincode

- checkout form contract

- accessibility logic

- SEO/schema logic

- FAQ content

- legal footer text

Do not add:

- Bootstrap

- Tailwind

- jQuery

- Font Awesome

- animate.css

- React

- any other frontend framework/library

Use only the existing project assets and the requirements in README.md
and BRAND.md.

## Verification

After the changes, verify:

- all 8 product images exist

- all 8 product images are consistently sized/framed

- product images are under 150KB where possible

- hero image is under 250KB where possible

- logo is valid and sharp

- no broken image references

- all image alt text is still correct

- no incorrect product-to-image mapping

- structured-data image URLs/references still work

- no horizontal overflow at 360px

- existing automated test suites still pass

Please report:

1. Which images/assets you changed.

2. Old vs new file sizes.

3. Image formats used.

4. Any image that could not reasonably meet the target and why.

5. All tests you ran and their results.

Don't make unrelated code changes during this pass.

## Prompt 12

The main implementation work is now complete. Before making any more
changes, I want one final submission-readiness audit.

Do NOT modify any files yet.

Read README.md, BRAND.md, and the current index.html and images/
directory.

Then audit the final project against the original assessment
requirements.

Check every requirement, especially:

1. R1--R8 business rules

2. Cart, quantity, stock, sold-out behavior

3. Pricing, WELCOME10, Gifts exclusion, shipping, rounding

4. Search + category + sorting combined behavior

5. Pincode/delivery handling

6. Checkout form contract

7. Responsive behavior from 360px to desktop

8. Accessibility and keyboard behavior

9. SEO metadata

10. Organization/Product/FAQ structured data

11. FAQ content against BRAND.md

12. Newsletter placement and behavior

13. Image sizes, formats, aspect ratios, and references

14. Brand colors, typography, spacing, buttons, motion

15. No prohibited libraries/frameworks

16. PRODUCTS data unchanged

17. API object unchanged

18. Legal footer unchanged

19. No invented claims, reviews, ratings, awards, or company facts

20. Any contradictions from README/BRAND that still need to be
documented

Also check for:

- JavaScript console/runtime errors

- broken links or image references

- dead/unused code that could cause issues

- horizontal overflow

- accidental regressions

- anything that could realistically lose marks in the assessment

Run all existing automated test suites again.

Do not fix anything yet.

Give me a final report organized as:

- PASS --- requirements clearly satisfied

- WARNING --- something should be reviewed or documented

- FAIL --- something still needs fixing

- CONTRADICTIONS --- requirements that conflict and need to be
documented

- FINAL RECOMMENDATIONS --- only if a change is actually necessary

For every WARNING or FAIL, mention the exact file/code area and explain
why.

Be strict. Treat the README and BRAND.md as the source of truth rather
than assuming something is acceptable just because the current tests
pass.