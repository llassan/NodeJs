# NodeJs

## What is process in node.js?

In Node.js, the `process` is a global object that provides information about, and control over, the current Node.js process. It is automatically available, meaning you don't need to require it in your files.

*Key Concepts of process in Node.js:*
 - Represents the currently running Node.js instance.
 - Can be used to access environment variables, command-line arguments, process information, and control process exit.

### Commonly Used Features of process

You can access command-line arguments using:

- process.argv is an array where:

  - process.argv[0] is the path to the Node.js executable.
  - process.argv[1] is the path to the executing file.
  - From process.argv[2] onwards are the additional arguments passed.

### Environment Variables
Access environment variables like:
```bash
console.log(process.env.NODE_ENV);
```
You can set them like this:
```bash
NODE_ENV=production node app.js
```
### Exiting the Process
You can terminate a Node.js process:
```bash
process.exit(0); // Success exit
process.exit(1); // Failure exit
```
###  Current Working Directory
```bash
console.log(process.cwd());
```
### Memory Usage
```bash
console.log(process.memoryUsage());
```
### Process Events
```bash
process.on('exit', (code) => {
  console.log(`Process is exiting with code: ${code}`);
});
```
### Standard Input/Output
```bash
process.stdout.write('Hello, World!\n');

process.stdin.on('data', (data) => {
  console.log(`You typed: ${data}`);
});
```
## What Are SIGINT and SIGTERM?

These are process signals sent to a Node.js application by the operating system (or manually by the user) to control the process lifecycle.

`SIGINT` (Signal Interrupt)
- Typically sent when you press Ctrl + C in the terminal.
- Used to gracefully stop a running process.
```bash
process.on('SIGINT', () => {
  logger.info('Shutting down Detection Middleware...');
  process.exit(0);
});
```
`SIGTERM` (Signal Terminate)
- A termination signal that politely asks a process to stop.
- Sent by system tools (like kill <pid>, Docker, Kubernetes) to stop processes.
```bash
process.on('SIGTERM', () => {
  logger.info('Service terminated.');
  process.exit(0);
});
```

## Node.js Standard Exit Code Guide
| Exit Code | Meaning                          | When to Use                                 |
| --------- | -------------------------------- | ------------------------------------------- |
| `0`       | Success                          | Normal, successful completion               |
| `1`       | Uncaught Fatal Exception         | Unexpected error, generic failure           |
| `2`       | Misuse of Shell Builtins         | Incorrect usage of CLI or invalid arguments |
| `3`-`125` | Custom Application Errors        | App-specific exit codes (e.g. DB failure)   |
| `126`     | Command invoked cannot execute   | File permission errors                      |
| `127`     | Command not found                | CLI command or script missing               |
| `128`     | Invalid Exit Argument            | Improper usage of `process.exit`            |
| `128 + N` | Fatal Signal (e.g. SIGINT = 2)   | Termination via OS signals                  |
| `130`     | Script terminated by Ctrl + C    | SIGINT (128 + 2)                            |
| `137`     | Killed (Out of Memory / SIGKILL) | Common in Docker or memory issues           |
| `255`     | Exit code out of range           | Typically a wrapped negative exit code      |

### Recommended Custom Codes (Optional)

- 10 - Database Connection Failed
- 11 - Configuration Error
- 12 - Third-party Service Unavailable
- 13 - Invalid User Input
- 14 - Authentication Failure

