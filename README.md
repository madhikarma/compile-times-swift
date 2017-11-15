Tips for reducing compile times in Swift

See https://github.com/madhikarma/ios-resources to links with more info.

Note. your project's results may be different, be sure to profile and check if it helps or not.

`defaults write com.apple.dt.Xcode ShowBuildOperationDuration YES`

## Project settings
- Disable dwarf dSYM files being being generated for Debug builds http://holko.pl/2016/10/18/dsym-debug/
- NOTE. this will force a full compile that will take longer than if an incremental build were to be performed and will also make your laptop noisy. It is faster than a clean build overall but best kept for CI builds perhaps as those are often clean and on desktop hardware IMO:
Make sure that "Whole Module Optimisation" is switched in your build configurations but disable the optimisations part for Debug builds by adding "-O none" to Other Swift Flags. This will enable WMO - making all the code compile if it were one file - but skip the optimisations bit for Debug builds. Big improvement (~30% -50% faster) compile times but builds will have less log output per file compiling as essentially the whole module is compiling. Alternatively add the legacy "SWIFT_WHOLE_MODULE_OPTIMIZATION" as a user defined setting but note. Xcode will warn that it's deprecated as WMO is now the default. http://roadfiresoftware.com/2017/01/improving-swift-compile-times/
- Tweak Xcode GUI and / or xcodebuild (CLI) compiler settings of build processes and subtasks https://gist.github.com/nlutsenko/ee245fbd239087d22137

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

## Relevant links

https://www.linkedin.com/pulse/best-hardware-build-swift-what-you-might-think-jacek-suliga
