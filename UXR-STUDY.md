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