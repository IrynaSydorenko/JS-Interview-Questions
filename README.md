<details>
  <summary><strong>1. How to iterate over the properties of an object in JavaScript?</strong></summary>
  
  <p>In JavaScript, you can iterate over the properties of an object using several methods, depending on whether you want to include inherited properties, enumerable properties, or only own properties. Here are the most common approaches:</p>

  <h4>1. <code>for...in</code> loop</h4>
  <p>This loop iterates over all enumerable properties of an object, including inherited ones. To exclude inherited properties, use <code>hasOwnProperty()</code>.</p>
  
  ```js
  const user = {
    name: "Iryna",
    age: 25
  };

  for (const key in user) {
    if (user.hasOwnProperty(key)) { // excludes inherited properties
      console.log(key, user[key]);
    }
  }
  ```

  <h4>2. <code>Object.keys()</code></h4>
  <p>Returns an array of the object's own enumerable property names (keys).</p>
  
  ```js
  const user = {
    name: "Iryna",
    age: 25
  };

  Object.keys(user).forEach(key => {
    console.log(key, user[key]);
  });
  ```

  <h4>3. <code>Object.values()</code></h4>
  <p>Returns an array of the object's own enumerable values.</p>
  
  ```js
  const user = {
    name: "Iryna",
    age: 25
  };

  Object.values(user).forEach(value => {
    console.log(value);
  });
  ```

  <h4>4. <code>Object.entries()</code></h4>
  <p>Returns an array of the object's own enumerable [key, value] pairs.</p>
  
  ```js
  const user = {
    name: "Iryna",
    age: 25
  };

  Object.entries(user).forEach(([key, value]) => {
    console.log(key, value);
  });
  ```

  <p><strong>Conclusion:</strong> Use <code>for...in</code> with <code>hasOwnProperty()</code> if you want to iterate over all enumerable properties but exclude inherited ones. Prefer <code>Object.keys()</code>, <code>Object.values()</code>, or <code>Object.entries()</code> for a more modern and cleaner approach when working with own properties only.</p>
</details>

<details>
  <summary><strong>2. What are source maps in JavaScript?</strong></summary>

  <p><strong>Source maps</strong> are files that map the transformed, compiled, or minified JavaScript code back to the original source code (like from TypeScript, SCSS, or ES6+ code). This is especially helpful when debugging in the browser, as it allows developers to see the original code instead of the transformed output.</p>

  <h4>Why source maps are important:</h4>
  <ul>
    <li>Modern web development often involves code compilation (e.g., TypeScript to JavaScript, or bundling with Webpack).</li>
    <li>Browsers run the compiled code, but as a developer, you want to debug the original code you wrote.</li>
    <li>Source maps bridge the gap, enabling you to debug as if you were working with your original files.</li>
  </ul>

  <h4>How it works:</h4>
  <p>When you build your project, a source map file is generated (e.g., <code>app.js.map</code>). This file contains mappings between your minified/compiled code and the original source files.</p>

  <p>A comment like the one below is typically added to the end of your compiled JavaScript file to link to the source map:</p>

  ```js
  //# sourceMappingURL=app.js.map
  ```

  <p>When DevTools detects this comment and finds the corresponding source map file, it loads the original source code in the debugger.</p>

  <h4>Example with TypeScript:</h4>

  TypeScript config (<code>tsconfig.json</code>):
  ```json
  {
    "compilerOptions": {
      "sourceMap": true
    }
  }
  ```
  <p>This setting tells the TypeScript compiler to generate <code>.map</code> files alongside the compiled JavaScript files.</p>

  <h4>Key points:</h4>
  <ul>
    <li>Source maps are not needed for the app to run — they're only for development/debugging.</li>
    <li>You can (and should) exclude them from production builds for performance and security.</li>
    <li>They improve the developer experience significantly.</li>
  </ul>

  <p><strong>Conclusion:</strong> Source maps make debugging easier by allowing you to view and debug your original source code in the browser, even after it's been compiled or minified.</p>
</details>

