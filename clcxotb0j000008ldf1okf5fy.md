# Hey!! Lets make URL SHORTNER

Creating a URL shortener using React JS can be a fun and useful project. Here's a step-by-step guide on how to build one, with code examples and explanations:

1. Begin by setting up a new React project using Create React App. This will give you a basic structure to work with and save you time on configuration.
    

```plaintext
Copy codenpx create-react-app my-url-shortener
cd my-url-shortener
npm start
```

1. Next, create a new component for your URL shortener form. This component should include a form with an input field for the long URL and a button to submit the form.
    

```javascript
Copy codeimport React, { useState } from 'react';

const Shortener = () => {
  const [longUrl, setLongUrl] = useState("");
  const [shortUrl, setShortUrl] = useState("");

  const handleSubmit = async (e) => {
    e.preventDefault();
    // Send a POST request to your server with the long URL
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Enter long URL"
        value={longUrl}
        onChange={(e) => setLongUrl(e.target.value)}
      />
      <button type="submit">Shorten</button>
      {shortUrl && <p>Short URL: {shortUrl}</p>}
    </form>
  );
};

export default Shortener;
```

In the above code snippet, we've created a component `Shortener` which has two state variables `longUrl` and `shortUrl` and an event handler `handleSubmit` which gets triggered on form submission.

1. Now, you'll need to create a function that will handle the form submission. This function should use the `fetch` method to send a POST request to your backend server with a long URL.
    

```javascript
Copy codeconst handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await fetch("http://your-server.com/shorten", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ longUrl }),
      });
      const { shortUrl } = await response.json();
      setShortUrl(shortUrl);
    } catch (error) {
      console.log(error);
    }
  };
```

In the above code snippet, we've added the`fetch` call inside,`handle submit` which sends a post request to the server with the longer body.

1. On the server side, you'll need to create a route that can handle the POST request. This route should generate a short URL using a library like `shortid` and then save the long and short URLs to a database.
    

```javascript
Copy codeconst express = require("express");
const shortid = require("shortid");
const router = express.Router();

router.post("/shorten", async (req, res) => {
  try {
    const { longUrl } = req.body;
    const shortUrl = shortid.generate();
    // save the long and short URLs to your database
    res.json({ shortUrl });
  }
```