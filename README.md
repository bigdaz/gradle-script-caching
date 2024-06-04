This repo contains a very simple build that demonstrates some build caching behaviour with Gradle that appears odd to me.

The build cache is enabled via `gradle.properties`, and each run is done starting with an empty Gradle User Home.

### Build cache is disabled for settings script in Gradle 8.8 (and earlier)

Running `gradle -g HOME --info help` with Gradle 8.8 produces the following output (only relevant lines retained).

```
Welcome to Gradle 8.8!

Generating /Users/daz/dev/playground/script-caching/HOME/caches/8.8/generated-gradle-jars/gradle-api-8.8.jar

Caching disabled for Kotlin DSL script compilation (Settings/TopLevel/stage1) because:
  Build cache is disabled
Settings evaluated using settings file '/Users/daz/dev/playground/script-caching/settings.gradle.kts'.
Using local directory build cache for the root build (location = /Users/daz/dev/playground/script-caching/HOME/caches/build-cache-1, removeUnusedEntriesAfter = 7 days).
Projects loaded. Root project using build file '/Users/daz/dev/playground/script-caching/build.gradle.kts'.

> Configure project :
Evaluating root project 'script-caching' using build file '/Users/daz/dev/playground/script-caching/build.gradle.kts'.
Build cache key for Kotlin DSL version catalog plugin accessors for classpath '99ebceb82eafe0a891fa1d711c47211d' is 7fd42c83204b4913165dd17b77f03b3e
Stored cache entry for Kotlin DSL version catalog plugin accessors for classpath '99ebceb82eafe0a891fa1d711c47211d' with cache key 7fd42c83204b4913165dd17b77f03b3e
Build cache key for Kotlin DSL plugin specs accessors for classpath '99ebceb82eafe0a891fa1d711c47211d' is f23b3ddc0de662268fad688884cab339
Stored cache entry for Kotlin DSL plugin specs accessors for classpath '99ebceb82eafe0a891fa1d711c47211d' with cache key f23b3ddc0de662268fad688884cab339
Build cache key for Kotlin DSL script compilation (Project/TopLevel/stage1) is c8fd1fdab47c4f409cc7f700726147eb
Stored cache entry for Kotlin DSL script compilation (Project/TopLevel/stage1) with cache key c8fd1fdab47c4f409cc7f700726147eb
Build cache key for Kotlin DSL accessors for root project 'script-caching' is 7c3a627d276ed7aa4226be8aa3f1e11c
Stored cache entry for Kotlin DSL accessors for root project 'script-caching' with cache key 7c3a627d276ed7aa4226be8aa3f1e11c
Build cache key for Kotlin DSL script compilation (Project/TopLevel/stage2) is 538c9cbc4677508b2946e3826286ebb9
Stored cache entry for Kotlin DSL script compilation (Project/TopLevel/stage2) with cache key 538c9cbc4677508b2946e3826286ebb9
```

Note the "Build cache is disabled" reason given for not caching the compiled settings script.

### Build cache is disabled for root project accessors with Gradle 8.9 nightly

Running `./gradlew -g HOME --info help` with Gradle 8.9 nightly produces the following output.

```
Welcome to Gradle 8.9-20240604002415+0000!

Caching disabled for Kotlin DSL script compilation (Settings/TopLevel/stage1) because:
  Build cache is disabled
Settings evaluated using settings file '/Users/daz/dev/playground/script-caching/settings.gradle.kts'.
Using local directory build cache for the root build (location = /Users/daz/dev/playground/script-caching/HOME/caches/build-cache-1, removeUnusedEntriesAfter = 7 days).
Projects loaded. Root project using build file '/Users/daz/dev/playground/script-caching/build.gradle.kts'.

> Configure project :
Evaluating root project 'script-caching' using build file '/Users/daz/dev/playground/script-caching/build.gradle.kts'.
Caching disabled for Kotlin DSL version catalog plugin accessors for classpath '8199ba89eabce6e651972fe30dc3f8c7' because:
  Build cache is disabled
Caching disabled for Kotlin DSL plugin specs accessors for classpath '8199ba89eabce6e651972fe30dc3f8c7' because:
  Build cache is disabled
Build cache key for Kotlin DSL script compilation (Project/TopLevel/stage1) is f75301c7780d9139dd75bdadd497a226
Stored cache entry for Kotlin DSL script compilation (Project/TopLevel/stage1) with cache key f75301c7780d9139dd75bdadd497a226
Caching disabled for Kotlin DSL accessors for root project 'script-caching' because:
  Build cache is disabled
Build cache key for Kotlin DSL script compilation (Project/TopLevel/stage2) is 0e0fb5b41d526ebbbef7acecda541bc9
Stored cache entry for Kotlin DSL script compilation (Project/TopLevel/stage2) with cache key 0e0fb5b41d526ebbbef7acecda541bc9
```

Note that this time, the caching is disabled for Kotlin DSL accessors as well as the settings script.
In each case, the reason is given (incorrectly) as "Build cache is disabled".
