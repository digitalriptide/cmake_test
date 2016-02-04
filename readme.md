CMake Does Not Use -isystem with GCC on OS X
===========================================

This is the simplest possible test case that I was able to generate to demonstrate
that CMake fails to pass the -isystem flag for includes marked as SYSTEM on OS X
with GCC, while Clang properly obtains the -system flag. This comparison was 
performed on OS X 10.11.3 with Apple LLVM version 7.0.2 and GCC 5.3.0 installed 
via the Homebrew tool. The tested version of CMake is 3.4.3.

To see that Clang properly picks up the flag, run:

    mkdir clang_build
    cd clang_build
    cmake ..
    VERBOSE=1 make

This will generate the following build command. Notice the *presence* of -isystem:

    /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++    -isystem /usr/local/include  -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk   -o CMakeFiles/cmake_test.dir/main.cpp.o -c /Users/jbauer/cmake_test/main.cpp

To see that GCC does not pick up the flag, run:

    mkdir gcc_build
    cd gcc_build
    CXX=g++-5 cmake ..
    VERBOSE=1 make

This will generate the following build command. Notice the *lack* of -isystem:

    /usr/local/bin/g++-5    -I/usr/local/include  -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk   -o CMakeFiles/cmake_test.dir/main.cpp.o -c /Users/jbauer/cmake_test/main.cpp
