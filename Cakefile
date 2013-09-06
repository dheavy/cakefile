fs      = require 'fs'
path    = require 'path'
{print} = require 'sys'
{spawn} = require 'child_process'

COFFEE_DIR = 'coffeescript/'
OUTPUT_DIR = './'

readAndBuildRecursively = (dir, watch) ->

  # Get all files/directories and their path from dir param, store them in array
  files = fs.readdirSync dir

  # Process each array member:
  # - if it's a .coffee file, compile to ./ with directory structure similar to original one
  # - ...or dive deeper and recursively if it's itself a directory
  files.forEach(
    (file) ->
      currentFile = path.normalize "#{dir}/#{file}"
      stats = fs.statSync currentFile
      if stats.isFile()
        outputDir =  path.dirname(currentFile.replace(COFFEE_DIR, OUTPUT_DIR))
        cmd = '-c'
        cmd += 'w' if watch is true
        coffee = spawn 'coffee', [cmd, '-o', outputDir, currentFile] unless path.extname(currentFile) isnt '.coffee'
        coffee.stdout.on 'data', (data) -> console?.log data.toString().trim()
        coffee.stderr.on 'data', (data) -> console?.log data.toString().trim()
      else
        readAndBuildRecursively currentFile, watch
  )

build = (watch = false)->
  readAndBuildRecursively(COFFEE_DIR, watch)

task 'build', 'compile coffeescript files during prebuild', -> build()
task 'watch', 'watch directory for changes and build if any', -> build(true)