This appscript library keeps application 'kinda' nodejs-portable

## Usage

```
var app = new Gexpress.App()

app.get('/ping',function(req,res){
  res.set('content-type','application/json')
  res.send( {pong:true} )
  res.end()
})

app.get('/js',function(req,res){
  res.set('content-type','application/javascript')
  res.send( 'console.log("hello world")' )
  res.end()
})

app.get( /.*/, function(req,res){ // default to homepage
  Logger.log("defaulting to homepage")
  var html = HtmlService.createTemplateFromFile('index') // this will get the index.html-file from your appscript project
  html.title = 'Hello'
  res.set('content-type','text/html')
  res.send( html.evaluate().setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL).getContent() )
  res.end()
})

function doGet(e) {
  return app.doGet(e)
}

function doPost(e){
  return app.doPost(e)
}
```

> As you can see it's not 100% express-compatible, since appscript only supports synchronous requests. Using next() is not really supported.
