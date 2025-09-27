# Solution Document

## Bug Investigation & Fixes

### Issues Identified

1. **Caching Issue** – The cache `Map` did not use a fully unique key, leading to cache contamination and inconsistencies.
2. **Performance Issue** – Batch processing did not leverage parallel execution, resulting in slower processing times.

## Solutions Implemented

1. **Cache Fix** – Updated the logic to generate a unique cache key by combining `country` and `regulatoryId`, ensuring consistent and isolated cache entries.
2. **Performance Fix** – Refactored batch logic to correctly use parallel processing. Added timing measurements and enhanced return values to include performance statistics.
