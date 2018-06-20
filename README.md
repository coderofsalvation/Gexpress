This appscript library keeps application 'kinda' nodejs-portable

## Usage

```
var app = new Gexpress.App()

app.use(function(req,res,next){
  req.user = Session.getActiveUser().getEmail()
  next()
})

app.get('/ping',function(req,res,next){
  Logger.log(req)
  res.set('content-type','application/json')
  res.send( req )
  res.end()
})

app.get('/js',function(req,res,next){
  res.set('content-type','application/javascript')
  res.send( 'console.log("hello world")' )
  res.end()
})

app.get( /.*/, function(req,res,next){ // default to homepage
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

> NOTE: appscript does not allow async responses, therefore next() can only be used to stop further middleware-execution

## RESTFUL-ish

Webtraffic to Google Appscript Webapps are limited in many ways. 
Why? Because hackers.
This forces Gexpress to expose endpoints in a slightly different, but still convenient way:

| Gexpress method | Listens to webrequest(s) | Anonymous webrequest |
|-|-|-|
| app.get('/foo',..)     | GET  /exec?path=/foo            | yes            |
|                        | GET  /exec/foo                  | triggers login |
|                        | POST /exec?path=/foo&method=GET | yes            |
|                        |       
