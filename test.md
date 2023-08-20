
[![Read Prev](/assets/imgs/prev.png)](/Readme.md)

# 1 What the hell is a web server any way?

If you do not wish to read about the basics of web/http, you can safely jump to the [coding section](/chapters/ch02-your-first-nodejs-server.md).

Before diving straight into writing JavaScript code to create web servers, it's essential to grasp the fundamental concepts that are the basic building blocks of web server. Web servers are like the traffic controllers of the internet. They manage requests from users (like you!) and send back the right information. But what makes up a web server, and how does it even work? Let's break it down into simple terms.

# 1.1 Parts of a Web Server:

Web servers are like friendly translators that help computers understand each other. Imagine if you and a friend speak different languages. To have a conversation, you'd need a common language that both of you understand. In the same way, web servers and computers need a common set of rules to talk to each other effectively. These rules are called protocols, which are like languages specifically designed for computers. When you type a website address in your browser and hit "Enter," your computer sends a message to the web server. This message follows the web's language rules, known as the HTTP protocol (Hypertext Transfer Protocol).

HTTP is like a code that tells the web server what you want (e.g., a webpage or an image) and how to respond. The web server reads your message, understands it because it knows the HTTP protocol too, and then sends back the information you requested using the same rules. This information could be a webpage, a picture, or any other content.

Just like you need to speak the same language to have a successful conversation with someone, computers and web servers need to use the same protocol to communicate effectively. This way, they can understand each other's requests and provide the right responses, allowing you to enjoy the content you're looking for on the internet. HTTP establishes a standardised set of rules for how your computer's request (like asking for a webpage) should be structured and how the server's response (the webpage itself) should be formatted. This ensures seamless communication between different devices, regardless of their underlying technologies.

