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

Rayfield:Notify({
    Title = "Sucesso",
    Content = "Script carregado com sucesso!",
    Duration = 3
})
