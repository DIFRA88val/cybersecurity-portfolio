# 💾 Software Basics: Data Representation

**Module:** Software Basics  
**Chapter:** Data Representation  
**Objective:** Analyze how computer hardware processes, stores, and represents digital information using binary configurations, hexadecimal notation, and character encoding schemes.

---

## 🏗️ Core Concept Foundations

Computers do not understand text, images, or high-level code directly. At the lowest hardware layer, all information is converted into electrical signals represented by numbers. Understanding how data is structured is critical for analyzing network packets, malware payloads, and memory dumps.

### 1. The Binary System (Base-2)
* **Definition:** The fundamental language of computers consisting entirely of `0`s and `1`s.
* **Mechanism:** Each individual digit is called a **Bit** (Binary Digit). A collection of 8 bits forms a **Byte** (or Octet).
* **Security Application:** Analyzing raw bitstreams helps identify hidden data structures or obfuscated malware indicators.

### 2. The Hexadecimal System (Base-16)
* **Definition:** A numbering system that uses sixteen distinct symbols: `0-9` and `A-F` (where A=10, B=11, C=12, D=13, E=14, F=15).
* **Mechanism:** Used to simplify long binary strings. One hexadecimal character represents exactly 4 bits (a nibble).
* **Security Application:** Reading hex dumps is standard practice when inspecting compiled malware binaries or analyzing packet payloads in Wireshark.

---

## 🎨 Application: Color Representation in Binary & Hexadecimal

Computers translate visual elements like colors into numbers using the **RGB (Red, Green, Blue)** color model. By adjusting the electrical current intensity across these three core color channels, modern monitors display millions of unique hues.

### 1. The Foundational 8-Color Palette (3-Bit Model)
If a hardware system uses exactly **1 bit** per color channel, each knob can only be toggled `0` (Off) or `1` (On). This 3-bit combination yields exactly 8 possible base colors:

| Binary Matrix | Channel Mapping Status | Resulting Color Name |
| :--- | :--- | :--- |
| `000` | All color outputs are disabled | **Black** |
| `100` | Red channel is active exclusively | **Red** |
| `010` | Green channel is active exclusively | **Green** |
| `001` | Blue channel is active exclusively | **Blue** |
| `110` | Red and Green channels are active | **Yellow** |
| `101` | Red and Blue channels are active | **Magenta** |
| `011` | Green and Blue channels are active | **Cyan** |
| `111` | All color channels are active | **White** |

### 2. High-Fidelity Color Spaces (24-Bit / True Color)
Modern infrastructure assigns an **8-bit byte** to *each* individual channel, expanding the threshold to 256 unique light intensity levels per color (`0` to `255`).
* **Mathematical Matrix Structure:** 256 Red × 256 Green × 256 Blue = 16,777,216 discrete color combinations.
* **Storage Footprint:** 3 channels × 8 bits = 24 bits total allocation per pixel (3 Bytes).

### 3. Streamlining Data with Hexadecimal Notation
Every single byte (8 bits) of information inside a software payload or memory block is neatly represented by exactly **two hexadecimal digits**.
* **Example Target Value (Green Tone):**
  * *Binary Notation:* `10100011 11101010 00101010`
  * *Hexadecimal Compression:* `#A3EA2A`

---

## 🚩 Flag Captures & Lab Solutions

* **Question 1:** Preview the color #3BC81E. In one word, what does this color appear to be?
  * **Answer:** `green`
* **Question 2:** What is the binary representation of the color #EB0037?
  * **Answer:** `111010110000000000110111`
* **Question 3:** What is the decimal representation of the color #D4D8DF?
  * **Answer:** `212, 216, 223`

---

## 📝 Technical Summary & Key Terms Index

* **Decimal (Base-10):** The standard numerical framework used in daily operations, processing digits from `0` through `9`.
* **Binary (Base-2):** The native language of computing systems, tracking raw electronic state shifts using `0` (Off) and `1` (On).
* **Hexadecimal (Base-16):** An efficient, compressed format where every **4 binary digits (bits)** are grouped into a single character symbol ranging from `0–9` and `A–F`.
* **Octal (Base-8):** A system where every **3 binary digits (bits)** are grouped into an individual digit ranging from `0` to `7`. This format is common in legacy infrastructure and Linux file permission structures.
* **Bit (Binary Digit):** The smallest standalone unit of data storage on a computer, carrying an exact logic state value of either `0` or `1`.
* **Byte (Octet):** A foundational data building block comprised of exactly **8 contiguous bits**, capable of representing 256 unique variations (`0` to `255`).
