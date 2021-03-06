---
title: Release 0.3
version: 0.3
milestone: 8
---

## Event Infrastructure

For flexibility going forward, LensKit 0.3 introduces a new
`Event` infrastructure (#issue(48)) for representing user data.
Users have sequences of events, represented by the `Event`
interface.  Each event has an ID and an optional timestamp.  Events
may be associated with items (subclasses of the `ItemEvent`
interface).  Ratings are now **rating events**; they store a
`Preference`, which represents the preference expressed by the
user.  All of these interfaces and classes have been moved to the
package `org.grouplens.lenskit.data.event` (with the preferences
in `o.g.l.d.pref`).

As a result of this, all ratings must now have globally-unique
rating IDs (which are synthesized when they are not present in the
data set).  Rating object equality is now defined solely by the
event ID as well.  Also, the `Ratings.fromUserVector` method has
been removed, as it no longer works.

Rating snapshots now discard timestamps and return
`IndexedPreference` objects, and the `IndexedRating` object
has gone away.

User profiles have also been changed; the `UserRatingProfile`
class has been removed and replaced with `UserHistory` for a
generalized representation of user histories.

### Restructured DAO interfaces

   The new event infrastructure has necessitated some overhauling of
   the DAO interface.  The new interface is hopefully simpler.  There
   are no longer multiple DAO interfaces in LensKit; everything is
   consolidated into the `DataAccessObject` interface (and the old
   `UserItemDataAccessObject` and `RatingDataAccessObject`
   interfaces have been removed).  The methods are renamed to be
   `getEvents` and friends.  Also, sorting is no longer supported
   for item and user event sequences.  

   Integrators may still wish to subclass `DataAccessObject` to
   make additional information, such as user or item metadata,
   available to recommenders.

   Implementations have also been renamed:

   * `SimpleFileDAO` has been renamed to
     `SimpleFileRatingDAO`, as it only supports rating data.

   * `RatingCollectionDAO` has been renamed to
     `EventCollectionDAO`.  It contains some optimizations to
     speed filtering.

## Other Changes

-   The deprecated `SparseVector.wrap` methods now return
    `SparseVector` rather than `MutableSparseVector`, so that
    they can be overridden in `ImmutableSparseVector`.

-   Removed `NoSessionException`, as it has not been in use for
    some time.

-   `PackedRatingSnapshot` now randomizes the order of ratings in
    the snapshot to prevent ordering effects from impacting iterative
    methods.

-   Updated to GL Common 0.3.  This introduces the requirement that
    all elements in cursors be non-null.

-   Made `IdentityVectorNormalizer` serializable (#issue(106)).

-   Fixed bug where the recommender engine opened multiple DAOs for
    each session (#issue(108)).

-   Made POMs runnable with Maven 2, so Maven 2 can be used to compile
    LensKit (although it cannot generate the site) (#issue(72)).
