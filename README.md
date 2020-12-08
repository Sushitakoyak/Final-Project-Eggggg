# Final-Project-Eggggg
-- EGGGGG
-- WenLong Zheng

-- Note: Code written here are not written by me, only using for reference toward the type of game I am creating

-- Link to video:
-- https://youtu.be/bUH1mRLdBkU


-- Credit:
-- https://github.com/Jeepzor/Platformer-tutorial
-- https://www.youtube.com/channel/UCcq3KOawHtjyN0ibjL4RzXQ
-- https://www.youtube.com/watch?v=AWGd6T3B2OA
-- https://www.youtube.com/watch?v=XfIDHUyLpQ0




# main.lua

local STI = require("sti")
require("player")

function love.load()
    Map = STI("map/1.lua", {"box2d"})
    World = love.physics.newWorld(0, 0)
    World:setCallbacks(beginContact, endContact)
    Map:box2d_init(World)
    Map.layers.solid.visible = false
    background = love.graphics.newImage("assets/background.png")
    Player:load()
end

function love.update(dt)
    World:update(dt)
    Player:update(dt)
end

function love.draw()
    love.graphics.draw(background)
    Map:draw(0, 0, 2, 2)
    love.graphics.push()
    love.graphics.scale(2, 2)

    Player:draw()

    love.graphics.pop()
end

function beginContact(a, b, collision)
    Player:beginContact(a, b, collision)
end

function endContact(a, b, collision)
    Player:endContact(a, b, collision)
end

# conf.lua

function love.conf(t)
    t.title = "Eggggg"
    t.version = "11.3"
    t.console = true
    t.window.width = 1200
    t.window.height = 720
    t.window.vsync = 0
end

# player.lua

Player = {}

function Player:load()
    self.x = 100
    self.y = 0
    self.width = 20
    self.height = 60
    self.xVel = 0
    self.yVel = 100
    self.maxSpeed = 200
    self.acceleration = 4000
    self.friction = 3500
    self.gravity = 1500

    self.grounded = false


    self.physics = {}
    self.physics.body = love.physics.newBody(World, self.x, self.y, "dynamic")
    self.physics.body:setFixedRotation(true)
    self.physics.shape = love.physics.newRectangleShape(self.width, self.height)
    self.physics.fixture = love.physics.newFixture(self.physics.body, self.physics.shape)
end

function Player:update(dt)
    self:syncPhysics()
    self:move(dt)
    self:applyGravity(dt)
end

function Player:applyGravity(dt)
    if not self.grounded then 
    self.yVel = self.yVel + self.gravity * dt
    end
end

function Player:move(dt)
    if love.keyboard.isDown("d", "right") then
        if self.xVel < self.maxSpeed then
            if self.xVel + self.acceleration * dt < self.maxSpeed then
                self.xVel = self.xVel + self.acceleration * dt
            else
                self.xVel = self.maxSpeed
            end
        end
    elseif love.keyboard.isDown("a", "left") then
        if self.xVel > -self.maxSpeed then
            if self.xVel - self.acceleration * dt > -self.maxSpeed then
                self.xVel = self.xVel - self.acceleration * dt
            else
                self.xVel = -self.maxSpeed
            end
        end
    else
        self:applyFriction(dt)
    end
end

function Player:applyFriction(dt)
    if self.xVel > 0 then
        if self.xVel - self.friction * dt > 0 then
            self.xVel = self.xVel - self.friction * dt
        else
            self.xVel = 0
        end
    elseif self.xVel < 0 then
        if self.xVel + self.friction * dt < 0 then
            self.xVel = self.xVel + self.friction * dt
        else
            self.xVel = 0
        end
    end
end

function Player:syncPhysics()
    self.x, self.y = self.physics.body:getPosition()
    self.physics.body:setLinearVelocity(self.xVel, self.yVel)
end

function Player:beginContact(a, b, collision)
    if self.grounded == true then return end
    local nx, ny = collision:getNormal()
    if a == self.physics.fixture then
        if ny > 0 then
            self:land()
        end
    elseif b == self.physics.fixture then
        if ny < 0 then
            self:land()
        end
    end
end

function Player:land()
    self.yVel = 0
    self.grounded = true
end


function Player:endContact(a, b, collision)

end


function Player:draw()
    love.graphics.rectangle("fill", self.x, self.y, self.width / 2, self.height / 2)
end


--------------------------------------------------------------------------------------------------------

Afterthoughts: 

The overall experience on creating a game was very exciting, however, I was not able to fully understand the coding languages, resulting me to use someone else's code instead. However, I was able to get a gist of what he was doing in the code, and I was very fortunate to find this video. I was very happy with the result because I was able to take out something that I was thinking and code it into VisualStudioCode creating the first phase of the game. I could have went further into the video to create a much better outcome, but the codes are not created by myself and the outcomes later are different from my ideas because it just went from phase one to phase 10 or something, so I ended on phase one of my game. And I was pretty happy with the result. I was never so desperate about the ideas I have because it was just something that I thought of, but creating it was different, it was fun. If I have a higher understanding of the coding language I would definitly create my own game. The thing is that I hated to ask questions, and the result was, limited understandability to the things that I search up on the web. I do not regret the outcome of the final project, but indeed satisfied to create something out of someone else's code. Thank you for this assignment I really enjoyed it.

Ideas of the game satisfied:

Phase 1:
- Create map
- Set Background
- Make character move
