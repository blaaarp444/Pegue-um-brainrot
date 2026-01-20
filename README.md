-- cath brainrot by derickexe
print("Script Atualizado: Versão Limpa e Funcional")

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Derick God Hub 🧠",
   LoadingTitle = "Finalizando Configurações...",
   ConfigurationSaving = { Enabled = false }
})

-- VARIÁVEIS
_G.AutoCollect = false
_G.AutoAttackBoss = false
_G.AutoClaimSpin = false
_G.AutoSpin = false
_G.AutoPlaceLucky = false
_G.AutoOpenLucky = false
_G.AutoRecall = false

_G.AutoBuy = false
_G.SelectedSwords = {}
_G.SelectedCubes = {}

-- ABAS
local MainTab = Window:CreateTab("Main", 4483362458)
local LuckyTab = Window:CreateTab("Auto Lucky Block", 4483362458)
local SpinTab = Window:CreateTab("Spin Well", 4483362458)
local BuyTab = Window:CreateTab("Auto Buy", 4483362458)

-------------------------------------------------------------------------------
-- MAIN
-------------------------------------------------------------------------------
_G.AutoFarm = false

MainTab:CreateToggle({
   Name = "Auto Farm",
   CurrentValue = false,
   Callback = function(v)
       _G.AutoFarm = v
   end,
})

MainTab:CreateToggle({
   Name = "Auto Collect Coins",
   CurrentValue = false,
   Callback = function(v) _G.AutoCollect = v end,
})

MainTab:CreateToggle({
   Name = "Auto Attack Boss",
   CurrentValue = false,
   Callback = function(v) _G.AutoAttackBoss = v end,
})

-------------------------------------------------------------------------------
-- LUCKY BLOCK
-------------------------------------------------------------------------------
LuckyTab:CreateToggle({
   Name = "Auto Place Lucky Block",
   CurrentValue = false,
   Callback = function(v) _G.AutoPlaceLucky = v end,
})

LuckyTab:CreateToggle({
   Name = "Auto Open Lucky Block",
   CurrentValue = false,
   Callback = function(v) _G.AutoOpenLucky = v end,
})

LuckyTab:CreateToggle({
   Name = "Auto Recall Brainrots",
   CurrentValue = false,
   Callback = function(v) _G.AutoRecall = v end,
})

-------------------------------------------------------------------------------
-- SPIN
-------------------------------------------------------------------------------
SpinTab:CreateToggle({
   Name = "Claim Daily Spin",
   CurrentValue = false,
   Callback = function(v) _G.AutoClaimSpin = v end,
})

SpinTab:CreateToggle({
   Name = "Spin Wheel",
   CurrentValue = false,
   Callback = function(v) _G.AutoSpin = v end,
})

-------------------------------------------------------------------------------
-- AUTO BUY
-------------------------------------------------------------------------------
BuyTab:CreateToggle({
   Name = "Auto Buy (ON/OFF)",
   CurrentValue = false,
   Callback = function(v) _G.AutoBuy = v end,
})

local swordList = {"Nenhuma"}
for i = 1, 13 do table.insert(swordList, "Sword_"..i) end

BuyTab:CreateDropdown({
   Name = "Select Swords",
   Options = swordList,
   CurrentOption = {},
   MultipleOptions = true,
   Callback = function(opts)
       _G.SelectedSwords = opts
   end,
})

local cubeList = {"Nenhuma"}
for i = 1, 6 do table.insert(cubeList, "Cube_"..i) end

BuyTab:CreateDropdown({
   Name = "Select Pokeballs",
   Options = cubeList,
   CurrentOption = {},
   MultipleOptions = true,
   Callback = function(opts)
       _G.SelectedCubes = opts
   end,
})

-------------------------------------------------------------------------------
-- LUCKY BLOCK LOOPS
-------------------------------------------------------------------------------
task.spawn(function()
    while task.wait(0.1) do
        if _G.AutoPlaceLucky then
            for i = 1, 165 do
                if not _G.AutoPlaceLucky then break end
                game:GetService("ReplicatedStorage").Remotes.Events.Place:FireServer("Slot"..i)
                task.wait(0.5)
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.1) do
        if _G.AutoOpenLucky then
            for i = 1, 165 do
                if not _G.AutoOpenLucky then break end
                game:GetService("ReplicatedStorage").Remotes.Events.OpenLuckyBlock:FireServer("Slot"..i)
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.1) do
        if _G.AutoRecall then
            for i = 1, 165 do
                if not _G.AutoRecall then break end
                game:GetService("ReplicatedStorage").Remotes.Events.Recall:FireServer("Slot"..i)
            end
        end
    end
end)

-------------------------------------------------------------------------------
-- LOOPS PRINCIPAIS
-------------------------------------------------------------------------------
game:GetService("RunService").Heartbeat:Connect(function()
    if _G.AutoAttackBoss then
        local bossF = workspace:FindFirstChild("BossEvent") and workspace.BossEvent:FindFirstChild("Server")
        if bossF then
            for _, boss in ipairs(bossF:GetChildren()) do
                game:GetService("ReplicatedStorage").Remotes.Events.AttackBoss:FireServer({[1] = boss})
            end
        end
    end

    if _G.AutoClaimSpin then
        game:GetService("ReplicatedStorage").Remotes.Events.ClaimDailySpin:FireServer()
    end

    if _G.AutoSpin then
        game:GetService("ReplicatedStorage").Remotes.Events.SpinWheel:FireServer()
    end
end)

