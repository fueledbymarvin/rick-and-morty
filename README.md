This is a Vue 3 implementation of a Rick and Morty character explorer. It uses the graphql version of the Rick and Morty API. Apollo automatically provides caching of already requested resources. Search is debounced by a 200ms so that it doesn't spam the API. In order to save time, I didn't use a router and instead just made it a single page.

To run:
- `yarn`
- `yarn dev`
