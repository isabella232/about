<span class="timestamp-wrapper"><span class="timestamp">[2020-09-09 Wed 09:24]</span></span>

First thing to accomplish is establishing a baseline of performance we want to monitor and improve. I think picking a small number of queries which we regularly run against sourcegraph.com and record the time it takes. We want to measure things that are important, here is a list of ideas to seed the discussion.

-   The sort of queries generated by code intel (type:symbol on a repo). Currently this is by far our most common query, and deserves some measurement and investigation. My gut tells me there are quick wins in our graphql layer. Possibly some more fundemental changes for how symbols works in Zoekt.
-   The sort of queries our users generally do:
    -   We should probably analyse a days worth of searches to try and understand this. Below are some guesses.
    -   Global literal search with 50k results. A user trying out sourcegraph and just typing something in.
    -   Global literal search with ~1k results. Somewhat representative of a search where the user has a goal in mind (ie somewhat specific like an error code).
    -   Global literal search with <5 results. Needle in haystack search.
-   The sort of queries structural search generates. IE regex literals broken up with \`.\*\` between them. We may want to add a goal around structural search perf rather. eg pick a query done by the new homepage and try make that fast (and scale). That should inform zoekt work as well as how structural search interacts with zoekt. (imagine if strutural search was integrated into zoekt's matcher? Or if structural search directly spoke to zoekt and asked zoekt to return file contents in results. So structural search doesn't fetch archives)

Some links to discussions around zoekt performance:

-   zoekt: ideas to improve performance and memory usage [#10445](https://github.com/sourcegraph/sourcegraph/issues/10445)
-   Reduce memory usage of zoekt-webserver [google/zoekt#86](https://github.com/google/zoekt/issues/86)
-   index: Use roaring bitmaps for content posting lists [zoekt#10](https://github.com/sourcegraph/zoekt/pull/10)
-   [Indexing all OSS Code · search · Sourcegraph](https://github.com/orgs/sourcegraph/teams/search/discussions/3)
-   [Ideas for longer term code search infrastructure projects · search · Sourcegraph](https://github.com/orgs/sourcegraph/teams/search/discussions/1)
-   [3.17 search backend planning · search · Sourcegraph](https://github.com/orgs/sourcegraph/teams/search/discussions/2)
-   [The first notes on search being a product in Sourcegraph · Cloud · Sourcegraph](https://github.com/orgs/sourcegraph/teams/cloud/discussions/2)
-   sourcegraph.com search performance regularly poor [#9359](https://github.com/sourcegraph/sourcegraph/issues/9359)
-   searching for rare strings with count:x qualifier in large installation (xxx k repos) times out [#11395](https://github.com/sourcegraph/sourcegraph/issues/11395)