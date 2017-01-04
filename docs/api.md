# API

When executing commands against Git, there are additional optional parameters
which are available through the `IGitExecutionOptions` interface:

 - `env`: a collection of key-value pairs which are assigned to the created
    process
 - `stdin`: a string or `Buffer` which is written to the created process
 - `stdinEncoding`: if you need to set the encoding of the input
 - `processCallback`: a callback to inject additional code which will be invoked
    after the child process is spawned

Example code:

```ts
const path = 'C:/path/to/repo/'

const options: IGitExecutionOptions = {
  // enable diagnostics
  env: {
    'GIT_HTTP_USER_AGENT': 'git-kitchen-sink/2.10.0',
    'GIT_TRACE': '1',
    'GIT_CURL_VERBOSE': '1'
  },
  processCallback: (process: ChildProcess) => {
    byline(process.stderr).on('data', (chunk: string) => {
      // read line from progress and convert to percentage
    })
  }
}

const result = await GitProcess.exec([ 'pull', 'origin' ], path, options)
```