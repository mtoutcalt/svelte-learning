<!-- Lesson 1: Svelte Basics - Components and Reactivity -->
<!-- To run: Create a new SvelteKit project with `npm create svelte@latest my-app` -->
<!-- Then replace src/routes/+page.svelte with this content -->
<!-- Run with: npm run dev -->

<script>
    // This is the script section - where your JavaScript/TypeScript goes
    console.log("Hello, Svelte world!");
    
    // REACTIVE VARIABLES
    // In Svelte, variables are automatically reactive
    let message = "Hello, Svelte!";
    let count = 0;
    
    // Variables can be of any type
    let name = "World";
    let isVisible = true;
    let temperature = 22;
    
    // REACTIVE DECLARATIONS
    // Use $: to create reactive statements that re-run when dependencies change
    $: greeting = `Hello, ${name}!`;
    $: doubleCount = count * 2;
    $: temperatureDescription = temperature > 25 ? "warm" : "cool";
    
    // Reactive statements can contain any code
    $: {
        console.log(`Count changed to: ${count}`);
        if (count > 10) {
            console.log("Count is getting high!");
        }
    }
    
    // FUNCTIONS / EVENT HANDLERS
    function increment() {
        count += 1; // This will trigger reactivity
    }
    
    function decrement() {
        count -= 1;
    }
    
    function reset() {
        count = 0;
        name = "World";
    }
    
    function toggleVisibility() {
        isVisible = !isVisible;
    }
    
    // Functions can take parameters
    function addToCount(amount) {
        count += amount;
    }
    
    // ARRAYS AND OBJECTS (Reactive)
    let fruits = ["apple", "banana", "orange"];
    let person = {
        firstName: "John",
        lastName: "Doe",
        age: 30
    };
    
    // Reactive declaration for computed values from arrays/objects
    $: fruitCount = fruits.length;
    $: fullName = `${person.firstName} ${person.lastName}`;
    
    // Svelte only triggers reactivity on ASSIGNMENTS (=), not on mutations.
//     Why This Design Choice?
// Performance - Svelte doesn't have to watch every method call on every object
// Predictability - You can easily see what triggers reactivity (look for =)
// Simplicity - The compiler just watches for assignments to variables


// The Mental Model:
// Think of variables as boxes:
// fruits.push() = putting more stuff in the same box (Svelte doesn't notice)
// fruits = [...] = getting a completely new box (Svelte notices!)

    function addFruit() {
        // To trigger reactivity with arrays, use assignment
        fruits = [...fruits, "grape"]; // This works
        // fruits.push("grape"); // This WON'T trigger reactivity
    }
    
    function updatePersonAge() {
        // For objects, this triggers reactivity
        person.age += 1;
        person = person; // Force reactivity (or use person = {...person, age: person.age + 1})
    }
    
    // CONSTANTS
    // Constants work the same as in JavaScript
    const APP_NAME = "My Svelte App";
    const MAX_COUNT = 100;
    
    // You can also have non-reactive variables (rarely used) - its because the html doesnt use it
    // Every let variable in Svelte is reactive. Whether you SEE the reactivity depends on whether you USE the variable in places that affect the UI.
    let staticValue = "This won't trigger updates if changed";
</script>

<!-- HTML TEMPLATE -->
<!-- This is where your HTML goes, with Svelte's special syntax -->