<details>
  <summary><strong>3. How are numbers stored in JavaScript? Why does 0.1 + 0.2 !== 0.3?</strong></summary>

  <h4>How numbers are stored:</h4>
  <p>In JavaScript, all numbers (both integers and floating-point) are stored using the <strong>IEEE 754 double-precision 64-bit floating-point format</strong>. This format is binary-based and has limited precision (about 15–17 decimal digits).</p>

  <h4>Why 0.1 + 0.2 !== 0.3?</h4>
  <p>Some decimal numbers like <code>0.1</code> and <code>0.2</code> cannot be represented exactly in binary. When these numbers are stored in memory, the nearest possible binary approximation is used.</p>

  <p>This leads to small rounding errors when performing arithmetic with these values. For example:</p>

  ```js
  console.log(0.1 + 0.2); // 0.30000000000000004
  console.log(0.1 + 0.2 === 0.3); // false
  ```

  <h4>What's really happening:</h4>
  <ul>
    <li><code>0.1</code> becomes something like <code>0.10000000000000000555...</code></li>
    <li><code>0.2</code> becomes something like <code>0.20000000000000001110...</code></li>
    <li>Their sum is slightly more than <code>0.3</code>, so strict comparison fails</li>
  </ul>

  <h4>How to compare floats safely:</h4>
  <p>Instead of using <code>===</code>, compare the difference between the numbers with a small threshold (called "epsilon"):</p>

  ```js
  function isApproximatelyEqual(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
  }

  console.log(isApproximatelyEqual(0.1 + 0.2, 0.3)); // true
  ```

  <h4>Key point:</h4>
  <p>This isn't a bug in JavaScript — it's a limitation of binary floating-point arithmetic that affects all programming languages using IEEE 754 format.</p>

  <p><strong>Conclusion:</strong> JavaScript uses binary floating-point arithmetic, which can't represent some decimal fractions exactly. This leads to small rounding errors like <code>0.1 + 0.2 !== 0.3</code>. Always be cautious with floating-point comparisons and use a tolerance when checking for equality.</p>
</details>

<details>
  <summary><strong>4. What types of errors exist in JavaScript and what do they mean? Do logical errors stop script execution?</strong></summary>

### JavaScript Error Types

JavaScript errors can be categorized into three main types:

1. **Syntax Errors**

   * These occur when code breaks the syntax rules of the language.
   * **Example:** Missing a closing brace or parenthesis.

   ```js
   if (true {
     console.log('Missing closing parenthesis');
   }
   ```

   **Result:** Script stops immediately at the error.

2. **Runtime Errors** (also called Exceptions)

   * These happen during code execution, after the script has passed syntax checking.
   * Examples include trying to access an undefined variable or calling a method on `undefined`.

   ```js
   const user = undefined;
   console.log(user.name); // TypeError: Cannot read property 'name' of undefined
   ```

   **Result:** Script execution is halted at the error line.

3. **Logical Errors**

   * The code runs without crashing, but the result is incorrect due to a mistake in logic.
   * These are the hardest to catch because there’s no visible error message.

   ```js
   const discount = price * 1.2; // Should be * 0.8 to apply 20% discount
   ```

   **Result:** Script runs successfully, but behavior is wrong.
   **Logical errors do NOT stop execution.**

---

### Summary

* **Syntax Errors:** Stop execution immediately.
* **Runtime Errors:** Stop execution where they occur unless caught with `try...catch`.
* **Logical Errors:** Do *not* stop script execution, but lead to incorrect results.

---

**Conclusion:**
Logical errors do *not* throw exceptions and don’t halt the script, but can silently break functionality.
Understanding the different types of errors helps you debug more effectively and write more reliable code.

</details>

