---
title: A Simple Yet Deceptive CTF Challenge
description: >-
 A writeup for one of the challenge that I had solved at RVCTF-2024
author: anorak
date: 2024-06-10 00:00:00 +0530
categories: [GUIDE,CTF_WRITEUP]
tags: [Writeup, Cryptography, Reverse Engineering, Python, Forensics, CTF]
pin: false
---


# The Challenge:
- The task was pretty straightforward at first but the given java program made the challenge go haywire:

  
### PROBLEM STATEMENT:
- Here is a screenshot of the image:
 ![statement](/assets/img/202406/unscramble.png){: width="720" height="380" .w-75 .normal}


### Java Code:
- Below is the java code that was provided along with it:
  
  ```
  
            import java.util.Base64;
            import java.util.Scanner;
            
            public class mysteryMethods {
            
                public static void main(String[] args) {
                    Scanner scanner = new Scanner(System.in);
                    System.out.print("Flag: ");
                    String userInput = scanner.nextLine();
                    String encryptedInput = encryptInput(userInput);
            
                    // Print the encrypted input for debugging
                    System.out.println("Encrypted input: " + encryptedInput);
            
                    if (checkFlag(encryptedInput)) {
                        System.out.println("Correct flag! Congratulations!");
                    } else {
                        System.out.println("Incorrect flag! Please try again.");
                    }
                }
            
                public static String encryptInput(String input) {
                    String flag = input;
                    flag = toBase64(flag, 345345345);
                    System.out.println("After first Base64: " + flag);
                    flag = reverseStr(flag, 54321);
                    System.out.println("After reversing: " + flag);
                    flag = toBase64(flag, 12345);
                    System.out.println("After second Base64: " + flag);
                    flag = shift(flag, 25, 67890);
                    System.out.println("After shifting: " + flag);
                    flag = toHex(flag, 98765);
                    System.out.println("After hex conversion: " + flag);
                    flag = xorWithKey(flag, 9, 56789);
                    System.out.println("After XOR: " + flag);
                    return flag;
                }
            
                public static boolean checkFlag(String encryptedInput) {
                    // Known encrypted flag
                    String knownEncryptedFlag = "465a38585060405f685f4465734d6a636d4f45705f4e67384565403d5d5c6e506d5d3c4d513a513c5862663c58636a736a5c404d504e6639453866676d4f3873";
                    return encryptedInput.equals(knownEncryptedFlag);
                }
            
                public static String toBase64(String input, int dummy) {
                    return Base64.getEncoder().encodeToString(input.getBytes());
                }
            
                public static String reverseStr(String input, int dummy) {
                    return new StringBuilder(input).reverse().toString();
                }
            
                public static String shift(String input, int amount, int dummy) {
                    StringBuilder result = new StringBuilder();
                    for (char c : input.toCharArray()) {
                        if (Character.isLetter(c)) {
                            char base = Character.isUpperCase(c) ? 'A' : 'a';
                            int offset = (c - base + amount) % 26;
                            if (offset < 0) {
                                offset += 26;
                            }
                            c = (char) (base + offset);
                        }
                        result.append(c);
                    }
                    return result.toString();
                }
            
                public static String toHex(String input, int dummy) {
                    StringBuilder hex = new StringBuilder();
                    for (char c : input.toCharArray()) {
                        hex.append(String.format("%02x", (int) c));
                    }
                    return hex.toString();
                }
            
                public static String xorWithKey(String hexInput, int key, int dummy) {
                    StringBuilder xored = new StringBuilder();
                    for (int i = 0; i < hexInput.length(); i += 2) {
                        int hexChar = Integer.parseInt(hexInput.substring(i, i + 2), 16);
                        hexChar ^= key;
                        xored.append(String.format("%02x", hexChar));
                    }
                    return xored.toString();
                }
              }
            
  ```
  
# The Solution:

### Analysis:
  - Following are the steps that Through which I came up with the solution:
    1. Reverse XOR Operation:
         - XOR each pair of hexadecimal digits in the encrypted flag with the key 9.
    2. Convert Hexadecimal to ASCII:
          - Convert each pair of hexadecimal digits from the result of the XOR operation to their corresponding ASCII characters.
    3. Reverse Caesar Shift:
          - Reverse the Caesar shift by shifting each letter in the result back by 25 positions in the alphabet.
    4. Base64 Decode:
          - Base64 decode the string obtained after reversing the Caesar shift.
    5. Reverse the String:
          - Reverse the string obtained from the Base64 decode.
    6. Base64 Decode Again:
          - Base64 decode the final reversed string to obtain the original flag.

### Python Code:
   - Below is the python code that I wrote after following through the above steps:

```
     import base64

    def xor_with_key(hex_input, key):
        xored = ""
        for i in range(0, len(hex_input), 2):
            hex_char = int(hex_input[i:i+2], 16)
            hex_char ^= key
            xored += chr(hex_char)
        return xored
    
    def hex_to_ascii(hex_input):
        ascii_str = ""
        for i in range(0, len(hex_input), 2):
            ascii_str += chr(int(hex_input[i:i+2], 16))
        return ascii_str
    
    def caesar_shift(input_str, amount):
        shifted = ""
        for c in input_str:
            if c.isalpha():
                base = 'A' if c.isupper() else 'a'
                shifted += chr((ord(c) - ord(base) - amount) % 26 + ord(base))
            else:
                shifted += c
        return shifted
    
    def main():
        encrypted_flag = "465a38585060405f685f4465734d6a636d4f45705f4e67384565403d5d5c6e506d5d3c4d513a513c5862663c58636a736a5c404d504e6639453866676d4f3873"

    # Step 1: XOR with key 9
    step1 = xor_with_key(encrypted_flag, 9)
    print(f"After XOR: {step1}")
    
    # Step 2: Convert from hexadecimal to ASCII
    step2 = hex_to_ascii(step1.encode('utf-8').hex())
    print(f"After hex to ASCII: {step2}")
    
    # Step 3: Reverse the Caesar shift by 25
    step3 = caesar_shift(step2, 25)
    print(f"After Caesar shift: {step3}")
    
    # Step 4: Base64 decode
    step4 = base64.b64decode(step3).decode()
    print(f"After Base64 decode 1: {step4}")
    
    # Step 5: Reverse the string
    step5 = step4[::-1]
    print(f"After reversing: {step5}")
    
    # Step 6: Base64 decode
    flag = base64.b64decode(step5).decode()
    print("The flag is:", flag)

    if __name__ == "__main__":
        main()

 ```

### OUTPUT:

- Below is the output that I ended up with after I ran the above Code:
  
  ![solution](/assets/img/202406/output.webp){: width="720" height="380" .w-75 .normal}


This initial challenge in the RVCTF Preliminary Round was quite tricky, despite being worth 100 points and straight forward. It proved to be more time consuming than the other challenges in the round.
