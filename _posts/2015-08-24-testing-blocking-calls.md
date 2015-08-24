---
layout: post
title:  "Testing blocking calls"
date:   2015-08-24 18:00:19
categories: 8thlight
---
Blocking calls are named that way because the execution waits until the call has finished. Most blocking calls just have to finish some processing without external interaction and then continue the execution con the caller.

But there are cases when the blocking call will not finish until an unblocking action has been done. Examples are: [`System.in.read()`](http://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html#read()), [`new ServerSocket(0).accept()`](http://docs.oracle.com/javase/7/docs/api/java/net/ServerSocket.html#accept())

The problem with testing blocking calls is that executing them in tests will block (obviously) them and not finish. No matter what, you'll need at least two threads: one will get blocked and the other will do the unblocking action. Now, you can decide where to put those threads: in test code or in production code.

An example of doing it in tests from my [http-server](https://github.com/demonh3x/server.java/blob/master/src/test/java/com/github/demonh3x/server/ConnectionListenerTest.java#L76):
{% highlight java %}
@Test
public void stopsWaitingForConnectionWhenAnotherThreadTellsToFinish()
            throws IOException, InterruptedException {
  server = new ServerSocket(9999);
  final ConnectionListener listener = new ConnectionListener(
      server,
      new NullConnectionHandler()
  );

  doAfterWaiting(50, new Action() { //Execute in another thread
      @Override
      public void run() {
          listener.finish(); //Unblocking from another thread
      }
  });

  listener.waitForConnection(); //Blocking call
}

...

public class ConnectionListener {
...
  public void waitForConnection() {
    ...

    Socket connection;
    try {
      connection = server.accept(); //Blocking call
    } catch (IOException e) {
      ...
    }
    ...
  }
}
{% endhighlight %}

Another example of doing it in code from my [http-server](https://github.com/demonh3x/server.java/blob/master/src/main/java/com/github/demonh3x/server/Server.java#L30)
{% highlight java %}
@Test
public void thePortIsUsedAfterStartingAndIsReleasedAfterStopping() {
  Server server = createServer(9999);
  server.start(); //Non-blocking call
  assertThat(isPortUsed(9999), is(true));
  server.stop();
  assertThat(isPortUsed(9999), is(false));
}

...

public class Server {
...
  public synchronized void start() {
  ...
    listener = new ConnectionListener(socket, connectionHandler);

    //Execute in another thread so this call does not block
    executor.execute(new Runnable() {
        @Override
        public void run() {
          while (!listener.isFinished())
            listener.waitForConnection(); //Blocking call
        }
    });
  }
}
{% endhighlight %}
