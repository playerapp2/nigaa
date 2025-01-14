-- Создаем функцию для изменения прозрачности
local function SetHighlightTransparency()
    for _, highlight in pairs(workspace:GetDescendants()) do
        if highlight:IsA("Highlight") then
            highlight.FillTransparency = 0.5
            highlight.OutlineTransparency = 0.5
        end
    end
end

-- Создаем подключение к сервису UserInputService
local UserInputService = game:GetService("UserInputService")

-- Обработчик нажатия клавиши
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.LeftControl then
        SetHighlightTransparency()
    end
end)

-- Подписываемся на появление новых объектов
workspace.DescendantAdded:Connect(function(descendant)
    if descendant:IsA("Highlight") then
        descendant.FillTransparency = 0.5
        descendant.OutlineTransparency = 0.5
    end
end)

-- Создаем функцию для изменения прозрачности
local function SetHighlightTransparency()
    for _, highlight in pairs(workspace:GetDescendants()) do
        if highlight:IsA("Highlight") then
            highlight.FillTransparency = 1
            highlight.OutlineTransparency = 1
        end
    end
end

-- Создаем подключение к сервису UserInputService
local UserInputService = game:GetService("UserInputService")

-- Обработчик нажатия клавиши
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.F2 then
        SetHighlightTransparency()
    end
end)

-- Подписываемся на появление новых объектов
workspace.DescendantAdded:Connect(function(descendant)
    if descendant:IsA("Highlight") then
        descendant.FillTransparency = 1
        descendant.OutlineTransparency = 1
    end
end)

local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Таблица для хранения оригинальных значений
local originalValues = {}

-- Функция для изменения параметров снарядов
local function modifyShells()
    local chassis = workspace.Vehicles.ChassisCursed_app.Gun
    for _, gun in pairs(chassis:GetChildren()) do
        if gun:FindFirstChild("Config") then
            if gun.Config:FindFirstChild("Shells") then
                for _, shellType in pairs(gun.Config.Shells:GetChildren()) do
                    -- Сохраняем оригинальные значения перед изменением
                    originalValues[shellType.Name] = {
                        ShellSpeed = shellType:FindFirstChild("ShellSpeed") and shellType.ShellSpeed.Value or nil,
                        RicochetAngle = shellType:FindFirstChild("RicochetAngle") and shellType.RicochetAngle.Value or nil,
                        Penetration = shellType:FindFirstChild("Penetration") and shellType.Penetration.Value or nil,
                        ExplosiveMult = shellType:FindFirstChild("ExplosiveMult") and shellType.ExplosiveMult.Value or nil,
                        BulletGravity = shellType:FindFirstChild("BulletGravity") and shellType.BulletGravity.Value or nil,
                        DamageMult = gun.Config:FindFirstChild("DamageMult") and gun.Config.DamageMult.Value or nil -- Сохраняем оригинальное значение DamageMult
                    }
                    
                    -- Выводим сохраненные значения
                    print("Сохранено для " .. shellType.Name .. ":")
                    for prop, value in pairs(originalValues[shellType.Name]) do
                        if value then
                            print(prop .. ": " .. tostring(value))
                        end
                    end
                    
                    -- Изменяем значения
                    if shellType:FindFirstChild("ShellSpeed") then
                        shellType.ShellSpeed.Value = 99999999
                    end
                    if shellType:FindFirstChild("RicochetAngle") then
                        shellType.RicochetAngle.Value = 99999999
                    end
                    if shellType:FindFirstChild("ExplosiveMult") then
                        shellType.ExplosiveMult.Value = 32.5
                    end
                    if shellType:FindFirstChild("Penetration") then
                        shellType.Penetration.Value = 99999999
                    end
                    if shellType:FindFirstChild("BulletGravity") then
                        shellType.BulletGravity.Value = 0
                    end
                end
            end
            
            -- Изменяем значение DamageMult
            if gun.Config:FindFirstChild("DamageMult") then
                originalValues["DamageMult"] = gun.Config.DamageMult.Value -- Сохраняем оригинальное значение
                gun.Config.DamageMult.Value = 10000 -- Устанавливаем новое значение
            end
        end
    end
end

