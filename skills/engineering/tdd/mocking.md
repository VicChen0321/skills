# mocking.md

Mock at the boundary of what your code doesn't control: network calls, the filesystem, the system clock, randomness, third-party services.
Everything on your side of that boundary should run for real in a test, not be mocked.

**Don't mock the thing under test.** A test that mocks its own subject isn't testing anything; it's just confirming the mock was called, which is a tautological assertion wearing different clothes.

**Prefer a fake over a mock when one exists.** An in-memory database instead of a mocked query layer, a fake clock instead of a mocked `Date.now()`, an in-memory queue instead of a mocked message broker. A fake still executes real logic on your side of the boundary; a mock only records that a call happened.

**A mock that returns canned data should return data shaped like the real thing.** A mocked API response that doesn't match the real API's actual shape passes today and breaks in production once the two drift apart.

**If several tests need the same mock setup, extract a shared test helper.** Not because the duplication is a style problem, but because mock setups drift silently: one copy gets updated when the real interface changes, the other doesn't, and the second test starts asserting against a fiction.
