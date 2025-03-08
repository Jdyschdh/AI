local function teleportToWorld(worldNumber)
    local worlds = {
        [1] = Vector3.new(0, 100, 0),  -- إحداثيات العالم الأول
        [2] = Vector3.new(5000, 100, 0), -- إحداثيات العالم الثاني
        [3] = Vector3.new(10000, 100, 0) -- إحداثيات العالم الثالث
    }

    if worlds[worldNumber] then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(worlds[worldNumber])
        print("Teleported to World " .. worldNumber)
    end
end

local function collectItem(itemName)
    for _, item in pairs(workspace:GetChildren()) do
        if item:IsA("Model") and item:FindFirstChild("Item") then
            if item.Name == itemName then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = item.PrimaryPart.CFrame
                print("Item collected: " .. itemName)
            end
        end
    end
end

local function collectMoney()
    local player = game.Players.LocalPlayer
    if player.leaderstats.Cash.Value < 100 then
        print("AI: Collecting money...")
        local money = workspace:FindFirstChild("MoneyBag")
        if money then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = money.CFrame
            print("Money collected")
        end
    end
end

local function farmLevel()
    local player = game.Players.LocalPlayer
    local currentLevel = player.leaderstats.Level.Value

    while currentLevel < 100 do
        print("AI: Farming to level up...")
        local enemies = workspace:FindPartsInRegion3(workspace.CurrentCamera.CFrame.Position, Vector3.new(50, 50, 50), nil)
        for _, enemy in pairs(enemies) do
            if enemy:IsA("Model") and enemy:FindFirstChild("Humanoid") then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame
                game.Players.LocalPlayer.Character.Humanoid:MoveTo(enemy.HumanoidRootPart.Position)
                print("AI: Attacking enemy to level up!")
            end
        end
        currentLevel = player.leaderstats.Level.Value
        wait(1)
    end
end

local function aiDecisionMaking()
    local player = game.Players.LocalPlayer
    local currentHealth = player.Character and player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.Health or 100
    local leaderstats = player:FindFirstChild("leaderstats")
    local currentCash = leaderstats and leaderstats:FindFirstChild("Cash") and leaderstats.Cash.Value or 0
    local currentLevel = leaderstats and leaderstats:FindFirstChild("Level") and leaderstats.Level.Value or 1
    local requirements = {}

    if currentHealth < 50 then table.insert(requirements, "HealthPotion") end
    if currentCash < 100 then table.insert(requirements, "Money") end

    local hasSword = false
    for _, item in pairs(player.Backpack:GetChildren()) do
        if item.Name == "Sword" then hasSword = true break end
    end
    if not hasSword then table.insert(requirements, "Sword") end
    if currentLevel < 100 then table.insert(requirements, "LevelUp") end

    for world = 1, 3 do
        teleportToWorld(world)
        wait(2)  

        for _, requirement in pairs(requirements) do
            if requirement == "Money" then collectMoney()
            elseif requirement == "Sword" then collectItem("Sword")
            elseif requirement == "HealthPotion" then collectItem("HealthPotion")
            elseif requirement == "LevelUp" then farmLevel()
            end
        end
    end

    print("AI: Finished farming in all worlds!")
end

aiDecisionMaking()