-- Функция для восстановления оригинальных значений
local function restoreOriginalValues()
    local chassis = workspace.Vehicles.ChassisCursed_app.Gun
    for _, gun in pairs(chassis:GetChildren()) do
        if gun:FindFirstChild("Config") and gun.Config:FindFirstChild("Shells") then
            for _, shellType in pairs(gun.Config.Shells:GetChildren()) do
                if originalValues[shellType.Name] then
                    -- Восстанавливаем каждое значение
                    if shellType:FindFirstChild("ShellSpeed") and originalValues[shellType.Name].ShellSpeed then
                        shellType.ShellSpeed.Value = originalValues[shellType.Name].ShellSpeed
                        print("Восстановлено ShellSpeed для " .. shellType.Name .. ": " .. originalValues[shellType.Name].ShellSpeed)
                    end
                    
                    if shellType:FindFirstChild("RicochetAngle") and originalValues[shellType.Name].RicochetAngle then
                        shellType.RicochetAngle.Value = originalValues[shellType.Name].RicochetAngle
                        print("Восстановлено RicochetAngle для " .. shellType.Name .. ": " .. originalValues[shellType.Name].RicochetAngle)
                    end

                    if shellType:FindFirstChild("ExplosiveMult") and originalValues[shellType.Name].ExplosiveMult then
                        shellType.ExplosiveMult.Value = originalValues[shellType.Name].ExplosiveMult
                        print("Восстановлено ExplosiveMult для " .. shellType.Name .. ": " .. originalValues[shellType.Name].ExplosiveMult)
                    end
                    
                    if shellType:FindFirstChild("Penetration") and originalValues[shellType.Name].Penetration then
                        shellType.Penetration.Value = originalValues[shellType.Name].Penetration
                        print("Восстановлено Penetration для " .. shellType.Name .. ": " .. originalValues[shellType.Name].Penetration)
                    end
                    
                    if shellType:FindFirstChild("BulletGravity") and originalValues[shellType.Name].BulletGravity then
                        shellType.BulletGravity.Value = originalValues[shellType.Name].BulletGravity
                        print("Восстановлено BulletGravity для " .. shellType.Name .. ": " .. originalValues[shellType.Name].BulletGravity)
                    end
                end
            end
            
            -- Восстанавливаем значение DamageMult
            if gun.Config:FindFirstChild("DamageMult") and originalValues["DamageMult"] then
                gun.Config.DamageMult.Value = originalValues["DamageMult"] -- Восстанавливаем оригинальное значение
                print("Восстановлено DamageMult: " .. originalValues["DamageMult"])
            end
        end
    end
    -- Очищаем таблицу оригинальных значений
    originalValues = {}
end

-- Функция для установки биндов
local function setupBind()
    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if input.KeyCode == Enum.KeyCode.sdf then
            modifyShells()
        elseif input.KeyCode == Enum.KeyCode.sdfsd then
            restoreOriginalValues()
        end
    end)
end

-- Устанавливаем бинд при первом запуске
setupBind()

-- Устанавливаем бинд после каждого респавна
player.CharacterAdded:Connect(function()
    setupBind()
end)

local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Таблица для хранения оригинальных значений
local originalValues = {}

