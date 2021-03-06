---
title: Release 0.5
version: 0.5
milestone: 10
layout: release
---

-   Reworked `IndexedPreference`, making it an interface that can
    be backed by indirect implementations in rating snapshots.  This
    seems to be faster.  Also added `getIndex()` method, returning
    a global index for the preference within a snapshot.

-   Adjusted the FunkSVD model builder to use global indices and an
    array of estimates, making model builds much faster.

-   Dropped by-user MAE from MAE evaluator and renamed “MAE.ByRating”
    in output to “MAE”, as per-user and global MAE are equivalent.

-   Moved `TaskTimer` to `lenskit-core` so it is available for
    logging elapsed times in algorithm implementations.
