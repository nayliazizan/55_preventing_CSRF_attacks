this is just exercise that i did when learning fullstack engineer at codecademy. therefore, the whole code wasnt mine. i only put a few lines to complete exercise

goal of this exercise is to apply what i've learn abt preventions for CSRF attacks.

what i've done in this exercise:
CONFIGURE CSURF PACKAGE
1. add const csurf = require('csurf'); at the top of app.js to the csurf library
2. configure a middleware named csrfMiddleware inside app.js and set it equal to csurf():
const csrfMiddleware = csurf({
  cookie: {
    maxAge: 3000,
    secure: true,
    sameSite: 'none'
  }
}); 
3. configure the app to use the middleware function at the application level:
app.use(csrfMiddleware);

ADDING ERROR-HANDLING
4. configure error message middleware if there is an error validating the CSRF token. then create if-else that check if err.code equals to certain string:
const errorMessage = (err, req, res, next) => {
  if (err.code === "EBADCSRFTOKEN") {
    res.render("csrfError");
  } else {
    next();
  }
}
5. configure the app to use the errorMessage() middleware at the application level using app.use():
app.use(errorMessage);

ADDING CSRF MIDDLEWARE TO 2 DIFF ROUTES
6. send the CSRF token in the / route to use a CSRF token inside a web form:
res.render('order', { csrfToken: req.csrfToken() })
7. generate a CSRF token in the /contact route as each web form should use a CSRF token to prevent CSRF attacks:
res.render('contact', { csrfToken: req.csrfToken() })

ADDING THE CSRF TOKEN TO TWO VIEWS
8. add the CSRF token to the form in order.ejs and contact.ejs as a hidden input element:
<input type="hidden" name="_csrf" value="<%= csrfToken %>">
