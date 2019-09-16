# InstantSearch.js

To run the `App`, run:

```sh
yarn && yarn start
```

You can find the custom build of InstantSearch under [`lib/instantsearch.production.min.js`](lib/instantsearch.production.min.js). The build contains:

- [`searchBox`](https://www.algolia.com/doc/api-reference/widgets/search-box/js/) widget + connector
- [`hits`](https://www.algolia.com/doc/api-reference/widgets/hits/js/) widget + connector
- [`stats`](https://www.algolia.com/doc/api-reference/widgets/stats/js/) widget + connector

The connector are always present because the widgets are built internaly with the connectors.

## How to make a new version

1. check out https://github.com/algolia/instantsearch.js
2. go to the branch you want to base this patch on
3. modify the main import file so instead of this:

```js
import * as connectors from '../connectors/index';
import * as widgets from '../widgets/index';
import * as helpers from '../helpers/index';

import * as routers from './routers/index';
import * as stateMappings from './stateMappings/index';

// ...
const instantsearch = (options: InstantSearchOptions) =>
  new InstantSearch(options);

instantsearch.routers = routers;
instantsearch.stateMappings = stateMappings;
instantsearch.connectors = connectors;
instantsearch.widgets = widgets;
instantsearch.version = version;
instantsearch.highlight = helpers.highlight;
instantsearch.snippet = helpers.snippet;
instantsearch.insights = helpers.insights;

export default instantsearch;
```

it looks like this:

```js
import {
  connectHits,
  connectStats,
  connectSearchBox,
} from './connectors/index';
import { hits, stats, searchBox } from './widgets/index';

// ...
const instantsearch = (options: InstantSearchOptions) =>
  new InstantSearch(options);

instantsearch.connectors = {
  connectHits,
  connectStats,
  connectSearchBox,
};
instantsearch.widgets = {
  hits,
  stats,
  searchBox,
};

instantsearch.version = version;

export default instantsearch;
```

Make sure to keep only what you are using in your build.

4. run the build script `VERSION=4.0.0-beta.0-custom-build yarn build:umd` (replace `4.0.0-beta.0` by the current version number)
5. replace the build files in this `lib` folder by the ones created in `dist` in InstantSearch.js
