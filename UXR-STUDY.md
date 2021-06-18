# UXR Study
This study will help us improve the developer experience of Compose integration inside Android
Studio.
Specifically today, you are going to test different scenarios around Layout Inspector and 
Recomposition.

## Task
This sample is a chat sample app. This specific version is a fork of the original one, with several
UI bugs introduced. The goal is to use Layout Inspector to help you fix them.

### Layout Inspector

#### Disappearing channel name bar
When opening the app, we see the channel name bar at the top but it disappear as soon as the
list of messages get loaded.

**Hint**
It's not clear if the bar is removed or not. Try to hide the rest of the page subtree in Layout
Inspector to verify if it's still there.

**Solution**
Inside `ConversationContent`, `ChannelNameBar` should be placed after 
`Column(Modifier.fillMaxSize())` containing `Messages`.

#### Overlapping jump to bottom button
When scrolling the list of messages, a jump to bottom button appears but it's not very visible.

**Hint**
The messages list page content is overlapping the jump to bottom button. Use the 3D view to figure 
out the bug

**Solution**
Inside `Messages`, move `JumpToBottom` below `LazyColumn`.

#### Different Mode
Have you tried the dark mode on this screen? Could you spot the difference and fix it?

**Hint**
Isolate building blocks by filtering out piece by piece the UI elements of the screen in the Layout 
Inspector, starting from the background to make dark elements more visible

**Solution**
In `AuthorNameTimestamp`, both `Text` elements are using `Color.Black` instead of depending on the 
MaterialTheme (which changes based on the light/dark mode). Remove the color attribute.
