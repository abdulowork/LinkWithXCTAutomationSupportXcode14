Starting with Xcode 14 XCTAutomationSupport and other private frameworks have an allowlist of libraries that can link with them. Linking with XCTAutomationSupport results in the error:

```
ld: cannot link directly with dylib/framework, your binary is not an allowed client of /Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/PrivateFrameworks/XCTAutomationSupport.framework/XCTAutomationSupport for architecture arm64 clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

The allowlist is specified with the `LC_SUB_CLIENT` load command. You can see who is allowed to link using: `otool -l XCTAutomationSupport | grep -A 2 LC_SUB_CLIENT`.

This example illustrates how you can use and link `XCElementSnapshot` API which is private to `XCTAutomationSupport`.

The idea is to reexport `XCTAutomationSupport` through a fake library (`IDETestingFoundationTests`) with the name from the allowlist by using the `-reexport_framework XCTAutomationSupport` linker setting. 

To run select the `LinkWIthXCTAutomationSupport` scheme. 
