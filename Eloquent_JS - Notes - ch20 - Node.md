# CHAPTER 20 - Node

## Background

TODO: type

## The node command

1. Run a JS file from node CLI

   ```JS
   $ node hello.js
   ```

2. Type JS in a cmd line shell

   ```JS
   $ node
   ```

3. Exit the cmd line shell -

   ```JS
   $ process.exit(0)
   ```

4. Show possible command line arguments for your script

   ```JS
   $ node showargv.js one --and two
   ["node", "/tmp/showargv.js", "one", "--and", "two"]
   ```

> Note: All the standard JavaScript global bindings, such as _Array, Math, and JSON_,
> are also present in Node’s environment. Browser-related functionality, such as
> _document_ or _prompt_, is not

## Modules

1.  Node comes with builtin commonJS module system

2.  Small App

    1.  Create _main.js_

        ```JS
        const {reverse} = require("./reverse");
        // Index 2 holds the first actual command line argument
        let argument = process.argv[2];
        console.log(reverse(argument));
        ```

    2.  _reverse.js_ contains

        ```JS
        exports.reverse = function(string) {
          return Array.from(string).reverse().join("");
        };
        ```

    3.  As Node uses commonJS modules we can call our tool like:

        ```JS
        $ node main.js JavaScript
        tpircSavaJ
        ```

## Installing with NPM

TODO: ask

## Package files

- For each Project, it is recommended to create such a file(_package.json_), either manually or by running `$ npm init`.

## Versions

1. NPM packages follow a schema called [_semantic versioning_](https://semver.org/).
2. A semantic version consists of three numbers, separated by periods, such as _2.3.0_
3. _Middle Number_: For each added _New Functionality_ - _increment the middle number_.
4. _Starting Number_: Every time compatibility is broken, so that existing code that uses the package might not work with the new version, the first number has to be incremented.
5. A _caret character (^)_ in front of the version number for a dependency in package.json indicates that any version compatible with the given number may be installed. So, for example, "^2.3.0" would mean that any version greater than or equal to 2.3.0 and less than 3.0.0 is allowed.

## The file system module

1.  _fs_ module exports functions for working with files and
    directories.

2.  For example, the function called [_readFile_](https://nodejs.org/api/fs.html#fs_fspromises_readfile_path_options) reads a file and then calls a callback with the file’s contents:

3.  The second argument(optional) to readFile indicates the character encoding used to decode the file into a string. When given:

    1. `"utf-8"`: reads a text file and returns a _`string`_.

       ```JS
       let {readFile} = require("fs");
       readFile("file.txt", "utf8", (error, text) => {
         if (error) throw error;
         console.log("The file contains:", text);
       });
       ```

    2. _second argument empty_: Node will assume you are interested
       in the _binary data_ and will give you a _`Buffer object`_ instead of a string. This is an array-like object that contains numbers representing the bytes (8-bit chunks of data) in the files.

       ```JS
       const {readFile} = require("fs");
       readFile("file.txt", (error, buffer) => {
         if (error) throw error;
         console.log("The file contained", buffer.length, "bytes.", "The first byte is:", buffer[0]);
       });
       ```

4.  A similar function, [writeFile](https://nodejs.org/api/fs.html#fs_filehandle_writefile_data_options), is used to write a file to disk.

    ```JS
    const {writeFile} = require("fs");
    writeFile("graffiti.txt", "Node was here", err => {
      if (err) console.log(`Failed to write file: ${err}`);
      else console.log("File written.");
    });
    // by default writes utf-8 string in a file
    ```

5.  _fs_ module also exports a promise object

    ```JS
    const {readFile} = require("fs").promises;
    readFile("file.txt", "utf8")
      .then(text => console.log("The file contains:", text));
    ```

6.  the synchronous version of readFile function

    ```JS
    const {readFileSync} = require("fs");
    console.log("The file contains:",
                readFileSync("file.txt", "utf8"));

    /*
    note: such synchronous call stop all operations after it;
          which might result in annoying delays to the end user
          on such server.
    */
    ```

## The HTTP module

1. It provides functionality for running
   HTTP servers and making HTTP requests

2. This is all it takes to start an HTTP server:

```JS
  const {createServer} = require("http");
  let server = createServer((request, response) => {
    response.writeHead(200, {"Content-Type": "text/html"});
    response.write(`
      <h1>Hello!</h1>
      <p>You asked for <code>${request.url}</code></p>`);
      response.end();
  });
  server.listen(8000);
  console.log("Listening! (port 8000)");
```

3. Visiting the url http://localhost:8000/hello after running the above script, results into a small HTML page as a response from the server.

4.
