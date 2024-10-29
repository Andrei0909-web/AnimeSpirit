# AnimeSpirit.lua
local function greet()
    print("Hello, welcome to Anime Spirit!")
end

greet()

local function greet()
    print("Hello, welcome to Anime Spirit!")
end

greet()

-- Variables and Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Create the Transformation Tool (as discussed previously)
local transformationTool = Instance.new("Tool")
transformationTool.Name = "TransformationTool"
transformationTool.RequiresHandle = true
transformationTool.CanBeDropped = true

-- Create a Handle for the Tool
local handle = Instance.new("Part")
handle.Name = "Handle"
handle.Size = Vector3.new(1, 1, 1)
handle.Anchored = false
handle.CanCollide = false
handle.Transparency = 1
handle.Parent = transformationTool

-- Parent the Tool to ReplicatedStorage
transformationTool.Parent = ReplicatedStorage

-- LocalScript for the transformation logic
local scriptToExecute = [[
    local Player = game.Players.LocalPlayer
    local Tool = script.Parent
    local transformed = false 
    local humanoid 

    local function transform(Character)
        humanoid = Character:FindFirstChildOfClass("Humanoid")
        
        if humanoid and humanoid.Health > 0 then
            if not transformed then
                humanoid.Health = humanoid.Health + 1000 
                humanoid.MaxHealth = humanoid.Health 

                transformed = true
                Player:SendNotification({
                    Title = "Transformation",
                    Text = "You have transformed!",
                    Duration = 2
                })
            else
                humanoid.Health = humanoid.Health - 1000 
                transformed = false
                Player:SendNotification({
                    Title = "Transformation",
                    Text = "You have returned to normal!",
                    Duration = 2
                })
            end
        end
    end

    Tool.Activated:Connect(function()
        if Player.Character then
            transform(Player.Character)
        end
    end)

    Player.CharacterAdded:Connect(function(newCharacter)
        transformed = false 
    end)

]]

-- Create a LocalScript within the Tool
local toolScript = Instance.new("LocalScript")
toolScript.Source = "loadstring(" .. game:GetService("HttpService"):JSONEncode(scriptToExecute) .. ")()"
toolScript.Parent = transformationTool

print("Transformation Tool created and added to ReplicatedStorage.")
