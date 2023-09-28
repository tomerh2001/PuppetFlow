# PuppetFlow
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