# Yara For Arm (Android)

If you need prebuilt version of yara, check `prebuilt` directory

# Compile

This instruction outlines how to compile yara for ARM. Specifically, for Android. We are going to start with 
arm8. You can repeat the below steps for the remaining
architectures. You can do this by chaning the target, 
    
        export TARGET=aarch64-linux-android (arm8)
        export TARGET=armv7a-linux-androideabi (arm7)
        export TARGET=i686-linux-android (x86)
        export TARGET=x86_64-linux-android (x86_64)

1. First clone yara, https://github.com/virustotal/yara
2. Next setup enviroment variables, 

        export TOOLCHAIN=$ANDROID_NDK_ROOT/toolchains/llvm/prebuilt/darwin-x86_64
        export TARGET=aarch64-linux-android
        # Set this to your minSdkVersion.
        export API=21
        # Configure and build.
        export AR=$TOOLCHAIN/bin/$TARGET-ar
        export AS=$TOOLCHAIN/bin/$TARGET-as
        export CC=$TOOLCHAIN/bin/$TARGET$API-clang
        export CXX=$TOOLCHAIN/bin/$TARGET$API-clang++
        export LD=$TOOLCHAIN/bin/$TARGET-ld
        export RANLIB=$TOOLCHAIN/bin/$TARGET-ranlib
        export STRIP=$TOOLCHAIN/bin/$TARGET-strip

3. Run autoreconf, 
    
        autoreconf --force --install

4. Configure

        CPPFLAGS="-I/Users/<user_name>/Desktop/yara/headers/" LDFLAGS="-L/Users/<user_name>/Desktop/yara/openssl_libs/x86_64" ./configure --host $TARGET --enable-static --disable-shared --with-crypto --enable-dex

        The flags should point to the dir where openssl headers and libs are located

5. Make
        
        make