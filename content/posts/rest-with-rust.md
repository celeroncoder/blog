+++
ShowToc = false
date = 2022-09-04T18:30:00Z
description = "Calling Rest APIs with Rust"
tags = ["rest_api", "guide", "rust"]
title = "REST with Rust"
[cover]
alt = "Rest with Rust Cover"
image = "/blog/uploads/rest-with-rust.webp"

+++
Calling REST APIs with rust may seem like a daunting task at first because of the steep and long learning curve of rust as a programming language.

We know that contacting with REST APIs is something that we come across creating almost any other app that comes to mind.

We’ll make use of the `reqwest` library to make our request that is essentially a higher level implementation of the default HTTP client.

    # Cargo.toml
    reqwest = { version = "0.11", features = ["json"] }
    tokio = { version = "1", features = ["full"] }
    dotenv = "0.15.0" # optional
    serde = {version = "1.0.144", features = ["derive"]}

We import the following libraries to make this work

| Library | Purpose |
| --- | --- |
| reqwest | to make our requests |
| tokio | to make async requests and other async stuff. |
| serde | to deserialize the json response into a rust struct |

### **Making Basic GET Requests**

    #[tokio::main]
    async fn main() -> Result<(), Box<dyn std::error::Error>> {
    	let resp = reqwest::get("<url>").await?;
    	let resp_json = resp.json::<HashMap<String, String>>().await?;
    	
    	println!("{:#?}", resp_json);
    
    	Ok(());
    }

Here we do the following key things:

* We add the `tokio::main` attribute to our main function to use await within our function.
* We change the return type of the main function from unit type - `()` to a `Result<(), Box<dyn std::error::Error>>` to catch errors from the request if any.
* Then we make the request using the `get` function and await on that, and also we use the `turbofish` operator to only get the return type of the `future`
  * More info on why we are using this operator, [here](https://rust-lang.github.io/async-book/07_workarounds/02_err_in_async_blocks.html).
* Then we deserialize the JSON response to a `HashMap<String, String>` for convenience and await on that.

### Adding headers to our request

To add headers to our request, we first make a request client and use that to add headers to our application.

    use reqwest::header::HeaderMap;
    
    #[tokio::main]
    async fn main() {
    	...
    	let client = reqwest::Client::new();
    	let mut headers = HeaderMap::new();
    	headers.insert("content-type", "application/json".parse().unwrap());
    
    
    use reqwest::header::HeaderMap;
    
    #[tokio::main]
    async fn main() {
    	...
    	let client = reqwest::Client::new();
    	let mut headers = HeaderMap::new();
    	headers.insert("content-type", "application/json".parse().unwrap());
    	headers.insert("Authorization", format!("Bearer {}", API_TOKEN).parse().unwrap());
    	
    	let resp = client.get("<url>")
    								.headers(headers)
    								.send()
    								.await?;
    	
    	...
    }

Here’s what we did above:

* We create a request client to send our request.
* We create a mutable instance of the `HeaderMap` Instance that is similar to `HashMap`
* We insert our headers as key-value pairs to the header.
  * Also, we use `.parse().unwrap()` on the `&str` to convert the string type of the _header value_ type.
* We then add our headers to the client request by using the `.headers()` method.
* Also, one thing different from directly using the get method is that we have to call the `send` method on our request before awaiting on it.

### Sending post request with JSON body

We send the post request by using the `post` method on the _request client_ or directly from the library and use the `json` method to add body to the post request.

The body here is just a `HashMap` in rust.

    ...
    let mut body = HashMap::new();
    body.insert("username", "myusername");
    let resp = client.post("<url>")
    	.json(&body)
    	.send()
    	.await?;
    ...

### Deserializing the JSON response

We can deserialize the JSON response from the API by using the `json` method on the sent request, and get that into our preferred shape or type by calling it generically with that type.

One thing to note is that the **type must implement the Deserialize trait**.

First thing first we create the type we want our JSON response in and implement the deserialize trait on it, implementing the trait ourselves is tedious and unreliable, so we use the `serde` library that we imported before to do that for us.

    use serde::Deserialize;
    
    // deriving the Debug trait also to be able to print it
    #[derive(Debug, Deserialize)]
    struct APIResponse {
    	message: String;
    	error: String;
    }

We can then use the above struct to deserialize our response as:

    ...
    let resp_json = resp.json::<APIResponse>().await?;
    ...

### References

* `reqwest` [library documentation](https://docs.rs/reqwest),
* [Rust Async documentation](https://rust-lang.github.io/async-book),
* [YouTube Tutorial by Tom](https://youtu.be/j9MsMYz9hBw).