-- Функция для изменения параметров снарядов
local function modifyShells()
    local chassis = workspace.Vehicles.ChassisCursed_app.Gun
    for _, gun in pairs(chassis:GetChildren()) do
        if gun:FindFirstChild("Config") then
            if gun.Config:FindFirstChild("Shells") then
                for _, shellType in pairs(gun.Config.Shells:GetChildren()) do
                    -- Сохраняем оригинальные значения перед изменением
                    originalValues[shellType.Name] = {
                        ShellSpeed = shellType:FindFirstChild("ShellSpeed") and shellType.ShellSpeed.Value or nil,
                        RicochetAngle = shellType:FindFirstChild("RicochetAngle") and shellType.RicochetAngle.Value or nil,
                        Penetration = shellType:FindFirstChild("Penetration") and shellType.Penetration.Value or nil,
                        ExplosiveMult = shellType:FindFirstChild("ExplosiveMult") and shellType.ExplosiveMult.Value or nil,
                        BulletGravity = shellType:FindFirstChild("BulletGravity") and shellType.BulletGravity.Value or nil,
                        DamageMult = gun.Config:FindFirstChild("DamageMult") and gun.Config.DamageMult.Value or nil -- Сохраняем оригинальное значение DamageMult
                    }
                    
                    -- Выводим сохраненные значения
                    print("Сохранено для " .. shellType.Name .. ":")
                    for prop, value in pairs(originalValues[shellType.Name]) do
                        if value then
                            print(prop .. ": " .. tostring(value))
                        end
                    end
                    
                    -- Изменяем значения
                    if shellType:FindFirstChild("ShellSpeed") then
                        shellType.ShellSpeed.Value = (shellType.ShellSpeed.Value or 0) + 1 -- Увеличиваем ShellSpeed на 1
                    end
                    if shellType:FindFirstChild("RicochtAngle") then
                        shellType.RicocetAngle.Value = 99999999
                    end
                    if shellType:FindFirstChild("ExplosieMult") then
                        shellType.ExposveMult.Value = 4
                    end
                    if shellType:FindFirstChild("Penetration") then
                        shellType.ShellSpeed.Value = (shellType.ShellSpeed.Value or 0) + 200 -- Увеличиваем ShellSpeed на 1
                    end
                    if shellType:FindFirstChild("Buletravity") then
                        shellType.Buletravity.Value = 0
                    end
                end
            end
            
            -- Изменяем значение DamageMult
            if gun.Config:FindFirstChild("DamageMult") then
                originalValues["DamageMult"] = gun.Config.DamageMult.Value -- Сохраняем оригинальное значение
                gun.Config.DamageMult.Value = 10000 -- Устанавливаем новое значение
            end
        end
    end
end

-- Функция для восстановления оригинальных значений
local function restoreOriginalValues()
    local chassis = workspace.Vehicles.ChassisCursed_app.Gun
    for _, gun in pairs(chassis:GetChildren()) do
        if gun:FindFirstChild("Config") and gun.Config:FindFirstChild("Shells") then
            for _, shellType in pairs(gun.Config.Shells:GetChildren()) do
                if originalValues[shellType.Name] then
                    -- Восстанавливаем каждое значение
                    if shellType:FindFirstChild("ShellSpeed") and originalValues[shellType.Name].ShellSpeed then
                        shellType.ShellSpeed.Value = originalValues[shellType.Name].ShellSpeed
                        print("Восстановлено ShellSpeed для " .. shellType.Name .. ": " .. originalValues[shellType.Name].ShellSpeed)
                    end
                    
                    if shellType:FindFirstChild("RicochetAngle") and originalValues[shellType.Name].RicochetAngle then
                        shellType.RicochetAngle.Value = originalValues[shellType.Name].RicochetAngle
                        print("Восстановлено RicochetAngle для " .. shellType.Name .. ": " .. originalValues[shellType.Name].RicochetAngle)
                    end

                    if shellType:FindFirstChild("ExplosiveMult") and originalValues[shellType.Name].ExplosiveMult then
                        shellType.ExplosiveMult.Value = originalValues[shellType.Name].ExplosiveMult
                        print("Восстановлено ExplosiveMult для " .. shellType.Name .. ": " .. originalValues[shellType.Name].ExplosiveMult)
                    end
                    
                    if shellType:FindFirstChild("Penetration") and originalValues[shellType.Name].Penetration then
                        shellType.Penetration.Value = originalValues[shellType.Name].Penetration
                        print("Восстановлено Penetration для " .. shellType.Name .. ": " .. originalValues[shellType.Name].Penetration)
                    end
                    
                    if shellType:FindFirstChild("BulletGravity") and originalValues[shellType.Name].BulletGravity then
                        shellType.BulletGravity.Value = originalValues[shellType.Name].BulletGravity
                        print("Восстановлено BulletGravity для " .. shellType.Name .. ": " .. originalValues[shellType.Name].BulletGravity)
                    end
                end
            end
            
            -- Восстанавливаем значение DamageMult
            if gun.Config:FindFirstChild("DamageMult") and originalValues["DamageMult"] then
                gun.Config.DamageMult.Value = originalValues["DamageMult"] -- Восстанавливаем оригинальное значение
                print("Восстановлено DamageMult: " .. originalValues["DamageMult"])
            end
        end
    end
    -- Очищаем таблицу оригинальных значений
    originalValues = {}
end

-- Функция для установки биндов
local function setupBind()
    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if input.KeyCode == Enum.KeyCode.F7 then
            modifyShells()
        elseif input.KeyCode == Enum.KeyCode.F8 then
            restoreOriginalValues()
        end
    end)
