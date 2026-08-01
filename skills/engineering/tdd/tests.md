# tests.md

A good test reads like a specification of behavior a reader could understand without ever opening the implementation.

**Test behavior, not implementation.** A test coupled to *how* something works breaks the moment that changes, even when *what* it does hasn't.

Bad, coupled to an internal function name:

```js
test('applies the promo', () => {
  const spy = jest.spyOn(pricing, 'calculateDiscount')
  applyPromo(cart, 'SAVE10')
  expect(spy).toHaveBeenCalledWith(0.1)
})
```

Rename or inline `calculateDiscount` and this test breaks, even though the discount still works correctly.

Good, testing the observable outcome instead:

```js
test('a 10%-off promo code reduces the cart total by 10%', () => {
  const cart = cartWithTotal(100)
  applyPromo(cart, 'SAVE10')
  expect(cart.total).toBe(90)
})
```

This only breaks if the actual discount behavior changes.

**Assertions can't share logic with the code under test.** If the expected value is computed the same way the implementation computes it, a bug in that logic produces a wrong answer on both sides, and the test passes anyway.

Bad, tautological:

```js
test('order total is the sum of line items', () => {
  const order = buildOrder(items)
  expect(order.total).toBe(items.reduce((sum, i) => sum + i.price, 0))
})
```

Good, a fixed expected value:

```js
test('order total is the sum of line items', () => {
  const order = buildOrder([{ price: 10 }, { price: 25 }])
  expect(order.total).toBe(35)
})
```
