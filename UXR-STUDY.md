# UXR Study
This study will help us improve the developer experience of Compose integration inside Android
Studio.
Specifically today, you are going to test different scenarios around Layout Inspector and 
Recomposition.

## Layout Inspector
This sample is a chat sample app. This specific version is a fork of the original one, with several
UI bugs introduced. The goal is to use Layout Inspector to help you fix them.

### Disappearing channel name bar
When opening the app, we see the channel name bar at the top but it disappear as soon as the
list of messages get loaded.

**Hint**
It's not clear if the bar is removed or not. Try to hide the rest of the page subtree in Layout
Inspector to verify if it's still there.

**Solution**
Inside `ConversationContent`, `ChannelNameBar` should be placed after 
`Column(Modifier.fillMaxSize())` containing `Messages`.

### Overlapping jump to bottom button
When scrolling the list of messages, a jump to bottom button appears but it's not very visible.

**Hint**
The messages list page content is overlapping the jump to bottom button. Use the 3D view to figure 
out the bug

**Solution**
Inside `Messages`, move `JumpToBottom` below `LazyColumn`.

### Different Mode
Have you tried the dark mode on this screen? Could you spot the difference and fix it?

**Hint**
Isolate building blocks by filtering out piece by piece the UI elements of the screen in the Layout 
Inspector, starting from the background to make dark elements more visible

**Solution**
In `AuthorNameTimestamp`, both `Text` elements are using `Color.Black` instead of depending on the 
MaterialTheme (which changes based on the light/dark mode). Remove the color attribute.

## Recomposition

Updating a state, scrolling the screen or having a configuration change trigger Compose to re-render
the app's UI, we call this a [recomposition](https://developer.android.com/jetpack/compose/mental-model#recomposition).

### How would you check the recomposition
How would you identify a recomposition? Which tool would be your preferred one? (We don't currently  
have a tool, we want your opinion)

### Identify recomposition
Navigate to the profile screen. Can you identify which composable is recomposing?

**Hint**
There is a flashing element, find it in the project.

**Solution**
Check the `LaunchedEffect` inside `ProfileScreen`, which modify the state, triggering a 
recomposition.

### Reduce the number of recompositions
The FAB button is supposed to flash every ten seconds but it's currently flashing more often. Can 
you fix it?

**Hint**
We're verifying the current timestamp every 100 milliseconds. The verifying condition is happening 
at the `ProfileFab` level. Could it moved somewhere else?

**Solution**
By setting the current timestamp `flashingCondition` in the `LaunchedEffect` every 100 milliseconds,
we're triggering a recomposition every time (plus the fade-in animation). Instead, you should move
the condition check in the `LaunchedEffect`.

```kotlin
var flashingCondition by remember { mutableStateOf(false) }

LaunchedEffect(Unit) {
    while(true) {
        println("Checking timestamp ...")
        flashingCondition = Instant.now().epochSecond % 10 == 0L
        delay(100L)
    }
}

// ...

Crossfade(targetState = flashingCondition) {
    ProfileFab(
        extended = scrollState.value == 0,
        userIsMe = userData.isMe(),
        modifier = Modifier.align(Alignment.BottomEnd).background(if (it) Color.Blue else Color.Red),
        onFabClicked = { functionalityNotAvailablePopupShown = true }
    )
}
```