<details>
  <summary><strong>5. How can you check if a variable exists in JavaScript?</strong></summary>

  <p>Checking whether a variable exists in JavaScript depends on how the variable is declared and in what context you’re performing the check. Here are the most common and safe methods:</p>

  <h4>1. Using <code>typeof</code> (Safe for undeclared variables)</h4>
  <p>This is the safest way because <code>typeof</code> does not throw an error even if the variable is not declared.</p>

  ```js
  if (typeof myVar !== 'undefined') {
    console.log('myVar exists');
  }
  ```

  <strong>Use this</strong> when you’re not sure whether a variable has ever been declared.

  <h4>2. Using <code>window</code> (for global variables in browsers)</h4>
  <p>In browsers, global variables are properties of the <code>window</code> object. You can check existence like this:</p>

  ```js
  if ('myVar' in window) {
    console.log('myVar exists');
  }
  ```

  <strong>Note:</strong> This only works for global scope variables.

  <h4>3. Checking for <code>null</code> or <code>undefined</code></h4>
  <p>If the variable is declared but may be undefined or null:</p>

  ```js
  if (myVar != null) {
    console.log('myVar is not null or undefined');
  }
  ```

  <strong>Note:</strong> This assumes the variable is already declared — otherwise it will throw a ReferenceError.

  <h4>4. Optional chaining (for object properties)</h4>
  ```js
  if (user?.name) {
    console.log('user.name exists');
  }
  ```

  <p>Good for avoiding errors when accessing nested object properties that might be undefined.</p>

  <h4>⚠️ Common Mistake:</h4>
  <p>Trying to access an undeclared variable directly throws a <code>ReferenceError</code>:</p>

  ```js
  if (myVar) { // ❌ ReferenceError if myVar is not declared
    ...
  }
  ```

  <h4>Summary:</h4>
  <ul>
    <li>Use <code>typeof variable !== 'undefined'</code> to safely check undeclared variables.</li>
    <li>Use <code>'varName' in window</code> to check global variables in browsers.</li>
    <li>Use optional chaining <code>?.</code> for safe nested property access.</li>
    <li>Do <strong>not</strong> directly check undeclared variables without <code>typeof</code>.</li>
  </ul>
</details>

<details>
  <summary><strong>6. What is recursion? What should you consider first when working with recursion? What happens if the base case is missing?</strong></summary>

  <h4>What is recursion?</h4>
  <p>Recursion is a programming technique in which a function calls itself in order to solve a problem. It breaks the problem into smaller sub-problems of the same type until it reaches a condition where no further recursion is needed. This condition is called the <strong>base case</strong>.</p>

  <h4>What should you consider first when working with recursion?</h4>
  <ul>
    <li><strong>Base case (termination condition):</strong> Always define a clear base case that stops the recursion. Without it, the function will call itself forever.</li>
    <li><strong>Progress towards the base case:</strong> Each recursive call should bring the problem closer to the base case. Otherwise, the function will never stop.</li>
    <li><strong>Stack size:</strong> Each recursive call adds a new frame to the call stack. Deep or infinite recursion can cause a <code>RangeError</code> due to stack overflow.</li>
  </ul>

  <h4>What happens if the base case is missing?</h4>
  <p>If there's no base case, or it is incorrectly implemented, the recursive function will keep calling itself endlessly. This leads to a <strong>stack overflow error</strong>:</p>

  ```js
  function infiniteRecursion() {
    return infiniteRecursion(); // No base case
  }
  infiniteRecursion(); // ❌ RangeError: Maximum call stack size exceeded
  ```

  <h4>Correct example with a base case:</h4>

  ```js
  function factorial(n) {
    if (n === 0) return 1; // ✅ base case
    return n * factorial(n - 1); // recursive step
  }

  console.log(factorial(5)); // 120
  ```

  <h4>Summary:</h4>
  <ul>
    <li>Recursion = a function calling itself.</li>
    <li>Always define a base case to avoid infinite loops.</li>
    <li>Ensure each call moves toward the base case.</li>
    <li>Use recursion only when it makes the logic clearer or simpler (e.g., tree traversal, factorial, Fibonacci).</li>
  </ul>
</details>

