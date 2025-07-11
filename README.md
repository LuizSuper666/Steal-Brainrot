-- Steal Brainrot Menu Mobile
-- Criado por: SpooferLuck

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Variáveis de estado
local skywalkEnabled = false
local espEnabled = false
local speedBoostEnabled = false
local antiMovementEnabled = false
local noClipEnabled = false
local godmodeEnabled = false -- Nova variável de estado
local normalSpeed = 16

-- Variáveis para sistema de movimento
local isDragging = false
local dragStart = nil
local startPos = nil

-- Variáveis para Anti-Movement
local originalCFrameValue = nil
local antiMovementConnections = {}
local lastServerPosition = nil
local antiMovementTolerance = 5 -- Tolerância para movimentos normais

-- Variáveis para NoClip
local noClipConnections = {}
local originalCanCollide = {}

-- Variável para Godmode
local godmodeConnection

-- Limpar GUI anterior se existir
if PlayerGui:FindFirstChild("StealBrainrotMenu") then
    PlayerGui:FindFirstChild("StealBrainrotMenu"):Destroy()
end

-- Criar ScreenGui principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "StealBrainrotMenu"
screenGui.Parent = PlayerGui
screenGui.ResetOnSpawn = false

-- Tela de carregamento (Interface mantida)
local loadingFrame = Instance.new("Frame")
loadingFrame.Name = "LoadingFrame"
loadingFrame.Size = UDim2.new(0, 250, 0, 100)
loadingFrame.Position = UDim2.new(0.5, -125, 0.5, -50)
loadingFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
loadingFrame.BackgroundTransparency = 0.1
loadingFrame.Parent = screenGui
local loadingCorner = Instance.new("UICorner", loadingFrame)
loadingCorner.CornerRadius = UDim.new(0, 15)
local loadingLabel = Instance.new("TextLabel", loadingFrame)
loadingLabel.Size = UDim2.new(1, -20, 1, -20)
loadingLabel.Position = UDim2.new(0, 10, 0, 10)
loadingLabel.BackgroundTransparency = 1
loadingLabel.Text = "Carregando..."
loadingLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
loadingLabel.TextScaled = true
loadingLabel.Font = Enum.Font.SourceSansBold
spawn(function()
    local dots = ""
    while loadingFrame.Visible do
        dots = dots .. "."
        if #dots > 3 then dots = "" end
        loadingLabel.Text = "Carregando" .. dots
        wait(0.5)
    end
end)

-- Menu principal (Interface mantida)
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 250, 0, 380) -- Altura aumentada para novo botão
mainFrame.Position = UDim2.new(0, 20, 0, 20)
mainFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = screenGui
local corner = Instance.new("UICorner", mainFrame)
corner.CornerRadius = UDim.new(0, 15)

-- Título e sistema de arrastar (Mantido)
local titleLabel = Instance.new("TextLabel", mainFrame)
titleLabel.Size = UDim2.new(1, 0, 0, 40)
titleLabel.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
titleLabel.Text = "Steal Brainrot"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.SourceSansBold
local titleCorner = Instance.new("UICorner", titleLabel)
titleCorner.CornerRadius = UDim.new(0, 15)

titleLabel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then isDragging = false end
        end)
    end
