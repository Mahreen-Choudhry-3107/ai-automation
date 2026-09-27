# What is API?

API stands for **Application Programming Interface**.

Think of it like ordering food at a restaurant:

- You are the **client** (the HTTP Request node). You want something.
- The **Kitchen** is the **server** (the Webhook node). It has the data / service you want.
- The **API** is the waiter and the menu. It's the set of rules and options you have for making a request and getting a response.

# Action Required for n8n Cloud Users

If you are using the **n8n Cloud** version, the default setting in this node will cause an **access to env vars denied** error.

This is normal and easy to fix.

Just follow these steps:

## 1. Find Your Webhook URL

- Open any **Webhook Trigger** node in this workflow (like the one for `GET /menu`).
- Copy the **Production URL**.
- You only need the base part.

# Example

If your URL is:

`https://my-instance.app.n8n.cloud/webhook/1/abc/xyz`

...........

You need to only copy this:

`https://my-instance.app.n8n.cloud/webhook`

## 2. Update This "BASE URL" Node

- In this node, replace the entire expression in the value field with the URL you just copied.

### REPLACE THIS:

`{{env.....}}`

### WITH YOUR URL:

`https://my-instance.app.n8n.cloud/webhook`

- Don't forget the `/tutorial......` part.

# Lesson 1: The Basics (Method & URL)

This is the simplest possible request.

- **URL (Uniform Resource Locator):** This is the **address of the restaurant's kitchen**. The HTTP Request node needs to know exactly where to send the order. We use a special n8n expression to get the Webhook's test address automatically.

- **Method: GET:** This is what you want to do. GET means you simply want to **retrieve or get information**. It's like asking the waiter, "What's on the menu today?"

➡️ **Look at the output of the HTTP Request node.** It received exactly what the Webhook is configured to listen to!



# Lesson 2: Customizing a Request (Query Parameters)

What if you want to customize your order? That’s what Query Parameters are for.

**Query Parameters:** These are simple options added to the end of the URL after `?`. They are key-value pairs used to filter, sort, or specify what you want.

It's like telling the waiter, “I’ll have the pizza... and can you add extra cheese?”

`extra_cheese=true` is the query parameter.

➡️ **The Webhook node uses an IF node to check for this parameter and changes its response accordingly.**

Try setting the value to `false` in the HTTP Request node and run it again!



# Lesson 3: Sending Data (POST & Body)

Sometimes, you don’t want to get data, you want to **send it**.

- **Method: POST:** This method is used to **send new data** to the server. It’s like handing the waiter a completed customer feedback card.

- **Body:** This is the actual data you are sending. Since you’re sending more complex information than a simple query parameter, it goes in a separate “package” called the body.

➡️ **The HTTP Request sends a JSON object in its body. The Webhook receives it and includes your comment in its response.**



# Lesson 4: Identification (Headers & Auth)

Headers contain meta-information about your request. They're not part of the data itself, but they provide important context. Authentication is a common use case.

- **Headers:** Think of this as showing your VIP membership card or whispering a secret password to the waiter. It's information that proves who you are or what your request's properties are.

- **Authentication (Auth):** This is the process of proving your identity. Here, we use a custom header (`x-auth-token`) as a "secret key."

➡️ **The Webhook checks for the correct secret key in the headers. If it’s wrong or missing, it denies the request!**



# Lesson 5: Being Patient (Timeout)

An API request isn’t instant. What if the kitchen is really busy?

- **Timeout:** This is the **maximum amount of time (in milliseconds)** you are willing to wait for a response before you give up and walk away.

In this example:

- The **Kitchen (Webhook)** has a 3-second delay.
- The **Customer (HTTP Request)** is only willing to wait for 2 seconds (2000 ms).

➡️ **This request is designed to FAIL!** The customer gives up before the kitchen can finish the order. This is crucial for preventing your workflows from getting stuck forever waiting for a slow service.