<details>
  <summary><strong>7. What is the difference between a recursive and an iterative process?</strong></summary>

  <h4>Recursive process:</h4>
  <p>A recursive process is one where a function solves a problem by calling itself with a smaller input. The function keeps calling itself until it reaches a base case that stops the recursion.</p>

  <h4>Example of recursion:</h4>

  ```js
  function factorial(n) {
    if (n === 0) return 1; // base case
    return n * factorial(n - 1); // recursive step
  }

  console.log(factorial(5)); // 120
  ```

  <ul>
    <li>Uses the call stack to keep track of function calls.</li>
    <li>Each call adds a new frame to the stack.</li>
    <li>Can lead to a stack overflow if the recursion is too deep.</li>
    <li>Often more elegant and readable for problems with hierarchical or nested structure (e.g., tree traversal).</li>
  </ul>

  <h4>Iterative process:</h4>
  <p>An iterative process solves a problem using loops (<code>for</code>, <code>while</code>) instead of calling itself.</p>

  <h4>Example of iteration:</h4>

  ```js
  function factorial(n) {
    let result = 1;
    for (let i = 1; i <= n; i++) {
      result *= i;
    }
    return result;
  }

  console.log(factorial(5)); // 120
  ```

  <ul>
    <li>Uses a loop and a mutable variable to accumulate results.</li>
    <li>Does not add extra frames to the call stack.</li>
    <li>More memory-efficient for large inputs.</li>
    <li>May be more verbose but often faster in practice.</li>
  </ul>

  <h4>Comparison summary:</h4>
  <table>
    <thead>
      <tr><th>Aspect</th><th>Recursive</th><th>Iterative</th></tr>
    </thead>
    <tbody>
      <tr><td>Performance</td><td>Slower due to stack usage</td><td>Faster, uses simple loops</td></tr>
      <tr><td>Memory</td><td>Uses more memory (call stack)</td><td>More efficient</td></tr>
      <tr><td>Readability</td><td>More elegant for some problems</td><td>May be longer</td></tr>
      <tr><td>Risk</td><td>Stack overflow if base case is missing</td><td>Less risk of overflow</td></tr>
    </tbody>
  </table>

  <h4>Conclusion:</h4>
  <p>Recursion is often clearer for problems that are naturally recursive (e.g., tree or graph traversal), while iteration is preferred for performance-sensitive tasks. Always make sure a recursive solution has a valid base case to prevent infinite loops.</p>
</details>

<details>
  <summary><strong>8. What are JavaScript modules and what are they used for? What is a default export? Can we export/import multiple values separately? Will there be name conflicts if multiple imported modules use the same variable names internally?</strong></summary>

  <h4>What are modules in JS?</h4>
  <p>Modules in JavaScript allow developers to split code into separate files, each containing specific functionality. This makes code easier to maintain, reuse, and test. JavaScript modules are supported natively in modern browsers and also used in Node.js with slightly different syntax.</p>

  <h4>Why modules are needed:</h4>
  <ul>
    <li>Encapsulation: Keeps variables and functions scoped to their file unless explicitly exported.</li>
    <li>Reusability: Allows sharing functionality across files.</li>
    <li>Maintainability: Organizing code into modules improves clarity and debugging.</li>
  </ul>

  <h4>Default export:</h4>
  <p>You can export a single default value from a module. It can be a function, class, object, etc.</p>

  ```js
  // math.js
  export default function add(a, b) {
    return a + b;
  }

  // usage
  import add from './math.js';
  console.log(add(2, 3)); // 5
  ```

  <h4>Named exports (multiple exports):</h4>
  <p>You can export multiple variables or functions by name.</p>

  ```js
  // utils.js
  export const PI = 3.14;
  export function multiply(a, b) {
    return a * b;
  }

  // usage
  import { PI, multiply } from './utils.js';
  console.log(PI); // 3.14
  console.log(multiply(2, 4)); // 8
  ```

  <h4>Can we import both default and named exports?</h4>
  <p>Yes, you can import both in the same statement:</p>

  ```js
  import defaultFunc, { namedFunc1, namedFunc2 } from './module.js';
  ```

  <h4>Name conflicts:</h4>
  <p>No, there will be no name conflicts from different modules having the same variable or function names internally. This is because the scope of those names is local to the module.</p>

  ```js
  // moduleA.js
  const secret = 'A';
  export const getSecret = () => secret;

  // moduleB.js
  const secret = 'B';
  export const getSecret = () => secret;

  // main.js
  import { getSecret as getASecret } from './moduleA.js';
  import { getSecret as getBSecret } from './moduleB.js';

  console.log(getASecret()); // 'A'
  console.log(getBSecret()); // 'B'
  ```

  <h4>Conclusion:</h4>
  <p>Modules allow splitting code into smaller, reusable parts. Use default exports for a single main value and named exports for multiple values. Name conflicts do not occur because each module has its own scope.</p>