end)
titleLabel.InputChanged:Connect(function(input)
    if isDragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Abas (Interface Mantida)
local tabFrame = Instance.new("Frame", mainFrame)
tabFrame.Size = UDim2.new(1, 0, 0, 35)
tabFrame.Position = UDim2.new(0, 0, 0, 50)
tabFrame.BackgroundTransparency = 1
local mainTab = Instance.new("TextButton", tabFrame)
mainTab.Size = UDim2.new(0.33, -3, 1, 0)
mainTab.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
mainTab.Text = "Main"
mainTab.TextColor3 = Color3.fromRGB(255, 255, 255)
mainTab.TextScaled = true
local main2Tab = Instance.new("TextButton", tabFrame)
main2Tab.Size = UDim2.new(0.33, -3, 1, 0)
main2Tab.Position = UDim2.new(0.33, 3, 0, 0)
main2Tab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
main2Tab.Text = "Main 2"
main2Tab.TextColor3 = Color3.fromRGB(255, 255, 255)
main2Tab.TextScaled = true
local creditsTab = Instance.new("TextButton", tabFrame)
creditsTab.Size = UDim2.new(0.33, -3, 1, 0)
creditsTab.Position = UDim2.new(0.66, 6, 0, 0)
creditsTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
creditsTab.Text = "Créditos"
creditsTab.TextColor3 = Color3.fromRGB(255, 255, 255)
creditsTab.TextScaled = true
-- ... (código da UI das abas e conteúdos omitido por brevidade, é o mesmo)
local mainContent = Instance.new("Frame", mainFrame)
mainContent.Size = UDim2.new(1, 0, 1, -95)
mainContent.Position = UDim2.new(0, 0, 0, 95)
mainContent.BackgroundTransparency = 1
local main2Content = Instance.new("Frame", mainFrame)
main2Content.Size = UDim2.new(1, 0, 1, -95)
main2Content.Position = UDim2.new(0, 0, 0, 95)
main2Content.BackgroundTransparency = 1
main2Content.Visible = false
local creditsContent = Instance.new("Frame", mainFrame)
creditsContent.Size = UDim2.new(1, 0, 1, -95)
creditsContent.Position = UDim2.new(0, 0, 0, 95)
creditsContent.BackgroundTransparency = 1
creditsContent.Visible = false
local function switchTab(activeTab, activeContent)
    mainTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    main2Tab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    creditsTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    mainContent.Visible = false
    main2Content.Visible = false
    creditsContent.Visible = false
    activeTab.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
    activeContent.Visible = true
end
mainTab.MouseButton1Click:Connect(function() switchTab(mainTab, mainContent) end)
main2Tab.MouseButton1Click:Connect(function() switchTab(main2Tab, main2Content) end)
creditsTab.MouseButton1Click:Connect(function() switchTab(creditsTab, creditsContent) end)
-- Fim da UI

-- ======== Funções Aprimoradas e Novas ========

-- Estilo dos Botões
local buttonSize = UDim2.new(0.9, 0, 0, 40)
local buttonYSpacing = 50

-- Função para criar botões e evitar repetição
local function createButton(parent, text, yPos)
    local button = Instance.new("TextButton")
    button.Size = buttonSize
    button.Position = UDim2.new(0.05, 0, 0, yPos)
    button.BackgroundColor3 = Color3.fromRGB(220, 53, 69) -- Red (OFF)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextScaled = true
    button.Font = Enum.Font.SourceSansBold
    button.Parent = parent
    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 10)
    return button
end

-- Função para atualizar a aparência do botão (ON/OFF)
local function updateButton(button, featureName, isEnabled)
    if isEnabled then
        button.Text = featureName .. ": ON"
        button.BackgroundColor3 = Color3.fromRGB(40, 167, 69) -- Green
    else
        button.Text = featureName .. ": OFF"
        button.BackgroundColor3 = Color3.fromRGB(220, 53, 69) -- Red
    end
end

-- --- ABA MAIN ---
local skywalkButton = createButton(mainContent, "Skywalk: OFF", 10)
local espButton = createButton(mainContent, "ESP: OFF", 10 + buttonYSpacing)
local speedButton = createButton(mainContent, "Speed Boost: OFF", 10 + buttonYSpacing * 2)

-- --- ABA MAIN 2 ---
local antiMovementButton = createButton(main2Content, "Anti-Movement: OFF", 10)
local noClipButton = createButton(main2Content, "NoClip: OFF", 10 + buttonYSpacing)
local godmodeButton = createButton(main2Content, "Godmode: OFF", 10 + buttonYSpacing * 2) -- Novo botão

-- --- ABA CRÉDITOS ---
local creditsLabel = Instance.new("TextLabel", creditsContent)
creditsLabel.Size = UDim2.new(0.9, 0, 1, 0)
creditsLabel.Position = UDim2.new(0.05, 0, 0, 0)
creditsLabel.BackgroundTransparency = 1
creditsLabel.Text = "Criado por: SpooferLuck"
creditsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
creditsLabel.TextScaled = true
creditsLabel.Font = Enum.Font.SourceSans


