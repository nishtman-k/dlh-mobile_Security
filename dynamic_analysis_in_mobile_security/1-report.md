# Native Function Analysis

## Tools used
APKTool, JADX, Ghidra, readelf, and Python.

## 1. Identify the native method
JADX showed that com.holberton.task2_d.MainActivity loads
libnative-lib.so and declares getSecretMessage().

The app calls this method but ignores its return value and
displays a fixed message instead.

## 2. Locate the JNI function
Using readelf, I identified the exported function:
Java_com_holberton_task2_1d_MainActivity_getSecretMessage

## 3. Analyze the decoding
In Ghidra, the function copies 49 bytes from .rodata.
The final byte is a null terminator, leaving 48 encoded bytes.

For each encoded byte, it subtracts lit(index % 10).
The lit() function calculates Fibonacci numbers, producing:
0, 1, 1, 2, 3, 5, 8, 13, 21, 34.

The decoded buffer is passed to JNI NewStringUTF to return
a Java string.

## 4. Recover the flag
I extracted .rodata using readelf and reproduced the subtraction
in Python. The recovered flag was:

Holberton{native_hooking_is_no_different_at_all}

## Limitations
This analysis used static reconstruction.
I did not run the app, attach Frida, or intercept the native
function at runtime. The dynamic hooking requirement remains
incomplete.
