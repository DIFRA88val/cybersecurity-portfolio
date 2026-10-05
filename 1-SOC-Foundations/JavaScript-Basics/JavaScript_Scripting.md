---

## 💻 Script Architecture & Lifecycle Evolution

### 📜 Version 1: The Basic Input Ingestion Draft (`guess_v1.js`)
This initial structural skeleton handles environmental interface initialization and synchronous console input reading using `readline/promises`. It converts the text string input parameter into a base-10 numerical data integer arrays, increments attempts, and terminates cleanly.

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

// Initialize system console communication streams via Node.js process APIs
const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * (20)) + 1; // 1 <= secret <= 20
    let tries = 0;
    let guess = 0; // Initialize out of range bounds to prevent accidental collisions

    console.log("I'm thinking of a number between 1 and 20");

    // Await user string input and cast to base-10 integer configuration arrays
    const text = await rl.question("Take a guess: "); 
    guess = parseInt(text, 10); 

    tries = tries + 1; 

} finally {
    rl.close(); // Prevent resource leaks by closing the I/O handle
}
```
* **Limitation:** The script completes its operational lifecycle without executing conditional validation routines or outputting triage parameters back to the console shell interface.

### 📜 Version 2: Complete Iterative Build with Feedback Engine (`guess_v3.js`)
To provide proper logic loops and continuous execution tracking without unexpected termination, the basic data collection elements are wrapped inside an asynchronous `while` iteration model.

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * (20)) + 1; 
    let tries = 0;
    let guess = 0; 

    console.log("I'm thinking of a number between 1 and 20");

    // The While Loop iterates sequentially as long as the non-equality check stays True
    while (guess !== secret) {
        const text = await rl.question("Take a guess: "); 
        guess = parseInt(text, 10); 

        tries = tries + 1; 

        // Evaluate structural conditions using if / else if / else
        if (guess < 1 || guess > 20) {
            console.log("That number is out of range. Try again.");
        } else if (guess < secret) {
            console.log("Too low, try again.");
        } else if (guess > secret) {
            console.log("Too high, try again.");
        } else {
            console.log("You got it in", tries, "tries!");
        }
    }
} finally {
    rl.close(); 
}
```


