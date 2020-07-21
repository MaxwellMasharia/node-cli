# Build Cli Apps With Commander.js And Inquire.js

- This is a tutorial on building cli apps using commander.js and inquire.js
- It includes all the notes and the code demos\

## **`Commander.js`**

## **Start**

- `require` program from `commander`

```js
const program = require("commander");
// Set the version
program.version("0.0.1");
// Set the description
program.description("Description");
```

## **Options**

- Options are defined using the `.option()` method,also serving as a documentation for the option
- Each option can have a short flag _single character_ and a long name separated by
  - comma
  - whitespace
  - vertical bar
- The options can be accessed from the `command` object, multiple-word options such as `--template-engine` are camel-cased becoming `program.templateEngine`
- Multiple short flags can be combined in a single argument containing the dash
- The last flag may take a value

```bash
    -a -b -p 80 # Example One
    -ab -p80    # Example Two
    -abp80      # Example Three
```

- You can use `--` to indicate the end of options eg

```bash
     # Use command `do` to exec git --version
     do -- git --version # The --version is not interpreted as an option
```

```js
/**
 * @example
 */
const { program } = require("commander");

// Set the version
program.version("0.0.1");
// Set the description
program.description("Description");

// Set the options
program
  .option("-d, --debug", "Debug")
  .option("-s, --status", "Gets the status of the app");

// Read the arguments passes
program.parse(process.argv);

// Check for the given arguments
if (program.debug) {
  // Do something
}
if (program.status) {
  // Do something
}
```

- `program.parse(arguments)` processes the arguments leaving the unconsumed arguments in the `program.args` array

## **Default Option Values**

- Set the default value of an argument

```js
program.option("-c ,--cheese", "Set the type of cheese", "blue"); // The last param "blue" is the default value
```

- You can specify an option which functions as a flag but may also take a value (declared using square brackets).

## **Custom Option Processing**

- You can set custom processing by creating a function to process the option and passing it to the `.option(,,,function)`

```js
    /**
     * @example
    */

   program.option("-c, --cheese","Set the type of cheese",(value,_)=>return value.toUpperCase())
```

## **Variadic option**

- You can make your options variadic by appending `...` to the value place holder 👇

```js
program.option("-n, --numbers <numbers...>", "Gets an array of numbers");
```

## **Commands**

- You can add commands like 👇

```js
program
  .command("add")
  .alias("a")
  .description("Adds numbers in an array")
  .option("-n, --numbers <numbers...>", "Converts the string to numbers")
  .on("--help", () => {
    console.log("\n" + "Example : \n" + "\tadd -numbers 20 40 \n");
  })
  .action((options) => {
    let sum = 0;
    options.numbers.forEach((n) => {
      sum += parseInt(n);
    });
    console.log(`Sum : ${sum}`);
  });
```

## **`inquire.js`**

**String Input Type**

```js
const question = {
  type: "input",
  name: "cake_type",
  message: "What type of cake do you want",
  default: "vanilla",
};
```

**List Input Type**

```js
const question = {
  name: "fav_car",
  message: "What is your favorite car",
  type: "list",
  choices: ["bugatti", "benz", "suzuki"],
};
```

**Raw List Type**

```js
const question = {
  name: "fav_number",
  message: "What is your favorite number",
  default: 1,
  choices: ["one", "two", "three", "four", "five"],
  type: "rawlist",
};
```

**Checkbox Type**

```js
const question = {
  name: "fav_number",
  message: "What is your favorite number",
  default: ["one", "three"],
  choices: [{ value: "one" }, { value: "two" }, { value: "three" }],
  type: "checkbox",
};
```

**Confirm**

```js
const question = {
  name: "loveMangoes",
  message: "Do you love mangoes ?",
  default: false,
  type: "confirm",
};
```

**Password Type**

```js
const question = {
  name: "password",
  message: "What is your password",
  type: "password",
  mask: true,
};
```
