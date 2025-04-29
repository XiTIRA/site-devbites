+++
title = "Animation"
description = ""
date = 2021-05-01T08:00:00+00:00
updated = 2021-05-01T08:00:00+00:00
draft = false
weight = 10
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = ''
toc = true
top = false
+++

_Assuming a new cross-platform desktop project_

_[Animation - Source](https://github.com/devbitesau/quickshots/blob/main/animation/AnimationGame.cs)_

## Assets

This excercise will be using elements from [Pixel Adventure 1](https://pixelfrog-assets.itch.io/pixel-adventure-1) - full credit to Pixel Frog for the creation.

Load these into the content tool, remembering to build afterward.

- Up : ![up.png](up.png)
- Down : ![down.png](down.png)
- Idle : ![idle.png](idle.png)
- Run : ![run.png](run.png)

## Intro
There are many different ways to generate an animation in MonoGame, but one common method is a simple flip book style.

The general concept is that you swap out one texture for another once a certain amount of time (per animation frame) 
elapses.

## One Image Per Frame

For the first animation, we will simply toggle between two textures (seperate .png image files). Note that normally a
sprite animation stores multiple frames in a single image, but we'll get to that soon enough.

In our game class, start by creating some private members. These will be used in the update and draw loops later.

```csharp
    // Stores original textures for the animation
    private List<Texture2D> _animIndividualFrames = new List<Texture2D>();

    // The current frame texture to draw
    private Texture2D _currentTexture;

    // The animations world position
    private Vector2 _animationPosition;

    // Variables to track the current frame and frame time
    private float _frameCountdown;
    private int _currentFrame;
    private const float SecondsPerFrame = .2f;
```

In `LoadContent()` we'll grab the screen width and height. Strictly not necessary but for this document makes positioning
the sprites easier.

```csharp
        // Grab the screen width to help with positioning
        int screenWidth = _graphics.PreferredBackBufferWidth;
        int yPos = _graphics.PreferredBackBufferHeight / 2;
```

From here, load the Up and Down images into respective variables and add them to the animation frames list. Lists are in 
the order we load them, so frame 1 would be 'Down' and so on.

```csharp
        // Read in our frame textures. We save them in a list, so a local variable is fine.
        Texture2D frogDown = Content.Load<Texture2D>("Down");
        Texture2D frogUp = Content.Load<Texture2D>("Up");

        // And add them to our frame list
        _animIndividualFrames.Add(frogUp);
        _animIndividualFrames.Add(frogDown);
```

Set the initial position of the animation so that we can see it nicely on the screen. Note `* .25` here is effectively 
1/4 th of the total screen width. Ie, `new Vector2(middle left x, center y)`.
```csharp
        // And give it a position
        _animationPosition = new Vector2((float)(screenWidth * .25), yPos);
```

The first frame of the animation needs to be set before we start updates and draws - If it's not set an exception will
be thrown because it can't load a null texture. This does not make a copy of the texture, just copies the reference to it.

Also set the current frame or index here. 
```csharp
        // Then we need to set the first frame
        _currentTexture = frogUp;
        _currentFrame = 0;
```

Create a method `FlipBookAnimationSeperateTextures` so that we can keep the update loop a bit cleaner. Use the code below,
taking note how we use the length of the animation frame list to work out when to cycle back to the first image. This way,
extending the image only requires adding new frames and not adjusting hard set frame counts.
```csharp
    private void FlipBookAnimationSeperateTextures(GameTime gameTime)
    {
        // Reduce the frame timer by the elapsed number of seconds.
        _frameCountdown -= (float)gameTime.ElapsedGameTime.TotalSeconds;

        // If the timer is complete...
        if (_frameCountdown <= 0.0f)
        {
            // Reset the frame timer.
            _frameCountdown = SecondsPerFrame;

            // Increase the frame index, and roll back to the first (0) if we've reached the end.
            _currentFrame++;
            if (_currentFrame > _animIndividualFrames.Count - 1) // -1 is important, we check 0->n-1 instead of 1->n.
            {
                _currentFrame = 0;
            }

            // Set the current texture (frame) to the current frame index.
            _currentTexture = _animIndividualFrames[_currentFrame];
        }
    }
```

Now going back to the update method of the game class, updating the animation timer and frames is as simple as calling
`FlipBookAnimationSeperateTextures(gameTime);`

Jump over to the games `Draw()` method. Start a sprite batch, draw the `_currentTexture` and close the sprite batch'

```csharp
        // Always start a sprite batch with SpriteBatch.Begin()
        _spriteBatch.Begin();

        // Draw the current texture of the animation
        _spriteBatch.Draw(_currentTexture, _animationPosition, Color.White);

        // Always end a sprite batch with SpriteBatch.End()
        _spriteBatch.End();
```