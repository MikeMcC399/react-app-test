https://github.com/jeffbski/wait-on/issues/137

# wait-on failure for localhost of react-app Node.js 18

## Environment

ubuntu-22.04 on GitHub
Node.js 18.14.1

wait-on 7.0.1

## Problem description

wait-on fails waiting for http://localhost:3000 on GitHub in an `ubuntu-22.04` runner using the current default Node.js 18.14.1. The problem does not happen using Node.js 16.19.1.

Node.js 18 is the default version on GitHub runners since Feb 18, 2023.

verbose logs show:

```text
Run npx wait-on http://localhost:3000 -t 15000 -v
  npx wait-on http://localhost:3000 -t 15000 -v
  shell: /usr/bin/bash -e {0}
waiting for 1 resources: http://localhost:3000
making HTTP(S) head request to  url:http://localhost:3000 ...
  HTTP(S) error for http://localhost:3000 Error: connect ECONNREFUSED ::1:3000
  .
  repeated multiple times
  .
  .
Error: Timed out waiting for: http://localhost:3000
making HTTP(S) head request to  url:http://localhost:3000 ...
    at /home/runner/work/react-app-test/react-app-test/node_modules/wait-on/lib/wait-on.js:132:31
  HTTP(S) error for http://localhost:3000 Error: connect ECONNREFUSED ::1:3000
    at doInnerSub (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/operators/mergeInternals.js:22:31)
    at outerNext (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/operators/mergeInternals.js:17:70)
    at OperatorSubscriber._this._next (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/operators/OperatorSubscriber.js:33:21)
    at Subscriber.next (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/Subscriber.js:51:18)
making HTTP(S) head request to  url:http://localhost:3000 ...
    at AsyncAction.work (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/observable/timer.js:28:28)
  HTTP(S) error for http://localhost:3000 Error: connect ECONNREFUSED ::1:3000
    at AsyncAction._execute (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/scheduler/AsyncAction.js:79:18)
wait-on(1851) Timed out waiting for: http://localhost:3000; exiting with error
    at AsyncAction.execute (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/scheduler/AsyncAction.js:67:26)
    at AsyncScheduler.flush (/home/runner/work/react-app-test/react-app-test/node_modules/rxjs/dist/cjs/internal/scheduler/AsyncScheduler.js:38:33)
    at listOnTimeout (node:internal/timers:569:17)
Error: Process completed with exit code 1.
```

netstat shows the following listener:

```text
Run netstat -lnt | grep 3000
tcp        0      0 0.0.0.0:3000            0.0.0.0:*               LISTEN
```

## Steps to reproduce

### GitHub

View workflow log https://github.com/MikeMcC399/react-app-test/actions/workflows/test-react-app-wait-on.yml or

fork https://github.com/MikeMcC399/react-app-test and run
[.github/workflows/test-react-app-wait-on.yml](https://github.com/MikeMcC399/react-app-test/blob/master/.github/workflows/test-react-app-wait-on.yml)

## Workaround

wait for http://127.0.0.1:3000 instead of http://localhost:3000

## Related issues

- https://github.com/jeffbski/wait-on/issues/133
- https://github.com/jeffbski/wait-on/issues/109
- https://github.com/bahmutov/start-server-and-test/issues/358
- https://github.com/cypress-io/github-action/issues/802
