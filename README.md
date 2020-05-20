# BlazorThreadSample
An Example of background thread blocking Blazor Server App UI, demonstrating 
the problem highlighted in [this issue](https://github.com/dotnet/aspnetcore/issues/21171).

Compile and run the app, go to the 'Counter' screen and click the counter button several times. 
Each click will be processed, and the counter will increment.

Now check the checkbox; this will cause a low-priority background thread to run each time the button
is clicked. Because it's on another thread, it should have no impact to the application - the work
should continue in the background and the foreground should remain responsive, with the button
responding to clicks and the counter incrementing. However, what actually happens is that the GUI
entirely freezes until the thread completes, and any button clicks are lost.