</details>

<details>
  <summary><strong>9. What is a pure function? What is a deterministic function? Is it necessary for a function to be both free of side effects and deterministic to be considered pure, or is one of them enough?</strong></summary>

  <h4>What is a pure function?</h4>
  <p>A <strong>pure function</strong> is a function that satisfies two main conditions:</p>
  <ul>
    <li><strong>No side effects</strong>: It does not modify anything outside of its scope (no changing global variables, no DOM manipulation, no API calls, etc.).</li>
    <li><strong>Deterministic</strong>: Given the same input, it always returns the same output.</li>
  </ul>

  ```js
  // Pure function
  function square(x) {
    return x * x;
  }

  // Not pure: modifies external variable
  let counter = 0;
  function increment() {
    counter++;
  }

  // Not pure: non-deterministic (depends on external state)
  function getRandomNumber() {
    return Math.random();
  }
  ```

  <h4>What is a deterministic function?</h4>
  <p>A <strong>deterministic function</strong> is a function that produces the same result for the same input every time you call it.</p>

  ```js
  function add(a, b) {
    return a + b; // always the same output for same inputs
  }

  function getTimeBasedGreeting() {
    return new Date().getHours() < 12 ? 'Good morning' : 'Good evening';
    // not deterministic
  }
  ```

  <h4>Do both conditions need to be met for a function to be pure?</h4>
  <p>Yes, a function must be both <strong>deterministic</strong> <em>and</em> <strong>have no side effects</strong> to be considered <strong>pure</strong>.</p>

  <ul>
    <li>If a function has no side effects but is not deterministic — it's not pure.</li>
    <li>If a function is deterministic but causes side effects — it's not pure either.</li>
  </ul>

  <h4>Why are pure functions important?</h4>
  <ul>
    <li>Easier to test and debug.</li>
    <li>Predictable behavior.</li>
    <li>Useful in functional programming and when using React hooks like <code>useMemo</code> and <code>useCallback</code>.</li>
  </ul>

  <h4>Conclusion:</h4>
  <p>A pure function must <strong>not produce side effects</strong> and must be <strong>deterministic</strong>. Both conditions are essential for a function to be considered truly pure.</p>
</details>