end

-- Устанавливаем бинд при первом запуске
setupBind()

-- Устанавливаем бинд после каждого респавна
player.CharacterAdded:Connect(function()
    setupBind()
end)

local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local Players = game:GetService("Players")

-- Функция для обработки существующих транспортных средств
local function handleExistingVehicles()
    for _, vehicle in ipairs(Workspace.Vehicles:GetChildren()) do
        local highlight = vehicle:FindFirstChild("Highlight")
        if highlight then
            highlight:Destroy()
        end
    end
end

-- Функция для обработки новых транспортных средств
local function onVehicleAdded(vehicle)
    local highlight = vehicle:FindFirstChild("Highlight")
    if highlight then
        highlight:Destroy()
    end
end

-- Обработка нажатия клавиши
local function onInputBegan(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.LeftControl then
        handleExistingVehicles()
        -- Подключаем обработчик новых транспортных средств
        Workspace.Vehicles.ChildAdded:Connect(onVehicleAdded)
    end
end

-- Подключаем обработчик нажатия клавиши для локального игрока
local player = Players.LocalPlayer
UserInputService.InputBegan:Connect(onInputBegan)

local function createHighlight(vehicle)
    if not vehicle:FindFirstChild("Highlight") then
        local highlight = Instance.new("Highlight")
        highlight.Name = "Highlight"
        highlight.Parent = vehicle
    end
end

-- Следим за существующими объектами
for _, vehicle in ipairs(workspace.Vehicles:GetChildren()) do
    createHighlight(vehicle)
end

-- Следим за новыми объектами
workspace.Vehicles.ChildAdded:Connect(function(vehicle)
    createHighlight(vehicle)
end)

-- Следим за изменениями в существующих объектах
workspace.Vehicles.DescendantRemoving:Connect(function(instance)
    if instance:IsA("Highlight") then
        local vehicle = instance.Parent
        if vehicle and vehicle:IsDescendantOf(workspace.Vehicles) then
            -- Небольшая задержка для уверенности, что Highlight действительно был удален
            task.defer(function()
                createHighlight(vehicle)
            end)
        end
    end
end)

local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

local function configureTurrets()
    local turret = workspace.Vehicles.ChassisCursed_app.Turret

    for _, tank in ipairs(turret:GetChildren()) do
        if tank:IsA("Model") then
            local damageMult = tank:FindFirstChild("Mantlet") and tank.Mantlet:FindFirstChild("Secondary") and tank.Mantlet.Secondary:FindFirstChild("Config") and tank.Mantlet.Secondary.Config:FindFirstChild("DamageMult")
            if damageMult then
                damageMult.Value = 10000
            end
        end
    end

    local turretFolder = workspace.Vehicles.ChassisCursed_app.Turret
    local shells = turretFolder:GetChildren()

    for _, tankModel in ipairs(shells) do
        if tankModel:IsA("Model") then
            local shellConfig = tankModel:FindFirstChild("Mantlet") and tankModel.Mantlet:FindFirstChild("Secondary") and tankModel.Mantlet.Secondary:FindFirstChild("Config") and tankModel.Mantlet.Secondary.Config:FindFirstChild("Shells")
            
            if shellConfig then
                local apShells = shellConfig:FindFirstChild("AP")
                if apShells then
                    local shellSpeed = apShells:FindFirstChild("ShellSpeed")
                    if shellSpeed and shellSpeed:IsA("IntValue") then
                        shellSpeed.Value = 2000
                    end

                    local bulletGravity = apShells:FindFirstChild("BulletGravity")
                    if bulletGravity and bulletGravity:IsA("IntValue") then
                        bulletGravity.Value = 0
                    end
                end
            end
        end
    end
end

-- Функция для обработки нажатия клавиши 'M'
local function onInputBegan(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.F10 then
        configureTurrets()
    end
end

-- Подписка на событие InputBegan
UserInputService.InputBegan:Connect(onInputBegan)

-- Функция для обработки события CharacterAdded
local function onCharacterAdded(newCharacter)
    -- Обновляем ссылку на character
    player.Character = newCharacter
    
    -- Дополнительно, можно повторно подписаться на события или выполнить какую-либо логику, если нужно
end

-- Подписка на событие CharacterAdded
player.CharacterAdded:Connect(onCharacterAdded)
