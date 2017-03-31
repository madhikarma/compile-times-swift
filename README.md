Tips for reducing compile times in Swift

See https://github.com/madhikarma/ios-resources to links with more info.

Note. your project's results may be different, be sure to profile and check if it helps or not.

`defaults write com.apple.dt.Xcode ShowBuildOperationDuration YES`

## Project settings
- Disable dwarf dSYM files being being generated for Debug builds.
- Set Whole Module Optimisation on but disable the optimisations even for Debug builds (to compile all code as if it were one file which is faster than individual file dependency analysis). Big improvement (~30% -50% faster) compile times but will have less log output.

# Project setup
- Consider using modules so the compiler can check dependencies across sections of the app. (thought could modules be used like Java packages?)
- Precompile code into frameworks using `lipo` if possible instead of linking via CocoaPods or Carthage. Note. strip simulator architectures from the frameworks as a build phase for release builds otherwise iTunes Connect will fail the validation of the binary.

## Code
- Add `final` to classes that don't need to be subclassed. Helps the compiler not having to check for subclasses and also optimizes speed of function invocation at run time (static instead of dynamic)
- Use `fileprivate` and `private` to private methods, structs and enums. Helps the compiler do less scope checking
- Add type info (avoid type inference) to complex types like Dictionaries e.g. `[String: Any]`
- Use `fus` tool to find unused code and remove them.
- Use Build Time Analyzer to profile functions that take long to compile (and optimize them)
- Beware lazy properties especially. closures, the type checking will chek each of these * the number of files. Move implementation to a private method instead.
- More private methods to break up larger functions (less scope for the compiler to check e.g. variables) 
- Reduce reliance on Extensions especially ones in separate files
- Profile and remove if needed nil coalescing operator e.g. `someOptional ?? 1 : 0` or ternary operators
- DRY up code where possible
- Look at your dependencies and move into frameworks if possible

