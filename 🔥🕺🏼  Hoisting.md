Hoisting is one of those terms that every JS dev has heard of because you googled your annoying error and ended up on StackOverflow, where this person told you that this error was caused because of hoisting 🙃 So, what is hoisting? (FYI - scope will be covered in another post, I like to keep posts small and focused)

If you’re new to JavaScript, you may have experienced “weird” behavior where some variables are randomly undefined, ReferenceErrors get thrown, and so on. Hoisting is often explained as putting variables and functions to the top of the file but nah, that’s not what’s happening, although the behavior might seem like it 😃

1)When the JS engine gets our script, the first thing it does is setting up memory for the data in our code. No code is executed at this point, it’s simply just preparing everything for execution. The way that function declarations and variables are stored is different. 
Functions are stored with a reference to the entire function.