task.spawn(function()
    while task.wait(10) do
        if _G.AutoCollect then
            for _, item in ipairs(game.Players.LocalPlayer.PlayerData.Inventory:GetChildren()) do
                game:GetService("ReplicatedStorage").Remotes.Events.CollectCash:FireServer(item.Name)
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.3) do
        if _G.AutoBuy then
            for _, sword in ipairs(_G.SelectedSwords) do
                if sword ~= "Nenhuma" then
                    game:GetService("ReplicatedStorage").Remotes.Events.PurchaseSword:FireServer(sword)
                end
            end
            for _, cube in ipairs(_G.SelectedCubes) do
                if cube ~= "Nenhuma" then
                    game:GetService("ReplicatedStorage").Remotes.Events.PurchaseCube:FireServer(cube)
                end
            end
        end
    end
end)

-- ================= AUTO FARM (SEM GUI / CONTROLADO PELO RAYFIELD) =================

-- Usa: _G.AutoFarm = true / false

-- ================= CONFIG =================
local TELEPORT_DISTANCE = 11
local WAIT_BEFORE_THROW = 5
-- ==========================================

-- Cubes normais
local normalCubes = {
    ["Cube.009"] = true,
    ["Cube.136"] = true,
    ["Cube.082"] = true,
    ["Cube.047"] = true,
    ["Cube.041"] = true,
    ["Cube.079"] = true
}

-- Cubes DIAMOND
local diamondOnlyCubes = {
    ["Cube.071"] = true,
    ["Cube.015"] = true,
    ["Cube.106"] = true,
    ["Cube.121"] = true
}

-- Serviços
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local camera = workspace.CurrentCamera

-- Remotes
local AttackRemote = ReplicatedStorage.Remotes.Events.Attack
local EquipCube = ReplicatedStorage.Remotes.Events.EquipCube
local ThrowCube = ReplicatedStorage.Remotes.Events.ThrowCube

-- Controle: 1 pokeball por vida
local alreadyThrown = {}

-- ===== THROW (INTACTO) =====
local THROW_POWER = 100
local function throwBallExact()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")

    local direction = camera.CFrame.LookVector
    local id = player.UserId .. "_" .. os.clock()
    local origin = root.Position
    local velocity = direction * THROW_POWER

    ThrowCube:FireServer(direction, id, origin, velocity)
end
-- ======================================

-- Teleport + câmera
local function teleportAndLookAtCube(cube)
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")

    local targetPos = cube.Position
    local dir = (targetPos - root.Position).Unit
    local teleportPos = targetPos - (dir * TELEPORT_DISTANCE)

    local cf = CFrame.lookAt(teleportPos, targetPos)
    root.CFrame = cf
    camera.CFrame = cf
end

-- ================= LOOP PRINCIPAL =================
task.spawn(function()
    while true do
        if not _G.AutoFarm then
            task.wait(0.3)
            continue
        end

        local Brainrots = workspace:FindFirstChild("Brainrots")
        if not Brainrots then
            task.wait(0.2)
            continue
        end

        local Client = Brainrots:FindFirstChild("Client")
        local Server = Brainrots:FindFirstChild("Server")
        if not Client or not Server then
            task.wait(0.2)
            continue
        end

        for _, clientSpawn in pairs(Client:GetChildren()) do
            if not _G.AutoFarm then break end
            if alreadyThrown[clientSpawn] then continue end

            for _, cube in pairs(clientSpawn:GetChildren()) do
                if not _G.AutoFarm then break end

                local valid = false
                if normalCubes[cube.Name] then valid = true end
                if diamondOnlyCubes[cube.Name] and cube:FindFirstChild("Star Sparks") then
                    valid = true
                end

                if valid then
                    local fullId = clientSpawn.Name
                    local baseId = fullId:match("^(%d+)_")
                    if not baseId then break end

                    local serverBase = Server:FindFirstChild(baseId)
                    if not serverBase then break end

                    local serverSpawn = serverBase:FindFirstChild(fullId)
                    if not serverSpawn then break end

                    AttackRemote:FireServer({ serverSpawn })
                    teleportAndLookAtCube(cube)
                    EquipCube:FireServer()

                    local t = 0
                    while t < WAIT_BEFORE_THROW and _G.AutoFarm do
                        task.wait(0.1)
                        t += 0.1
                    end

                    alreadyThrown[clientSpawn] = true
                    clientSpawn.AncestryChanged:Connect(function(_, parent)
                        if not parent then
                            alreadyThrown[clientSpawn] = nil
                        end
                    end)

                    if Client:FindFirstChild(fullId) then
                        throwBallExact()
                    end
                    break
                end
            end
        end

        task.wait(0.2)
    end
end)

-- ========================================================================

Rayfield:Notify({
    Title = "Sucesso",
    Content = "Script carregado com sucesso!",
    Duration = 3
})
