# Android Cryptography Analysis

## Tools used
APKTool, JADX, and Python.

## 1. Inspect the APK
I decompiled app-release-task2.apk using APKTool and JADX.
The package name was com.holberton.task3.

The decoded manifest contained no INTERNET permission.
The inspected flag-generation code used local data rather
than an encrypted server response.

## 2. Identify the decoding logic
In MainActivityKt.java, performslowDecryption() performs
these operations:

1. Base64-decodes an embedded string.
2. Calculates Fibonacci(150) using slowRecursive().
3. Converts the result to a decimal string.
4. Uses that string as a repeating XOR key.

Base64 is an encoding, not encryption. The decoding operation
here uses XOR, not AES or RSA.

## 3. Replace the slow calculation
The recursive implementation repeatedly calculates the same
Fibonacci values, making it impractical for this input.

I used an iterative Python calculation requiring 150 steps.

The exact Fibonacci value is:
9969216677189303386214405760200

## 4. Handle the integer-overflow difference
The Android function returns a signed 64-bit Java long.
Fibonacci(150) exceeds its range and wraps to:
6792540214324356296

The readable flag was recovered using the exact mathematical
value, not the overflowed value. Therefore, this recovery
does not reproduce the app's arithmetic exactly.

## 5. Result
Holberton{fibonacci_slow_computation_optimization}

## Limitations and task mismatch
The supplied instructions describe HTTP interception,
response manipulation, and AES or RSA analysis.

The inspected flag path instead uses embedded data,
Fibonacci computation, and XOR. No network traffic was
captured or modified, and no runtime hooking was performed.

The flag was recovered through static analysis and a local
Python reconstruction. The stated network-interception
requirements were not demonstrated.
