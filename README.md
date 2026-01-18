
--cath brainrot by derickexe
print("Script Atualizado: Adicionado Recall Spam...")

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Derick God Hub 🧠",
   LoadingTitle = "Finalizando Configurações...",
   ConfigurationSaving = { Enabled = false }
})

-- Variáveis de Controle
local _G = {
    AutoAttack = false,
    AttackRange = 60,
    AutoCollect = false,
    AutoAttackBoss = false,
    AutoClaimSpin = false,
    AutoSpin = false,
    AutoPlaceLucky = false,
    AutoOpenLucky = false,
    AutoRecall = false,
    SelectedSword = "Nenhuma",
    SelectedCube = "Nenhuma"
}

-- Abas
local MainTab = Window:CreateTab("Main", 4483362458)
local LuckyTab = Window:CreateTab("Auto Lucky Boock", 4483362458)
local SpinTab = Window:CreateTab("Spin Well", 4483362458)
local BuyTab = Window:CreateTab("Auto Buy", 4483362458)

-------------------------------------------------------------------------------
-- MAIN & BOSS
-------------------------------------------------------------------------------
MainTab:CreateToggle({
   Name = "Auto Attack Brainrots",
   CurrentValue = false,
   Callback = function(Value) _G.AutoAttack = Value end,
})

MainTab:CreateSlider({
   Name = "Distância do Ataque",
   Range = {10, 150},
   Increment = 5,
   Suffix = "Studs",
   CurrentValue = 60,
   Callback = function(Value) _G.AttackRange = Value end,
})

MainTab:CreateToggle({
   Name = "Auto Collect Money",
   CurrentValue = false,
   Callback = function(Value) _G.AutoCollect = Value end,
})

MainTab:CreateToggle({
   Name = "Auto Attack Boss",
   CurrentValue = false,
   Callback = function(Value) _G.AutoAttackBoss = Value end,
})

-------------------------------------------------------------------------------
-- LUCKY BLOCKS & BASE (PLACE, OPEN E RECALL)
-------------------------------------------------------------------------------
LuckyTab:CreateToggle({
   Name = "Auto Place Lucky Block",
   CurrentValue = false,
   Callback = function(Value) _G.AutoPlaceLucky = Value end,
})

LuckyTab:CreateToggle({
   Name = "Auto Open Lucky Block",
   CurrentValue = false,
   Callback = function(Value) _G.AutoOpenLucky = Value end,
})

LuckyTab:CreateToggle({
   Name = "Auto Recall Brainrots",
   CurrentValue = false,
   Callback = function(Value) _G.AutoRecall = Value end,
})

-- Loop de Colocar (Com Delay de 500ms)
task.spawn(function()
    while true do
        task.wait(0.1)
        if _G.AutoPlaceLucky then
            for i = 1, 165 do
                if not _G.AutoPlaceLucky then break end
                game:GetService("ReplicatedStorage").Remotes.Events.Place:FireServer("Slot" .. i)
                task.wait(0.5)
            end
        end
    end
end)

-- Loop de Abrir (Spam)
task.spawn(function()
    while true do
        task.wait(0.1)
        if _G.AutoOpenLucky then
            for i = 1, 165 do
                if not _G.AutoOpenLucky then break end
                game:GetService("ReplicatedStorage").Remotes.Events.OpenLuckyBlock:FireServer("Slot" .. i)
            end
        end
    end
end)

-- Loop de Recall (Spam)
task.spawn(function()
    while true do
        task.wait(0.1)
        if _G.AutoRecall then
            for i = 1, 165 do
                if not _G.AutoRecall then break end
                game:GetService("ReplicatedStorage").Remotes.Events.Recall:FireServer("Slot" .. i)
            end
        end
    end
end)

-------------------------------------------------------------------------------
-- SPIN WELL
-------------------------------------------------------------------------------
SpinTab:CreateToggle({
   Name = "Claim Daily Spin",
   CurrentValue = false,
   Callback = function(Value) _G.AutoClaimSpin = Value end,
})

SpinTab:CreateToggle({
   Name = "Spin Wheel",
   CurrentValue = false,
   Callback = function(Value) _G.AutoSpin = Value end,
})

-------------------------------------------------------------------------------
-- AUTO BUY
-------------------------------------------------------------------------------
local swordList = {"Nenhuma"}
for i = 1, 13 do table.insert(swordList, "Sword_"..i) end

BuyTab:CreateDropdown({
   Name = "Select Sword",
   Options = swordList,
   CurrentOption = {"Nenhuma"},
   MultipleOptions = false,
   Callback = function(Option) _G.SelectedSword = Option[1] or Option end,
})

local cubeList = {"Nenhuma"}
for i = 1, 6 do table.insert(cubeList, "Cube_"..i) end

BuyTab:CreateDropdown({
   Name = "Select Pokeballs",
   Options = cubeList,
   CurrentOption = {"Nenhuma"},
   MultipleOptions = false,
   Callback = function(Option) _G.SelectedCube = Option[1] or Option end,
})

-------------------------------------------------------------------------------
-- LOOPS TÉCNICOS
-------------------------------------------------------------------------------
game:GetService("RunService").Heartbeat:Connect(function()
    if _G.AutoAttack then
        local char = game.Players.LocalPlayer.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        if hrp then
            for _, s in ipairs(workspace.Brainrots.Server:GetChildren()) do
                for _, t in ipairs(s:GetChildren()) do
                    if (t:GetPivot().Position - hrp.Position).Magnitude <= _G.AttackRange then
                        game:GetService("ReplicatedStorage").Remotes.Events.Attack:FireServer({[1] = t})
                    end
                end
            end
        end
    end

    if _G.AutoAttackBoss then
        local bossF = workspace:FindFirstChild("BossEvent") and workspace.BossEvent:FindFirstChild("Server")
        if bossF then
            for _, boss in ipairs(bossF:GetChildren()) do
                game:GetService("ReplicatedStorage").Remotes.Events.AttackBoss:FireServer({[1] = boss})
            end
        end
    end

    if _G.AutoClaimSpin then game:GetService("ReplicatedStorage").Remotes.Events.ClaimDailySpin:FireServer() end
    if _G.AutoSpin then game:GetService("ReplicatedStorage").Remotes.Events.SpinWheel:FireServer() end
end)

task.spawn(function()
    while true do
        task.wait(10)
        if _G.AutoCollect then
            local inv = game:GetService("Players").LocalPlayer.PlayerData.Inventory:GetChildren()
            for _, item in ipairs(inv) do
                game:GetService("ReplicatedStorage").Remotes.Events.CollectCash:FireServer(item.Name)
            end
        end
    end
end)

task.spawn(function()
    while task.wait(0.1) do
        if _G.SelectedSword ~= "Nenhuma" then
            game:GetService("ReplicatedStorage").Remotes.Events.PurchaseSword:FireServer(_G.SelectedSword)
        end
        if _G.SelectedCube ~= "Nenhuma" then
            game:GetService("ReplicatedStorage").Remotes.Events.PurchaseCube:FireServer(_G.SelectedCube)
        end
    end
end)

Rayfield:Notify({Title = "Sucesso", Content = "Recall adicionado!", Duration = 3})