-- ==== LÓGICA DAS FUNÇÕES ====

-- Anti-Movement Aprimorado
local function toggleAntiMovement(forceState)
    antiMovementEnabled = (forceState == nil) and not antiMovementEnabled or forceState
    updateButton(antiMovementButton, "Anti-Movement", antiMovementEnabled)

    for _, conn in pairs(antiMovementConnections) do conn:Disconnect() end
    table.clear(antiMovementConnections)

    if antiMovementEnabled then
        local char = LocalPlayer.Character
        if not char or not char:FindFirstChild("HumanoidRootPart") then return end
        local hrp = char.HumanoidRootPart
        lastServerPosition = hrp.Position

        antiMovementConnections.cframe = hrp:GetPropertyChangedSignal("CFrame"):Connect(function()
            if not antiMovementEnabled then return end
            local humanoid = char:FindFirstChildOfClass("Humanoid")
            if not humanoid or humanoid:GetState() == Enum.HumanoidStateType.Dead then return end

            -- Permite movimento controlado pelo jogador
            if humanoid.MoveDirection.Magnitude > 0 then
                lastServerPosition = hrp.Position
                return
            end

            -- Calcula o teletransporte forçado
            local dist = (hrp.Position - lastServerPosition).Magnitude
            local tolerance = antiMovementTolerance
            if speedBoostEnabled then tolerance = tolerance * 2 end
            if noClipEnabled then tolerance = tolerance * 4 end

            -- Reverte se o movimento for muito grande e não intencional
            if dist > tolerance then
                hrp.CFrame = CFrame.new(lastServerPosition)
            end
        end)
    end
end

-- NoClip Aprimorado
local function toggleNoClip(forceState)
    noClipEnabled = (forceState == nil) and not noClipEnabled or forceState
    updateButton(noClipButton, "NoClip", noClipEnabled)
    
    for _, conn in pairs(noClipConnections) do conn:Disconnect() end
    table.clear(noClipConnections)

    local function setCollision(character, canCollide)
        for _, part in ipairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                if not canCollide and originalCanCollide[part] == nil then
                    originalCanCollide[part] = part.CanCollide
                end
                part.CanCollide = canCollide and originalCanCollide[part] or false
                if canCollide then originalCanCollide[part] = nil end
            end
        end
    end

    if noClipEnabled then
        noClipConnections.heartbeat = RunService.Heartbeat:Connect(function()
            local char = LocalPlayer.Character
            if char then
                local humanoid = char:FindFirstChildOfClass("Humanoid")
                if humanoid and humanoid:GetState() ~= Enum.HumanoidStateType.Dead then
                    setCollision(char, false)
                    humanoid:ChangeState(Enum.HumanoidStateType.Running) -- Ajuda a manter o estado de noclip
                end
            end
        end)
    else
        local char = LocalPlayer.Character
        if char then setCollision(char, true) end
        table.clear(originalCanCollide)
    end
end

-- Godmode (NOVA FUNÇÃO)
local function toggleGodmode(forceState)
    godmodeEnabled = (forceState == nil) and not godmodeEnabled or forceState
    updateButton(godmodeButton, "Godmode", godmodeEnabled)
    
    if godmodeConnection then godmodeConnection:Disconnect() end
    
    if godmodeEnabled then
        local char = LocalPlayer.Character
        if not char or not char:FindFirstChildOfClass("Humanoid") then return end
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        
        godmodeConnection = humanoid.HealthChanged:Connect(function(health)
            if health < humanoid.Health and health > 0 then
                humanoid.Health = humanoid.MaxHealth
            end
        end)
    end
end