<details>
  <summary><strong>10. What is scope? What is the difference between var / let / const? What is hoisting?</strong></summary>

  <h4>What is scope?</h4>
  <p><strong>Scope</strong> determines the visibility and accessibility of variables in different parts of your code.</p>
  <ul>
    <li><strong>Global Scope:</strong> Accessible everywhere in the code.</li>
    <li><strong>Function Scope:</strong> Created with <code>var</code>. Only accessible inside the function where it's declared.</li>
    <li><strong>Block Scope:</strong> Created with <code>let</code> and <code>const</code>. Only accessible inside the block (like <code>if</code>, <code>for</code>, or <code>{}</code>).</li>
  </ul>

  ```js
  if (true) {
    var a = 1;
    let b = 2;
    const c = 3;
  }
  console.log(a); // 1
  console.log(b); // ReferenceError
  console.log(c); // ReferenceError
  ```

  <h4>Difference between var, let, and const</h4>
  <ul>
    <li><strong>var</strong>:
      <ul>
        <li>Function-scoped</li>
        <li>Can be re-declared and updated</li>
        <li>Hoisted (initialized as <code>undefined</code>)</li>
      </ul>
    </li>
    <li><strong>let</strong>:
      <ul>
        <li>Block-scoped</li>
        <li>Can be updated but not re-declared in the same scope</li>
        <li>Hoisted but not initialized (temporal dead zone)</li>
      </ul>
    </li>
    <li><strong>const</strong>:
      <ul>
        <li>Block-scoped</li>
        <li>Cannot be updated or re-declared</li>
        <li>Must be initialized at the time of declaration</li>
      </ul>
    </li>
  </ul>

  ```js
  function example() {
    console.log(x); // undefined due to var hoisting
    var x = 5;

    // console.log(y); // ReferenceError due to TDZ
    let y = 10;
  }
  ```

  <h4>What is hoisting?</h4>
  <p><strong>Hoisting</strong> is JavaScript's behavior of moving declarations to the top of the current scope before code execution.</p>
  <ul>
    <li><code>var</code> declarations are hoisted and initialized with <code>undefined</code>.</li>
    <li><code>let</code> and <code>const</code> declarations are hoisted but not initialized. Accessing them before declaration causes a <strong>ReferenceError</strong>.</li>
    <li>Function declarations are hoisted entirely (definition + body).</li>
  </ul>

  ```js
  greet(); // "Hello!"
  function greet() {
    console.log("Hello!");
  }
  ```

  <h4>Conclusion:</h4>
  <p>Understanding scope and hoisting helps avoid unexpected behaviors and bugs. Prefer <code>let</code> and <code>const</code> over <code>var</code> for safer, clearer code.</p>
</details>

<details>
  <summary><strong>11. What is a closure in JavaScript?</strong></summary>

  <h4>Definition:</h4>
  <p>A <strong>closure</strong> is a function that <strong>remembers the variables from its lexical scope</strong> even when it's executed outside of that scope.</p>

  <h4>How it works:</h4>
  <p>When a function is created inside another function, it forms a closure. The inner function has access to:</p>
  <ul>
    <li>Its own scope (variables defined inside it)</li>
    <li>The outer function’s variables</li>
    <li>The global scope</li>
  </ul>

  ```js
  function outer() {
    let counter = 0; // this variable is part of the closure

    return function inner() {
      counter++;
      console.log(counter);
    };
  }

  const increment = outer();
  increment(); // 1
  increment(); // 2
  ```

  <h4>Why closures are useful:</h4>
  <ul>
    <li>They help create <strong>private variables</strong>.</li>
    <li>Used in <strong>callbacks, event handlers, setTimeout, setInterval</strong>.</li>
    <li>Important in <strong>functional programming</strong> and working with <code>useState</code>, <code>useEffect</code> in React.</li>
  </ul>

  <h4>Common mistake:</h4>
  <p>Closures keep references to variables, not copies. Be careful with loops:</p>

  ```js
  for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
  }
  // logs: 3, 3, 3 (because of var)

  // Fix using let:
  for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
  }
  // logs: 0, 1, 2
  ```

  <h4>Conclusion:</h4>
  <p>A closure allows a function to remember and access variables from the outer scope even after that outer function has finished executing. This is a powerful concept used throughout JavaScript development.</p>
</details>

<details>
  <summary><strong>12. What does "functions as data" and "higher-order functions" mean in JavaScript?</strong></summary>

  <h4>Functions as Data:</h4>
  <p>In JavaScript, <strong>functions are first-class citizens</strong>. This means:</p>
  <ul>
    <li>You can assign a function to a variable</li>
    <li>You can pass a function as an argument to another function</li>
    <li>You can return a function from another function</li>
  </ul>

  ```js
  const greet = function(name) {
    return `Hello, ${name}`;
  };

  console.log(greet("Iryna")); // Hello, Iryna
  ```

  <h4>Higher-Order Functions:</h4>
  <p>A <strong>higher-order function</strong> is a function that either:</p>
  <ul>
    <li><strong>Takes one or more functions</strong> as arguments, or</li>
    <li><strong>Returns a function</strong> as its result</li>
  </ul>

  ```js
  // Takes a function as an argument
  function repeat(n, action) {
    for (let i = 0; i < n; i++) {
      action(i);
    }
  }

  repeat(3, console.log); // logs: 0, 1, 2

  // Returns a function
  function multiplier(factor) {
    return function (number) {
      return number * factor;
    };
  }

  const double = multiplier(2);
  console.log(double(5)); // 10
  ```

  <h4>Common Use Cases:</h4>
  <ul>
    <li>Array methods like <code>map</code>, <code>filter</code>, <code>reduce</code></li>
    <li>Event listeners and callbacks</li>
    <li>Creating reusable utilities (e.g., function factories)</li>
  </ul>

  <h4>Conclusion:</h4>
  <p>JavaScript treats functions as values, allowing them to be passed around and composed. This makes the language very flexible and enables patterns like functional programming and abstraction using higher-order functions.</p>
