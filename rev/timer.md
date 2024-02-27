### [timer](https://play.picoctf.org/practice/challenge/381)

This challenge gives us an APK file named `timer.apk`. As this is a challenge of rev, we will decompile it first using online tools like [decompiler.com](https://www.decompiler.com/).

After we decompile it, we will see two folders.

- resources
- sources

Usually, the important build files are found in `com` folder which is inside `sources folder`.

- resources
- sources
    - android/support/v4 	
    - androidx 	
    - com
        - example/timer 	
        - google
    - kotlin 	
    - org

We can see that `com` folder has another two folders named `example/timer` and `google`. Lets check the first one. 

- resources
- sources
    - android/support/v4 	
    - androidx 	
    - com
        - example/timer
            - BuildConfig.java 	
            - MainActivity$$ExternalSyntheticLambda0.java 	
            - MainActivity$startCountingDown$1.java 	
            - MainActivity.java 	
            - R.java	
        - google
    - kotlin 	
    - org

Lets check the contents of each file.

`BuildConfig.java`

```java
    package com.example.timer;

public final class BuildConfig {
    public static final String APPLICATION_ID = "com.example.timer";
    public static final String BUILD_TYPE = "debug";
    public static final boolean DEBUG = Boolean.parseBoolean("true");
    public static final int VERSION_CODE = 1;
    public static final String VERSION_NAME = "picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}";
}
```

Here is our flag!

Flag: `picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}`