-- Skywalk Aprimorado
local skywalkConnection
local skywalkPlatform
local function toggleSkywalk(forceState)
    skywalkEnabled = (forceState == nil) and not skywalkEnabled or forceState
    updateButton(skywalkButton, "Skywalk", skywalkEnabled)

    if skywalkConnection then skywalkConnection:Disconnect() end
    if skywalkPlatform then skywalkPlatform:Destroy() end

    if skywalkEnabled then
        skywalkPlatform = Instance.new("Part")
        skywalkPlatform.Name = "SkywalkPlatform_BySpooferLuck"
        skywalkPlatform.Size = Vector3.new(8, 0.5, 8)
        skywalkPlatform.Material = Enum.Material.Air -- Totalmente invisível
        skywalkPlatform.Transparency = 1
        skywalkPlatform.Anchored = true
        skywalkPlatform.Parent = workspace

        skywalkConnection = RunService.Heartbeat:Connect(function()
            local char = LocalPlayer.Character
            if not char or not char:FindFirstChild("HumanoidRootPart") then
                toggleSkywalk(false) -- Desativa se o personagem morrer
                return
            end
            local hrp = char.HumanoidRootPart
            skywalkPlatform.Position = hrp.Position - Vector3.new(0, 3.5, 0)
            -- A plataforma só se torna sólida se o jogador estiver caindo
            skywalkPlatform.CanCollide = (hrp.Velocity.Y < -1)
        end)
    end
end

-- ESP Aprimorado e mais performático
local espUpdateConnection
local espObjects = {}
local function toggleESP(forceState)
    espEnabled = (forceState == nil) and not espEnabled or forceState
    updateButton(espButton, "ESP", espEnabled)

    if espUpdateConnection then espUpdateConnection:Disconnect() end
    
    -- Limpa todos os ESPs visuais
    for _, obj in pairs(espObjects) do
        if obj.highlight then obj.highlight:Destroy() end
        if obj.billboard then obj.billboard:Destroy() end
    end
    table.clear(espObjects)

    if espEnabled then
        local function createESP(player)
            if espObjects[player] then return end -- Já existe
            local char = player.Character or player.CharacterAdded:Wait()
            if not char or player == LocalPlayer then return end

            local highlight = Instance.new("Highlight", char)
            highlight.FillTransparency = 0.5
            highlight.OutlineTransparency = 0
            highlight.Adornee = char

            local head = char:WaitForChild("Head")
            local billboard = Instance.new("BillboardGui", head)
            billboard.AlwaysOnTop = true
            billboard.Size = UDim2.new(0, 150, 0, 40)
            billboard.StudsOffset = Vector3.new(0, 2.5, 0)
            
            local nameLabel = Instance.new("TextLabel", billboard)
            nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
            nameLabel.BackgroundTransparency = 1
            nameLabel.Text = player.Name
            nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            nameLabel.Font = Enum.Font.SourceSansBold
            nameLabel.TextScaled = true

            local distLabel = Instance.new("TextLabel", billboard)
            distLabel.Size = UDim2.new(1, 0, 0.5, 0)
            distLabel.Position = UDim2.new(0, 0, 0.5, 0)
            distLabel.BackgroundTransparency = 1
            distLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
            distLabel.Font = Enum.Font.SourceSans
            distLabel.TextScaled = true

            espObjects[player] = {highlight=highlight, billboard=billboard, nameLabel=nameLabel, distLabel=distLabel, char=char}
        end
        
        -- Loop único para criar e atualizar todos os ESPs
        espUpdateConnection = RunService.Heartbeat:Connect(function()
            local myHrp = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if not myHrp then return end

            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer then
                    if player.Character and not espObjects[player] then
                        createESP(player)
                    elseif not player.Character and espObjects[player] then
                        if espObjects[player].highlight then espObjects[player].highlight:Destroy() end
                        if espObjects[player].billboard then espObjects[player].billboard:Destroy() end
                        espObjects[player] = nil
                    end

                    if espObjects[player] and espObjects[player].char:FindFirstChild("HumanoidRootPart") then
                        local dist = (myHrp.Position - espObjects[player].char.HumanoidRootPart.Position).Magnitude
                        espObjects[player].distLabel.Text = math.floor(dist) .. " studs"
                        
                        if dist < 50 then espObjects[player].highlight.FillColor = Color3.fromRGB(255, 0, 0)
                        elseif dist < 150 then espObjects[player].highlight.FillColor = Color3.fromRGB(255, 165, 0)
                        else espObjects[player].highlight.FillColor = Color3.fromRGB(0, 255, 0) end
                    end
                end
            end
        end)
    end