<main>
    <h1>Lesson 1: {APP_NAME}</h1>
    
    <!-- BASIC TEXT INTERPOLATION -->
    <!-- Use curly braces {} to insert JavaScript expressions -->
    <p>{message}</p>
    <p>Current count: {count}</p>
    <p>Double count: {doubleCount}</p>
    <p>Greeting: {greeting}</p>
    
    <!-- CONDITIONAL RENDERING -->
    <!-- Use {#if} blocks for conditional content -->
    {#if isVisible}
        <p>👀 This content is visible!</p>
        <p>Temperature is {temperature}°C - that's {temperatureDescription}!</p>
    {/if}
    
    {#if count > 5}
        <p>🎉 Count is greater than 5!</p>
    {:else if count < 0}
        <p>⚠️ Count is negative!</p>
    {:else}
        <p>📊 Count is between 0 and 5</p>
    {/if}
    
    <!-- EVENT HANDLING -->
    <!-- Use on:eventName to handle events -->
    <div class="button-group">
        <button on:click={increment}>+1</button>
        <button on:click={decrement}>-1</button>
        <button on:click={() => addToCount(5)}>+5</button>
        <button on:click={reset}>Reset</button>
    </div>
    
    <!-- INPUT BINDING -->
    <!-- Use bind:value for two-way data binding -->

    <!-- Common bind: attributes:
        bind:value - for text inputs, textareas, selects
        bind:checked - for checkboxes and radio buttons
        bind:group - for radio button groups
        bind:files - for file inputs
        bind:this - to get reference to the DOM element itself -->
    <div class="input-section">
        <label>
            Your name: 
            <input bind:value={name} placeholder="Enter your name" />
        </label>
        
        <label>
            Temperature: 
            <input type="number" bind:value={temperature} />
        </label>
        
        <label>
            <input type="checkbox" bind:checked={isVisible} />
            Show extra content
        </label>
    </div>
    
    <!-- LISTS AND ITERATION -->
    <!-- Use {#each} blocks to iterate over arrays -->
    <div class="fruits-section">
        <h3>Fruits ({fruitCount} total):</h3>
        <ul>
            {#each fruits as fruit, index}
                <li>{index + 1}. {fruit}</li>
            {/each}
        </ul>
        <button on:click={addFruit}>Add Grape</button>
    </div>
    
    <!-- OBJECT DATA -->
    <div class="person-section">
        <h3>Person Info:</h3>
        <p>Name: {fullName}</p>
        <p>Age: {person.age}</p>
        <button on:click={updatePersonAge}>Birthday! 🎂</button>
    </div>
    
    <!-- EXERCISES SECTION -->
    <div class="exercises">
        <h3>🎯 Exercises for You to Try:</h3>
        <ol>
            <li>Add a new reactive variable for your favorite color and display it</li>
            <li>Create a button that toggles between light/dark theme text</li>
            <li>Make a simple calculator with two number inputs and operation buttons</li>
            <li>Create a list of your hobbies with add/remove functionality</li>
            <li>Build a character counter for a text input</li>
        </ol>
    </div>
    
    <!-- DEBUG INFO -->
    <div class="debug-info">
        <h4>🐛 Debug Info:</h4>
        <p>All reactive variables update automatically!</p>
        <pre>{JSON.stringify({ count, name, temperature, isVisible }, null, 2)}</pre>
    </div>
</main>

<!-- STYLES -->
<!-- Styles are scoped to this component by default -->
 <!-- By default, Svelte adds unique identifiers to your CSS: -->
  <!-- Your component
<style>
  p { color: red; }
</style>

<p>Hello</p>
Svelte compiles this to:
cssp.svelte-xyz123 { color: red; } -->
<style>
    /* Component-scoped styles - only apply to this component */
    main {
        max-width: 800px;
        margin: 0 auto;
        padding: 2rem;
        font-family: Arial, sans-serif;
        line-height: 1.6;
    }
    
    h1 {
        color: #ff3e00; /* Svelte orange! */
        text-align: center;
        margin-bottom: 2rem;
    }
    
    .button-group {
        display: flex;
        gap: 0.5rem;
        margin: 1rem 0;
        flex-wrap: wrap;
    }
    
    button {
        background: #ff3e00;
        color: white;
        border: none;
        padding: 0.5rem 1rem;
        border-radius: 4px;
        cursor: pointer;
        font-size: 1rem;
    }
    
    button:hover {
        background: #cc3300;
    }
    
    .input-section {
        background: #f5f5f5;
        padding: 1rem;
        border-radius: 8px;
        margin: 1rem 0;
    }
    
    .input-section label {
        display: block;
        margin: 0.5rem 0;
    }
    
    input[type="text"], input[type="number"] {
        padding: 0.5rem;
        border: 1px solid #ccc;
        border-radius: 4px;
        margin-left: 0.5rem;
    }
    
    .fruits-section, .person-section {
        background: #e8f4f8;
        padding: 1rem;
        border-radius: 8px;
        margin: 1rem 0;
    }
    
    .exercises {
        background: #fff3cd;
        padding: 1rem;
        border-radius: 8px;
        border-left: 4px solid #ffc107;
        margin: 2rem 0;
    }
    
    .debug-info {
        background: #f8f9fa;
        padding: 1rem;
        border-radius: 8px;
        margin-top: 2rem;
        font-size: 0.9rem;
    }
    
    pre {
        background: #e9ecef;
        padding: 0.5rem;
        border-radius: 4px;
        overflow-x: auto;
    }
    
    ul {
        margin: 0.5rem 0;
    }
    
    /* Global styles would use :global() selector */
    /* :global(body) { margin: 0; } */
    /* Best Practices:
✅ Good uses of :global():

Styling <html>, <body>, or other root elements
CSS resets and utility classes
Styling third-party components you can't modify
App-wide typography/spacing rules

❌ Avoid :global() for:

Regular component styles (use scoped styles)
Styling your own components (pass props or use CSS custom properties instead)
Quick fixes (usually indicates architectural issues) */

</style>

<!--
LESSON 1 SUMMARY:
- Svelte components have three sections: <script>, HTML template, and <style>
- Variables are automatically reactive - changing them updates the UI
- Use $: for reactive declarations that compute values from other variables
- Event handlers: on:click, on:input, etc.
- Two-way binding: bind:value, bind:checked
- Conditional rendering: {#if} {/if}
- Lists: {#each} blocks for iteration
- Text interpolation: {expression} in HTML
- Styles are component-scoped by default
- Console.log() works for debugging reactive changes

KEY DIFFERENCES FROM VANILLA JS:
- No DOM manipulation needed - Svelte handles it
- Variables automatically trigger UI updates
- Built-in templating with {#if}, {#each}, etc.
- Scoped CSS by default
- Reactive declarations with $:
- Two-way data binding with bind:

NEXT LESSON PREVIEW:
- Component communication (props and events)
- Lifecycle functions (onMount, onDestroy)
- Stores for global state management
- More advanced reactivity patterns

TO GET STARTED:
1. Run: npm create svelte@latest my-svelte-lessons
2. Choose: Skeleton project, Yes to TypeScript (optional), Yes to ESLint/Prettier
3. Run: cd my-svelte-lessons && npm install
4. Replace src/routes/+page.svelte with this lesson content
5. Run: npm run dev
-->