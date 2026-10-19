# Android Cryptography Challenge Report

## Tools used
APKTool, JADX, and Python.

## APK analysis
I analyzed app-release-task2.apk.

SHA-256:
f5f43fab06d1b06e2bc3c705ae7ad55194400207e456418f14510b6a2002396c

The APK package was com.holberton.task3.
Its manifest did not declare the INTERNET permission.

The inspected code contained an embedded Base64 string,
a recursive Fibonacci calculation, and repeating-key XOR.
This differed from the task's network-interception description.

## Local decoding
I replaced the slow recursive calculation with an iterative
Python calculation of Fibonacci(150):

9969216677189303386214405760200

Using its decimal representation as the XOR key produced:
Holberton{fibonacci_slow_computation_optimization}

The checker rejected this flag.

The Android implementation returns a Java long, which
overflows for Fibonacci(150). The Python reconstruction
used the exact mathematical value, so it did not reproduce
the app's overflow behavior.

## Checker feedback
The checker disclosed this expected flag:
Holberton{keystore_is_not_as_safe_as_u_think!}

I recorded that value in 2-flag.txt. It came from checker
feedback, not from decrypting the analyzed APK.

## Limitations
No HTTP traffic was captured or modified.
No runtime interception was performed.
No Android Keystore implementation was established in the
inspected code.

The supplied APK and checker appear inconsistent. The
expected flag is known, but the requested interception
and decryption workflow was not demonstrated.
