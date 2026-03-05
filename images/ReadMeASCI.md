# ASCII Encoding and Decoding in Python

This Python script demonstrates how to encode a text string into ASCII bytes, get its ASCII numerical values, and decode it back to a string.

```python
# Original text
```
```
text = "jello world"
```
```
print("Original Text:")
print(text)
print("-" * 40)
```
# Encode text to ASCII bytes
```
ascii_encoded = text.encode("ascii")
print("ASCII Encoded (bytes):", ascii_encoded)
```
# Convert ASCII bytes to numerical values
```
ascii_numbers = list(ascii_encoded)
print("ASCII Values:", ascii_numbers)
print("-" * 40)
```
# Decode ASCII bytes back to string
```
ascii_decoded = ascii_encoded.decode("ascii")
print("ASCII Decoded:", ascii_decoded)
```
Original Text:
```
jello world
```
----------------------------------------
```
ASCII Encoded (bytes): b'jello world'
```
```
ASCII Values: [106, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100]
```
----------------------------------------
```
ASCII Decoded: jello world
```
