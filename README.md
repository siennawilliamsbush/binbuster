# trigger for when the recycle bin overlaps with general waste
def on_on_overlap(sprite2, otherSprite2):
    # destroy the general waste sprite using a disintegrate visual effect over 100ms
    otherSprite2.destroy(effects.disintegrate, 100)
    # play a negative sound effect to indicate penalty 
    music.knock.play()
    # subtract 1 from the player's life count
    info.change_life_by(-1)
# register when the recycle bin hits the general waste
sprites.on_overlap(SpriteKind.player, SpriteKind.enemy, on_on_overlap)

# trigger for when the recycle bin overlaps with a recyclable sprite
def on_on_overlap2(sprite, otherSprite):
    # destroy the binned recyclable using a spray visual effect over 100ms
    otherSprite.destroy(effects.spray, 100)
    # play a positive sound effect indicating a successful disposal
    music.ba_ding.play()
    # increase the player's score by 1 point
    info.change_score_by(1)
# register when the recycle bin hits the recyclable sprites
sprites.on_overlap(SpriteKind.player, SpriteKind.projectile, on_on_overlap2)

# assign global variables to reference different waste item sprites
general_waste: Sprite = None
plastic: Sprite = None
paper: Sprite = None
glass: Sprite = None

# set the background image of the game scene to a drawing of the earth
scene.set_background_image(assets.image("""
    background
    """))

# create the recycle bin image and set its kind to player
recycle_bin = sprites.create(assets.image("""
    hero
    """), SpriteKind.player)

# enable controller movement for the recycle bin sprite
controller.move_sprite(recycle_bin)

# restrict the recycle bin so it cannot move past the screen boundaries
recycle_bin.set_stay_in_screen(True)

# define a recurring timer that runs every 5 seconds to spawn items
def on_update_interval():
    # allow modifications to waste items inside this function
    global glass, paper, plastic, general_waste
    
    # create a glass sprite coming from the sides with randomised velocities
    glass = sprites.create_projectile_from_side(img("""
            . . . . . . . . . . . . . . . .
            . . . . . . . 6 6 . . . . . . .
            . . . . . . . 7 7 . . . . . . .
            . . . . . . . 7 7 . . . . . . .
            . . . . . . . 7 7 . . . . . . .
            . . . . . . 7 7 7 7 . . . . . .
            . . . . . 7 7 7 7 7 7 . . . . .
            . . . . . 7 7 7 7 7 7 . . . . .
            . . . . . 7 7 1 1 7 7 . . . . .
            . . . . . 7 1 7 7 1 7 . . . . .
            . . . . . 7 1 7 7 7 7 . . . . .
            . . . . . 7 1 7 1 1 7 . . . . .
            . . . . . 7 7 1 1 7 7 . . . . .
            . . . . . 7 7 7 7 7 7 . . . . .
            . . . . . 7 7 7 7 7 7 . . . . .
            . . . . . 7 7 7 7 7 7 . . . . .
            """),
        randint(-50, 50),
        randint(-50, 50))
    
    # create a paper sprite coming from the sides with randomised velocities
    paper = sprites.create_projectile_from_side(img("""
            . . . . . . . . . . . . . . . .
            . . . . . . . . . . . . . . . .
            . . . . . . . . . . . . . . . .
            . . 4 4 4 4 4 4 4 . . . . . . .
            . . 4 f f f f f 4 . . . . . . .
            . . 4 4 4 4 4 4 4 . . . . . . .
            . . 4 f f d d d d d d . . . . .
            . . 4 4 4 d f f f f d . . . . .
            . . 4 f f d f d d f d . . . . .
            . . 4 4 4 d f f f f d . . . . .
            . . 4 f f d f d d d d . . . . .
            . . 4 4 4 d f d d d d . . . . .
            . . . . . d f d d d d . . . . .
            . . . . . d d d d d d . . . . .
            . . . . . . . . . . . . . . . .
            . . . . . . . . . . . . . . . .
            """),
        randint(-50, 50),
        randint(-50, 50))
    
    # create a plastic sprite coming from the sides with randomised velocities
    plastic = sprites.create_projectile_from_side(img("""
            . . . . . . . . . . . . . . . .
            . . . . . . . . . . . . . . . .
            . . . . . . . 8 8 . . . . . . .
            . . . . . . . 9 9 . . . . . . .
            . . . . . . . 9 9 . . . . . . .
            . . . . . . 9 9 9 9 . . . . . .
            . . . . . 9 9 9 9 9 9 . . . . .
            . . . . . 8 8 8 8 8 8 . . . . .
            . . . . . 8 8 8 8 8 8 . . . . .
            . . . . . 8 8 8 8 8 8 . . . . .
            . . . . . 9 9 9 9 9 9 . . . . .
            . . . . . 9 9 9 9 9 9 . . . . .
            . . . . . 9 9 9 9 9 9 . . . . .
            . . . . . . 9 9 9 9 . . . . . .
            . . . . . . . . . . . . . . . .
            . . . . . . . . . . . . . . . .
            """),
        randint(-50, 50),
        randint(-50, 50))
    
    # create a general waste sprite coming from the sides with randomised velocities
    general_waste = sprites.create_projectile_from_side(img("""
            . . . . . . . . . . . . . . . .
            . . . . 2 2 2 2 . 2 2 2 2 . . .
            . . . . 2 f f 2 2 2 f f 2 . . .
            . . . . 2 2 f f f f f 2 2 . . .
            . . . . . . 2 2 2 2 2 2 . . . .
            . . . . 2 f f f f f f f 2 . . .
            . . . 2 f f 2 f f f 2 f f 2 . .
            . . . 2 f f f 2 f 2 f f f 2 . .
            . . . 2 f f f f 2 f f f f 2 . .
            . . . 2 f f f 2 f 2 f f f 2 . .
            . . . 2 f f 2 f f f 2 f f 2 . .
            . . . . 2 f f f f f f f 2 . . .
            . . . . . 2 2 2 2 2 2 2 . . . .
            . . . . . . . . . . . . . . . .
            . . . . . . . . . . . . . . . .
            . . . . . . . . . . . . . . . .
            """),
        randint(-50, 50),
        randint(-50, 50))
    
    # change the general waste sprite to be an enemy so it penalises the player if it collides with the recycle bin 
    general_waste.set_kind(SpriteKind.enemy)

# register the timer interval to trigger the spawning function every 5000 milliseconds
game.on_update_interval(5000, on_update_interval)
