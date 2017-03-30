Tips for reducing compile times in Swift

See https://github.com/madhikarma/ios-resources to links with more info

## Project settings
- Disable dSYM files being being generated for Debug builds.
- Set Whole Module Optimisation on but disable the optimisations even for Debug builds (to compile all code as if it were one file which is faster than individual file dependency analysis). Big improvement (~30% -50% faster) compile times)

# Project setup
- Consider using frameworks and modules
- Precompile code frameworks using `lipo` if possible instead of linking via CocoaPods or Carthage. Note. strip simulator architectures from the frameworks as a build phase for release builds otherwise iTunes Connect will fail the validation of the binary.

## Code
- Add `final`, `fileprivate` and `private` to classes and methods respectively
- Add type info (avoid type inferene) to certain types like Dictionaries e.g. `[String: Any]`
- Use `fus` tool to find unused code
- Use Build Time Analyzer to profile functions that take long to compile (and optimize them)
- Beware of generics and lazy closures
- Reduce reliance on Extensions especially ones in separate files
- Profile and remove if needed nil coalescing operator e.g. `someOptional ?? 1 : 0`
- DRY up code where possible
- Look at your dependencies and move into frameworks if possible

