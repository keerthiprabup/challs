# Roadmap

The source code for the challenge:

[roadmap.tar.xz](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/roadmap.tar.xz)

## Analysation of the app:

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled.png)

This is the index page where we can see a link to navigate to /make directory

In the /make page allows us to make roadmaps in the app which has a body part and also a checkbox for setting it as public

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%201.png)

As whenever a roadmap is set to public, the link for the roadmap is poped in the index page.

## Analysing the code:

After going throughout the source code to understand the working of the app, I focused on the flag part.

When there is no user id the following code snippet get executed:

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%202.png)

The code will setup the flag content as a roadmap in the app without defining the public value to the constructor.

This code reveals that we can’t access the roadmaps which all are undefined:

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%203.png)

Also this is the code for making a roadmap:

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%204.png)

While analysing the package.js, we can see that lodash v4.17.11 has been installed in the app.

Surfing the internet on the search of vulnerabilities on the package brought me on to the prototype pollution vulnerability which looks more similar to the challenge.

This vulnerability basically The function *defaultsDeep* could be tricked into adding or modifying properties of *Object.prototype* using a *constructor* payload.

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%205.png)

The payload format:

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%206.png)

First we will try burping the request and see the jsons passed:

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%207.png)

So we can inject the payload here in this json by setting up the prototype-public value to be true 
so that it will make all the roadmaps on the present constructor public.

![Untitled](Roadmap%20f8c8fae83fb942069593e8470dcc2c0a/Untitled%208.png)

The payload we used in making the roadmap:

```
{"content":"ayn","public":true,"constructor":{"prototype":{"public":true}}}
```

After successfully executing the payload, Now we can reload the index page as we have set the constructor is such a way that the roadmaps present in the app were set to public.
