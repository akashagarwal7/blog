---
layout: post
title: Understanding the difference between SSH local vs remote port-forwarding
# subtitle: Each post also has a subtitle
# gh-repo: akashagarwal7/blog
# gh-badge: [star, fork, follow]
tags: [ssh]
comments: true
---

I had been familiar with the existence of SSH port-forwarding for a while, but it was only last year when I completely understood the difference between local and remote port-forwarding, and which is appropriate when.

Here's my quick take on explaining the differences.

## Local port-forwarding

Let's say that you want to **forward requests originating from your machine to a remote machine**. An example of this could be that you want to access a remote HTTP server running on port 80, which is behind a firewall. This is when you use SSH local port-forwarding. If you use the command:

```sh
ssh -L 80:localhost:80 user@example.org
```

..and try to visit http://localhost or http://127.0.0.1 in your browser, your browser will initialize a request to http://127.0.0.1:80, which is then in turn forwarded to the remote server.

## Remote port-forwarding

The difference here lies in the origin of requests. Let's say that you have an HTTP server running on your local machine and you would like to let somebody over internet access your server, without obtaining a static public IP address and configuring your router as well as the firewall. A quick way to do this is to use a tool like [ngrok](https://ngrok.com), which uses the concept of remote port-forwarding.
To explain the process of how remote ports can be forwarded, let's again assume that you have access to a public server with a public IP address `10.2.3.4` with an exposed port 80. If you then use the command: 

```sh
ssh -R 80:localhost:80 user@10.2.3.4
```

..and ask your peer to visit http://10.2.3.4 (or do so yourself), their browser will send a request to http://10.2.3.4:80, **which in turn will be forwarded to your machine** as long as the SSH tunnel is established.

If you're running multiple servers, for instance, an SPA React server on port 80 and a backend Node.js server on port 8000, you would want to forward multiple ports by issuing the command:

```sh
ssh -R 80:localhost:80 8000:localhost:8000 user@10.2.3.4
```

Note: Make sure that the React application is configured to access http://10.2.3.4 and not http://localhost for your remote peer to use the application correctly.