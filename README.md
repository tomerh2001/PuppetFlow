# PuppetFlow
[![semantic-release: angular](https://img.shields.io/badge/semantic--release-angular-e10079?logo=semantic-release)](https://github.com/semantic-release/semantic-release)
[![XO code style](https://shields.io/badge/code_style-5ed9c7?logo=xo&labelColor=gray)](https://github.com/xojs/xo)
[![CodeQL](https://github.com/tomerh2001/PuppetFlow/actions/workflows/github-code-scanning/codeql/badge.svg)](https://github.com/tomerh2001/PuppetFlow/actions/workflows/github-code-scanning/codeql)

An end-to-end testing framework for orchestrating and validating communication flows across microservices.

## Example
```typescript
import supertest from 'supertest'

import webApp from './web/src/app'
import serverApp from './server/src/app'
import secondServerApp from './second-server/src/app'

const web = supertest(webApp)                      // React App
const server = supertest(serverApp)                // Express App
const secondServer = supertest(secondServerApp)    // Express App

theUrl('localhost:3034').means(server)
theUrl('localhost:3030').means(secondServer)

web.get('/payment/cc').then(() => { 
    expect(server)
        .toBeCalledAt('/charge/cc')
        .withMethod('POST')
        .then(() => {
            expect(secondServer)
                .toBeCalledAt('/charge/finalize/cc')
                .withMethod('POST')
                .andRespondWith(200, {success: true})
        })
})  
```
