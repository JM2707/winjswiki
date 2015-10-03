When working with UI controls which display collections of data, it can be useful for them to support some kind of virtualization for performance. This page contains a summary of different virtualization techniques:

- **Lazy loading.** Load the on screen items synchronously. Load the off screen items asynchronously in the background. This is the approach the `Hub` takes. Once loaded, an item is never unloaded.
- **Incremental loading.** Load some set of items synchronously. Load additional items when the user scrolls near the bottom of the list. Once loaded, an item is never unloaded.
- **Upgrading/downgrading.** Load all of the items. The on screen items are rendered at full detail. The off screen items are rendered at a lower level of detail. As the user scrolls, the new on screen items are upgraded to be rendered at full detail while any items that used to be on screen that are now off screen are downgraded to be rendered at a lower level of detail.
- **Item virtualization.** Only the on screen items are rendered. The off screen items aren't rendered at all but the scrollbar is set up as though all of the items are rendered. As the user scrolls around, the on screen items are rendered and all other items are destroyed.

These virtualization techniques are useful in different scenarios and can be useful in combination.

Instead of only building different types of virtualization into high policy controls (e.g. `Hub`, `ListView`) it would be interesting to consider building primitives that support these different kinds of virtualization that apps can use in whatever combinations they feel are appropriate.