end

-- Speed Boost Aprimorado
local speedConnection
local function toggleSpeed(forceState)
    speedBoostEnabled = (forceState == nil) and not speedBoostEnabled or forceState
    updateButton(speedButton, "Speed Boost", speedBoostEnabled)

    if speedConnection then speedConnection:Disconnect() end
    
    local char = LocalPlayer.Character
    local humanoid = char and char:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end

    if speedBoostEnabled then
        originalSpeed = humanoid.WalkSpeed
        humanoid.WalkSpeed = 50
        speedConnection = humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
            if speedBoostEnabled and humanoid.WalkSpeed ~= 50 then
                humanoid.WalkSpeed = 50
            end
        end)
    else
        humanoid.WalkSpeed = originalSpeed
    end
end


-- ==== CONEXÕES DOS BOTÕES ====
skywalkButton.MouseButton1Click:Connect(function() toggleSkywalk() end)
espButton.MouseButton1Click:Connect(function() toggleESP() end)
speedButton.MouseButton1Click:Connect(function() toggleSpeed() end)
antiMovementButton.MouseButton1Click:Connect(function() toggleAntiMovement() end)
noClipButton.MouseButton1Click:Connect(function() toggleNoClip() end)
godmodeButton.MouseButton1Click:Connect(function() toggleGodmode() end)


-- Gerenciador de Renascimento (Respawn) - Mais limpo e confiável
LocalPlayer.CharacterAdded:Connect(function(character)
    -- Espera o Humanoid e outras partes carregarem
    local humanoid = character:WaitForChild("Humanoid")
    wait(1)

    -- Reativa as funções que estavam ligadas
    if speedBoostEnabled then toggleSpeed(true) end
    if noClipEnabled then toggleNoClip(true) end
    if antiMovementEnabled then toggleAntiMovement(true) end
    if godmodeEnabled then toggleGodmode(true) end
    -- Funções que se reativam sozinhas (como ESP e Skywalk) não precisam ser chamadas aqui
    -- pois seus loops já lidam com a ausência/presença do personagem.
end)


-- Botão de minimizar/maximizar (Lógica Mantida)
local minimizeButton = Instance.new("TextButton", titleLabel)
minimizeButton.Size = UDim2.new(0, 30, 0, 20)
minimizeButton.Position = UDim2.new(1, -35, 0, 5)
minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
minimizeButton.Text = "-"
minimizeButton.TextColor3 = Color3.fromRGB(0, 0, 0)
minimizeButton.TextScaled = true
local minimizeCorner = Instance.new("UICorner", minimizeButton)
minimizeCorner.CornerRadius = UDim.new(0, 5)
local isMinimized = false
local originalSize = mainFrame.Size
minimizeButton.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    local activeContent
    if mainContent.Visible then activeContent = mainContent end
    if main2Content.Visible then activeContent = main2Content end
    if creditsContent.Visible then activeContent = creditsContent end

    if isMinimized then
        mainFrame:TweenSize(UDim2.new(0, 250, 0, 50), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3)
        minimizeButton.Text = "+"
        tabFrame.Visible = false
        if activeContent then activeContent.Visible = false end
    else
        mainFrame:TweenSize(originalSize, Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3)
        minimizeButton.Text = "-"
        tabFrame.Visible = true
        if activeContent then activeContent.Visible = true end
    end
end)

-- Tecla de atalho para abrir/fechar menu (Insert)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.Insert then
        mainFrame.Visible = not mainFrame.Visible
    end
end)

-- Animação de entrada
wait(2)
loadingFrame.Visible = false
mainFrame.Visible = true
mainFrame.Size = UDim2.new(0, 0, 0, 0)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame:TweenSizeAndPosition(
    originalSize,
    UDim2.new(0, 20, 0, 20),
    Enum.EasingDirection.Out,
    Enum.EasingStyle.Back,
    0.8
)
