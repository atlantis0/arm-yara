# Yara For Arm (Android)

If you need prebuilt version of yara, check `prebuilt` directory. It contains both cli executables
and static libary

# Compile

Use NDK Version 21+

This instruction outlines how to compile yara for ARM. Specifically, for Android. The below instruction were done on OSX. We are going to start with arm8. You can repeat the below steps for the remaining architectures. You can do this by chaning the target, 

    
        export TARGET=aarch64-linux-android (arm8)
        export TARGET=armv7a-linux-androideabi (arm7)
        export TARGET=i686-linux-android (x86)
        export TARGET=x86_64-linux-android (x86_64)

1. First clone yara, https://github.com/virustotal/yara
2. Next setup enviroment variables, 

        export TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/darwin-x86_64
        # Only choose one of these, depending on your device...
        export TARGET=aarch64-linux-android
        export TARGET=armv7a-linux-androideabi
        export TARGET=i686-linux-android
        export TARGET=x86_64-linux-android
        # Set this to your minSdkVersion.
        export API=21
        # Configure and build.
        export AR=$TOOLCHAIN/bin/llvm-ar
        export CC=$TOOLCHAIN/bin/$TARGET$API-clang
        export AS=$CC
        export CXX=$TOOLCHAIN/bin/$TARGET$API-clang++
        export LD=$TOOLCHAIN/bin/ld
        export RANLIB=$TOOLCHAIN/bin/llvm-ranlib
        export STRIP=$TOOLCHAIN/bin/llvm-strip

3. Run autoreconf, 
    
        autoreconf --force --install

4. Configure

        ./configure --host $TARGET --enable-static --disable-shared --disable-cuckoo --disable-dotnet --enable-dex

If you want to configure with openssl (--with-crypto), the flags should point to the dir where openssl headers and libs are located

        CPPFLAGS="-I/Users/sirack/Desktop/arm-yara/headers/" LDFLAGS="-L/Users/sirack/Desktop/arm-yara/openssl_libs/<ABI>" ./configure --host $TARGET --enable-static --disable-shared --disable-cuckoo --disable-dotnet --enable-dex --with-crypto


5. Make
        
        make

5. Find search for object files

        find libyara -type f -name "*.o" | paste -sd " " -

6. Build .a static library. Use $AR to archive object files

        $AR cr libyara.a <list of object files>

        <list of object files> - See #5 above

        E.g. $AR cr libyara.a libyara/proc/linux.o libyara/exefiles.o libyara/strutils.o libyara/proc.o libyara/compiler.o libyara/base64.o libyara/hex_lexer.o libyara/endian.o libyara/ahocorasick.o libyara/parser.o libyara/re.o libyara/bitmask.o libyara/threading.o libyara/hex_grammar.o libyara/libyara.o libyara/stream.o libyara/sizedstr.o libyara/re_grammar.o libyara/modules.o libyara/filemap.o libyara/scanner.o libyara/grammar.o libyara/atoms.o libyara/stopwatch.o libyara/stack.o libyara/exec.o libyara/object.o libyara/lexer.o libyara/re_lexer.o libyara/mem.o libyara/rules.o libyara/modules/pe/pe.o libyara/modules/pe/pe_utils.o libyara/modules/tests/tests.o libyara/modules/math/math.o libyara/modules/time/time.o libyara/modules/dex/dex.o libyara/modules/elf/elf.o libyara/modules/console/console.o libyara/arena.o libyara/notebook.o libyara/hash.o libyara/scan.o