</details>

<details>
  <summary><strong>13. What is a guard expression (early return) in JavaScript?</strong></summary>

  <h4>Definition:</h4>
  <p>A <strong>guard expression</strong>, also called an <strong>early return</strong>, is a coding practice where you check for invalid or undesired conditions at the beginning of a function and return early if they're met. This helps to reduce nesting and makes the code easier to read and maintain.</p>

  <h4>Example:</h4>
  ```js
  function divide(a, b) {
    if (b === 0) {
      return 'Cannot divide by zero'; // guard clause
    }
    return a / b;
  }

  console.log(divide(10, 0)); // "Cannot divide by zero"
  console.log(divide(10, 2)); // 5
  ```

  <h4>Benefits:</h4>
  <ul>
    <li><strong>Improves readability</strong> by avoiding deep nesting</li>
    <li><strong>Highlights edge cases</strong> and handles them early</li>
    <li><strong>Makes logic flow clearer</strong> and less error-prone</li>
  </ul>

  <h4>Without Guard Expression (Nested):</h4>
  ```js
  function divide(a, b) {
    if (b !== 0) {
      return a / b;
    } else {
      return 'Cannot divide by zero';
    }
  }
  ```

  <h4>With Guard Expression (Preferred):</h4>
  ```js
  function divide(a, b) {
    if (b === 0) return 'Cannot divide by zero';
    return a / b;
  }
  ```

  <h4>Conclusion:</h4>
  <p>Guard expressions help simplify function logic and improve clarity by handling invalid cases early and exiting before continuing the rest of the logic.</p>
</details>

<details>
  <summary><strong>14. What is the difference between arrow functions and regular functions in JavaScript?</strong></summary>

  <h4>Definition:</h4>
  <p>Arrow functions (<code>() => {}</code>) were introduced in ES6 and provide a shorter syntax for writing functions. Regular functions use the <code>function</code> keyword. The main differences are in syntax, handling of <code>this</code>, <code>arguments</code>, and usage as constructors.</p>

  <h4>Syntax:</h4>
  ```js
  // Regular function
  function greet(name) {
    return `Hello, ${name}`;
  }

  // Arrow function
  const greet = (name) => `Hello, ${name}`;
  ```

  <h4>Key Differences:</h4>
  <ul>
    <li><strong>this binding:</strong> 
      <ul>
        <li>Arrow functions do <strong>not</strong> have their own <code>this</code>. They inherit <code>this</code> from the surrounding lexical scope.</li>
        <li>Regular functions have their <strong>own</strong> <code>this</code> depending on how they're called.</li>
      </ul>
    </li>
    <li><strong>arguments object:</strong>
      <ul>
        <li>Regular functions have an <code>arguments</code> object.</li>
        <li>Arrow functions do <strong>not</strong>. You must use rest parameters instead.</li>
      </ul>
    </li>
    <li><strong>Constructor usage:</strong>
      <ul>
        <li>Regular functions can be used with <code>new</code> to create instances.</li>
        <li>Arrow functions <strong>cannot</strong> be used as constructors.</li>
      </ul>
    </li>
    <li><strong>Implicit return:</strong>
      <ul>
        <li>Arrow functions can return expressions implicitly (without <code>return</code> keyword).</li>
        <li>Regular functions require an explicit <code>return</code>.</li>
      </ul>
    </li>
  </ul>

  <h4>Example of this binding difference:</h4>
  ```js
  const person = {
    name: 'Iryna',
    regular: function () {
      console.log('regular:', this.name); // 'Iryna'
    },
    arrow: () => {
      console.log('arrow:', this.name); // undefined
    }
  };

  person.regular(); // "regular: Iryna"
  person.arrow();   // "arrow: undefined"
  ```

  <h4>Conclusion:</h4>
  <p>Use arrow functions when you want lexical <code>this</code> and simpler syntax. Use regular functions when you need your own <code>this</code>, <code>arguments</code>, or plan to use the function as a constructor.</p>
