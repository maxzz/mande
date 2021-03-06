# mande [![Build Status](https://badgen.net/circleci/github/posva/mande/master)](https://circleci.com/gh/posva/mande) [![npm package](https://badgen.net/npm/v/mande)](https://www.npmjs.com/package/mande) [![coverage](https://badgen.net/codecov/c/github/posva/mande/master)](https://codecov.io/github/posva/mande) [![thanks](https://badgen.net/badge/thanks/♥/pink)](https://github.com/posva/thanks)

> Simple, light and easy to use wrapper around fetch

**Requires [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) support.**

_mande_ has better defaults to communicate with APIs using `fetch`, so instead of writing:

```js
// creating a new user
fetch('/api/users', {
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'Dio',
    password: 'irejectmyhumanityjojo',
  }),
})
  .then((response) => {
    if (response.status >= 200 && response.status < 300) {
      return response.json()
    }
    // reject if the response is not 2xx
    throw new Error(response.statusText)
  })
  .then((user) => {
    // ...
  })
```

You can write:

```js
const users = mande('/api/users')

users
  .post({
    name: 'Dio',
    password: 'irejectmyhumanityjojo',
  })
  .then((user) => {
    // ...
  })
```

## Installation

```sh
npm install mande
yarn add mande
```

## Usage

Creating a small layer to communicate to your API:

```js
// api/users
import { mande } from 'mande'

const users = mande('/api/users', globalOptions)

export function getUserById(id) {
  return users.get(id)
}

export function createUser(userData) {
  return users.post(userData)
}
```

Adding _Authorization_ tokens:

```js
// api/users
import { mande } from 'mande'

const todos = mande('/api/todos', globalOptions)

export function setToken(token) {
  // todos.options will be used for all requests
  todos.options.headers.Authorization = 'Bearer ' + token
}

export function clearToken() {
  delete todos.options.headers.Authorization
}

export function createTodo(todoData) {
  return todo.post(todoData)
}
```

```js
// In a different file, setting the token whenever the login status changes. This depends on your frontend code, for instance, some libraries like Firebase provide this kind of callback but you could use a watcher on Vue.
onAuthChange((user) => {
  if (user) setToken(user.token)
  else clearToken()
})
```

You can also globally add default options to all _mande_ instances:

```js
import { defaults } from 'mande'

defaults.headers.Authorization = 'Bearer token'
```

## API

Most of the code can be discovered through the autocompletion but the API documentation is available at https://posva.net/mande/.

## Related

- [fetchival](https://github.com/typicode/fetchival): part of the code was borrowed from it and the api is very similar
- [axios](https://github.com/axios/axios):

## License

[MIT](http://opensource.org/licenses/MIT)
