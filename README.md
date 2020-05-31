# BlazorThreadSample
An Example of background thread blocking Blazor Server App UI, demonstrating 
the problem highlighted in [this issue](https://github.com/dotnet/aspnetcore/issues/21171).

To repro: 

1. Launch the app and go to the counter page
2. Click the Counter button *once*
3. Within the next 5 seconds, click on the menu on the left-hand side, to switch to different pages, and see if the GUI responds immediately, or if nothing happens until after the 5 seconds are up and the thread exits
4. Also note whether the counter increments immediately after clicking, or if it takes 5 seconds before the UI updates with the new value.
5. Now check the checkbox; this will cause a low-priority background thread to run each time the button
is clicked. 
6. Repeat steps 2/3/4

What I see is that with the checkbox unchecked, clicking the Counter button updates the counter in the UI 
immediately, and I can click around and switch to other pages immediately. 

With the checkbox checked, the counter value does not update in the UI until the 5-second background thread 
work completes, and during that 5 seconds if I click the menu to navigate to other pages, the page doesn't 
update until the 5-second background work completes.

This seems wrong - because the work is being done on another thread, it should have no impact to the 
application - the work should continue in the background and the foreground should remain responsive, with 
the button responding to clicks and the counter incrementing. However, what actually happens is that the GUI
entirely freezes until the thread completes, and any button clicks are lost. Also, the counter doesn't
update in the UI until the thread completes.

![alt text](https://github.com/Webreaper/BlazorThreadSample/blob/master/BlazorThreadSample.gif "Application in use")
