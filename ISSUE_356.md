https://github.com/bahmutov/start-server-and-test/issues/356

This is a follow-on from the issue https://github.com/bahmutov/start-server-and-test/issues/348 "Next.js server start hangs".

## Environment

```text
$ npm ls
react-app-test@0.1.0
├── @testing-library/jest-dom@5.16.5
├── @testing-library/react@13.4.0
├── @testing-library/user-event@13.5.0
├── cypress@12.6.0
├── react-dom@18.2.0
├── react-scripts@5.0.1
├── react@18.2.0
├── start-server-and-test@1.15.4
└── web-vitals@2.1.4
```

Node.js 18.14.x
Windows 11 local
ubuntu-22.04 on GitHub

## Problem description

start-server-and-test fails waiting for http://localhost:3000 locally on Windows 11 with Node.js 18.14.2 and on GitHub in an `ubuntu-22.04` runner using the current default Node.js 18.14.1.

Debug logs show

```text
making HTTP(S) head request to  url:http://localhost:3000 ...
  HTTP(S) error for http://localhost:3000 Error: connect ECONNREFUSED ::1:3000
```

So the IPv6 address ( `::1`) is being tested whilst the server is only listening on the IPv4 address (`127.0.0.1`).

## Steps to reproduce

### Windows 11 local

Clone https://github.com/MikeMcC399/react-app-test

```bash
nvm use 18.14.2
npm ci
export DEBUG=start-server-and-test
npm run test:cypress
```

test hangs

### GitHub

View workflow log https://github.com/MikeMcC399/react-app-test/actions/workflows/test-react-app.yml or
fork https://github.com/MikeMcC399/react-app-test and run
[.github/workflows/test-react-app.yml](https://github.com/MikeMcC399/react-app-test/blob/master/.github/workflows/test-react-app.yml)

## Workaround

wait for http://127.0.0.1:3000 instead of http://localhost:3000

On Windows 11

```bash
npm run test:cypress:workaround
```

## Related issues

- https://github.com/cypress-io/github-action/issues/802
