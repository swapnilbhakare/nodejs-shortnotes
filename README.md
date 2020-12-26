# nodejs-shortnotes

  
// NPM module 
// const color = require('cli-color')
// console.log(color.green('Hello Node js'))

// Local module
// const auth = require('./auth')

// auth.register('codersGyan')
// auth.login('codersGyan', 'secret')

// Core modules
const path = require('path')


// dirname 
// console.log('Folder name: ', path.dirname(__filename))

// filename
// console.log('File name: ', path.basename(__filename))

// Extension
// console.log('Ext name: ', path.extname(__filename))

// parse 
// console.log('Parse : ', path.parse(__filename))

// Join 
// console.log('Join : ', path.join(__dirname, 'order', 'app.js'))

// File system 
const fs = require('fs')
// Make a directory 
// fs.mkdir(path.join(__dirname, '/test'), (err) => {
//     if(err) {
//         console.log(err)
//         return
//     }
//     console.log('Folder created...')
// })

// Create a file
// fs.writeFile(path.join(__dirname, 'test', 'test.txt'), 'Hello Node\n', (err) => {
//     if(err) {
//         throw err
//     }
//     fs.appendFile(path.join(__dirname, 'test', 'test.txt'), 'More data', (err) => {
//         if(err) {
//             throw err
//         } 
//         console.log('Data added...') 
//     })
//     console.log('File created...')
// })

// Read a file 
fs.readFile(path.join(__dirname, 'test', 'test.txt'), 'utf-8', (err, data) => {
    if(err) {
        throw err
    }
    //  const content = Buffer.from(data)
    // console.log(content.toString())

    console.log(data)
})



















// Os module 
const os = require('os')

// console.log('Os type: ', os.type())

// console.log('Os platform: ', os.platform()) 

// console.log('Cpu architecture: ', os.arch())

// console.log('Cpu details: ', os.cpus())

// console.log('Free memory : ', os.freemem())

// console.log('Total memory : ', os.totalmem())

// console.log('Uptime : ', os.uptime())


// Events module 
const Emitter = require('events')

// const myEmitter = new Emitter()

// myEmitter.on('somename', (data) => {
//     console.log(data)
// })

// myEmitter.emit('somename', {
//     name: 'Rakesh'
// })

// class Auth extends Emitter{
//     register(username) {
//         console.log('Registered successfully...')
//         this.emit('registered', username)
//     }
// }

// const auth = new Auth()

// Listen

// Verify email
// auth.on('registered', (data) =>{
//     console.log(`Sending email to ${data}`)
// })

// welcome email 
// auth.on('registered', (data) =>{
//     console.log(`Sending welcome email to ${data}`)
// })


// auth.register('abc')






// Http module 
const http = require('http')
const fs = require('fs')
const path = require('path')

const app = http.createServer((req, res) => {

    res.writeHead(200, {
        'Content-Type': 'text/html'
    })

    // if(req.url === '/') {
    //     fs.readFile(path.join(__dirname, 'public', 'index.html'), (err, content) => {
    //         if(err) {
    //             throw err
    //         }
    //         res.end(content)
    //     })
    // } else if(req.url === '/about') {
    //     fs.readFile(path.join(__dirname, 'public', 'about.html'), (err, content) => {
    //         if(err) {
    //             throw err
    //         }
    //         res.end(content)
    //     })
    // }

    let filePath = path.join(__dirname, 'public', req.url === '/' ? 'index.html' : req.url) 

    let conteType = 'text/html'

    let ext = path.extname(filePath)
    if(!ext) {
      filePath += '.html' 
    }

    switch(ext) {
        case '.css': 
            contentType = 'text/css'
            break
        case '.js': 
            contentType = 'text/javascript'
            break
        default: 
            contentType = 'text/html' 
    }

    fs.readFile(filePath, (err, content) => {
        if(err) {
            fs.readFile(path.join(__dirname, 'public', 'error.html'), (err, data) => {
                if(err) {
                    res.writeHead(500)
                    res.end('Error!!!')  
                } else {
                    res.writeHead(404, {
                       'Content-Type': contentType 
                    })
                    res.end(data)
                }
                
            })
        } else {
            res.writeHead(200, {
                'Content-Type': contentType 
            })
            res.end(content)
        }
        

    })
    
})
const PORT = process.env.PORT || 3000

app.listen(PORT, () => {
    console.log(`Listening on port ${PORT}`)
})
