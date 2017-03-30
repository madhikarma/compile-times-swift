# compile-times-swift
Tips for reducing compile times in Swift

## Project settings
- Disable dSYM files being being generated for Debug builds.
- Set Whole Module Optimisation on but disable the optimisations even for Debug builds (to compile all code as if it were one file which is faster than individual file dependency analysis). Big improvement(~30% -50% faster) compile times)

# Project setup
- Consider using frameworks and modules
- Precompile code frameworks using `lipo` if possible instead of linking via CocoaPods or Carthage. Note. strip simulator architectures from the frameworks as a build phase for release builds otherwise iTunes Connect will fail the validation of the vinary.

## Code
- Add `final`, `fileprivate` and `private` to classes and methods respectively
- Use `fus` tool to find unused code
- Use Build Time Analyzer to profile functions that take long to compile (and optimize them)
- Reduce reliance on Extensions especially ones in seperate files
- Remove nil coalesciping operator usage e.g. `someOptional ?? 1 : 0`
- DRY up code
- Beware of generics and lazy closures

See https://github.com/madhikarma/ios-resources to links with more info
