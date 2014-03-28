# Bugfix for https://github.com/jey9711/fibonacci

File ViewController.m ([e4d4b93](https://github.com/jey9711/fibonacci/blob/e4d4b93a4d18e1ba155e42835d06532c43447ccd/Fibonacci/ViewController.m)):

1. [e4d4b93](https://github.com/jey9711/fibonacci/blob/e4d4b93a4d18e1ba155e42835d06532c43447ccd/Fibonacci/ViewController.m) contains instance method `fibSe`, which calculates all Fibonacci numbers possible on the iPhone using unsigned integers, but does nothing with them -- effectively making loading time longer with no benefit.

2. [e4d4b93](https://github.com/jey9711/fibonacci/blob/e4d4b93a4d18e1ba155e42835d06532c43447ccd/Fibonacci/ViewController.m) has instance method `viewDidLoad` with an incorrect while() loop condition.  The correct condition is `while (oneAgo + twoAgo <= UINT_MAX)`, as it appears in subtractive form in method `fibSe` (to avoid overflow), on line 29.  The changed while condition, on line 43, is equivalent to `while (oneAgo + twoAgo <= UINT_MAX - current)` and is incorrect.  For `UINT_MAX` == 2³² - 1 (== 4,294,967,295), the last Fibonacci number under this is 2,971,215,073, and so the incorrect condition causes the search domain to be limited to 4,294,967,295 - `current` == 4,294,967,295 - 1,836,311,903 == 2,458,655,392.  In summary, `viewDidLoad` incorrectly stops at Fibonacci number 1,836,311,903 instead of 2,971,215,073.

3. [e4d4b93](https://github.com/jey9711/fibonacci/blob/e4d4b93a4d18e1ba155e42835d06532c43447ccd/Fibonacci/ViewController.m) instance method `fibSe` creates a correct NSMutableArray `fib` but it goes unused, while `viewDidLoad` creates an incorrect NSMutableArray `fib` and copies it into the NSArray instance variable `fibTerms`.

Fixes (see [fb7a1e79e0](https://github.com/thomastan/fibonacci/commit/fb7a1e79e017eb4387c44265aca636ef387defe6): [ViewController.h](https://github.com/thomastan/fibonacci/blob/fb7a1e79e017eb4387c44265aca636ef387defe6/Fibonacci/ViewController.h) and [ViewController.m](https://github.com/thomastan/fibonacci/blob/fb7a1e79e017eb4387c44265aca636ef387defe6/Fibonacci/ViewController.m)):
- The instance variable is made and kept a NSMutableArray, set by a correct instance method and the incorrect instance method is removed.
- A proper class interface (.h header file) is written.
- Subtitles with ordinals.