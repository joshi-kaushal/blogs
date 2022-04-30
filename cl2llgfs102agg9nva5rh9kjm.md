## How to Keep SSH Connection Alive for Longer Durations

A few weeks back, I was working on a project that had Husky pre-push hook configured. After working on it for a few days, I noticed that sometimes code is not getting pushed to the remote repository. 

After a headache of trial and error, I realized that pre-push hook is taking so much time to finish. And when it did, there was no message of code being pushed to the remote repo in the terminal. Something was odd. 

I learned that the SSH connection between the terminal and GitHub initiates when we execute `git push` and terminates when the code is pushed successfully. However, if the connection is inactive for a certain period of time, it terminates itself due to timeout. 

Since Husky pre-push was taking so long time to finish its job, the SSH connection with the GitHub server was timing out. The dead connection was resulting in the code not getting pushed at all.

I needed a way to keep the connection alive for a few more minutes. Luckily, we can do this in the terminal itself. 

### Prerequisites
Your terminal must be connected to your GitHub account via SSH. You can find more information [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).  


## Solution
As I said, the solution is to keep the connective alive until husky finishes its task. It's easier than I thought.

In your terminal, go to the directory that stores SSH information. Usually, it's `/.ssh` folder in the home directory. Execute this command in the terminal:
```bash
cd ~/.ssh
```
The tilde symbol(~) represents `/home` directory. Since our desired destination folder is in the home directory itself, this command will change the current working directory to `/home/.ssh`.

Create a file named `config` without any extension. Add the following text to the file:
```
Host *
  ServerAliveInterval 60
  ServerAliveCountMax 30
```


![ssh-config.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1650864613611/6NbEYHYsA.gif)


Connect to the host once again and you'd see the success. If not, increase the duration by a few minutes until you find your sweet spots. The way to understand this setting is explained in the next section.

## Explanation 

But what does `ServerAliveInterval` and `ServerAliveCountMax` actually do?

According to SSH manual:

> **ServerAliveInterval:**
Sets a timeout interval in seconds after which if no data has been received from the server, ssh will send a message through the encrypted channel to request a response from the server. The default is 0, indicating that these messages will not be sent to the server. This option applies to protocol version 2 only.

> **ServerAliveCountMax:**
The default value is 3. If, for example, `ServerAliveInterval` is set to 15 and `ServerAliveCountMax` is left at the default, if the server becomes unresponsive, SSH will disconnect after approximately 45 seconds. This option applies to protocol version 2 only.


So to summarize:

1. The client will wait idle for 60 seconds i.e. `ServerAliveInterval` time.
2. Then, it will send a *no-op null packet* to the server and expect a response.
3. If no response comes, then it will keep trying the above process till 30 times, defined with `ServerAliveCountMax`. So it will wait for 1800 seconds, which is half an hour.
4. If the server still doesn't respond, then the client disconnects the SSH connection.


## Things to Know

### Who waits for the response

In our case, the client is waiting for the server's response. Because the GitHub server sends a response back when the code is pushed. If you want the client to send a response, you should use `ClientAliveInterval` and `ClientAliveCountMax`.

### Different config settings for different hosts
The asterisk symbol (*) sets this configuration for every connected host. If you want to set this for a particular host, or only one host, you can do it with following way:

```
Host hostname1
    SSH_OPTION value
    SSH_OPTION value

Host hostname2
    SSH_OPTION value

Host *
    SSH_OPTION value
```

The list of available connected hosts could be found in the `known_hosts` file which is located in the same directory.


## Wrapping Up

Server alive messages are useful when an SSH server has been configured to close connections after a period of time with no traffic. Setting these two options keeps the session alive by sending a packet every *ServerAliveInterval *seconds for a maximum of *ServerAliveCountMax* times.

As a developer, we come across new problems every single day. Some of those challenges our skills, others challenge our patience. For me, this problem falls into the latter category. Hence I decided to write an article about it. 

I hope you found this article helpful. If you want to say hi, my DMs are always open. I am most active on [Twitter](https://twitter.com/notifications), [LinkedIn](https://www.linkedin.com/in/7JKaushal/) and [Showwcase](https://www.showwcase.com/clumsycoder).

Happy coding! 👨‍💻

