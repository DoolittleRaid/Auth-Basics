# Auth-Basics
Basics of Authentication w/ Passport.js

1. Install/Require relevant packages:
	express 
	mongoose
	passport
	body-parser 
	passport-local
	passport-local-mongoose 
	express-session* 

2. Setup:
	
``app.use(require('express-session')({
	secret: <string>, 
	resave: false,
	saveUninitialized: false 
}));``
Secret is used to code and decode a session--can be anything;
both latter two options required.

``app.use(passport.initialize());
app.use(passport.session());``
Methods used anytime passport is used

``passport.use(new LocalStrategy(User.authenticate()));``
LocalStrategy is variable that passport-local was saved to.  
Authenticate method is ran on User as it is from the user model.

``passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());``
Responsible for reading the session, taking data and encoding/unencoding information.  
These methods require passport-local-mongoose to be required on user model where User is already defined. 
User model needs .plugin(<passportLocalMongoose>) method on the user schema.

3. Usage/Routes:

handling user sign up
``app.post("/register", function(req, res){
    User.register(new User({username: req.body.username}), req.body.password, function(err, user){``
    	Password passed in after user is created.  New user is created but not saved to database yet.  
    	Password is used as second argument for user.register, which hashes password for storage.  
    	Function returns a new user with hashed password and username.
        ``if(err){
            console.log(err);
            return res.render('register');
        }
        passport.authenticate("local")(req, res, function(){``
        	Logs user in.  Passport method can be changed for different uses i.e. Twitter or 
        	Facebook as opposed to local.
          `` res.redirect("/secret");
        });
    });
});``

handling user login 
``app.post("/login", passport.authenticate("local", {
	successRedirect: "/secret",
	failureRedirect: "/login"
}) ``
middleware: passport.authenticate is ran before the handler function.  
Authenticate method authenticates credentials for you. 
Passport automatically grabs information from form. 
``function(req, res){
});``

