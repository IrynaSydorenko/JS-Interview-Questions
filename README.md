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

