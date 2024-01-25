### *Using a Hidden Form Field*
In HTML forms, you can use a hidden field to specify the desired HTTP method, and then handle it server-side.
```
<form action="/resource" method="post">   
<input type="hidden" name="_method" value="PUT" />   
<!-- other form fields here --> 
</form>
```

Server-side, you would look for this `_method` field and use its value as the HTTP method for processing the request.
### *Using a Custom HTTP Header*
Some frameworks and libraries support method overriding via custom HTTP headers. For example, you might use the `X-HTTP-Method-Override` header in a POST request to indicate the intended method. `POST /resource HTTP/1.1 X-HTTP-Method-Override: PUT`

Server-side, you would check for this header and use its value as the HTTP method.
### *Using a Query Parameter*
You can include the desired method as a query parameter in the URL, and then handle it server-side. `POST /resource?_method=PUT HTTP/1.1`

Server-side, you would parse the query string and use the `_method` parameter's value as the HTTP method.
### *Using Middle-ware in Frameworks* 
Many web frameworks offer middle-ware to handle method overriding. For example, in Express.js, you can use the `method-override` middle-ware:
```
const methodOverride = require('method-override'); app.use(methodOverride('_method'));
```

This middle-ware will look for the `_method` field in the request (either as a query parameter or in the body) and use its value as the HTTP method.
### *List of methods with associated frameworks*
```
- _method: 'PUT' (Laravel, Rails, Sinatra, Express.js)
- _method: 'DELETE' (Laravel, Rails, Sinatra, Express.js)
- _method: 'PATCH' (Laravel, Rails, Sinatra, Express.js)
- X-HTTP-Method-Override: 'PUT' (Spring, ASP.NET, Google Guice)
- X-HTTP-Method-Override: 'DELETE' (Spring, ASP.NET, Google Guice)
- X-HTTP-Method-Override: 'PATCH' (Spring, ASP.NET, Google Guice)
- X-HTTP-Method: 'PUT' (ASP.NET)
- X-HTTP-Method: 'DELETE' (ASP.NET)
- X-HTTP-Method: 'PATCH' (ASP.NET)
- X-METHOD-OVERRIDE: 'PUT' (Google Guice)
- X-METHOD-OVERRIDE: 'DELETE' (Google Guice)
- X-METHOD-OVERRIDE: 'PATCH' (Google Guice)
```