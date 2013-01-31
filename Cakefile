{exec} = require 'child_process'
fs = require 'fs'

lastChange = {}

scripts = ['lib']

compileCoffee = (file) ->
    exec "coffee -c #{file}", (err, stdout, stderr) ->
        return console.error err if err
        console.log "Compiled #{file}"

watchFile = (file, fn) ->
    try
        fs.watch file, (event, filename) ->
            return if event isnt 'change'
            # ignore repeated event misfires
            fn file if Date.now() - lastChange[file] > 1000
            lastChange[file] = Date.now()
    catch e
        console.log "Error watching #{file}"

watchFiles = (files, fn) ->
    for file in files
        lastChange[file] = 0
        watchFile file, fn
        console.log "Watching #{file}"

task 'build', 'Compile *.coffee', ->
    compileCoffee(f) for f in scripts

task 'watch', 'Compile + watch *.coffee', ->
    watchFiles scripts, compileCoffee
