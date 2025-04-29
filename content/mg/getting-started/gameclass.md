+++
title = "The Game Class"
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

## Game1.cs

What we get is a Game1.cs file that contains our game. Just one file? Yes, how you want to split it is up to you.

But this is the heart of the game, where execution begins for our purposes (Technically, our platform code runs first)
and our game loop exists.

_Why Game1.cs?_ No idea, but if you want to change the class name to SomeEpicGame feel free. Just remember to change any
other references of Game1.

Let's break down some of the starting boilerplate

### Privates 
We're given two private variables here. These hold references to the GraphicsDeviceManager and the SpriteBatch objects. 
These are required later on for handling various aspects of display and rendering. Keep them, they are friends.

```csharp
    private GraphicsDeviceManager _graphics;
    private SpriteBatch _spriteBatch;
```

### Constructor
The constructor for your game. Generally in here is where you would set up anything required *functionally* for your game.
For example, here we get a copy of the GraphicsDeviceManager, set the content folder and tell MonoGame to keep the
mouse visible.

```csharp
    public Game1()
    {
        _graphics = new GraphicsDeviceManager(this);
        Content.RootDirectory = "Content";
        IsMouseVisible = true;
    }
```

### Initialize()
Initialize is called after your game has been constructed but before content is loaded. Additional functional items 
should be added here such as screen resize methods. The main difference between Initialize and the constructor is 
that GraphicsDeviceManager and some other internals are guaranteed to be available from this point.

_Note it is only called once_

```csharp
    protected override void Initialize()
    {
        // TODO: Add your initialization logic here

        base.Initialize();
    }
```

### LoadContent()
Called after initialize, this is where 99.9% of your content should be loaded. Textures, sounds, effects etc.
You can also put object instantiation here, for example after a player image has been loaded, you can set the
original position for player in here.

_Note it is only called once_

```csharp
    protected override void LoadContent()
    {
        _spriteBatch = new SpriteBatch(GraphicsDevice);

        // TODO: use this.Content to load your game content here
    }
```

### Update
_Most of the magic really does happen in here_

Updates x times a second. This would be considered the traditional game loop idea. This is where you would check for inputs, check collisions, move sprites around etc.

```csharp
    protected override void Update(GameTime gameTime)
    {
        if (GamePad.GetState(PlayerIndex.One).Buttons.Back == ButtonState.Pressed || Keyboard.GetState().IsKeyDown(Keys.Escape))
            Exit();

        // TODO: Add your update logic here

        base.Update(gameTime);
    }
```

### Draw
_And this makes all that juicy magic visible_

Updates x times a second. This is where everything is drawn to the display. Generally speaking logic should not be placed in here and handle only enough code to actually draw to the screen.

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // TODO: Add your drawing code here

    base.Draw(gameTime);
}
```