Think of HTTP as a detailed script for a play. It outlines every step, from introducing characters (your request) to their dialogues (data transmission) and the grand finale (the server's response). This structured script eliminates misunderstandings and ensures that both sides know what to expect at each stage of the conversation.

But the protocol game isn't limited to HTTP. The web's secure version, HTTPS (Hypertext Transfer Protocol Secure), adds an extra layer of protection through encryption. This way, even if someone tries to [eavesdrop](https://en.wikipedia.org/wiki/Eavesdropping) on your conversation, they'll only hear garbled nonsense.

Protocols extend beyond web browsing too. Email, file sharing, and even the way your phone connects to Wi-Fi rely on various protocols to ensure reliable and efficient communication. Each protocol serves a specific purpose, just like different languages for different scenarios in real life.

# 1.1.1 Navigating the World of Protocols: A Quick Overview

Like I explained above, to re-iterate - Protocols are like the rules that enable devices to communicate effectively on the internet. They define how data is formatted, transmitted, and understood by different systems. Just as people follow social etiquette during conversations, devices follow protocols to ensure smooth communication. Here's a glimpse into some major types of protocols:

- **TCP/IP (Transmission Control Protocol/Internet Protocol):** a set of rules for exchanging data over a network.
- **HTTP (Hypertext Transfer Protocol):** a protocol for transmitting data between a web server and a web client.
- **HTTPS (Hypertext Transfer Protocol Secure):** an extension of HTTP that encrypts data in transit.
- **UDP (User Datagram Protocol):** a protocol for transmitting data between networked devices without requiring a connection or reliability guarantees.
- **FTP (File Transfer Protocol):** a protocol for transferring files between computers on a network.
- **SMTP (Simple Mail Transfer Protocol):** a protocol for sending email messages between servers.
- **POP3 (Post Office Protocol 3) and IMAP (Internet Message Access Protocol):** protocols for retrieving email messages from a server.
- **DNS (Domain Name System):** a protocol for translating domain names into IP addresses.
- **DHCP (Dynamic Host Configuration Protocol):** a protocol for automatically assigning IP addresses to devices on a network.

In order to become a proficient backend engineer, it is important to have a solid understanding of different networking protocols. While HTTP(s) is the main focus of this guide, having knowledge of other protocols such as FTP, SMTP, and DNS can prove beneficial in the long run. FTP (File Transfer Protocol) is commonly used for transferring files between servers, while SMTP (Simple Mail Transfer Protocol) is used for sending emails. DNS (Domain Name System) is responsible for translating domain names into IP addresses.

> If you're programming game servers, it's important to have a solid understanding of UDP. UDP is faster but less reliable than TCP, making it ideal for applications that can tolerate occasional data loss, such as video streaming or online gaming. Unlike TCP, UDP is a "fire and forget" protocol, meaning data is sent without any error-checking or acknowledgment mechanisms.

# 1.1.1 The Relationship Between HTTP and TCP: Ensuring Reliable Web Communication

HTTP (Hypertext Transfer Protocol) and TCP (Transmission Control Protocol) form a strong partnership when it comes to web communication. The reason HTTP prefers TCP lies in the very nature of their roles and responsibilities within the world of networking.

# 1.1.1 1. Data Integrity and Order

HTTP is used to send web content, like web pages, images, and videos, from a server to a user's browser. Imagine if you requested a webpage and the images were missing or the text was scrambled. That wouldn't be a good experience, right? HTTP has to make sure that the data is delivered correctly and in order.

TCP helps with this. It was designed to make sure that data is delivered in the right order and without errors. TCP breaks up the data into small pieces called packets, sends them to the destination, and makes sure they arrive in the correct order. If any packet is lost during the process, TCP asks for it to be sent again. This is important for web pages because everything needs to be presented in a way that makes sense.

> A packet is a small unit of data that is sent over a network. In the context of web communication, TCP breaks up the data into small pieces called packets, sends them to the destination, and makes sure they arrive in the correct order. If any packet is lost during the process, TCP asks for it to be sent again.

# 1.1.1 2. Acknowledgment Mechanism

HTTP is a way to request a webpage, and the server sends back the content you asked for. To make sure the data is received correctly, an [acknowledgment mechanism](<https://en.wikipedia.org/wiki/Acknowledgement_(data_networks)>) is needed.

TCP provides this mechanism by waiting for your browser to confirm that it has received each packet of data sent from the server. If your browser does not confirm, TCP sends the packet again, so that both the server and browser can be sure that the data is being received properly.

# 1.1.1 3. Complex Interactions

HTTP transactions involve multiple steps, like requesting a webpage, receiving the HTML structure, fetching linked assets (images, stylesheets), and more. These interactions require precise data handling and sequencing.

TCP works well with HTTP for handling complex interactions. TCP's mechanisms guarantee that every piece of data reaches its intended destination and fits into the bigger interaction. For instance, when you visit a webpage, your browser makes several HTTP requests for different assets. TCP helps ensure that these requests and responses occur in an orderly and dependable manner.

# 1.1.1 4. Transmission Overhead

TCP adds some extra information to every message to make sure it gets to its destination without errors. This extra information includes acknowledgments, sequence numbers, and error-checking. Even though it adds a little more data to every message, it's still worth it because it helps make sure the data is accurate and in the right order. This is especially important when communicating over the web.

# 1.1 Asking and Getting: How Web Servers Respond to Your Requests

Imagine you're at home, sitting in front of your computer, and you decide to visit a website, let's say "example.com." This simple action initiates a series of events that highlight the "Asking and Getting" process in web communication.

# 1.1.1 The Request:

# 1.1.1.1 1. Your Request:

You type "example.com" into your browser's address bar and hit Enter. This is like you telling your computer, "Hey, I want to see what's on this website!"

# 1.1.1.1 2. Finding the Address:

Your computer knows the basics of websites, but it needs the exact address of "example.com" to connect to it. So, it reaches out to a special helper called a [DNS resolver](https://en.wikipedia.org/wiki/Domain_Name_System#Address_resolution_mechanism).

# 1.1.1.1 3. Resolving the Address:

The DNS resolver is like a digital address book. It takes "example.com" and looks up the actual IP address associated with it. This IP address is like the specific coordinates of the website's location on the internet.

> A website URL like https://google.com also be referred to as a **domain name**

# 1.1.1 The Response:

# 1.1.1.1 1. Return Address:

The DNS resolver finds the IP address linked to "example.com" and sends it back to your computer. It's like the DNS resolver telling your computer, "The website is located at this IP address."

# 1.1.1.1 2. Sending the Request:

Now that your computer knows the IP address, it can send a request to the web server that holds the website's content. This request includes the IP address and a message saying, "Hey, can you please give me the content of your website?"

# 1.1.1.1 3. Preparing the Content:

The web server receives your request and understands that you want to see the content of "example.com." It then gathers the necessary files – HTML, images, stylesheets, scripts – to create the webpage.

# 1.1.1.1 4. Sending the Response:

The web server packages the content into a response and sends it back to your computer through the internet. It's like the server sending a digital package to your doorstep.

# 1.1.1.1 5. Enjoying the Content:

Your computer receives the response from the web server. Your browser interprets the HTML, displays images, and applies styles, creating a complete webpage. This is what you see on your screen – the final result of your request.

> A quick disclaimer: our learning approach will prioritize clarity and thoroughness. I will introduce a topic, break it down, and if we come across any unfamiliar concepts, we will explore them until everything is fully understood.

[![Read Prev](/assets/imgs/next.png)](/chapters/ch02-your-first-nodejs-server.md)

![](https://uddrapi.com/api/img?page=ch01)
[![Read Prev](/assets/imgs/prev.png)](/chapters/ch01-what-is-a-web-server-anyway.md)

# 1 Your first web server with [node.js](https://nodejs.org)

The following section assumes that you have nodejs installed locally and are ready to follow along. You can check whether you have nodejs installed by running this command on your terminal -

```bash
node --version

# 1 Outputs
# 1 v18.17.0
```

If you see a `Command not found` error, that means you do not have nodejs installed. Follow the instructions [here to download and install it](https://nodejs.org/en/download).

# 1.1 What exactly is node or nodejs?

From the official website -

> Node.js® is an open-source, cross-platform JavaScript runtime environment.

What does a “runtime” mean?

Simply put, when you write code in a programming language like JavaScript, you need something to execute that code. For compiled languages like C++ or Rust, you use a compiler. The runtime environment takes care of executing the code, ensuring that it works well with the computer's hardware and other software components.

For Node.js, being a JavaScript runtime environment means it has everything needed to execute JavaScript code **_outside of a web browser_**. It includes the V8 JavaScript engine (which compiles and executes JavaScript code), libraries, APIs for file, network, and other system-related tasks, and an event loop for asynchronous, non-blocking operations.

> We’ll discuss what exactly an event loop means, and implement our own version of event loop to understand how it works, later on in the guide.

# 1.1 Your first node.js program

Let’s begin by writing some code. Let’s create a new folder, and name it whatever you wish. I’ve named it `intro-to-node`. Inside it, create a new file `index.js` and add the following content inside it.

```jsx
// Write the string `Let's learn Nodejs` to the standard output.
process.stdout.write("Let's learn Node.js");
```

To execute the code, open your terminal and `cd` into the folder containing the `index.js` file, and run `node index.js` or `node index`. You may alternatively run the command by specifying the relative or absolute path of the `index.js` file -

```bash
node ../Code/intro-to-node/index.js

# 1or

node /Users/ishtmeet/Code/intro-to-node/index.js
```

This should output

```
Let's learn Node.js
```

> You might also see a trailing `%` at the end due to the absence of a newline character (**`\n`**) at the end of the string you're writing to the standard output (stdout). You can modify the code as `process.stdout.write("Let's learn Node.js\n");` to get rid of that trailing modulo.

**What is the code above doing?**

There’s too much going on in the code above, and I simply chose it over `console.log()` to explain a huge difference between the Javascript API and the Nodejs API.

JavaScript and Node.js are closely related, but they serve different purposes and have different environments, which leads to some differences in their APIs (Application Programming Interfaces).

JavaScript was created to make web pages more interactive and dynamic. It was meant for creating user interfaces and responding to user actions on the client side, **inside the browser**. However, as web applications became more complex, relying only on client-side JavaScript was not enough. This led to the development of Node.js, which allows JavaScript to be executed on the server side. Node.js extends JavaScript's capabilities, introducing APIs for file system operations, network communication, creating web servers, and more. This means developers can use one programming language throughout the entire web application stack, making development simpler.

So let’s jump back to the code above, and understand why did I use `process.stdout.write` instead of `console.log`.

Simply put, `console.log` is a method that outputs a message to the web console or the browser console. However, Node.js does not run on the web, which means it does not recognize what a console is.

But if you change your code inside `index.js` to this

```jsx
console.log("Let's learn Node.js");

// Outputs -> Let's learn Node.js
```

It works. However, isn't it the case that I just mentioned Node.js being unfamiliar with the concept of a browser console? Indeed, that's correct. However, Node.js has made it easier for developers who are only used to working with JavaScript in a web context. It has included all the important features of browser-based JavaScript in its framework.

Expanding upon this topic, it's important to understand that Node.js, despite its roots in server-side development, strives to bridge the gap between traditional web development and server-side scripting. By incorporating features commonly associated with browser-based JavaScript, Node.js has made it more accessible for developers who are already well-versed in the language but might be new to server-side programming.

# 1.1 How does `console.log()` work in Node.js?

The **`node:console`** module offers a wrapper around the standard console functionalities that javascript provides. This wrapper aims to provide a consistent and familiar interface for logging and interacting with the Node.js environment, just as developers would in a web browser's developer console.

The module exports two specific components:

-   A `Console` class with methods like `console.log()`, `console.error()`, and `console.warn()`. These can be used to write to any Node.js **stream**.
-   A global `console` instance that is set up to write to `process.stdout` and `process.stderr`.

(Note that `Console` is not `console` (lowercase). `console` is a special instance of `Console`)

> You can use the global `console` without having to call `require('node:console')` or `require('console')`. This global availability is a feature provided by the Node.js runtime environment. When your Node.js application starts running, certain objects and modules are automatically available in the global scope without the need for explicit importing.
>
> Here are some of the examples of globally available objects/modules in Node.js - `console`, `setTimeout`, `setInterval`, `__dirname`, `__filename`, `process`, `module`, `Buffer`, `exports`, and the `global` object.

As I mentioned earlier, Node.js provides the global `console` instance to output text to `process.stdout` and `process.stderr`. So if you’re writing this

```jsx
console.log("Something");
```

the above code is just an abstraction of the code below.

```jsx
process.stdout.write("Something\n");
```

However, even after reading this, the code above may still be confusing. You may not yet be familiar with the `process` object, or with `stdout` and `stderr`.

# 1.1.1 The **`process` Object**:

The **`process`** object in Node.js tells you about the environment where the Node.js app is running. It has various properties, methods, and event listeners to help you work with the process and access info about the runtime environment.

These are some of the useful properties and functions that are provided by the `process` object. Copy paste the code below and paste it inside your `index.js` file. Try to execute it, using `node path/to/index/file`.

```jsx
console.log(process.version);
// v18.17.0

console.log(process.platform);
// darwin

console.log(process.uptime());
// 0.023285791

console.log(process.cpuUsage());
// { user: 31466, system: 6772 }

console.log(process.resourceUsage().systemCPUTime);
// 6865

console.log(process.memoryUsage());
// {
//  rss: 39239680,
//  heapTotal: 6406144,
//  heapUsed: 5388408,
//  external: 425804,
//  arrayBuffers: 17694
// }

console.log(process.cwd());
// /Users/ishtmeet/Code/intro-to-node

console.log(process.title);
// node

console.log(process.argv);
// [
//  '/usr/local/bin/node',
//  '/Users/ishtmeet/Code/intro-to-node/index.js'
// ]

console.log(process.pid);
// 39328
```

> We will discuss most of these properties/functions further down the line when we talk about implementing our own framework.

# 1.1.1 The `stdout` **property of the `process` object**:

In Node.js, the `stdout` property is a part of the `process` object. This property represents the standard output **_stream_**, which is used for writing data to the console or other output destinations. Anything written to the `stdout` stream is displayed in the console when you run your program.

Now you may ask, what is a `stream`?

**_Streams_** are used in programming to efficiently handle data flow, especially when working with large datasets or network communication. A stream is a sequence of data elements that is made available over time. Instead of loading all the data into memory, streams allow you to process and transmit data in smaller, more manageable pieces.

Streams can also be classified as input streams and output streams. Input streams are used to take in data from a source, while output streams are used to send data to a destination.

Streams have an important advantage of supporting parallelism. Instead of processing data one after the other, streams can process data in parallel and concurrently. This is helpful when working with large datasets because it speeds up processing time significantly.

Node.js provides a comprehensive implementation of streams, which can be categorized into several types:

1. **Readable Streams**: These streams represent a source of data from which you can read. Examples include reading files, reading data from an HTTP request, or even generating data programmatically.
2. **Writable Streams**: Writable streams are destinations where you can write data. Examples include writing data to files, standard output (`stdout`), standard error output (`stderr`) and many more.
3. **Duplex Streams**: Duplex streams represent both a readable and a writable side. This means you can both read from and write to these streams concurrently. An example of a Duplex stream is a TCP socket. It can both receive data from the client and send data back to the client concurrently.
4. **Transform Streams**: These are a specific type of duplex stream that allow you to modify or transform data as it's being read or written. They are often used for data manipulation tasks, like compression or encryption.

> Streams are incredibly versatile and efficient because they work with small chunks of data at a time, which is particularly useful when dealing with data that doesn't fit entirely into memory or when you want to process data in real-time. They also make it possible to start processing data before the entire dataset is available, reducing memory consumption and improving performance.

Now you know what streams are, and what is the standard output (`stdout`), we can simplify the code below.

```jsx
process.stdout.write("Hello from Node.js");
```

We're simply writing to **`stdout`** or the standard output stream which Node.js provides, which means that we're sending data or messages from our program to the console where you see the program's output. The data we write to **`stdout`** is displayed in the order it's written, giving us a way to communicate with developers or users and provide insights into the program's execution in real-time.

Working with **`process.stdout`** can be rather cumbersome, and in practice, you tend to use it sparingly. Instead, developers frequently opt for the more user-friendly **`console.log`** method. Instances of code employing **`process.stdout`** are typically encountered when there's a need for a greater level of control over output formatting or when integrating with more complex logging mechanisms.

> **_Warning_**: The ways of the global console object are not always synchronous like the browser APIs they resemble, nor are they always asynchronous like all other Node.js streams. For more information, please see the [note on process I/O](https://nodejs.org/api/process.html#a-note-on-process-io).

[![Read Next](/assets/imgs/next.png)](/chapters/ch03-working-with-files.md)

![](https://uddrapi.com/api/img?page=ch02)
[![Read Prev](/assets/imgs/prev.png)](/chapters/ch02-your-first-nodejs-server.md)
# 1 Working with files

Now that we've covered the basics of logging in Node.js, let's explore a real-world example. Let us understand the low level of files and how to interact with them. After that, we'll build a logging library [logtar](https://github.com/ishtms/logtar) that writes logs to a log file. It also has a support for tracing and rolling file creation. We’ll use this library as the central mechanism of logging for our web framework, that we build further into this guide.

# 1.1 What will the logging library do

- Log messages to a file
- Choose log location, or simply generate a new file
- Support for log levels (debug, info, warning, error, critical)
- Timestamps on log messages
- Customizable log message format
- Automatic log rotation based on file size or time interval
- Support for console output in addition to log files
- Simple and easy-to-use API for logging messages

# 1.1.1 How do you work with files anyway?

A file in Node.js is represented by a JavaScript object. This object has properties that describe the file, such as its name, size, and last modified date. The object also has methods that can be used to read, write, and delete the file.

In order to work with files and access file-related helper methods, you can import the `fs` **_module_** from the Node.js standard library.

**Wait, what exactly is a `module`?**

In Node.js, every JavaScript file is like a little package, called a module. Each module has its own space, and anything you write in a module can only be used in that module, unless you specifically share it with others.

When you make a **`.js`** file in Node.js, it can be a module right away. This means you can put your code in that file, and if you want to use that code in other parts of your application, you can share it using the **`module.exports`** object. On the other hand, you can take code from other modules and use it in your file using the **`require`** function.

This modular approach is important for keeping your code organized and separate, and making it easy to reuse parts of your code in different places. It also helps keep your code safe from errors that can happen when different parts of your code interact in unexpected ways.

Let’s see an example by creating a module called `calculate`

Create a file `calculator.js` and add the following contents inside it

```jsx
// calculator.js

function add(num_one, num_two) {
    return num_one + num_two;
}

function subtract(num_one, num_two) {
    return num_one - num_two;
}

function multiply(num_one, num_two) {
    return num_one * num_two;
}

function divide(num_one, num_two) {
    return num_one / num_two;
}

// Only export add and subtract
module.exports = {
    add,
    subtract,
};
```

By specifying the `exports` property on the global `module` object, we declare which specific methods or properties should be publicly exposed and made accessible from all other modules/files during runtime.

Note, we haven’t exported `multiply` and `divide` and we’ll see in a moment what happens when we try to access them and invoke/call those functions.

> Note: Provide the relative path to `calculator.js`. In my case, it is located in the same directory and at the same folder level.

In your `index.js` file, you can import the exported functions as shown below.

```jsx
const { add, divide, multiply, subtract } = require("./calculator");

// You may also write it this way, but it's preferred to omit the `.js` extension
const { add, divide, multiply, subtract } = require("./calculator.js");
```

Notice that we're importing the functions `multiply` and `divide` even though we're not exporting them from the `calculator` module. This won't cause any issues until we try to use them. If you run the code above with `node index`, it runs fine but produces no output. Let’s try to understand why it doesn't fail.

The `module.exports` is basically a javascript `Object`, and when you `require` it from another file, it tries to evaluate the fields with the names provided (destructuring in short).

So, you can think of it as something like this:

```jsx
const my_module = {
    fn_one: function fn_one() {...},
    fn_two: function fn_two() {...}
}

const { fn_one, fn_two, fn_three } = my_module;
fn_one;   // fn_one() {}
fn_two;   // fn_two() {}
fn_three; // undefined
```

This may clear up why we don't get an error if we try to include a function/property that is not being explicitly exported from a module. If that identifier isn't found, it's simply `undefined`.

So, the `multiply` and `divide` identifiers above are just `undefined`. However, if we try to add this line:

```jsx
// index.js

let num_two = multiply(1, 2);
```

the program crashes:

```jsx
/Users/ishtmeet/Code/intro-to-node/index.js:5
let num_two = multiply(1, 2);
              ^

TypeError: multiply is not a function
    at Object.<anonymous> (/Users/ishtmeet/Code/intro-to-node/index.js:5:15)
    at Module._compile (node:internal/modules/cjs/loader:1256:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1310:10)
    at Module.load (node:internal/modules/cjs/loader:1119:32)
    at Module._load (node:internal/modules/cjs/loader:960:12)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:23:47
```

We cannot invoke an `undefined` value as a function. `undefined()` doesn't make any sense.

Let’s export all the functions from the `calculator` module.

```jsx
// calculator.js

function add(num_one, num_two) {...}

function subtract(num_one, num_two) {...}

function multiply(num_one, num_two) {...}

function divide(num_one, num_two) {...}

// Only export add and subtract
module.exports = {
  add,
  subtract,
  multiply,
  divide,
};
```

In the `index.js` file, call all those functions to see if everything’s working as expected.

```jsx
// index.js

const { add, divide, multiply, subtract } = require("./calculator");

console.log(add(1, 2));
console.log(subtract(1, 2));
console.log(multiply(1, 2));
console.log(divide(1, 2));

// outputs
3 - 1;
2;
0.5;
```

Recall what was just stated above: `module.exports` is simply an object. We only add fields to that object that we wish to export.

So instead of doing `module.exports = { add, subtract, .. }`, you could also do this

```jsx
// calculator.js

module.exports.add = function add(num_one, num_two) {
    return num_one + num_two;
};

module.exports.subtract = function subtract(num_one, num_two) {
    return num_one - num_two;
};

module.exports.multiply = function multiply(num_one, num_two) {
    return num_one * num_two;
};

module.exports.divide = function divide(num_one, num_two) {
    return num_one / num_two;
};
```

It’s a matter of preference. But there’s a big downside and nuance to this approach. You cannot use these functions in the same file.

_We’ll use the term `file` and `module` interchangeably, even though they’re not actually the same in theory_

```jsx
// calculator.js
module.exports.add = function add(num_one, num_two) {..}
module.exports.subtract = function subtract(num_one, num_two) {..}
module.exports.multiply = function multiply(num_one, num_two) {..}
module.exports.divide = function divide(num_one, num_two) {..}

divide(1,2)

// Outputs
/Users/ishtmeet/Code/intro-to-node/calculator.js:16
divide(1, 2);
^

ReferenceError: divide is not defined
    at Object.<anonymous> (/Users/ishtmeet/Code/intro-to-node/calculator.js:16:1)
    at Module._compile (node:internal/modules/cjs/loader:1256:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1310:10)
    at Module.load (node:internal/modules/cjs/loader:1119:32)
    at Module._load (node:internal/modules/cjs/loader:960:12)
    at Module.require (node:internal/modules/cjs/loader:1143:19)
    at require (node:internal/modules/cjs/helpers:110:18)
    at Object.<anonymous> (/Users/ishtmeet/Code/intro-to-node/index.js:1:45)
    at Module._compile (node:internal/modules/cjs/loader:1256:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1310:10)
```

This is because `divide` and all the other functions declared in this module are a part of `module.exports` object, and they’re not available in the scope. Let’s break it down into an easy example

```jsx
let person = {};
person.get_age = funtion get_age() {...}

// `get_age` is not defined as it can only be accessed using
// `person.get_age()`
get_age();
```

I hope this makes it clear. Instead you could do something like this

```jsx
// calculator.js

...

// Can do this
module.exports.add = add;
module.exports.subtract = subtract;
module.exports.multiply = multiply;
module.exports.divide = divide;

// Or this
module.exports = {
  add,
  subtract,
  multiply,
  divide,
};
```

But this isn’t the best way to create your library’s API. The second option is more concise and easier to read. It clearly shows that you're exporting a group of functions as properties of an object. This can be particularly useful when you have many functions to export. Also, everything is nicely placed at a single place. You don’t have to keep searching for `module.exports.export_name` to find out what this module exports.

# 1.1.1 Let’s get back to `files`

In Node.js, a `file` is a way to interact with the data in a file. The **`fs`** module is used to handle file operations. It works by using unique identifiers assigned by the operating system to each file, called [file descriptors](https://en.wikipedia.org/wiki/File_descriptor).

With the **`fs`** module, you can perform several operations on files, such as reading, writing, updating, and deleting. Node.js provides both synchronous and asynchronous methods for these operations. The synchronous methods can slow down your application's responsiveness, while the asynchronous methods allow non-blocking execution.

Node.js interacts (indirectly, through ) with the operating system's I/O subsystem to manage file operations, making system calls such as **`open`**, **`read`**, **`write`**, and **`close`**. When you open a file, Node.js requests the operating system to allocate a file descriptor, which is used to read or write data from the file. Once the operation is complete, the file descriptor is released.

> A file descriptor is a way of representing an open file in a computer operating system. It's like a special number that identifies the file, and the operating system uses it to keep track of what's happening to the file. You can use the file descriptor to read, write, move around in the file, and close it.

# 1.1.1 A little more about file descriptors

When a file is opened by a process, the operating system assigns a unique file descriptor to that open file. This descriptor is essentially an integer value that serves as an identifier for the open file within the context of that process. File descriptors are used in various [system calls](https://en.wikipedia.org/wiki/System_call) and APIs to perform operations like reading, writing, seeking, and closing files.

In Unix-like systems, including Linux, file descriptors are often managed using a data structure called a [file table](https://man7.org/linux/man-pages/man5/table.5.html) or [file control block](https://en.wikipedia.org/wiki/File_Control_Block#:~:text=A%20File%20Control%20Block%20(FCB,not%20in%20operating%20system%20memory.). This table keeps track of the properties and status of each open file, such as the file's current position, permissions, and other relevant information. The file descriptor acts as an **_index_** or key into this table, allowing the operating system to quickly look up the details of the open file associated with a particular descriptor, which is more efficient, and more performant than to iterate over a vector/array of files and find a particular file.

When you interact with files or file descriptors, you're typically dealing with numeric values. For instance, in C, the **`open()`** system call returns a file descriptor, and other functions like **`read()`**, **`write()`**, and **`close()`** require this descriptor to operate on the corresponding file. In a runtime like Node.js, the **`fs`** module abstracts the direct use of file descriptors by providing a more user-friendly API, but it still relies on them behind the scenes to manage file operations.

> A file descriptor is a small, non-negative integer that serves as an index to an entry in the process's table of open file descriptors. This integer is used in subsequent system calls (such as read, write, lseek, fcntl, etc.) to refer to the open file. The successful call will **_return the lowest-numbered file_** descriptor that is not currently open for the process.

# 1.1.1 Creating our first file

The `node:fs` module lets you work with the file system using standard [POSIX](https://en.wikipedia.org/wiki/POSIX) functions. Node.js provides multiple ways to work with files. It exposes many flavours of its FileSystem API. A _promise-based asynchronous_ _API_, a _callback-based API_ and a _synchronous API._

Let’s create a new module, `files.js`, in the same folder where your `calculator` module and the `index.js` file lives. Let’s import the `fs` module to start working with files.

```jsx
// Promise based API
const fs = require("node:fs/promises");

// Sync/Callback based API
const fs = require("node:fs");
```

A general rule of thumb is - always prefer asynchronous API, unless you’re dealing with a situation that specifically demands synchronous behaviour.

Asynchronous APIs have two main benefits: they make your code more responsive and scalable. These APIs let your code keep running while it waits for slow tasks like I/O operations or network requests. By not blocking other operations, these APIs allow your application to handle many tasks at once, which improves its overall performance.

Asynchronous code is better for managing multiple tasks happening at the same time than traditional callback-based approaches. With callbacks, it can be hard to keep track of what's going on, leading to a **callback hell**. Using promises and async/await helps make the code easier to read and manage, making it less likely to have issues with complex nested callbacks.

> I will be using the promise-based API of Node.js. However, you may use other options to see what issues arise when your code becomes more complex.

Inside `files.js` add this snippet of code

```jsx
// files.js
const fs = require("node:fs/promises");

async function open_file() {
    const file_handle = await fs.open("calculator.js", "r", fs.constants.O_RDONLY);
    console.log(file_handle);
}

module.exports = open_file;
```

and in `index.js`

```jsx
// index.js
const open_file = require("./files");

open_file();

/*
FileHandle {
  _events: [Object: null prototype] {},
  _eventsCount: 0,
  _maxListeners: undefined,
  close: [Function: close],
  ..
}
 */
```

Let’s break this down.

```jsx
const fs = require("node:fs/promises");
```

This line brings in the **`fs`** module from Node.js. It specifically imports the **`fs/promises`** sub-module, which provides file system operations that can be executed asynchronously and are wrapped in Promises.

```jsx
fs.open("calculator.js", "r", fs.constants.O_RDONLY);
```

The **`fs.open`** function is used to open a file. It takes three arguments - file’s `path`, `flag`, and a `mode`.

The `path` takes an argument of type **`PathLike`** which is a type that represents a file path. It's a concept used in Node.js API to indicate that a value should be a string representing a valid file path. Let’s the see type definition of `PathLike`

```jsx
export type PathLike = string | Buffer | URL;
```

# 1.1.1 `path` argument

1. String **Paths:**
   The most common way to represent file paths is as strings. A string path can be either a relative or an absolute path. It's simply a sequence of characters that specifies the location of a file on the computer.
   - Example of relative string path: **`"./calculator.js"`**
   - Example of absolute string path: **`"/Users/ishtmeet/Code/intro-to-node/calculator.js"`**
2. **Buffer Paths:**
   While strings are the most common way to represent paths, Node.js also allows you to use **`Buffer`** objects to represent paths. A **`Buffer`** is a low-level data structure that can hold binary data. In reality, using **`Buffer`** objects for paths is less common. Read about [Buffers](#buffers) here
3. **URL Paths:**
   With the **`URL`** module in Node.js, you can also represent file paths using URLs. The URL must be of scheme file.
   Example URL path:

```jsx
const url_path = new URL("file:///home/user/projects/calculator.js");
```

# 1.1.1 `flag` argument

The `flag` argument indicates the mode (not to confused by `mode` argument) in which you wish to open the file. Here are the supported values as a `flag` -

- `'a'`: Open file for appending. The file is created if it does not exist.
- `'ax'`: Like `'a'` but fails if the path exists.
- `'a+'`: Open file for reading and appending. The file is created if it does not exist.
- `'ax+'`: Like `'a+'` but fails if the path exists.
- `'as'`: Open file for appending in synchronous mode. The file is created if it does not exist.
- `'as+'`: Open file for reading and appending in synchronous mode. The file is created if it does not exist.
- `'r'`: Open file for reading. An exception occurs if the file does not exist.
- `'rs'`: Open file for reading in synchronous mode. An exception occurs if the file does not exist.
- `'r+'`: Open file for reading and writing. An exception occurs if the file does not exist.
- `'rs+'`: Open file for reading and writing in synchronous mode. Instructs the operating system to bypass the local file system cache.
- `'w'`: Open file for writing. The file is created (if it does not exist) or truncated (if it exists).
- `'wx'`: Like `'w'` but fails if the path exists.
- `'w+'`: Open file for reading and writing. The file is created (if it does not exist) or truncated (if it exists).
- `'wx+'`: Like `'w+'` but fails if the path exists.

> You do not need to remember all of these, but it can be useful to write consistent APIs to ensure that no undefined behavior occurs.

Let’s use `wx+` to show a small example. `wx+` will open a file for read and write, but fail to open a file if it already exists. If teh file doesn’t exists it will create a file and work just fine.

```jsx
// calculator.js
const file_handle = await fs.open(
    "calculator.js",
    "wx+",
    fs.constants.O_RDONLY
  );

// Outputs
node:internal/process/promises:288
            triggerUncaughtException(err, true /* fromPromise */);
            ^

[Error: EEXIST: file already exists, open 'calculator.js'] {
  errno: -17,
  code: 'EEXIST',
  syscall: 'open',
  path: 'calculator.js'
}
```

It’s a good practice to specify the `flag` argument.

# 1.1.1 `mode` argument

The `mode` argument specifies the permissions to set for the file when its created. `mode`s are always interpreted **in octal.** For example,

- **`0o400`** (read-only for the owner)
- **`0o600`** (read and write for the owner)
- **`0o644`** (read for everyone, write only for the owner)

You don’t need to remember the octal representation. Simply use the `fs.constants.your_mode` to access it.

In our case, the permissions are specified as `fs.constants.O_RDONLY`. Here is a list of available `modes` that can be used. Notice the `O_` prefix, which is short for `Open`. This prefix tells us that it will only work when used with `fs.open()`.

**Modes to use with `fs.open()`**

```jsx
/** Flag indicating to open a file for read-only access. */
const O_RDONLY: number;

/** Flag indicating to open a file for write-only access. */
const O_WRONLY: number;

/** Flag indicating to open a file for read-write access. */
const O_RDWR: number;

/** Flag indicating to create the file if it does not already exist. */
const O_CREAT: number;

/** Flag indicating that opening a file should fail if the O_CREAT flag is set and the file already exists. */
const O_EXCL: number;

/** Flag indicating that if the file exists and is a regular file, and the file is opened successfully for write access, its length shall be truncated to zero. */
const O_TRUNC: number;

/** Flag indicating that data will be appended to the end of the file. */
const O_APPEND: number;

/** Flag indicating that the open should fail if the path is not a directory. */
const O_DIRECTORY: number;

/** Flag indicating that the open should fail if the path is a symbolic link. */
const O_NOFOLLOW: number;

/** Flag indicating that the file is opened for synchronous I/O. */
const O_SYNC: number;

/** Flag indicating that the file is opened for synchronous I/O with write operations waiting for data integrity. */
const O_DSYNC: number;

/** Flag indicating to open the symbolic link itself rather than the resource it is pointing to. */
const O_SYMLINK: number;

/** When set, an attempt will be made to minimize caching effects of file I/O. */
const O_DIRECT: number;

/** Flag indicating to open the file in nonblocking mode when possible. */
const O_NONBLOCK: number;
```

Going back to the code we wrote in the `files` module

```jsx
// files.js
const fs = require("node:fs/promises");

async function open_file() {
    const file_handle = await fs.open("calculator.js", "r", fs.constants.O_RDONLY);
    console.log(file_handle);
}

module.exports = open_file;
```

The return type of `fs.open()` is a `FileHandle`. A file handle is like a connection between the application and the file on the storage device. It lets the application work with files without worrying about the technical details of how files are stored on the device.

We previously discussed **file descriptors**. You can check which descriptor is assigned to an opened file.

```jsx
// files.js

..

async function open_file() {
    const file_handle = await fs.open("calculator.js", "r", fs.constants.O_RDONLY);
    console.log(file_handle.fd); // Print the value of the file descriptor `fd`
}

..

// Outputs -> 20
```

You may get the same integer value for the file descriptor if you try to run the program multiple times. But if you try to create another file handle, it should have a different file descriptor

```jsx
// files.js

..

async function open_file() {
    const file_handle     = await fs.open("calculator.js", "r", fs.constants.O_RDONLY);
    const file_handle_two = await fs.open("calculator.js", "r", fs.constants.O_RDONLY);
    console.log(file_handle.fd);
    console.log(file_handle_two.fd);
}

..

// Outputs ->
20
21
```

> Note that if a `FileHandle` is not closed using the `file_handle.close()` method, it will try to automatically close the file descriptor and emit a process warning, helping to prevent memory leaks. It’s always good practice to call `file_handle.close()` to explicitly close it. However, in our case, the program exits just after running the `open_file` function, so it doesn't matter in our case.

One import thing to note is, `open`ing a file can fail, and will throw an exception.

`fs.open()` can throw errors in various scenarios, including:

- `EACCES`: Access to the file is denied or permission is lacking, or the file doesn't exist and parent directory isn't writable.
- `EBADF`: The directory file descriptor is invalid.
- `EBUSY`: The file is a block device in use or mounted.
- `EDQUOT`: Disk quota for user is exceeded when creating a file.
- `EEXIST`: File already exists while trying to create it exclusively.
- `EFAULT`: Path is outside accessible memory.
- `EFBIG` / `EOVERFLOW`: File is too large to open.
- `EINTR`: Opening a slow device is interrupted by a signal.
- `EINVAL`: Invalid flags or unsupported operations.
- `EISDIR`: Attempting to write to a directory, or using `O_TMPFILE` on a version that doesn't support it.
- `ELOOP`: Too many symbolic links encountered.
- `EMFILE`: Process reached its limit of open file descriptors.
- `ENAMETOOLONG`: Pathname is too long.
- `ENFILE`: System-wide limit on open files is reached.
- `ENOENT`: File or component in path doesn't exist.
- `ENOMEM`: Insufficient memory for FIFO buffer or kernel memory.
- `ENOSPC`: No space left on device.
- `ENOTDIR`: Component in path is not a directory.
- `ENXIO`: File doesn't correspond to device, socket, or FIFO.
- `EOPNOTSUPP`: Filesystem doesn't support `O_TMPFILE`.
- `EROFS`: File is on read-only filesystem.
- `ETXTBSY`: File is being executed, used as swap, or read by kernel.
- `EPERM`: Operation prevented by file seal or mismatched privileges.
- `EWOULDBLOCK`: `O_NONBLOCK` specified, incompatible lease held on the file.

Make sure to handle errors gracefully. There may be cases where you don't need to handle the errors and want the program to fail, exit, or throw an error to the client. For example, if you're writing a CLI application that compresses an image using the `path/to/image` provided as an argument, you want it to fail to let the user know that there is an issue with the file/path provided.

To catch errors, wrap the code inside a `try/catch` block.

```jsx
// files.js

..

async function open_file() {
  try {
    const file_handle = await fs.open("config", "r", fs.constants.O_WRONLY);
        // do something with the `file_handle`
  } catch (err) {
    // Do somethign with the `err` object
  }
}

..
```

# 1.1 Reading from a file

Too much of the theory. We’ll work on a real example now. Let’s try to read from a file. We’ll create a `log_config.json` file, in the `config` folder. The directory structure will look something like this (get rid of the `calculator` module)

```
.
├── config
│   └── log_config.json
├── files.js
└── index.js
```

Add these content inside the `log_config.json` file

```jsx
// log_config.json

{
  "log_prefix": "[LOG]: "
}
```

Node.js provides many utility methods for reading from a specific file using the `file_handle`. There are different ways to handle interactions with the files from the `node:fs` and the `node:fs/promises` modules. But we’re specifically going to use a `file_handle` for now.

```jsx
// files.js

const fs = require("node:fs/promises");

// This function asynchronously opens a file, reads it line by line
// and logs each line on the console.
async function read_file() {
    try {
        // open the file in read-only mode.
        const file_handle = await fs.open("./index.js", "r", fs.constants.O_RDONLY);

        // create a stream to read the lines of the file.
        let stream = file_handle.readLines({
            // start reading from the beginning of the file.
            start: 0,

            // read till the end of the file.
            end: Infinity,

            // specify the encoding to be utf8, or else the stream
            // will emit buffer objects instead of strings.
            encoding: "utf8",

            /**
             * If autoClose is false, then the file descriptor won't be closed,
             * even if there's an error. It is the application's responsibility
             * to close it and make sure there's no file descriptor leak. If
             * autoClose is set to true (default behavior), on 'error' or 'end' the
             * file descriptor will be closed automatically.
             */
            autoClose: true,

            /**
             * If emitClose is true, then the `close` event will be emitted
             * after reading is finished. Default is `true`.
             */
            emitClose: true,
        });

        // The 'close' event is emitted when the file_handle has been closed
        // and can no longer be used.
        stream.on("close", () => {
            console.log("File handle %d closed", file_handle.fd);
        });

        // The 'line' event be fired whenver a line is read from the file.
        stream.on("line", (line) => {
            console.log("Getting line -> %s", line);
        });
    } catch (err) {
        console.error("Error occurred while reading file: %o", err);
    }
}

module.exports = read_file;
```

This outputs

```
Getting line -> const open_file = require("./files");
Getting line ->
Getting line -> open_file();
File handle 20 closed
```

The code above has a function called `open_file` that does three things: open a file, read each line, and show each line on the console.

This function uses the `fs` module. It opens a read-only file and creates a stream to read it. The function can read only some lines using the `start` and `end` options. The function also needs to know the file's characters using the `encoding` option.

This function also sets two options to handle the file descriptor automatically when the reading is finished. Finally, this function creates two listeners to handle two events: `close` and `line`. The `close` event tells the function that the file handle has been closed. The `line` event tells the function that it has read a line from the file.

If there's an error while reading the file, the function shows an error message on the console.

One thing to note is that we used string substitution `%s` instead of template literals. When passing a string to one of the methods of the `console` object that accepts a string, you may use these substitution strings:

- `%o` or `%O`: Outputs a JavaScript object. Clicking the object name opens more information about it in the inspector (browser).
- `%d`: Outputs an integer. Number formatting is supported. For example, `console.log("Foo %d", 1)` will output the number as an number (will retain floating point value).
- `%i`: Outputs an integer. Number formatting is supported. For example, `console.log("Foo %i", 1.1)` will output the number as an integer (will truncate the floating point value).
- `%s`: Outputs a string.
- `%f`: Outputs a floating-point value. Formatting is supported. For example, `console.log("Foo %f", 1.1)` will output "Foo 1.1".

> Using `%o` to show the output on terminal, just prints the whole object as a string, this is something that the string substitution has an advantage over template literals.

We can simplify the above code. I included all possible option keys previously just to show that they exist, and you could use them if you want to have more control over what you’re doing.

The simplified version looks like this

```jsx
// files.js

...

async function read_file() {
  try {
    const file_handle = await fs.open("./index.js");
    const stream = file_handle.readLines();

    for await (const line of stream) {
      console.log("Reading line of length %d -> %s", line.length, line);
    }
  } catch (err) {
    console.error("Error occurred while reading file: %o", err);
  }
}

...
```

This outputs

```
Reading line of length 59 -> const { read_entire_file, read_file } = require("./files");
Reading line of length 0 ->
Reading line of length 12 -> read_file();
```

Notice that we get rid of all those options since they are already set to default values for our convenience. Only specify them if you wish to choose values other than the defaults.

# 1.1 Reading the `json` file

However, reading a json file line by line isn’t the best way. The `readLine` is a very memory-efficient way to read files. It does not load all the contents of the file into memory, which is usually what we want. But if the file is small, and you know before hand, that the file is not really big, it’s usually quicker, and more performant to load the entire file at once into the memory.

> If you're dealing with large files, it's usually better to use a buffered version, i.e `createReadStream()` or `readLines()`

Let’s update the code

```jsx
...

async function read_file() {
  try {
    const file_handle = await fs.open("./config/log_config.json");
    const stream = await file_handle.readFile();

    console.log("[File contents] ->\n %s", stream);
  } catch (err) {
    console.error("Error occurred while reading file: %o", err);
  }
}

...
```

Outputs

```jsx
[File contents] ->
 {
  "log_prefix": "[LOG]: "
}
```

Nice. But what happens, if we do not use the string substitution with `%s`?

```jsx
console.log("[File contents] ->\n", stream);
```

Strangely, this outputs some weird looking stuff

```
[File contents] ->
 <Buffer 7b 0a 20 20 22 6c 6f 67 5f 70 72 65 66 69 78 22 3a 20 22 5b 4c 4f 47 5d 3a 20 22 0a 7d 0a>
```

Why is it so? And what is a `Buffer`? This is one of the most unvisited topic of programming. Let’s take a minute to understand it.

# 1 Buffers

`Buffer` objects are used to represent a fixed-length sequence of bytes, in memory. **`Buffer`** objects are more memory-efficient compared to JavaScript strings when dealing with data, especially very large datasets. This is because strings in JavaScript are UTF-16 encoded, which can lead to higher memory consumption for certain types of data.

Q: But why does the `readLines()` method returned strings if it’s not “efficient”?

Well turns out, they do indeed use buffers internally to efficiently read and process data from files or streams. `readLines()` is a special variant of `createReadStream()` which is designed to provide a convenient interface for working with lines of text content, making it easier for developers to interact with the data without needing to handle low-level buffer operations directly.

So, what you're looking at when you see the value of a buffer is just a raw representation of binary data in **_hexadecimal format_**. This raw data might not make much sense to us as humans because it's not in a readable format like text.

To print the json file to the console, we have 3 ways.

**First method**

```jsx
console.log("[File contents] ->\n", stream.toString("utf-8"));
```

**Second method**

String substitution to the rescue again

```jsx
console.log("[File contents] ->\n %s", stream);
```

The second method is much more user friendly. They automatically serialize the binary content into a string. But, to use and manipulate the string contents, we’ll have to fall back to the first method.

**Third method**

Set the `encoding` option to `utf-8`

```jsx
const stream = await file_handle.readFile({ encoding: "utf-8" });
console.log("[File contents] ->\n", stream);
```

# 1.1 Parsing the `json` file

To read the `log_prefix` property that we specified into the `config/log_config.json` file, let’s parse the contents of the file.

> Many people use the `require('file.json')` way, but there are several drawbacks to it. First, the entire file is loaded into memory when your program encounters the require statement. Second, if you update the json file during runtime, the program will still refer to the old data. It is recommended to use `require()` only when you expect the file not to change, and it is not excessively large; otherwise, it will always remain in memory.

```jsx
// files.js

...

const stream = await (await fs.open("./config/log_config.json")).readFile();
const config = JSON.parse(stream);

console.log('Log prefix is: "%s"', config.log_prefix);

...

// Outputs ->
// Log prefix is: "[LOG]: "
```

This looks fine, but it is not a very good practice to specify paths like this. Using **`"./config/log_config.json"`** assumes that the **`config`** directory is located in the same directory as the current working directory of the terminal. This might not always be the case, especially if your script is being run from a different working directory, eg. from the config folder. To test this behavior, `cd config` and run `node ../index.js`

```jsx
Error occurred while reading file: [Error: ENOENT: no such file or directory, open './config/log_config.json'] {
  [stack]: "Error: ENOENT: no such file or directory, open './config/log_config.json'",
  [message]: "ENOENT: no such file or directory, open './config/log_config.json'",
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: './config/log_config.json'
}
```

This expects the path is relative to the current working directory, hence not what we expect. We should be able to run the script from anywhere, no matter what folder we are in. This is very useful for large projects having folders multiple levels deep.

Update the code to include the `path` module in scope

```jsx
// files.js

const path = require('path');

...

const log_path = path.join(__dirname , 'config' , 'log_config.json');
const stream = await (await fs.open(log_path)).readFile();

...
```

Using **`__dirname`** and the **`path`** module ensures that you are referencing the correct path regardless of the current working directory you’re in.

`__dirname` is a special (module-level) variable that represents the absolute path of the directory containing the current JavaScript file. Isn’t it magic?

`path.join()` method joins all given `path` segments together using the **platform-specific separator** as a delimiter, then normalizes the resulting path. Zero-length `path` segments are ignored. If the joined path string is a zero-length string then `'.'` will be returned, representing the current working directory.

The full code of `files.js` looks like this now.

```jsx
const fs = require("node:fs/promises");
const path = require("path");
async function read_file() {
    try {
        const log_path = path.join(__dirname, "config", "log_config.json");
        const stream = await (await fs.open(log_path)).readFile();
        const config = JSON.parse(stream);

        console.log('Log prefix is: "%s"', config.log_prefix);
    } catch (err) {
        console.error("Error occurred while reading file: %o", err);
    }
}
```

Now you can run the code from whatever directory, no matter how much deeply nested it is, it is going to work fine unless you move the `files.js` file to a different location.

[![Read Next](/assets/imgs/next.png)](/chapters/ch04-logtar-our-logging-library.md)

![](https://uddrapi.com/api/img?page=ch03)
[![Read Prev](/assets/imgs/prev.png)](/chapters/ch03-working-with-files.md)

# 1 `logtar` our own logging library

> Note: The entire code we write here can be found [here](/code/chapter_04). This will be a single file, and we'll refactor in subsequent chapters.

Logging is an important part of creating robust and scaleable application. It helps developers find and fix problems, keep an eye on how the application is working, and see what users are doing.

# 1.1 Initializing a new project

Let’s create a new project. Close your current working directory, or scrap it.

```bash
# 1 Create a new directory, and cd into it
mkdir logtar && cd logtar

# 1 Initializes a new package
npm init -y
```

This creates a new npm package/project, creates a `package.json` file and sets up some basic config inside it. This should be the contents of your `package.json`

```jsx
{
  "name": "logtar",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Let's change the version from `1.0.0` to `0.0.1`.

# 1.1 A little about `SemVer`

Starting versioning from **`0.0.1`** is a good practice in software development because version numbers have semantic meaning. Using **`0.0.1`** as the initial version indicates that the software is in its initial development stages or that it's undergoing rapid changes and improvements. This convention aligns with Semantic Versioning ([SemVer](https://semver.org/)), which is a widely adopted versioning scheme that helps developers understand the compatibility and significance of changes in software releases.

Starting with **`0.0.1`** is particularly beneficial for a few reasons:

1. **Clarity of Development Stage**: Starting with **`0.0.1`** clearly communicates that the software is in its early stages of development. This helps other developers and users understand that the API and features might change more rapidly and might not yet be stable.
2. **Semantic Versioning**: Semantic Versioning consists of three numbers separated by dots: **`MAJOR.MINOR.PATCH`**. Starting from **`0.0.1`** indicates that you're in the process of making minor patches and potentially significant changes as you develop the software.
3. **Incremental Progress**: Starting with **`0.0.1`** allows for a clear sequence of version increments as development progresses. Each release can follow the rules of [SemVer](https://semver.org/): incrementing the **`MAJOR`** version for backward-incompatible changes, the **`MINOR`** version for backward-compatible additions, and the **`PATCH`** version for backward-compatible bug fixes.
4. **User Expectations**: When users or other developers encounter a software version that starts with **`0.0.1`**, they'll understand that the software might not be feature-complete or entirely stable. This helps manage expectations and reduces confusion.
5. **Preventing Confusion**: If you started with version **`1.0.0`**, there might be an expectation of stability and feature completeness that could lead to confusion if the software is actually in an early stage of development.

# 1.1 Creating a `LogLevel` class

Log level is a basic concept in logging libraries. It helps control how much detail the application's log messages show. Developers use log levels to filter and manage the output. This is especially useful when debugging issues or dealing with complex systems.

Usually logging libraries have these 5 log levels (from least to most severe):

1. **Debug**: Detailed debugging information. Usually not used in production environments because it generates too much data.
2. **Info**: Informative messages about the regular flow of the application. Shows what the application is doing.
3. **Warning**: Indicates potential issues that might require attention. Warnings suggest that something might be going wrong.
4. **Error**: Reports errors or exceptional conditions that need to be addressed. These messages indicate that something has gone wrong and might affect the application's functionality.
5. **Critical/Fatal**: For severe errors that might crash the application or cause major malfunctions. These messages require immediate attention as they indicate critical failures.

> I prefer using `Class` over functions or objects to provide a better API. It's a powerful system in JavaScript, and I find it superior to factory functions. `Class` do have some draw backs but for our use case, they’re the best possible solution. If you’re un-aware of how classes work in javascript, just [go through this page quickly.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_classes) This should be enough to follow along.

When building a complex system or anticipating scalability, it's best to start simply and refactor as necessary. You don't need to use best practices from day one.

The process should be: write code, make it work, test it, and then refactor it.

Let’s create a new file `index.js` inside the `logtar` directory, and add a new class `LogLevel`

```jsx
// index.js

class LogLevel {
    static Debug = 0;
    static Info = 1;
    static Warn = 2;
    static Error = 3;
    static Critical = 5;
}

module.exports = {
    LogLevel,
};
```

You might be wondering about the use of a class `LogLevel` instead of an object, or maybe some constants that can be easily exported, like this -

```jsx
module.exports.LogLevel = {
    Debug: 0
    ...
}

// or

module.exports.Debug = 0
...
```

You could do something like this too, and that’s totally fine. But instead, I chose a different method i.e using a utility class `LogLevel` which encapsulates all log levels within a class, you create a clear namespace for these constants. This helps avoid potential naming conflicts with other variables or constants in your application. There’s more to this!

You will see another powerful feature by using this method, in a bit.

Let’s add a helper method to our `LogLevel` class which checks and verifies whether the `LogLevel` provided by our client (user of our library) is correct and supported.

```jsx
// index.js

class LogLevel {
    ...

    static assert(log_level) {
       if (
            log_level !== LogLevel.Debug ||
            log_level !== LogLevel.Info ||
            log_level !== LogLevel.Warn ||
            log_level !== LogLevel.Error ||
            log_level !== LogLevel.Critical
        ) {
            throw new Error(
            `log_level must be an instance of LogLevel. Unsupported param ${JSON.stringify(log_level)}`);
        }
     }
}
```

What is this `assert` method going to do? `assert` will be a method used inside our library, which verifies that the value of type `LogLevel` provided as an argument is valid and supported.

However, we can refactor the code above to look more readable, and not repeat too much.

```jsx
// index.js

static assert(log_level) {
    if (![LogLevel.Debug, LogLevel.Info, LogLevel.Warn, LogLevel.Error, LogLevel.Critical].includes(log_level)) {
        throw new Error(`log_level must be an instance of LogLevel. Unsupported param ${JSON.stringify(log_level)}`);
    }
 }
```

This way, if we wish to add more log levels, we can simply add them to the array. This is another powerful use-case of classes over normal object values. Everything is namespace’d. Even without typescript, we can tell the client (someone who uses our library) what arguments are we expecting. If they fail to provide an invalid argument, our `assert` method will throw an error.

Also, even if the user is unaware of the values we’re using for each log level, it does not matter as long as they’re using the `LogLevel.any_level` syntax. If we in future change the internals of the library, as long as the public API is consistent, everyone will be good. This is a key to build reliable APIs.

> I am going to use the terms `client` and `user` interchangeably. A `client` is someone who uses our library’s API.

The `LogLevel` looks fine for now. Let’s introduce a new class `Logger` which will be the backbone of our logging library.

# 1.1 The `Logger` class

```jsx
// index.js

class LogLevel {...}

class Logger {
    /* Set the default value of `log_level` to be `LogLevel.Debug` for every
    * new instance of the `Logger` class
    */
    log_level = LogLevel.Debug;

    // Sets the log level to whatever user passed as an argument to `new Logger()`
    constructor(log_level) {
      this.level = log_level;
    }
}
```

There’s an issue. The user may construct the `Logger` with whatever value they want. This can be some useless value like `100000` or `“Hello”` and that’s not what we would expect.

For example

```jsx
// makes no sense
const my_logger = new Logger("test");
```

Let’s make use of the `LogLevel.assert()` static method that we just defined.

```jsx
// index.js

class LogLevel {...}

class Logger {
    log_level = LogLevel.Debug;

    constructor(log_level) {
        // Throw an error if the `log_level` is an unsupported value.
        LogLevel.assert(log_level);
        this.level = log_level;
    }
}
```

Now if we try to pass an invalid argument to the construction of `Logger` we get an error. Exactly what we want

```jsx
const logger = new Logger(6);

// outputs
Error: log_level must be an instance of LogLevel. Unsupported param "6"
    at LogLevel.assert (/Users/ishtmeet/Code/logtar/index.js:10:13)
    at new __Logger (/Users/ishtmeet/Code/logtar/index.js:86:14)
    at Object.<anonymous> (/Users/ishtmeet/Code/logtar/index.js:91:16)
    at Module._compile (node:internal/modules/cjs/loader:1256:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1310:10)
    at Module.load (node:internal/modules/cjs/loader:1119:32)
    at Module._load (node:internal/modules/cjs/loader:960:12)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:23:47
```

Everything looks good! No, not yet. What if we try to do

```jsx
const logger = new Logger(LogLevel.Error);
logger.log_level = 1000;
```

Again, this breaks our whole library’s functionality. How do we prevent this? Seems like Javascript has us covered.

# 1.1.1 Encapsulation with `private` fields

Class fields are public by default, which means anyone can change it from anywhere in the code. Even the clients of our library can change it too. However, you can create private class features by adding a hash `#` prefix. JavaScript enforces the privacy encapsulation of these class features.

Before this syntax existed, private members were not native to the language. In prototypical inheritance, their behavior can be emulated with `WeakMap` objects or closures. But using `#` syntax is more ergonomic than these methods.

> `members` of a class mean any thing that is defined inside the `class` block. That includes variables, static variables, or even methods. Note that if a function is defined inside a class, it's referred to as a `method` and not a `function`

Let's update the code above by incorporating `encapsulation`.

```javascript
// index.js

class LogLevel {...}

class Logger {
    // introduce a new `private` variable `#level`
    #level;

    constructor(log_level) {
        // You refer to private variables using the `#` prefix.
        this.#log_level = log_level;
    }
}

const logger = new Logger(LogLevel.Debug);
console.log(logger.#level); // Error: Private field '#level' must be declared in an enclosing class
logger.#level = LogLevel.Info; // Error: Private field '#level' must be declared in an enclosing class
```

This is looking good. We can refer to `#level` member variable only inside the class. No one can change it. But we do need to provide a way to know the current log level of our logger. Let's add a `getter` method.

```js
// index.js

class LogLevel {...}

class Logger {
    ...

    get level() {
        return this.#level;
    }
}

const logger = new Logger(LogLevel.Debug);
console.log(logger.#level); // Error: Private field '#level' must be declared in an enclosing class
console.log(logger.level); // Good. Calls the `get level()` method
```

> Note: If you create a getter using `get()`, you do not need to specify the parenthesis after `level`. Javascript knows that we're referring to the `get level()` getter.

Now, add the `LogLevel.assert` method inside the constructor, to make sure the clients pass in a correct value for `log_level`

```js
// index.js
class Logger {
    ..
    constructor(log_level) {
        LogLevel.assert(log_level);
        this.#level = log_level;
    }
    ..
}

const logger = new Logger(100); // Throws an error
const logger  = new Logger(3); // Good
const logger = new Logger(LogLevel.Debug); // Best practice
```

It's always a good practice to allow clients create an object without specifying a value in the constructor, in that case we should use some set defaults. The full code of the `index.js` should look like this

```js
// index.js

class LogLevel {
    static Debug = 0;
    static Info = 1;
    static Warn = 2;
    static Error = 3;
    static Critical = 4;

    static assert(log_level) {
        if (![LogLevel.Debug, LogLevel.Info, LogLevel.Warn, LogLevel.Error, LogLevel.Critical].includes(log_level)) {
            throw new Error(
                `log_level must be an instance of LogLevel. Unsupported param ${JSON.stringify(log_level)}`
            );
        }
    }
}

class Logger {
    // set a default value for the log level
    #level = LogLevel.Info;

    constructor(log_level) {
        // only set/check the log level if the client has provided it
        // otherwise use the default value, i.e `LogLevel.Info`
        if (arguments.length > 0) {
            LogLevel.assert(log_level);
            this.#level = log_level;
        }
    }

    get level() {
        return this.#level;
    }
}

module.exports = {
    Logger,
    LogLevel,
};
```

Let's try to test this.

```js
new Logger("OK"); // throws error
new Logger(LogLevel.Debug); // works fine
new Logger(); // works fine

let logger = new Logger(LogLevel.Warning);
logger.level; // returns the `log_level` because of the getter `level()`
logger.#level; // throws error
logger.#level = LogLevel.Warning; // throws error
logger.level = 10; // throws error
```

Perfect! This all looks really good. We are confident that neither clients nor our own library's code will affect the internals of the library. Please note that only the `#level` member variable can be changed from within the class Logger's scope, which is exactly what we want.

# 1.1 The `LogConfig` class

We have a bare bones `Logger` setup, which isn't helpful at all. Let's make it a bit more useful.

You see, setting the log level inside the logger is fine until we start adding a lot of config settings. For example, we may add a `file_prefix` member variable, as well as `max_file_size` too. The `Logger` class will get cluttered too much, and that's not what we want.
Let's start refactoring now. Create a new class `LogConfig` that will contain all the utility helpers to deal with log config. Everything that take cares about the configuration will live inside it.

```js
// index.js

class LogConfig {
    /** Define necessary member variables, and make them private. */

    // log level will live here, instead of the `Logger` class.
    #level = LogLevel.Info;

    // We do not initialise it here, we'll do it inside the constructor.
    #rolling_config;

    // the prefix to be added to new files.
    #file_prefix = "Logtar_";

    /**
     * We're going to follow the convention of creating a static assert
     * method wherver we deal with objects. This is one way to write
     * safe code in vanilla javascript.
     */
    static assert(log_config) {
        // if there's an argument, check whether the `log_config` is an instance
        // of the `LogConfig` class? If there's no argument, no checks required
        // as we'll be using defaults.
        if (arguments.length > 0 && !(log_config instanceof LogConfig)) {
            throw new Error(
                `log_config must be an instance of LogConfig. Unsupported param ${JSON.stringify(log_config)}`
            );
        }
    }

    get level() {
        return this.#level;
    }

    get rolling_config() {
        return this.#rolling_config;
    }

    get file_prefix() {
        return this.#file_prefix;
    }

    get max_file_size() {
        return this.#max_file_size;
    }
}
```

All looks okay. We have a `LogConfig` class setup. Now, instead of using `#level` for storing the log level inside the `Logger` class, let's replace it with `#config`

```js
// before

class Logger {
    ...

    #level = LogLevel.Info;

    constructor(log_level) {
        // only set/check the log level if the client has provided it
        // otherwise use the default value, i.e `LogLevel.Info`
        if (arguments.length > 0) {
            LogLevel.assert(log_level);
            this.#level = log_level;
        }
    }

    ...
}

// now

class Logger {
    #config;
    ...

    constructor(log_config) {
        // we'll create the `with_defaults()` static method in just a bit.
        log_config = log_config || LogConfig.with_defaults();
        LogConfig.assert(log_config);
        this.#config = log_config;
    }
}
```

Awesome. Let's pause for a moment before adding further functionality inside the `LogConfig` class. Let me quickly introduce you to a very important topic in software engineering.

# 1.1 Design patterns

Software design patterns are a solution to a common problem that software engineers face while writing code. It's like a blueprint that shows how to solve a couple problem that can be used in many different situations. Those problems are - maintainability and organizing code.

This is a vast topic, and people have dedicated books for explaining the use of these patterns. However, we aren't going to explain each one of those. We'll use the one that suits best for our project. Always find the right tool for the right job. For us, the most reasonable design pattern to build our web framework, as well as our logging library will be the [`Builder pattern`](https://en.wikipedia.org/wiki/Builder_pattern)

# 1.1.1 The `Builder` pattern

Think of the Builder Pattern as a way to create complex objects step by step. Imagine you're building a house. Instead of gathering all the materials and putting them together at once, you start by laying the foundation, then building the walls, adding the roof, and so on. The Builder Pattern lets you do something similar. It helps you construct objects by adding parts or attributes one by one, ultimately creating a complete and well-structured object.

Just for a minute think that you're creating a web application where users can create personal profiles. Each profile has a `name`, an `age`, and a `description`. The Builder Pattern would be a great fit here because users might not provide all the information at once. Here's how it could work

```js
const user = new ProfileBuilder().with_name("Alice").with_age(25).with_description("Loves hiking and painting").build();
```

Doesn't this look so natural? Having to specify steps, without any specific order, and you get what you desired. Compare this to a traditional way of building using an object

```js
const user = create_profile({
    name: "Alice",
    age: 25,
    description: "Loves hiking and painting",
});
```

The object solution looks fine, and even has less characters. Then why the builder pattern? Imagine your library in a future update changes the `name` property to `first_name` and include a secondary `last_name` property. The code with object will fail to work properly. But in our builder pattern, it's obvious that the `name` means full name. That might not sound convincing. Let's see at a different example.

In a language like javascript (typescript solves this) you need to make sure that the params you pass as an argument are valid.

Here's a common way you'll write the function `create_profile`

```js
function create_profile({ name, age, description }) {
    let profile = {
        name: Defaults.name,
        age: Defaults.age
        description: Defaults.description
    }
    if (typeof name === 'string') { profile.name = name }
    if (typeof age === 'number' && age > 0) { profile.age = age }
    if (typeof description === 'string') { profile.description = description }
}
```

Notice how cluttered this code becomes if there are 10 fields? The function `create_profile` should not be responsible for testing. Its role is to create a profile. We could group other functions, such as `validate_name` and `validate_email`, and call them inside the `create_profile` function. However, this code would not be reusable. I have made this mistake in the past and ended up with code that was difficult to refactor.

Instead, let's use the builder pattern to validate whether the fields are valid:

```js
class ProfileBuilder {
    name = Defaults.name;
    age = Defaults.age;
    description = Defaults.description;

    with_name(name) {
        validate_name(name);
        this.name = name;
        return this;
    }

     with_age(age) {
        validate_age(age);
        this.age = age;
        return this;
    }

    with_description(description) {...}
}
```

Do you notice the difference? All of the related validations and logic for each field are separated and placed in their respective locations. This approach is much easier to maintain over time, and reason about.

# 1.1 Using `builder` pattern with the `LogConfig` class

Here's what I'd like the API of `LogConfig` to look like

```js
const config = LogConfig.with_defaults()
    .with_file_prefix("LogTar_")
    .with_log_level(LogLevel.Critical);
```

Our current `LogConfig` class looks like this

```js
// index.js

class LogConfig {
    #level = LogLevel.Info;

    // We'll talk about rolling config in just a bit.
    #rolling_config = RollingConfig.Hourly;
    #file_prefix = "Logtar_";

    static assert(log_config) {
        if (arguments.length > 0 && !(log_config instanceof LogConfig)) {
            throw new Error(
                `log_config must be an instance of LogConfig. Unsupported param ${JSON.stringify(log_config)}`
            );
        }
    }
}
```

Add the required methods

```js
// index.js

class LogConfig {
    ...
    // This can be called without a `LogConfig` object
    // eg. `LogConfig.with_defaults()`
    static with_defaults() {
        return new LogConfig();
    }

    // Validate the `log_level` argument, set it to the private `#level` variable
    // and return this instance of the class back. So that other methods can mutate
    // the same object, instead of creating a new one.
    with_log_level(log_level) {
        LogLevel.assert(log_level);
        this.#level = log_level;
        return this;
    }

    // We'll talk about rolling config in just a bit, bare with me for now.
    with_rolling_config(rolling_config) {
       this.#rolling_config = RollingConfig.from_json(config);
       return this;
    }

    with_file_prefix(file_prefix) {
        if (typeof file_prefix !== "string") {
            throw new Error(`file_prefix must be a string. Unsupported param ${JSON.stringify(file_prefix)}`);
        }

        this.#file_prefix = file_prefix;
        return this;
    }
    ...
}
```

> Static methods in JavaScript are methods that belong to a class instead of an instance of the class. They can be used without creating an instance of the class and are often used for utility functions or operations that don't need any state. To create a static method in a class, use the `static` keyword before the method definition. For example:
> ```jsx
>  class MyClass {
>    static my_static_method() {
>      console.log('This is a static method.');
>    }
>  }
>  
>  MyClass.my_static_method();
> ```
> One thing to keep in mind is that you cannot access the `this` keyword inside a static method. This is because `this` refers to the instance of the class, and static methods are not called on an instance.

Subclasses can also inherit static methods, but they cannot be used on instances of the subclass. They are useful for organizing code and providing a namespace for related utility functions.

You may notice a difference now. Every method that we added is only responsible to validate a single input/argument. It does not care about any other options, whether they are correct or not.

# 1.1 jsdoc comments

If you're writing vanilla javascript, you may have trouble with the auto-completion or intellisense feature that most IDE provide, when working with multiple files. This is because javascript has no types (except primitives). Everything is an object. But don't we deserve those quality of life features if we're writing vanilla JS? Of course, we do. That's where `jsdoc` saves us.

We will not cover the entire feature set of `jsdoc`, but only focus on what we need for this particular purpose. We are concerned with two things: the parameter and the return type. This is because if a function returns a type, our auto-completion feature will not work across multiple files and will not display other associated methods of that return type in the dropdown.

Let's fix it.

```js
   /**
     * @param {string} file_prefix The file prefix to be set.
     * @returns {LogConfig} The current instance of LogConfig.
     * @throws {Error} If the file_prefix is not a string.
     */
    with_file_prefix(file_prefix) {
        if (typeof file_prefix !== "string") {
            throw new Error(`file_prefix must be a string. Unsupported param ${JSON.stringify(file_prefix)}`);
        }
   
        this.#file_prefix = file_prefix;
        return this;
    }
```

We create `jsdoc` comments with multi-line comment format using `/** ... */`. Then specify a tag using `@`. In the code snippet above, we specified two tags - `@params` and `@returns`. The tags have the following syntax

```textile
@tag {Type} <argument> <description>
```

The `Type` is the actual type of the `argument` you specified. that you're referring to. In our case it's the argument for `with_max_file_size` method is `max_file_size`. And the type for that is `number`. The description is the documentation part for that particular parameter.

Here's the `jsdoc` comments with `with_log_level` method

```js
    /**
     * @param {LogLevel} log_level The log level to be set.
     * @returns {LogConfig} The current instance of LogConfig.
     */
    with_log_level(log_level) {
        LogLevel.assert(log_level);
        this.#level = log_level;
        return this;
    }
```

I'll be not including the `jsdoc` comments to make the code snippets short, and easier to read. However, if you're writing vanilla javascript, it's a good practice to start incorporating these into your work flow. They'll save you a lot of time! There's much more than this that `jsdoc` helps us with. You can go through the documentation of `jsdoc` [here](https://jsdoc.app/).

# 1.1 The `RollingConfig` class

The **`RollingConfig`** class is going to be a vital part of our logging system that helps manage log files. It does this by rotating or rolling log files based on a set time interval or the size of file. This ensures that log files don't become too large and hard to handle.

The **`RollingConfig`** class's main purpose is to define settings for the log file's rolling process. This includes how often log files should be rolled, the maximum size of log files before they are rolled, and other relevant settings. By doing this, it helps keep log files organized and manageable while still preserving the historical data needed for analysis, debugging, and monitoring.

The **`RollingConfig`** class typically includes the two key features:

1. **Rolling Time Interval:** This setting determines how frequently log files are rolled. For example, you might set the logger to roll log files every few minutes, hours, or days, depending on how detailed you need your logs to be.
2. **Maximum File Size:** In addition to time-based rolling, the **`RollingConfig`** class may also support size-based rolling. When a log file exceeds a certain size limit, a new log file is created, with a new prefix to let you distinguish between different log files.

Before creating the `RollingConfig` class. Let's create 2 utility helper classes - `RollingSizeOptions` and `RollingTimeOptions`. As the name suggests, they are only going to support the `RollingConfig` class.

# 1.1.1 The `RollingSizeOptions` class

```js
// index.js

class RollingSizeOptions {
    static OneKB = 1024;
    static FiveKB = 5 * 1024;
    static TenKB = 10 * 1024;
    static TwentyKB = 20 * 1024;
    static FiftyKB = 50 * 1024;
    static HundredKB = 100 * 1024;

    static HalfMB = 512 * 1024;
    static OneMB = 1024 * 1024;
    static FiveMB = 5 * 1024 * 1024;
    static TenMB = 10 * 1024 * 1024;
    static TwentyMB = 20 * 1024 * 1024;
    static FiftyMB = 50 * 1024 * 1024;
    static HundredMB = 100 * 1024 * 1024;

    static assert(size_threshold) {
        if (typeof size_threshold !== "number" || size_threshold < RollingSizeOptions.OneKB) {
            throw new Error(
                `size_threshold must be at-least 1 KB. Unsupported param ${JSON.stringify(size_threshold)}`
            );
        }
    }
}
```

I've set some defaults, to make it easy for the clients of our library to use them. Instead of them having to declare an extra constant, they can quickly use `RollingSizeOptions.TenKB` or whatever they wish. However, they can also specify a number as a value, and that's where our `RollingSizeOptions.assert()` helper is going to do the validation for us.

# 1.1.1 The `RollingTimeOptions` class

```js
// index.js

class RollingTimeOptions {
    static Minutely = 60; // Every 60 seconds
    static Hourly = 60 * this.Minutely;
    static Daily = 24 * this.Hourly;
    static Weekly = 7 * this.Daily;
    static Monthly = 30 * this.Daily;
    static Yearly = 12 * this.Monthly;

    static assert(time_option) {
        if (
            ![this.Minutely, this.Hourly, this.Daily, this.Weekly, this.Monthly, this.Yearly].includes(time_option)
        ) {
            throw new Error(
                `time_option must be an instance of RollingConfig. Unsupported param ${JSON.stringify(
                    time_option
                )}`
            );
        }
    }
}
```

# 1.1 Finishing up the `RollingConfig` class

It's time to create our `RollingConfig` class. Let's add some basic functionality in it for now.

```js
// index.js
class RollingConfig {
    #time_threshold = RollingTimeOptions.Hourly;
    #size_threshold = RollingSizeOptions.FiveMB;

    static assert(rolling_config) {
        if (!(rolling_config instanceof RollingConfig)) {
            throw new Error(
                `rolling_config must be an instance of RollingConfig. Unsupported param ${JSON.stringify(
                    rolling_config
                )}`
            );
        }
    }

    // Provide a helper method for the clients, so instead of doing `new RollingConfig()`
    // they can simply use `RollingConfig.with_defaults()` that too without specifying the
    // `new` keyword.
    static with_defaults() {
        return new RollingConfig();
    }

    // Utilizing the `Builder` pattern here, to first verify that the size is valid.
    // If yes, set the size, and return the current instance of the class.
    // If it's not valid, throw an error.
    with_size_threshold(size_threshold) {
        RollingTimeOptions.assert_size(size_threshold);
        this.#size_threshold = size_threshold;
        return this;
    }

    // Same like above, but with `time`.
    with_time_threshold(time_threshold) {
        RollingTimeOptions.assert_time(time_threshold);
        this.#time_threshold = time_threshold;
        return this;
    }
    
     // Build from a `json` object instead of the `Builder`
     static from_json(json) {
        let rolling_config = new RollingConfig();

        Object.keys(json).forEach((key) => {
            switch (key) {
                case "size_threshold":
                    rolling_config = rolling_config.with_size_threshold(json[key]);
                    break;
                case "time_threshold":
                    rolling_config = rolling_config.with_time_threshold(json[key]);
                    break;
            }
        });
        
        return rolling_config;
    }
}
```

The `RollingConfig` class is ready to be used. It has no functionality, and is merely a configuration for our logger. It's useful to add a suffix like `Config`, `Options` for things that have no business logic inside them. It's generally a good design practice to stay focused on your naming conventions.

# 1.1.1 Let's recap

- `RollingConfig` - A class that maintains the configuration on how often a new log file file should be rolled out. It is based on the `RollingTimeOptions` and `RollingSizeOptions` utility classes which define some useful constants as well as an `assert()` method for the validation.

- `LogConfig` - A class that groups all other configurations into one giant class. This has a couple of private member variables - `#level` which is going to be of type `LogLevel` and keeps track of what logs should be written and what ignored; `#rolling_config` which is going to store the `RolllingConfig` for our logger; `#file_prefix` will be used to prefix log files.
  
  - `with_defaults` constructs and returns a new `LogConfig` object with some default values.
  
  - `with_log_level`, `with_file_prefix` and `with_rolling_config` mutates the current object after testing whether the input provided is valid. The example of what we learnt above - a `Builder` pattern.
  
  - `assert` validation for the `LogConfig` class.

- `Logger` - The backbone of our logger. It hardly has any functionality now, but this is the main class of our library. This is responsible to do all the hard work.

# 1.1 Adding more useful methods in the `LogConfig` class

The `LogConfig` class looks fine. But it's missing out on a lot of other features. Let's add them one by one. 

Firstly, not everyone is a fan of builder pattern, many people would like to pass in an object and ask the library to parse something useful out of it. It's generally a very good practice to expose various ways to do a particular task. 

We are going to provide an ability to create a `LogConfig` object from a json object.

```js
// index.js

...

class LogConfig {
    ...
    
    with_rolling_config(config) {
        this.#rolling_config = RollingConfig.from_json(config);
        return this;
    }

    static from_json(json) {
        // Create an empty LogConfig object.
        let log_config = new LogConfig();
        
        // ignore the keys that aren't needed for our purposes.
        // if a key matches, let's set it to the provided value.
        Object.keys(json).forEach((key) => {
            switch (key) {
                case "level":
                    log_config = log_config.with_log_level(json[key]);
                    break;
                case "rolling_config":
                    log_config = log_config.with_rolling_config(json[key]);
                    break;
                case "file_prefix":
                    log_config = log_config.with_file_prefix(json[key]);
                    break;
            }
        });

        // return the mutated log_config object
        return log_config;
    }

    ...
}

...
```

Now we can call it like this -

```js
const json_config = { level: LogLevel.Debug };
const config = LogConfig.from_json(json_config).with_file_prefix("Testing");

// or

const config = LogConfig.from_json({ level: LogLevel.Debug }).with_file_prefix('Test');

// or 

const config = LogConfig.with_defaults().with_log_level(LogLevel.Critical);

// Try to add an invalid value
const config = LogConfig.from_json({ level: 'eh?' }); // fails

Error: log_level must be an instance of LogLevel. Unsupported param "eh?"
    at LogLevel.assert (/Users/ishtmeet/Code/logtar/index.js:251:19)
    at LogConfig.with_log_level (/Users/ishtmeet/Code/logtar/index.js:177:18)
    at /Users/ishtmeet/Code/logtar/index.js:143:45
```

The API of our library is already looking solid. But there's one last thing that we wish to have as a convenience method to build `LogConfig` from. It's from a config file. Let's add that method

```js
// import the `node:fs` module to use the `readFileSync`
const fs = require('node:fs')

class LogConfig {
    ...

    /**
     * @param {string} file_path The path to the config file.
     * @returns {LogConfig} A new instance of LogConfig with values from the config file.
     * @throws {Error} If the file_path is not a string.
     */
    static from_file(file_path) {
        // `fs.readFileSync` throws an error if the path is invalid. 
        // It takes care of the OS specific path handling for us. No need to
        // validate paths by ourselves.
        const file_contents = fs.readFileSync(file_path);

        // Send this over to our `from_json` method to do the rest
        return LogConfig.from_json(JSON.parse(file_contents));
    }    

    ...
}
```

Do you notice how we reused the `from_json` method to parse the json into a `LogConfig` object? This is one thing you have to keep in mind while building good and maintainable APIs. Avoid code duplication, and make the methods/helpers re-usable. As much as you can.

# 1.1.1 Why `readFileSync`?

Loggers are usually initialized once when the program starts, and are not usually created after the initialization phase. As such, using `readFileSync` over the asynchronous version (readFile) can provide several benefits in this specific case.

`readFileSync` operates synchronously, meaning it blocks the execution of the code until the file reading is complete. For logger configuration setup, this is often desired because the configuration is needed to initialize the logger properly before any logging activity begins, since our application will be using the logger internally. 

We cannot let the application start before initializing the logger. Using asynchronous operations like `readFile` could introduce complexities in managing the timing of logger initialization.

Let's test using a config file. Create a `config.demo.json` file with the following contents

```json
{
    "level": 3,
    "file_prefix": "My_Prefix_",
    "rolling_config": {
        "size_threshold": 1024,
        "time_threshold": 3600
    }
}
```

Since we have added the support for files, the following code will work now

```js
const config = LogConfig.from_file('./config.demo.json')
const logger = Logger.with_config(config)
```

Everything works as expected. 

> Note: The entire code we write here can be found [here](/code/chapter_04/index.js). This will be a single file, and we'll refactor in subsequent chapters.

> Note: The entire code we write here can be found [here](/code/chapter_04). This will be a single file, and we'll refactor in subsequent chapters.

[![Read Next](/assets/imgs/next.png)](/chapters/ch04.1-refactoring-the-code.md)

![](https://uddrapi.com/api/img?page=ch04)
[![Read Prev](/assets/imgs/prev.png)](/chapters/ch04-logtar-our-logging-library.md)

# 1 Refactoring the code

> The code for the entire chapter can be found [at the code/chapter_04.1 directory](/code/chapter_04.1)

In this section, we will explore how to improve the organization and maintenance of our library before introducing more features. Currently, all the code is in one file i.e **`index.js`**. `index.js` also serves as the entry point of our project. We will show how to move the code to multiple files without changing how it works.

# 1.1 **The Need for Refactoring**

The code has become too large and difficult to manage. This chapter covers the benefits of breaking it into smaller files.

Splitting the code into separate files creates a more organized and manageable codebase. Each part should have a clear responsibility, making it easier to work with and understand. This simplification lays the foundation for future improvements, ensures that the project remains consistent and easy to work with, and allows for the introduction of new features.

Some of the key benefits of organizing/splitting your code into smaller re-usable pieces:

1. **Modularity:** Breaking the code into smaller files helps us manage each component better. This way, the codebase is easier to understand and work with.
2. **Readability:** Smaller files are easier to read and understand. Even people who haven't written any code in your library can quickly understand each file's purpose and contents.
3. **Maintainability:** When the codebase is organized into separate files by functionality, it becomes easier to maintain and update. Changes are limited to specific modules, reducing the risk of unintended consequences.
4. **Testing:** Individual components can be tested separately when code is in separate files. This leads to more thorough testing and fewer interdependent tests. (We'll cover testing in a later chapter of this book.)

# 1.1 **Creating Separate Files**

Let's work together to split the code in **`index.js`** into separate files for each class and utility. First, create a new directory called `lib`. Inside the `lib` directory, create two folders named `utils` and `config`. Add a file `logtar.js` inside the root of the `lib` directory.

Inside the `utils` directory, create two files named `rolling-options.js` and `log-level.js`. Inside the `config` directory, create two files named `rolling-config.js` and `log-config.js`.

Finally, create a file named `logger.js` at the root of the directory, where the `index.js` and `package.json` files are located.

Your directory structure should now look something like this:

```jsx
lib/
├── logtar.js
├── logger.js
├── utils/
│     ├── rolling-options.js
│     ├── log-level.js
├── config/
│     ├── rolling-config.js
│     ├── log-config.js
├── index.js (entry point)
├── package.json
└── logger.js
```

# 1.1.1 Explanation

The `logtar.js` file serves as the key file that exports all the necessary functionality to the client.

The `logger.js` file exports our `Logger` class and some related functionality.

The `utils/rolling-options.js` file exports our `RollingSizeOptions` and `RollingTimeOptions` classes.

The `index.js` file only contains a single line of code:

```jsx
module.exports = require('./lib/logtar');
```

The other files export functionality based on their names.

> Note: If you are not working with TypeScript and are using vanilla JavaScript, get into the habit of using `JSDoc` as much as possible. Use it for every function's argument and return type. Be explicit. This may take a bit of time, but it will be convenient in the long run. Using `JSDoc` will make your workflow much smoother as your project grows.

# 1.1 The `index.js` file

Here are the contents inside the `index.js` file

```jsx
module.exports = require('./lib/logtar')
```

# 1.1 The `lib/logtar.js` file

```jsx
module.exports = {
    Logger: require('./logger').Logger,
    LogConfig: require('./config/log-config').LogConfig,
    RollingConfig: require('./config/rolling-config').RollingConfig,
    LogLevel: require('./utils/log-level').LogLevel,
    RollingTimeOptions: require('./utils/rolling-options').RollingTimeOptions,
    RollingSizeOptions: require('./utils/rolling-options').RollingSizeOptions,
};
```

# 1.1 The `lib/logger.js` file

```jsx
const { LogConfig } = require("./config/log-config");
const { LogLevel } = require("./utils/log-level");

class Logger {
    /**
     * @type {LogConfig}
     */
    #config;

    /**
     * @returns {Logger} A new instance of Logger with default config.
     */
    static with_defaults() {
        return new Logger();
    }

    /**
     * 
     * @param {LogConfig} log_config 
     * @returns {Logger} A new instance of Logger with the given config.
     */
    static with_config(log_config) {
        return new Logger(log_config);
    }

    /**
     * @param {LogLevel} log_level
     */
    constructor(log_config) {
        log_config = log_config || LogConfig.with_defaults();
        LogConfig.assert(log_config);
        this.#config = log_config;
    }

    /**
     * @returns {LogLevel} The current log level.
     */
    get level() {
        return this.#config.level;
    }
}

module.exports = { Logger };
```

# 1.1 The `lib/config/log-config.js` file

```jsx
const fs = require("node:fs");

const { LogLevel } = require("../utils/log-level");
const { RollingConfig } = require("./rolling-config");

class LogConfig {
    /**
     * @type {LogLevel}
     * @private
     * @description The log level to be used.
     */
    #level = LogLevel.Info;

    /**
     * @type RollingConfig
     * @private
     */
    #rolling_config;

    /**
     * @type {string}
     * @private
     * @description The prefix to be used for the log file name.
     *
     * If the file prefix is `MyFilePrefix_` the log files created will have the name
     * `MyFilePrefix_2021-09-01.log`, `MyFilePrefix_2021-09-02.log` and so on.
     */
    #file_prefix = "Logtar_";

    constructor() {
        this.#rolling_config = RollingConfig.with_defaults();
    }

    /**
     * @returns {LogConfig} A new instance of LogConfig with default values.
     */
    static with_defaults() {
        return new LogConfig();
    }

    /**
     * @param {string} file_path The path to the config file.
     * @returns {LogConfig} A new instance of LogConfig with values from the config file.
     * @throws {Error} If the file_path is not a string.
     */
    static from_file(file_path) {
        const file_contents = fs.readFileSync(file_path);
        return LogConfig.from_json(JSON.parse(file_contents));
    }

    /**
     * @param {Object} json The json object to be parsed into {LogConfig}.
     * @returns {LogConfig} A new instance of LogConfig with values from the json object.
     */
    static from_json(json) {
        let log_config = new LogConfig();
        Object.keys(json).forEach((key) => {
            switch (key) {
                case "level":
                    log_config = log_config.with_log_level(json[key]);
                    break;
                case "rolling_config":
                    log_config = log_config.with_rolling_config(json[key]);
                    break;
                case "file_prefix":
                    log_config = log_config.with_file_prefix(json[key]);
                    break;
            }
        });
        return log_config;
    }

    /**
     * @param {LogConfig} log_config The log config to be validated.
     * @throws {Error} If the log_config is not an instance of LogConfig.
     */
    static assert(log_config) {
        if (arguments.length > 0 && !(log_config instanceof LogConfig)) {
            throw new Error(
                `log_config must be an instance of LogConfig. Unsupported param ${JSON.stringify(log_config)}`
            );
        }
    }

    /**
     * @returns {LogLevel} The current log level.
     */
    get level() {
        return this.#level;
    }

    /**
     * @param {LogLevel} log_level The log level to be set.
     * @returns {LogConfig} The current instance of LogConfig.
     * @throws {Error} If the log_level is not an instance of LogLevel.
     */
    with_log_level(log_level) {
        LogLevel.assert(log_level);
        this.#level = log_level;
        return this;
    }

    /**
     * @returns {RollingConfig} The current rolling config.
     */
    get rolling_config() {
        return this.#rolling_config;
    }

    /**
     * @param {RollingConfig} config The rolling config to be set.
     * @returns {LogConfig} The current instance of LogConfig.
     * @throws {Error} If the config is not an instance of RollingConfig.
     */
    with_rolling_config(config) {
        this.#rolling_config = RollingConfig.from_json(config);
        return this;
    }

    /**
     * @returns {String} The current max file size.
     */
    get file_prefix() {
        return this.#file_prefix;
    }

    /**
     * @param {string} file_prefix The file prefix to be set.
     * @returns {LogConfig} The current instance of LogConfig.
     * @throws {Error} If the file_prefix is not a string.
     */
    with_file_prefix(file_prefix) {
        if (typeof file_prefix !== "string") {
            throw new Error(`file_prefix must be a string. Unsupported param ${JSON.stringify(file_prefix)}`);
        }

        this.#file_prefix = file_prefix;
        return this;
    }
}

module.exports = { LogConfig };
```

# 1.1 The `lib/config/rolling-config.js` file

```jsx
const { RollingTimeOptions, RollingSizeOptions } = require("../utils/rolling-options");

class RollingConfig {
    /**
     * Roll/Create new file every time the current file size exceeds this threshold in `seconds`.
     *
     * @type {RollingTimeOptions}
     * @private
     *
     */
    #time_threshold = RollingTimeOptions.Hourly;

    /**
     * @type {RollingSizeOptions}
     * @private
     */
    #size_threshold = RollingSizeOptions.FiveMB;

    /**
     * @returns {RollingConfig} A new instance of RollingConfig with default values.
     */
    static with_defaults() {
        return new RollingConfig();
    }

    /**
     * @param {number} size_threshold Roll/Create new file every time the current file size exceeds this threshold.
     * @returns {RollingConfig} The current instance of RollingConfig.
     */
    with_size_threshold(size_threshold) {
        RollingSizeOptions.assert(size_threshold);
        this.#size_threshold = size_threshold;
        return this;
    }

    /**
     * @param {time_threshold} time_threshold Roll/Create new file every time the current file size exceeds this threshold.
     * @returns {RollingConfig} The current instance of RollingConfig.
     * @throws {Error} If the time_threshold is not an instance of RollingTimeOptions.
     */
    with_time_threshold(time_threshold) {
        RollingTimeOptions.assert(time_threshold);
        this.#time_threshold = time_threshold;
        return this;
    }

    /**
     * @param {Object} json The json object to be parsed into {RollingConfig}.
     * @returns {RollingConfig} A new instance of RollingConfig with values from the json object.
     * @throws {Error} If the json is not an object.
     */
    static from_json(json) {
        let rolling_config = new RollingConfig();

        Object.keys(json).forEach((key) => {
            switch (key) {
                case "size_threshold":
                    rolling_config = rolling_config.with_size_threshold(json[key]);
                    break;
                case "time_threshold":
                    rolling_config = rolling_config.with_time_threshold(json[key]);
                    break;
            }
        });

        return rolling_config;
    }
}

module.exports = { RollingConfig };
```

# 1.1 The `lib/utils/log-level.js` file

```jsx
class LogLevel {
    static #Debug = 0;
    static #Info = 1;
    static #Warn = 2;
    static #Error = 3;
    static #Critical = 4;

    static get Debug() {
        return this.#Debug;
    }

    static get Info() {
        return this.#Info;
    }

    static get Warn() {
        return this.#Warn;
    }

    static get Error() {
        return this.#Error;
    }

    static get Critical() {
        return this.#Critical;
    }

    static assert(log_level) {
        if (![this.Debug, this.Info, this.Warn, this.Error, this.Critical].includes(log_level)) {
            throw new Error(
                `log_level must be an instance of LogLevel. Unsupported param ${JSON.stringify(log_level)}`
            );
        }
    }
}

module.exports = { LogLevel };
```

# 1.1 The `lib/utils/rolling-options.js` class

```jsx
class RollingSizeOptions {
    static OneKB = 1024;
    static FiveKB = 5 * 1024;
    static TenKB = 10 * 1024;
    static TwentyKB = 20 * 1024;
    static FiftyKB = 50 * 1024;
    static HundredKB = 100 * 1024;

    static HalfMB = 512 * 1024;
    static OneMB = 1024 * 1024;
    static FiveMB = 5 * 1024 * 1024;
    static TenMB = 10 * 1024 * 1024;
    static TwentyMB = 20 * 1024 * 1024;
    static FiftyMB = 50 * 1024 * 1024;
    static HundredMB = 100 * 1024 * 1024;

    static assert(size_threshold) {
        if (typeof size_threshold !== "number" || size_threshold < RollingSizeOptions.OneKB) {
            throw new Error(
                `size_threshold must be at-least 1 KB. Unsupported param ${JSON.stringify(size_threshold)}`
            );
        }
    }
}

class RollingTimeOptions {
    static Minutely = 60; // Every 60 seconds
    static Hourly = 60 * this.Minutely;
    static Daily = 24 * this.Hourly;
    static Weekly = 7 * this.Daily;
    static Monthly = 30 * this.Daily;
    static Yearly = 12 * this.Monthly;

    static assert(time_option) {
        if (![this.Minutely, this.Hourly, this.Daily, this.Weekly, this.Monthly, this.Yearly].includes(time_option)) {
            throw new Error(
                `time_option must be an instance of RollingConfig. Unsupported param ${JSON.stringify(time_option)}`
            );
        }
    }
}

module.exports = {
    RollingSizeOptions,
    RollingTimeOptions,
};
```
See how we can still benefit from the strong jsdoc type completion for our classes, even if they exist in different files? This isn't something achievable with regular JavaScript – all credit goes to jsdoc.

> The code for the entire chapter can be found [at the code/chapter_04.1 directory](/code/chapter_04.1)

[![Read Next](/assets/imgs/next.png)](/chapters/ch04.2-writing-logs.md)

![](https://uddrapi.com/api/img?page=ch04.1)
[![Read Prev](/assets/imgs/prev.png)](/chapters/ch04.1-refactoring-the-code.md)

# 1 Writing logs

> The code for the entire chapter can be found [at the code/chapter_04.2 directory](/code/chapter_04.2/)

We've covered how to build the core utility helpers that will help us construct our logging library in a more modular way. However, we haven't gotten to the fun part of actually writing logs yet.

You might be wondering how we can call this a logging library if we haven't even learned how to write logs with it. Don't worry, we're about to get to that!

Before we begin writing logs to files, let's first introduce some concepts related to file-based logging and how we can make it more efficient and faster.

# 1.1 1. Re-using the File Handle

When logging to a file, it's important to manage the file handle efficiently. The file handle is the connection between our code and the log file on disk. Opening and closing the file for every log entry can be slow and use up resources. To avoid this, we want to reuse the file handle throughout the logging process.

To do this, we open the file handle once when we start the logging library and keep it open until the logging is done. This means we don't need to keep opening and closing the file, which can speed up our logging and save resources.

# 1.1 2. Log Rotation

Log rotation is a critical strategy in file-based logging to manage log files over time. As your application generates more log data, log files can become large and unwieldy. Log rotation involves creating new log files periodically or based on certain conditions, and optionally archiving or deleting older log files. This helps keep the log files at a manageable size, ensures easier log analysis, and prevents running out of disk space.

# 1.1 3. Asynchronous Logging

In network based apps, especially those with high levels of concurrency or those that involve asynchronous operations, implementing asynchronous logging can be beneficial, and that's the only reason we're using `node:fs/promises` instead of `node:fs`. Asynchronous logging ensures that the act of writing logs doesn't block or slow down the main execution thread of your application. This can prevent performance bottlenecks and maintain the responsiveness of your application.

When logging is performed synchronously, each log entry is written immediately to the log file or output, which can introduce delays and impact the overall performance of your application. Asynchronous logging, on the other hand, involves buffering log messages and writing them to the log file in batches or on a separate thread or process.

> We'll need to do more optimization than just using asynchronous file writing. Specifically, we should store the entire log contents in memory and write them periodically. This will make the process extremely fast and ensure that it doesn't consume too much memory.

By decoupling the logging process from the main application logic, we can achieve several advantages:

- **Improved Performance:** Asynchronous logging allows the main application thread to continue executing without waiting for log writes to complete. This can be crucial for applications that require high responsiveness and throughput.

- **Reduced I/O Overhead:** Writing log messages to disk can be an I/O-intensive operation. By batching multiple log messages together and writing them in one go, you reduce the frequency of disk I/O operations, which can improve efficiency.

- **Better Resource Utilization:** Asynchronous logging allows you to optimize resource utilization, such as managing concurrent log writes, handling errors without disrupting the main flow, and efficiently managing file handles.

- **Enhanced Scalability:** Applications with multiple threads or processes benefit from asynchronous logging because it minimizes contention for resources like the log file. This is particularly valuable in scenarios where multiple components are concurrently generating log messages

# 1.1 4. Getting Caller Information (Module and Line Number)

Including caller information, such as the file name and line number from which a log message originated, can significantly enhance the effectiveness of our logging library. This feature provides contextual insight into where specific events occurred in the codebase, making it easier to identify the source of issues and troubleshoot them.

When an application encounters an error or unexpected behavior, having access to the module and line number associated with the log message allows developers to:

- Quickly locate the exact location in the code where the event occurred.
- Understand the sequence of events leading up to the issue.
- Make precise code adjustments to fix problems.

Implementing this feature might involve using techniques from the programming language's stack trace or introspection mechanisms. Here's a high-level overview of how you could achieve this:

1. **Capture Caller Information:** When a log message is generated, our logging library will retrieve the caller information, including the module and line number. We'll learn it in a bit that how can we do that.

2. **Format the Log Message:** Combine the captured caller information with the log message and other relevant details like timestamp and log level.

3. **Output the Log Message:** Write the formatted log message to the desired output destinations, ensuring that the caller information is included.

Enough of the theory, let's start writing logs.

# 1 Testing our current API

Let's start with building small features that we need to have on our `Logger` class. Before that, we're going to do some testing on whatever we've built till now.

To do the testing, we'll create a new file `test.js` and a `config.json`. `test.js` will hold our code that we write while making use of our `logtar` library. The `config.json` will used to store our config for `LogConfig` as a `json` object.

```js
// test.js

const { Logger, LogConfig } = require('./index')

const logger = Logger.with_config(LogConfig.from_file('./config.json'));
```

The `config.json` file has the following contents. You may try to tweak the values as well. Try putting in the values that aren't supported by us, and see whether the `assert` methods that we created actually crash the program or not?

```js
// config.json

{
    "level": 3,
    "file_prefix": "LogTar_",
    "rolling_config": {
        "size_threshold": 1024000,
        "time_threshold": 86400
    }
}
```

Try executing the `test.js` file.

```bash
$ node test.js
```

Nothing happens. But this proves that the whole setup for our `Logger` and other classes is working perfectly fine.

To check whether the config is being loaded properly, we can create a couple of getter methods on our `Logger` class.

```js
// file: lib/logger.js

class Logger {
    ...
    get level() {
        return this.#config.level;
    }

    get file_prefix() {
        return this.#config.file_prefix;
    }

    get time_threshold() {
        return this.#config.rolling_config.time_threshold;
    }

    get size_threshold() {
        return this.#config.rolling_config.size_threshold;
    }
    ...  
}
```

Now, in `test.js` print those out to the standard output.

```bash
$node test.js

# 1 outputs
LogTar_
1024000
86400
3
```

Perfect! Everything works fine. But what happens if I try to put a value that's not supported by our Logger API? Let's change the `time_threshold` to `400` in `config.json` and re-run the app.

```bash
$node test.js

# 1 outputs
Error: time_option must be an instance of RollingConfig. Unsupported param 400
    at Function.assert (/Users/ishtmeet/Code/logtard/lib/utils/rolling-options.js:36:19)
```

Why? Let's take a quick look at our `RollingTimeOptions` utility class 

```js
// file: lib/util/rolling-options.js

class RollingTimeOptions {
    static Minutely = 60; // Every 60 seconds
    ... // Other options

    // Throw an error when the user sets the value which isn't one of those
    static assert(time_option) {
        if (![this.Minutely, this.Hourly, this.Daily, this.Weekly, this.Monthly, this.Yearly].includes(time_option)) {
            throw new Error(
                `time_option must be an instance of RollingConfig. Unsupported param ${JSON.stringify(time_option)}`
            );
        }
    }
}
```

You might argue that this isn't the best dev experience. However, I think it is. We should always constrain over how configurable our library is. You would hardly need any other duration other than we specified i.e `Minutely`, `Hourly`, `Daily` etc. and anything more than `Yearly`

Let's change the `time_threshold` in `config.json` back to `86400` which means 1 day.

# 1.1 Implementing logging methods

Since our logger supports 5 types of `log_level`s, let's write a public method for each one of those on our `Logger` class.

```js
// file lib/logger.js

class Logger {
    ...

    debug(message) { console.log('Debug: %s', message) }

    info(message) { console.log('Info: %s', message) }

    warn(message) { console.log('Warn: %s', message) }

    error(message) { console.log('Error: %s', message) }

    critical(message) { console.log('Critical: %s', message) }

    ...
}
```

And in `test.js` call these methods

```js
// file: test.js
const { Logger, LogConfig } = require('./index')

const logger = Logger.with_config(LogConfig.from_file('./config.json'));

console.log(logger.file_prefix);
console.log(logger.size_threshold);
console.log(logger.time_threshold);
console.log(logger.level);

logger.debug('Hello debug');
logger.info('Hello info');
logger.warn('Hello warning');
logger.error('Hello error');
logger.critical('Hello critical');


// outputs
LogTar_
1024000
86400
3
Debug: Hello debug
Info: Hello info
Warn: Hello warning
Error: Hello error
Critical: Hello critical
```

# 1.1 DRY (Don't Repeat Yourself)

The "Don't Repeat Yourself" (DRY) principle is a basic concept in software development that promotes code reusability and maintainability. The idea behind DRY is to avoid duplicating code or logic in multiple places within your codebase. Instead, you aim to create a single source for a particular piece of functionality, and whenever you need that functionality, you refer to that source.

DRY encourages developers to write clean, efficient, and modular code by:

- Reducing the chances of errors: Duplicated code increases the chances of mistakes or bugs when changes are made in one place but not in others.
- Simplifying maintenance: When a change is required, you only need to update the code in one place, making it easier to keep your codebase up-to-date and consistent.
- Enhancing readability: Code that is free from unnecessary duplication is easier to understand and follow, making it more accessible to other developers.

Although following the DRY principle is generally beneficial, there can be situations where duplication might not necessarily be a bad thing. Not every instance of code repetition needs to be eliminated, and striving for absolute **DRY**ness in all cases might lead to overcomplicated solutions or premature abstraction.

A guideline often mentioned is the "**Rule of Three**": If you find yourself repeating the same code or logic more than three times, it's a strong indication that you should consider refactoring that code into a reusable function, class, or module. This threshold helps you strike a balance between reusability and pragmatic simplicity.

Trying too hard to avoid repeating code can make it so complicated that it's hard to understand. It's better to find a balance where your code can be used again and again, but it's still easy to read and work with.

# 1.1 The `log` method

We'll introduce a `private` member method called `log`. The clients don't need to know about what's going inside the library, so making the crucial/core methods private is a nice thing.

Update the code of your `Logger` class to include the `log` method. Also, update the code so that all other helper methods call the `log` method, thus avoiding the code duplication.

```js
// lib/logger.js

const { LogLevel } = require("./utils/log-level");

class Logger {
    ...

    #log(message, log_level) {
        console.log('%s: %s', message, log_level)
    }

    debug(message) {
        this.#log(message, LogLevel.Debug);
    }

    info(message) {
        this.#log(message, LogLevel.Info);
    }

    warn(message) {
        this.#log(message, LogLevel.Warn);
    }

    error(message) {
        this.#log(message, LogLevel.Error);
    }

    critical(message) {
        this.#log(message, LogLevel.Critical);
    }
    ...
}
```

Executing the code gives us the desired output. The output includes the `LogLevel` as an integer. However, that's not helpful at all, we should be showing what the `LogLevel` is with a string representation of the level.

Let's introduce a new static method `to_string` inside the `LogLevel` class

```js
// file: lib/utils/log-level.js

class LogLevel {
    ...
    static to_string(log_level) {
        const levelMap = {
            [this.Debug]: "DEBUG",
            [this.Info]: "INFO",
            [this.Warn]: "WARN",
            [this.Error]: "ERROR",
            [this.Critical]: "CRITICAL"
        };

        if (levelMap.hasOwnProperty(log_level)) {
            return levelMap[log_level];
        }

        throw new Error(`Unsupported log level ${log_level}`);
    }
    ...
}
```

Change the code inside the `log()` method of `Logger` class.

```js
// lib/logger.js

const { LogLevel } = require("./utils/log-level");

class Logger {
    ...

    #log(message, log_level) {
        console.log('%s: %s', message, LogLevel.to_string(log_level))
    }
    ...
}
```

This outputs

```
Hello debug: DEBUG
Hello info: INFO
Hello warning: WARN
Hello error: ERROR
Hello critical: CRITICAL
```

Everything looks good.

# 1.1 Considering the `log_level` member variable

Notice that in our `config.json` we specified that the log level should be `3` that is `LogLevel.Error`. Specifying the log level means that we should only write logs that are equal to or above the specified level.

Imagine a production application, which is usually under a very heavy load. We'd like to specify the `level` as `LogLevel.Warn` or even `LogLevel.Info`. We don't care about the `LogLevel.Debug` logs at all. They might pollute the log files as the debug logs are usually very verbose.

We are going to add a small check to ignore the logs which are below the current `log_level`.

```js
// file: lib/logger.js

class Logger {
    ...
    #log(message, log_level) {
        if (log_level < this.#config.level) {
            return;
        }

        console.log('%s: %s', message, LogLevel.to_string(log_level))
    }
    ...    
}
```

On running, we get the following output.

```
Hello error: ERROR
Hello critical: CRITICAL
```

We only write logs based off the current logger's `log_level`

# 1.1 Writing to a file

We've been using `console.log()` to print the log messages to the standard output, or your terminal. However, there are many drawbacks to this approach. 

1. **Lack of Persistence:** Console logs are ephemeral and disappear once the application session ends (when you close the terminal or exit out of an ssh connection). This makes it challenging to review logs for past sessions.
2. **Performance Impact:** Continuous console output can impact application performance, especially when generating a high volume of logs. It can slow down the application and interfere with its responsiveness. There are certain ways to get mitigate this, we'll talk about this in a later chapter.

Create a private `log_file_handle` member

```js
// file: lib/logger.js 

const fs = require('node:fs/promises') 

class Logger {
    ...

    /**
     * @type {fs.FileHandle}
     */
     #log_file_handle;

    ...
}
```

The `log_file_handle` member variable will be our opened file handle that we'll reference again and again whenever we wish to write logs to a file. It will hold the opened log file, which will be created whenever the client initiates our logging library.

We'll expose a public method on the `Logger` called `init` and make that `async` so that it waits for the `init()` to finish initializing the file handle before moving further.

```js
// file: lib/logger.js
const fs = require('node:fs/promises') 

class Logger {
    ...

    #log_file_handle;

    async init() {
        const file_name = this.#config.file_prefix + new Date().toISOString().replace(/\..+/, "") + ".log";
        this.#log_file_handle = await fs.open(file_name, "a+");
    }
}
```

There's a lot going on in this method. Let's break it down.

# 1.1.1 A small primer on Regular Expressions

1. The `init` method is responsible for initializing the logger, which includes creating or opening a log file with a dynamically generated name based on the current date and time.

2. `this.#config.file_prefix` is used to prefix the name of the log file, which is actually set in the `config.json` file or can be passed as a json object or utilizing the `Builder` pattern.

3. `new Date().toISOString()` generates a string representation of the current date and time in the ISO 8601 format, such as "2023-08-18T15:30:00.000Z".

4. `.replace(/\..+/, "")` is a regular expression operation. Let's break down the regex:
   
   - `\.` matches a literal dot character. Since the dot (`.`) is a special character in regular expression, known as a wildcard. It matches any single character except for a newline character (`\n`). This is useful when you want to match any character, which is often used in pattern matching. 
     
     However, in this case, we want to match a literal dot character in the string (the dot in the date-time format "00.000Z"). To achieve this, we need to escape the dot character by preceding it with a backslash (`\`). When you place a backslash before a special character, it indicates that you want to match the literal character itself, rather than its special meaning.
   
   - `.+` matches one or more of any character except newline. We match for all the characters following the dot.
   
   - `/g` is the global flag, indicating that the replacement should be applied to all occurrences of the pattern.
   
   - So, the regex `\..+` matches the dot and all characters after it in the string.
   
   - The replacement `""` removes the dot and all characters after it.

5. The result of the `replace` operation is a modified version of the ISO string, which now includes only the date and time portion, without the fractional seconds.

6. `.log` is appended to the modified date and time string to form the final log file name.

7. `await fs.open(file_name, "a+")` opens or creates the log file using the `fs.open` function with "a+" flag. We talked about the modes in a [previous chapter](https://github.com/ishtms/learn-nodejs-hard-way/blob/master/chapters/ch03-working-with-files.md#flag-argument)
   
   - If the file doesn't exist, it will be created.
   - If the file exists, data can be appended to it.
   - The `"a+"` mode allows both reading and appending. Appending means, we begin writing to the end of the file instead of from the 1st line. However, if the file is empty, it starts from the beginning. 

This code initializes the logger by creating or opening a log file with a name based on the current date. The regular expression is used to remove the fractional seconds from the ISO date string, and the resulting string is used as part of the log file name. This allows for creating a new log file every time the `init` method is called, typically representing logs for a specific time period.

> Since regular expressions are one of the most important concepts which are usually neglected, we'll have an entire chapter dedicated to mastering regular expressions in case you'd like to get comfortable using them.

# 1.1.1 Testing the log file creation

Let us try to test the `init` method, and see if it creates a log file like we desire?

Before that, we'll look at a small nuance of calling an `async` function inside a `non-async` scope. Add the following log statement in the `init` method

```js
// file: lib/logger.js

class Logger {
    ...

    async init() {
        const file_name = this.#config.file_prefix + new Date().toISOString().replace(/\..+/, "") + ".log";
        this.#log_file_handle = await fs.open(file_name, "a+");
        console.log("File created.")
    }
    ...
}
```

And then call the `init` method from the `test.js`

```js
// file: test.js

const { Logger, LogConfig } = require('./index')

const logger = Logger.with_config(LogConfig.from_file('./config.json'));
await logger.init();
console.log("Program exits.")
```

However the output is strange

```bash
# 1 outputs
End of the file
Finished creating a file and opening
```

Why is it so? It is because we're using the `node:fs/promises` module, where all the functions are asynchronous. That means, they do not block any code and continue execution without waiting. This is the key to creating performant concurrent applications.

How do we fix it? Simply by `await`ing the `init()` method.

```js
// file: test.js

const { Logger, LogConfig } = require("./index");

async function main() {
    const logger = Logger.with_config(LogConfig.from_file("./config.json"));
    await logger.init();
    console.log("End of the file");
}

main();
```

If you try to run now, the output is what you would've expected.

```bash
# 1 Outputs
Finished creating a file and opening
End of the file
```

You'll also notice that the log file has also been created! For me the log file was created in the root directory. The name of the log file is

```
LogTar_2023-08-18T17:20:23.log
```

`LogTar_` is the prefix that I specified in the `config.json` file. Following it is the timestamp of when the file was created. We also add a `.log` extension at the end. All working as expected.

# 1.1 Another gotcha

If you're paying attention, you'll have already figured out that the way we're providing path to a file is not a nice way. If we wanted to run the `test.js` file from a different directory, say `lib/config` we'll get an error.

```bash
$ cd lib/config && node ../../test.js

// outputs
Error: ENOENT: no such file or directory, open './config.json'
```

Let's fix this by using the `__dirname` global variable that we used earlier.

```js
// file: test.js

const path = require('node:path')
const { Logger, LogConfig } = require("./index");

async function main() {
    const logger = Logger.with_config(LogConfig.from_file(path.join(__dirname, 'config.json')));
    await logger.init();
    console.log("End of the file");
}

main();
```

Now if we try to run it, it works irrespective of the directory you're running the code from.

```bash
$ node ../../test.js
# 1 Outputs
Finished creating a file and opening
End of the file
```

# 1.1 Logs directory configuration

When we run our code using the `node ../../test.js` command, the log files are being created in the same place where we typed that command, which is the `lib/config` folder. This is not what we want. Our library is a tool that other people will use. We should have complete say over where the log files go. This is because we want to make sure our library works well no matter how others use it.

Normally, logs (like records of what our code is doing) are put in a special folder called "log." This folder sits at the main spot of the entire project.

By centralizing the log files within the `log` directory at the root of the project that includes our library, we achieve a more organized and modularized approach to logging. This ensures that the log files don't clutter the main project's structure and are conveniently located in one designated place.

# 1.1.1 Our first script

Inside the `package.json` file, add a script `start` with the value of `node test.js` and get rid of the `test` script.

```json
// file: package.json

{
    ...
    "scripts": {
        "start": "node test.js"
    },
    ...    
}
```

If you run `npm start` from any folder of the project, it runs the `node test.js` command from the directory the `package.json` file is located.

The logs output in the root of the directory. Let's create them inside the `log` directory, instead of the root directory.

Update the code inside `Logger.init` method:

```js
// file: lib/logger.js

const path = require('node:path')

async init() {
    const file_name = this.#config.file_prefix + new Date().toISOString().replace(/\..+/, "") + ".log";
    this.#log_file_handle = await fs.open(path.join('logs', file_name), "a+");
}
```

If you do run `npm start`, it fails.

```bash
# 1 Outputs
[Error: ENOENT: no such file or directory, open 'logs/LogTar_2023-08-18T19:14:46.log'] 
```

It's failing because it could not find the `log` directory. If we mention something like this in our library's readme:

```bash
Hey please create a `log` folder in order for this library to work
```

They'll simply ignore the library and use something better. Let's make the whole process streamlined. Irrespective of where our library's code live, whether it's inside the `node_modules` folder or in the same directory.

# 1 The `require` object

We've been using `require` throughout our code, which just imports a module, and exposes all it's exported functionality - whether it's a variable, a function or a class. You may think of it as a function since `require('node:path')` indeed looks like a function call.

But, require is not just a function!

`require` also acts like an object that has several useful properties and methods associated with it. When you use `require` in your code, you are utilizing both its function-like behavior to load modules and its object-like properties and methods to interact with modules and the module system.

For example:

- `require.resolve('module-name')`: Returns the path of the module without actually loading it.
- `require.cache`: Provides access to the cache of loaded modules.
- `require.main`: Provides access to the `Module` object representing the entry script loaded when the Node.js process launched. This is exactly what we need.

The reason `require` might feel like both an object and a function is because JavaScript allows functions to have properties. You can test this yourself

```js
function my_fun() { console.log('hi'); }
my_fun.hey = 'there';

console.log(my_fun.hey); // there
my_fun(); // 'hi'
```

# 1.1 Adding a new helper to create log directory

Create a new file called `helpers.js` inside the `lib/utils` folder.

```js
// file: lib/utils/helpers.js

const fs_sync = require('node:fs');
const path = require('path')

/**
 * @returns {fs_sync.PathLike} The path to the directory.
 */
function check_and_create_dir(path_to_dir) {
    const log_dir = path.resolve(require.main.path, path_to_dir);
    if (!fs_sync.existsSync(log_dir)) {
        fs_sync.mkdirSync(log_dir, { recursive: true });
    }

    return log_dir
}

module.exports = {
    check_and_create_dir
}
```

Let's go through the `check_and_create_dir` function's code line by line.

1. The `path.resolve()` function creates an absolute path by combining a sequence of paths or path segments.
   
   It processes the sequence from right to left, with each subsequent path added to the beginning until an absolute path is created. For example, if the path segments are `/foo`, `/bar`, and `baz`, calling `path.resolve('/foo', '/bar', 'baz')` would return `/bar/baz` because `'/bar' + '/' + 'baz'` creates an absolute path, but `'baz'` does not.
   
   If, after processing all the segments in the given path, an absolute path hasn't been created yet, then the current working directory is used instead.

2. The `require.main.path` holds the absolute path to the directory of the entry point. The entry point is the `test.js` in our case. Or whatever file you specify while running `node file_name.js` command.

3. The `fs_sync.existsSync` function is checking if a given directory path exists in the file system. It's using the `log_dir` path (the result of path resolution) to see if the directory is already there.

4. If the directory doesn't exist, the `fs_sync.mkdirSync` function comes into play. It's used to create directories. The `{ recursive: true }` option ensures that not only the final directory is created, but also any necessary intermediate directories along the path. In case we wish to specify a log directory as `logs/new/today`

5. At the end, function returns the `log_dir`, which is the full path to the directory. Whether the directory existed before or was just created, this path is what you get as a result from the function.

# 1.1 Updating the `init` method

```js
    // file: lib/logger.js

    class Logger {
        ...
        async init() {
            const log_dir_path = check_and_create_dir("logs")

            const file_name = this.#config.file_prefix + new Date().toISOString().replace(/\..+/, "") + ".log";

            // Open/Create the file at the specified log directory and
            // save the file handle to the `#log_file_handle` for further
            // use
            this.#log_file_handle = await fs.open(path.join(log_dir_path, file_name), "a+");
        }
        ...
    }
```

# 1.1 Completing the `log` method

The current version of our `Logger.log` method looks like this:

```js
# 1log(message, log_level) {
    if (log_level < this.#config.level) {
        return;
    }

    console.log('%s: %s', message, LogLevel.to_string(log_level))
}
```

Instead of logging to the standard output, let's write it to the log file that is created when the `Logger` is initialized.

```js
// file: lib/logger.js

class Logger {
    ...
    async #log(message, log_level) {
        // dont' write to the file if 
        //     1. The `log_level` argument is less than the `#config.level` value
        //     2. If the `fd` (file descriptor) is either 0 or -1, which means the file
        //        descriptor is closed or not opened yet.
        if (log_level < this.#config.level || !this.#log_file_handle.fd) {
            return;
        }

        // write logs to the opened file
        await this.#log_file_handle.write(log_message);
    }
    ...
}
```

In the `test.js` file

```js
// file: test.js

const path = require("node:path");
const { Logger, LogConfig } = require("./index");

async function initialize_logger() {
    let logger = Logger.with_config(LogConfig.from_file(path.join(__dirname, "config.json")))
    await logger.init();

    return logger;
}
async function main() {
    let logger = await initialize_logger();
    logger.error('This is an error message');
}

main();
```

In `test.js` we create a new `async` function `initialize_logger` that creates a logger, `init`ializes it and returns it.

We call the `logger.error()` method to print the log to the file.

Let's run the code

```bash
$ npm start
```

A new log file will be created inside the `logs` directory. Open and see the contents -

```
This is an error message
```

Perfect! Everything seems to be working now. The logs are being saved, and we can be proud of it! But wait, the logs aren't useful. We don't know what are those logs, when did we write them and what function called the `logger.error()` method?

We'll take care of all this in the next chapter.

> The code for the entire chapter can be found [at the code/chapter_04.2 directory](/code/chapter_04.2/)

[![Read Next](/assets/imgs/next.png)](/chapters/ch04.3-capturing-metadata.md)

![](https://uddrapi.com/api/img?page=ch04.2)
[![Read Prev](/assets/imgs/prev.png)](/chapters/ch04.2-writing-logs.md)

# 1 Capturing metadata

> The code for the entire chapter can be found [at the code/chapter_04.3 directory](/code/chapter_04.3/)

The Logger is writing to the file as expected, but there is an issue: it is not capturing any useful metadata. Here is an example of what our logs currently look like:

```
Hello this is a log of type ERROR
```

This is no different than using the standard console.log method. For example, we could write:

```js
const logger = /* init logger */

function create_subscription() {
    const my_log_message = 'This is an error'
    logger.error(`[Error]: create_subscription() Line No. 69 ${my_log_message}`);       
}
```

This code appears to work. However, if you add more functionality above this function in the same file, the line number will change again and again, which is not ideal.

For a logging library, it's important to minimize the amount of unnecessary code that clients need to type out. We should do the heavy lifting for our clients, so they only need to call our logger.error (or other similar) method.

This will make our library easier to use and more user-friendly.

```bash
function my_deeply_nested_api_route() {
    logger.error('my error')
    logger.warn('my warning')
}
```

The output of the above code should look like this:

```bash
[2023-08-19T15:10:37.097Z] [ERROR]: my_deeply_nested_api_route (/Users/ishtmeet/Code/logtar/test.js:12) my error
[2023-08-19T15:10:37.097Z] [ERROR]: my_deeply_nested_api_route /Users/ishtmeet/Code/logtar/test.js:13 my warning
```

How cool would it be to achieve this functionality with a little bit of Javascript hack? 

Before we dive into the details of how we can extract the function name or its line number, let's take a small break to understand what a call stack is. But before we understand what a call stack is, we need to understand what a stack is.

# 1.1 What is a Stack?

A stack is a data structure that is widely used in in programming. It is designed to store a collection of elements and is based on the [Last In First Out (LIFO) principle](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)#:~:text=The%20order%20in%20which%20an%20element%20added%20to%20or%20removed%20from%20a%20stack%20is%20described%20as%20last%20in%2C%20first%20out%2C%20referred%20to%20by%20the%20acronym%20LIFO.%5Bnb%201%5D). This means that the most recently added element to the stack is the first one to be removed.

Stacks are used in a variety of applications, from handling function calls to undoing/redoing actions in software applications. Stacks can also be implemented in different ways, such as using arrays or linked lists.

# 1.1 Examples of Stacks

Stacks are a common occurrence in everyday life, and here are some examples:

- A stack of books
- A stack of files
- A stack of pizzas

In each of these cases, the most recent item is placed on top of the stack while the oldest is located at the bottom. For example, to access the pizza at the bottom, you will need to remove all the pizzas above it in the stack.

# 1.1 The Call Stack

A call stack is a special type of stack that keeps track of function calls. Whenever a function is called, its information is pushed onto the call stack. When the function returns, its information is popped off the stack.

Here is an example of a call stack in JavaScript:

```jsx
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

function calculate() {
  const x = 10;
  const y = 5;

  const result1 = add(x, y);
  const result2 = multiply(add(1, 2), result1);

  return result2;
}

calculate();
```

In the example provided, the `calculate` function calls two other functions (`add` and `multiply`). Each time a function is called, its information is added to the call stack. When a function returns, its information is removed from the stack.

To further illustrate this, consider the following graphic:

![call stack](/assets/imgs/call-stack.png)

When the `calculate` function is called, its information is added to the top of the stack. The function then calls the `add` function, which adds its information to the top of the stack. The `add` function then returns, and its information is removed from the stack. The `multiply` function is then called, and its information is added to the top of the stack. 

One important thing to note is, when multiply is called, the first argument is a call to `add(1, 2)`. That means we need to push the `add(..)` back to the top, above `multiply` in the call stack. When the `add` finishes executing, it's  removed from the top.

This process continues, with each function call adding its information to the top of the stack and returning, with its information being removed from the stack.

This call stack is important because it allows the program to keep track of where it is in the execution of a program. If a function call is not properly removed from the stack, it can cause a stack overflow error, which can crash the program.

> In compiled languages like C++ or Rust, the compiler is smart enough to inline the `add()` function above, and directly place the contents of the `add` function in the place of the function call. This can result in performance improvements by eliminating the overhead of a function call and return.

# 1.1 Getting the stack info

How do you even see the stack? Well, it's not that hard. When you `throw` an error, that error contains the information about the stack.

```js
function main() { child(); }

function child() { grand_child(); }

function grand_child() { 
    throw new Error('This is an error from grand_child()');
}

main()
```

If you run this file using `node your_file_name.js` it outputs this on the console

```bash
Error: This is an error from grand_child()
    at grand_child (/Users/ishtmeet/Code/logtard/test.js:10:11)
    at child (/Users/ishtmeet/Code/logtard/test.js:6:5)
    at main (/Users/ishtmeet/Code/logtard/test.js:2:5)
    at Object.<anonymous> (/Users/ishtmeet/Code/logtard/test.js:12:1)
    at Module._compile (node:internal/modules/cjs/loader:1198:14)
    ...
```

We only need to care about the top 5 lines. Rest of those are Node.js's internal mechanism of running and executing the files. Let's go through the error above line by line:

- The first line includes the error message `"This is an error from grand_child()"`, which is the custom message we provided when we used the `throw` statement in the `grand_child()` function.

- The second line of the stack trace indicates that the error originated in the `grand_child()` function. The error occurred at line 10, column 11 of the `test.js` file. This is where the `throw` statement is located within the `grand_child()` function. Therefore, `grand_child` was at the top of the stack when the error was encountered.

- The third line shows that the error occurred at line 6, column 5 of the `test.js` file. This line pinpoints where the `grand_child()` function is called within the `child()` function. That means, `child()` was the second top function on the call stack, below `grand_child()`.

- The fourth lines tells us that the `child()` function was called from within the `main()` function. The error occurred at line 2, column 5 of the `test.js` file. 

- The line 5th tells that the `main()` function was called from the top-level of the script. This anonymous part of the trace indicates where the script execution starts. The error occurred at line 12, column 1 of the `test.js` file. This is where the `main()` function is called directly from the script.

This is called a stack trace. The throw new Error() statement prints the entire stack trace, which unwinds back through the series of function calls that were made leading up to the point where the error occurred. Each function call is recorded in reverse order, starting from the function that directly caused the error and going back to the initial entry point of the script. 

This trace of function calls, along with their corresponding file paths and line numbers, provides developers with a clear trail to follow. It aids in identifying where and how the error originated.

This is exactly what we want to know where was the `logger.error` and the other methods are being called from.

# 1.1 Getting the `callee` name and the line number

How can we use the information above to obtain the line number from the client's code? Can you think about it?

Let's add the following in our `#log` method of the `Logger` class:

```js
// file: lib/logger.js

class Logger {
    ...
    
    async #log(message, log_level) {
        if (log_level < this.#config.level || !this.#log.file_handle.fd) {
            return;
        }
        
        /* New code inserted */
        let stack_trace;
        try {
            throw new Error();
        } catch(error) {
            stack_trace = error.stack;
        }
        console.log(stack_trace)
        /* New code ends */
        
        await this.#log_file_handle.write(log_message);
    }

    ...
}
```

Try to execute the `test.js` file. 

```js
// file: test.js

const {Logger} = require('./index')

async function initialize() {
    const logger = Logger.with_defaults();
    await logger.init();
    return logger;
}

async function main() {
    let logger = await initialize()
    logger.critical('Testing')
}

main()


```

This outputs

```js
Error
    at Logger.#log (/Users/ishtmeet/Code/logtard/lib/logger.js:98:19)
    at Logger.critical (/Users/ishtmeet/Code/logtard/lib/logger.js:141:18)
    at main (/Users/ishtmeet/Code/logtard/test.js:12:12)
```

Awesome. We now know who invoked the function. There are 4 lines. The important piece is on the last line.

```bash
at main (/Users/ishtmeet/Code/logtard/test.js:12:12)
```

This is where we called `logger.critical('Testing')`. However, you may think - "Yeah fine, it's always the last line". No, it is not. Let's add two nested functions in `test.js`

```js
// file: test.js

...

async function main() {
    let logger = await initialize()
    nested_func(logger)
}

function nested_func(logger) {
    super_nested(logger)
}

function super_nested(logger) {
    logger.critical('Testing')
}

...
```

After executing `node test.js` we get the following output.

```bash
Error
    at Logger.#log (/Users/ishtmeet/Code/logtard/lib/logger.js:98:19)
    at Logger.critical (/Users/ishtmeet/Code/logtard/lib/logger.js:141:18)
    at super_nested (/Users/ishtmeet/Code/logtard/test.js:20:12)
    at nested_func (/Users/ishtmeet/Code/logtard/test.js:16:5)
    at main (/Users/ishtmeet/Code/logtard/test.js:12:5)
```

However, this time, there are two more lines. If you read the stack trace from the bottom to the top, you can understand the sequence of steps that led to the error. The most useful information is not actually at the very bottom, but rather on the fourth line (including the "Error" line at the top of the stack trace).

```
at super_nested (/Users/ishtmeet/Code/logtard/test.js:20:12)
```

The 4th line will always be the one that invoked the method. Which is the line directly below the call to `Logger.critical`. Isn't that what we need?

# 1.1 A more ergonomic way

Looking at the code we just wrote in our `Logger.#log` method, it seems poorly written, making it hard to understand our desired outcome. Additionally, why are we throwing an Error? Someone unfamiliar with our code might consider it redundant and remove it.

We can make it even better by creating a helper method that extracts essential information from our stack trace.

Add a new function in the `lib/utils/helpers.js` file

```js
// file: lib/utils/helpers.js

function get_caller_info() {

}

module.exports = {
    check_and_create_dir,
    get_log_caller // Add this!
}
```

We are going to get our stack trace in a shorter and more efficient way. Here's what we're going to do

```js
// file: lib/util/helpers.js

function get_caller_info() {
    const error = {};
    Error.captureStackTrace(error);

    const caller_frame = error.stack.split("\n")[4];

    const meta_data = caller_frame.split("at ").pop();
    return meta_data
}
```

`Error.captureStackTrace(targetObject)` is a static method on the Error class that's used to customize or enhance the creation of stack traces when throwing errors. It doesn't throw an error itself, but rather it modifies the target object to include a custom stack trace.

It's designed specifically for capturing stack traces without generating a full error object, which can be helpful when you want to create your own custom error objects and still capture the stack trace efficiently. It directly associates the stack trace with the provided object.

```js
const caller_frame = error.stack.split("\n")[4];
```

We are extracting the 5th line of the stack trace. Note, it's the 5th line and not 4th like we talked in the previous section. This is because, we introduce one more function `get_log_caller`, that will also live on the call stack. You can imagine a call stack like this:

```
get_caller_info
Logger.#log
Logger.critical/Logger.debug etc
user_function // that called `logger.critical`
```

On the top of the stack trace there's a line that says

```
Error
```

So the entire stack trace can be imagined like this:

```
Error
    at get_caller_info line:number
    at Logger.#log line:number
    at Logger.critical line:number
    at user_function line:number // that called `logger.critical`
```

Right, we only care about the `5th` line.

```javascript
const meta_data = caller_frame.split("at ").pop();
```

In this line, we are retrieving the part of the string that follows the word "at" followed by a space, as we do not need to include it in our output. Finally, on the last line, we return the necessary metadata to display in the logs.

# 1.1 Using the `get_caller_info` function

Update the code in the `Logger` class to use the info provided by the `get_caller_info` function.

```js
// file: lib/logger.js

class Logger {
    ...    
    async #log(message, log_level) {
        if (log_level < this.#config.level || !this.#log_file_handle.fd) {
            return;
        }

        const date_iso = new Date().toISOString();
        const log_level_string = LogLevel.to_string(log_level)

        // add additional info to the log messages.
        const log_message = `[${date_iso}] [${log_level_string}]: ${get_caller_info()} ${message}\n`;
        await this.#log_file_handle.write(log_message);
    }
    ...
}
```

Now, we're going to test whether everything works like we expect?

In the `test.js` file let's write a couple of logs.

```js
// file: test.js

const {Logger} = require('./index')

async function initialize() { ... }

async function main() {
    let logger = await initialize()
    logger.critical('From the main() function')
    nested_func(logger)
}

function nested_func(logger) {
    logger.critical('From the nested_func() function')
    super_nested(logger)
}

function super_nested(logger) {
    logger.critical('From the super_nested() function')
}

main()


```

The log file shows the following -

```bash
[2023-08-19T19:11:51.888Z] [CRITICAL]: main (/Users/ihtmeet/Code/logtard/test.js:11:12) From the main() function
[2023-08-19T19:11:51.888Z] [CRITICAL]: nested_func (/Users/ishtmeet/Code/logtard/test.js:16:12) From the nested_func() function
[2023-08-19T19:11:51.888Z] [CRITICAL]: super_nested (/Users/ishtmeet/Code/logtard/test.js:21:12) From the super_nested() function


```

This all seems to work pretty well. We now have helpful logs. However, before we start using this logging library in our personal projects, there are a lot of things that need to be taken care of. This includes logging crashes, handling SIGINT and SIGTERM signals, as well as properly utilizing the file_handle.

We'll take care of this in the next chapter.

> The code for the entire chapter can be found [at the code/chapter_04.3 directory](/code/chapter_04.3/)

![](https://uddrapi.com/api/img?page=ch04.3)

