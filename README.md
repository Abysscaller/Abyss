--[[
  ☠️ Abyss Hab Script ☠️
  Feito para: AbyssCallerX
  Executor: Delta
  Funções: Auto Farm, Teleporte, Aimbot, Proteções
]]


-- GUI Base
local AbyssHab = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")

AbyssHab.Name = "AbyssHab"
AbyssHab.Parent = game.CoreGui

MainFrame.Name = "MainFrame"
MainFrame.Parent = AbyssHab
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.25, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 400, 0, 300)
MainFrame.Active = true
MainFrame.Draggable = true

Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Text = "☠️ Abyss Hab - by AbyssCallerX ☠️"
Title.TextColor3 = Color3.fromRGB(255, 0, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16


-- Auto Farm
function AutoFarm()
    while _G.AutoFarm do
        local player = game.Players.LocalPlayer
        local character = player.Character
        local enemy = workspace.Enemies:FindFirstChild("Bandit")

        if enemy then
            repeat
                character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
                wait(0.2)
                game:GetService("VirtualInputManager"):SendKeyEvent(true, "Z", false, game)
                wait(0.1)
            until enemy.Humanoid.Health <= 0 or not _G.AutoFarm
        end
        wait(0.5)
    end
end

_G.AutoFarm = true
spawn(AutoFarm)


-- Teleporte
function TeleportToIsland(islandName)
    local island = workspace.Map:FindFirstChild(islandName)
    if island then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = island.Position + Vector3.new(0, 30, 0)
    end
end

function TeleportToPlayer(playerName)
    local target = game.Players:FindFirstChild(playerName)
    if target then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame
    end
end


-- Aimbot
local RunService = game:GetService("RunService")
local targetPlayer = nil

function GetClosestEnemy()
    local closest, shortest = nil, math.huge
    for _, v in pairs(workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            local dist = (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude
            if dist < shortest then
                shortest = dist
                closest = v
            end
        end
    end
    return closest
end

RunService.RenderStepped:Connect(function()
    if _G.Aimbot then
        targetPlayer = GetClosestEnemy()
        if targetPlayer then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(
                game.Players.LocalPlayer.Character.HumanoidRootPart.Position,
                targetPlayer.HumanoidRootPart.Position
            )
        end
    end
end)

_G.Aimbot = true


-- Proteções
game:GetService("Players").LocalPlayer.Idled:Connect(function()
    game:GetService("VirtualUser"):Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    wait(1)
    game:GetService("VirtualUser"):Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

function AutoHop()
    local Players = game:GetService("Players")
    for _, player in pairs(Players:GetPlayers()) do
        if player:IsInGroup(1200769) then
            game:GetService("TeleportService"):Teleport(game.PlaceId)
        end
    end
end

while true do
    wait(60)
    pcall(AutoHop)
end



-- Auto Farm de Sia (Sea Event)
_G.SeaFarm = true

function FarmSeaEvent()
    while _G.SeaFarm do
        for _, entity in pairs(workspace.Enemies:GetChildren()) do
            if entity:FindFirstChild("Humanoid") and entity:FindFirstChild("HumanoidRootPart") then
                if entity.Name:find("Sea") or entity.Name:find("Shark") or entity.Name:find("Pirate") then
                    repeat
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
                            entity.HumanoidRootPart.CFrame * CFrame.new(0, 20, 0)
                        wait(0.2)
                        game:GetService("VirtualInputManager"):SendKeyEvent(true, "Z", false, game)
                        wait(0.1)
                    until entity.Humanoid.Health <= 0 or not _G.SeaFarm
                end
            end
        end
        wait(1)
    end
end

spawn(FarmSeaEvent)



-- Auto Coletor de Frutas físicas no mapa
_G.FruitFarm = true

function FruitSniper()
    while _G.FruitFarm do
        for _, v in pairs(workspace:GetChildren()) do
            if v:IsA("Tool") and v.Handle and string.find(v.Name:lower(), "fruit") then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                wait(0.5)
                firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v.Handle, 0)
                firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, v.Handle, 1)
                print("🍇 Fruta coletada: "..v.Name)
                wait(2)
            end
        end
        wait(5)
    end
end

spawn(FruitSniper)
