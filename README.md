# samb

`samb` offers a structured language to define RESTful HTTP APIs. `samb` provides syntax support for languages similar to those used to write infrastructure as code. It offers a language that ChatGPT can interprete to write actual server code in any language.

Once you finish writing your code, you may then, deploy your project to your cloud provider of choice. 


### Documentation
Learn more about `samb` code generation : [here](https://github.com/cheikhshift/samb/wiki). Scroll down to find more samples.


-----

Checkout the [wiki](https://github.com/cheikhshift/samb/wiki).

### Samples

Find more sample projects [here](https://github.com/cheikhshift/samb-examples).
Here is a sample server definition (in Nginx like language) :

```
server {
    host 127.0.0.1;
    port 8080;

    # Import web route definitions
    require "./endpoints.se";


    start {
    	do println("Hello");
    	do println("Hello again");
    }

    shutdown {
	# directive do will execute passed
	# golang code
    	do println("Bye");
    }  
}

```

In YAML:

```
# Go package import path of your project.
package: github.com/cheikhshift/samb-examples/yaml-example

# import providers
require: ["./providers.se"]


# Globals are exported via package 
# globals
global:
  - name: Foo
    type : bool
    # Adding comments to exported variable.
    comment: Foo decides if a process should run
    return : false
  - name: AnotherVariable
    type : bool
    return: true

server: 
  host: 127.0.0.1
  port: 8081
  # Import web route definitions
  require : ["./endpoints.yml"]

  start:
    do:
      - println("HelloWorld")
      - println("Starting...")
```

##### Sample Route (file named endpoints.se)

```
# Routes' definition
# Import Go packages with directive import
# For example import "net/http";
# samb source format checking.


routes {
    provide r;

    route {
	    method *;
	    # Defines route path.
	    # all sub routes have this path
	    # prepended.
	    path "/hello/";

	    # Provider variables
	    # within scope of entire 
	    # route.
	    provide w;
	    provide r;


	    go {
	    	do println("Hello");
	    	# The following commented
	    	# do directive will stop a
	    	# request :
	    	# do return;
	    }

	    route {
	    	method GET;
	    	path "Foo";

	    	go {
	    		do println("Hello");
	    	}

	    	# Handler can be any function.
	    	# Should be a function that handles the request
	    	# response. using provided variable r
		# with handler. This code will fail,
		# the referenced handler is not defined
	    	handler virtualPackage.Handle(r);
	    }


	}
}
```

##### Sample Providers

```
# Providers are used
# within endpoint requests.
provider {
	name r;
	type *http.Request;
    # directive return is not used here.
    # return can be used to define how your
    # provider is initialized. For example,
    # providing a variable with value "Foo"
    # : return string("Foo") 
}

provider {
	name w;
	type *http.ResponseWriter;
}
```