</details>

<details>
  <summary><strong>15. How to check if an object has a property in JavaScript?</strong></summary>

  <h4>1. Using <code>in</code> operator:</h4>
  <p>Checks if the property exists in the object or its prototype chain.</p>
  ```js
  const user = { name: 'Iryna' };
  console.log('name' in user); // true
  console.log('toString' in user); // true (inherited from Object.prototype)
  ```

  <h4>2. Using <code>hasOwnProperty()</code>:</h4>
  <p>Checks if the property exists <strong>only directly on the object</strong>, not on its prototype chain.</p>
  ```js
  console.log(user.hasOwnProperty('name')); // true
  console.log(user.hasOwnProperty('toString')); // false
  ```

  <h4>3. Using <code>Object.hasOwn()</code> (ES2022+):</h4>
  <p>Modern alternative to <code>hasOwnProperty</code>, avoids issues if the method is shadowed.</p>
  ```js
  Object.hasOwn(user, 'name'); // true
  ```

  <h4>4. Optional chaining with strict equality:</h4>
  <p>This is helpful if you just want to know whether the property has a defined value:</p>
  ```js
  if (user?.name !== undefined) {
    console.log('Property exists and is defined');
  }
  ```

  <h4>Conclusion:</h4>
  <ul>
    <li>Use <code>in</code> if you care about inherited properties.</li>
    <li>Use <code>hasOwnProperty()</code> or <code>Object.hasOwn()</code> if you want only the object’s own properties.</li>
  </ul>
</details>

<details>
  <summary><strong>1. How to make a copy of an object in JavaScript?</strong></summary>

  <p>In JavaScript, copying an object can be done in two main ways: shallow copy and deep copy. Each has different use cases depending on whether you need to preserve nested object structures independently.</p>

  <h4>1. Shallow copy</h4>
  <p>A shallow copy copies only the top-level properties. If the original object contains nested objects, those nested objects are shared between the original and the copy.</p>

  <h5>Using the spread operator</h5>
  ```js
  const original = { name: "Iryna", skills: { js: true } };
  const copy = { ...original };

  copy.name = "Anna";
  copy.skills.js = false;

  console.log(original.name); // "Iryna"
  console.log(original.skills.js); // false — nested object was shared
  ```

  <h5>Using <code>Object.assign()</code></h5>
  ```js
  const original = { name: "Iryna", skills: { js: true } };
  const copy = Object.assign({}, original);
  ```

  <h4>2. Deep copy</h4>
  <p>A deep copy creates a completely independent copy, including all nested objects.</p>

  ### Using `JSON.parse(JSON.stringify())`
  <p>This method works well for simple objects (no functions, undefined, circular refs).</p>
  ```js
  const original = { name: "Iryna", skills: { js: true } };
  const deepCopy = JSON.parse(JSON.stringify(original));
  ```
  ---
  ### Using Lodash `cloneDeep`
  For complex objects (functions, circular structures, etc.), use Lodash.
  ```js
  import cloneDeep from "lodash/cloneDeep";

  const original = { name: "Iryna", skills: { js: true } };
  const deepCopy = cloneDeep(original);
  ```

  <p><strong>Conclusion:</strong> Use shallow copy for flat objects or when shared references are acceptable. Use deep copy when you need full independence between the original and the copy.</p>
</details>
