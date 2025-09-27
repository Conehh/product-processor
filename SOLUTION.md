# Solution Document

## Bug Investigation & Fixes

### Issues Identified

1. **Caching Issue** – The cache `Map` did not use a fully unique key, leading to cache contamination and inconsistencies.

## Solutions Implemented

1. **Cache Fix** – Updated the logic to generate a unique cache key by combining `country` and `regulatoryId`, ensuring consistent and isolated cache entries.
