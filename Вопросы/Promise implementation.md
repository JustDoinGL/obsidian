---
tags:
  - javascript
links:
  - https://walborn.notion.site/Promise-implementation-5238760e5d1841cd95bcc3fcf6717b19
comment: интенсив кобезза
---
```js
class WalbornPromise {
  constructor(callback) {
    this.state = 'pending'
    this.value = undefined
    this.chain = { resolve: [], reject: [] }

    try {
      callback(this.resolve, this.reject)
    } catch (err) {
      this.reject(err)
    }
  }

  resolve = (value) => {
    // To make the processing async wrap it in queueMicrotask
    if (this.state !== 'pending') return
    if (value instanceof WalbornPromise) return value.then(this.resolve, this.reject)

    this.value = value
    this.state = 'fulfilled'

    // We have multiple chain because add them for .finally block too
    this.chain.resolve.forEach(cb => cb(value))
    this.chain.resolve = []
  }

  reject = (value) => {
    if (this.state !== 'pending') return
    if (thenable(value)) return value.then(this.resolve, this.reject)

    this.value = value
    this.state = 'rejected'

    this.chain.reject.forEach(cb => cb(value))
    this.chain.reject = []
  }

  then(onResolve, onReject) {
    return new WalbornPromise((resolve, reject) => {
      this.chain.resolve.push((value) => {
        if (!onResolve) return resolve(value)
        try {
          return resolve(onResolve(value))
        } catch (err) {
          return reject(err)
        }
      })
      if (this.state === 'fulfilled') {
        this.chain.resolve.forEach(cb => cb(this.value))
        this.chain.resolve = []
      }

      this.chain.reject.push((value) => {
        if (!onReject) return reject(value)
        try {
          return resolve(onReject(value))
        } catch (err) {
          return reject(err)
        }
      })

      if (this.state === 'rejected') {
        this.chain.reject.forEach(cb => cb(this.value))
        this.chain.reject = []
      }
    })
  }

  catch(onReject) {
    return this.then(null, onReject)
  }

  // Finally block returns a promise which fails or succeedes with the previous promise resove value
  finally(callback) {
    return new WalbornPromise((resolve, reject) => {
      let value
      let rejected = false
      this
        .then(
          (res) => {
            rejected = false
            value = res
            return callback()
          },
          (err) => {
            rejected = true
            value = err
            return callback()
          })
        // If the callback didn't have any error we resolve/reject the promise based on promise state
        .then(() => rejected ? reject(value) : resolve(value))
    })
  }
}

const testPromiseWithLateResolve = new WalbornPromise((resolve) => {
  setTimeout(() => resolve('Promise 1 has been resolved'), 1000)
})

testPromiseWithLateResolve
  .then((value) => {
    console.log(value)
    return 'next'
  })
  .then((value) => {
    console.log(value)
  })

const testPromiseWithLateReject = new WalbornPromise((resolve, reject) => {
  setTimeout(() => {
    reject('Promise 2 has been rejected')
  }, 1000)
})

testPromiseWithLateReject
  .then((value) => {
    console.log(value)
  }).catch((err) => {
    console.log(err)
  })

const testPromiseWithRejectFinally = new WalbornPromise((resolve, reject) => {
  setTimeout(() => {
    reject('Promise 3 has been rejected')
  }, 1000)
})

testPromiseWithRejectFinally
  .finally(() => {
    console.log('finally called')
  })
  .catch((err) => {
    console.log('value rejected after finally', err)
  })



const testPromiseImmediatelyResolve = new WalbornPromise((resolve) => {
  resolve('Promise 4 has been resolved immediately')
})

testPromiseImmediatelyResolve.then((value) => {
  console.log(value)
})

setTimeout(() => {
  testPromiseImmediatelyResolve
    .then((value) => {
      console.log(value)
    })
}, 1000)
```