# Revealing Hidden Functions

## Tools used
APKTool, JADX, and Python.

## 1. Locate the hidden function
I decompiled task3_d.apk and searched for flag-related
methods and decoding operations.

In com.holberton.task4_d.MainActivityKt, I found this
private static method:

aBcDeFgHiJkLmNoPqRsTuVwXyZ123456

It accepts a Kotlin Function1 callback and passes the
decoded string to that callback.

The search of the decompiled application package showed
its declaration but no direct call site.

## 2. Understand the decoding
The method contains a Base64-encoded string.

After Base64 decoding, it processes each byte as follows:

1. Interpret the byte as an unsigned value.
2. XOR it with 19.
3. Rotate it right by two bits within eight bits.
4. Subtract the zero-based index multiplied by 3.
5. Normalize the result modulo 256.
6. Multiply by 183 and reduce modulo 256.
7. Convert the result to a character.

The characters are joined into the flag.

## 3. Recover the flag
I reproduced these operations in Python without changing
the APK's source or binary.

Recovered flag:
Holberton{calling_uncalled_functions_is_now_known!}

## Limitations
This recovery used static analysis and local reconstruction.
I did not invoke the hidden function inside the running app
or attach Frida, Objection, or GDB.

The task's runtime-invocation requirement remains incomplete.
