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

-- Limpar GUI anterior se existir
if PlayerGui:FindFirstChild("StealBrainrotMenu") then
    PlayerGui:FindFirstChild("StealBrainrotMenu"):Destroy()
end

-- Criar ScreenGui principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "StealBrainrotMenu"
screenGui.Parent = PlayerGui
screenGui.ResetOnSpawn = false

-- Tela de carregamento menor
local loadingFrame = Instance.new("Frame")
loadingFrame.Name = "LoadingFrame"
loadingFrame.Size = UDim2.new(0, 250, 0, 100)
loadingFrame.Position = UDim2.new(0.5, -125, 0.5, -50)
loadingFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
loadingFrame.BackgroundTransparency = 0.1
loadingFrame.Parent = screenGui

local loadingCorner = Instance.new("UICorner")
loadingCorner.CornerRadius = UDim.new(0, 15)
loadingCorner.Parent = loadingFrame

local loadingLabel = Instance.new("TextLabel")
loadingLabel.Size = UDim2.new(1, -20, 1, -20)
loadingLabel.Position = UDim2.new(0, 10, 0, 10)
loadingLabel.BackgroundTransparency = 1
loadingLabel.Text = "Carregando..."
loadingLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
loadingLabel.TextScaled = true
loadingLabel.Font = Enum.Font.SourceSansBold
loadingLabel.Parent = loadingFrame

-- Animação de carregamento
local loadingDots = ""
spawn(function()
    while loadingFrame.Visible do
        loadingDots = loadingDots .. "."
        if #loadingDots > 3 then
            loadingDots = ""
        end
        loadingLabel.Text = "Carregando" .. loadingDots
        wait(0.5)
    end
end)

-- Menu principal menor
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 250, 0, 320)
mainFrame.Position = UDim2.new(0, 20, 0, 20)
mainFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = screenGui

-- Arredondar cantos
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 15)
corner.Parent = mainFrame

-- Título com área de arrastar
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 40)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
titleLabel.BorderSizePixel = 0
titleLabel.Text = "Steal Brainrot"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.Parent = mainFrame

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 15)
titleCorner.Parent = titleLabel

-- Sistema de arrastar
local function updateInput(input)
    if isDragging then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end

titleLabel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                isDragging = false
            end
        end)
    end
end)

titleLabel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        updateInput(input)
    end
end)

-- Abas (agora com 3 abas)
local tabFrame = Instance.new("Frame")
tabFrame.Size = UDim2.new(1, 0, 0, 35)
tabFrame.Position = UDim2.new(0, 0, 0, 50)
tabFrame.BackgroundTransparency = 1
tabFrame.Parent = mainFrame

local mainTab = Instance.new("TextButton")
mainTab.Size = UDim2.new(0.33, -3, 1, 0)
mainTab.Position = UDim2.new(0, 0, 0, 0)
mainTab.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
mainTab.BorderSizePixel = 0
mainTab.Text = "Main"
mainTab.TextColor3 = Color3.fromRGB(255, 255, 255)
mainTab.TextScaled = true
mainTab.Font = Enum.Font.SourceSans
mainTab.Parent = tabFrame

local main2Tab = Instance.new("TextButton")
main2Tab.Size = UDim2.new(0.33, -3, 1, 0)
main2Tab.Position = UDim2.new(0.33, 3, 0, 0)
main2Tab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
main2Tab.BorderSizePixel = 0
main2Tab.Text = "Main 2"
main2Tab.TextColor3 = Color3.fromRGB(255, 255, 255)
main2Tab.TextScaled = true
main2Tab.Font = Enum.Font.SourceSans
main2Tab.Parent = tabFrame

local creditsTab = Instance.new("TextButton")
creditsTab.Size = UDim2.new(0.33, -3, 1, 0)
creditsTab.Position = UDim2.new(0.66, 6, 0, 0)
creditsTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
creditsTab.BorderSizePixel = 0
creditsTab.Text = "Créditos"
creditsTab.TextColor3 = Color3.fromRGB(255, 255, 255)
creditsTab.TextScaled = true
creditsTab.Font = Enum.Font.SourceSans
creditsTab.Parent = tabFrame

-- Cantos arredondados das abas
local mainTabCorner = Instance.new("UICorner")
mainTabCorner.CornerRadius = UDim.new(0, 8)
mainTabCorner.Parent = mainTab

local main2TabCorner = Instance.new("UICorner")
main2TabCorner.CornerRadius = UDim.new(0, 8)
main2TabCorner.Parent = main2Tab

local creditsTabCorner = Instance.new("UICorner")
creditsTabCorner.CornerRadius = UDim.new(0, 8)
creditsTabCorner.Parent = creditsTab

-- Conteúdo Main
local mainContent = Instance.new("Frame")
mainContent.Size = UDim2.new(1, 0, 1, -95)
mainContent.Position = UDim2.new(0, 0, 0, 95)
mainContent.BackgroundTransparency = 1
mainContent.Parent = mainFrame

-- Conteúdo Main 2
local main2Content = Instance.new("Frame")
main2Content.Size = UDim2.new(1, 0, 1, -95)
main2Content.Position = UDim2.new(0, 0, 0, 95)
main2Content.BackgroundTransparency = 1
main2Content.Visible = false
main2Content.Parent = mainFrame

-- Conteúdo Créditos
local creditsContent = Instance.new("Frame")
creditsContent.Size = UDim2.new(1, 0, 1, -95)
creditsContent.Position = UDim2.new(0, 0, 0, 95)
creditsContent.BackgroundTransparency = 1
creditsContent.Visible = false
creditsContent.Parent = mainFrame

-- Botões menores
local buttonSize = UDim2.new(0.9, 0, 0, 40)
local buttonSpacing = 50

-- Botão Skywalk (Main)
local skywalkButton = Instance.new("TextButton")
skywalkButton.Size = buttonSize
skywalkButton.Position = UDim2.new(0.05, 0, 0, 10)
skywalkButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
skywalkButton.BorderSizePixel = 0
skywalkButton.Text = "Skywalk: OFF"
skywalkButton.TextColor3 = Color3.fromRGB(255, 255, 255)
skywalkButton.TextScaled = true
skywalkButton.Font = Enum.Font.SourceSansBold
skywalkButton.Parent = mainContent

local skywalkCorner = Instance.new("UICorner")
skywalkCorner.CornerRadius = UDim.new(0, 10)
skywalkCorner.Parent = skywalkButton

-- Botão ESP (Main)
local espButton = Instance.new("TextButton")
espButton.Size = buttonSize
espButton.Position = UDim2.new(0.05, 0, 0, 60)
espButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
espButton.BorderSizePixel = 0
espButton.Text = "ESP: OFF"
espButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espButton.TextScaled = true
espButton.Font = Enum.Font.SourceSansBold
espButton.Parent = mainContent

local espCorner = Instance.new("UICorner")
espCorner.CornerRadius = UDim.new(0, 10)
espCorner.Parent = espButton

-- Botão Speed Boost (Main)
local speedButton = Instance.new("TextButton")
speedButton.Size = buttonSize
speedButton.Position = UDim2.new(0.05, 0, 0, 110)
speedButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
speedButton.BorderSizePixel = 0
speedButton.Text = "Speed Boost: OFF"
speedButton.TextColor3 = Color3.fromRGB(255, 255, 255)
speedButton.TextScaled = true
speedButton.Font = Enum.Font.SourceSansBold
speedButton.Parent = mainContent

local speedCorner = Instance.new("UICorner")
speedCorner.CornerRadius = UDim.new(0, 10)
speedCorner.Parent = speedButton

-- Botão Anti-Movement (Main 2)
local antiMovementButton = Instance.new("TextButton")
antiMovementButton.Size = buttonSize
antiMovementButton.Position = UDim2.new(0.05, 0, 0, 10)
antiMovementButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
antiMovementButton.BorderSizePixel = 0
antiMovementButton.Text = "Anti-Movement: OFF"
antiMovementButton.TextColor3 = Color3.fromRGB(255, 255, 255)
antiMovementButton.TextScaled = true
antiMovementButton.Font = Enum.Font.SourceSansBold
antiMovementButton.Parent = main2Content

local antiMovementCorner = Instance.new("UICorner")
antiMovementCorner.CornerRadius = UDim.new(0, 10)
antiMovementCorner.Parent = antiMovementButton

-- Botão NoClip (Main 2)
local noClipButton = Instance.new("TextButton")
noClipButton.Size = buttonSize
noClipButton.Position = UDim2.new(0.05, 0, 0, 60)
noClipButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
noClipButton.BorderSizePixel = 0
noClipButton.Text = "NoClip: OFF"
noClipButton.TextColor3 = Color3.fromRGB(255, 255, 255)
noClipButton.TextScaled = true
noClipButton.Font = Enum.Font.SourceSansBold
noClipButton.Parent = main2Content

local noClipCorner = Instance.new("UICorner")
noClipCorner.CornerRadius = UDim.new(0, 10)
noClipCorner.Parent = noClipButton

-- Label de créditos
local creditsLabel = Instance.new("TextLabel")
creditsLabel.Size = UDim2.new(0.9, 0, 1, 0)
creditsLabel.Position = UDim2.new(0.05, 0, 0, 0)
creditsLabel.BackgroundTransparency = 1
creditsLabel.Text = "Criado por:\n\nSpooferLuck"
creditsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
creditsLabel.TextScaled = true
creditsLabel.Font = Enum.Font.SourceSans
creditsLabel.Parent = creditsContent

-- Funções das abas
local function switchTab(activeTab, activeContent)
    -- Resetar todas as abas
    mainTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    main2Tab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    creditsTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    
    -- Ocultar todos os conteúdos
    mainContent.Visible = false
    main2Content.Visible = false
    creditsContent.Visible = false
    
    -- Ativar aba e conteúdo selecionados
    activeTab.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
    activeContent.Visible = true
end

mainTab.MouseButton1Click:Connect(function()
    switchTab(mainTab, mainContent)
end)

main2Tab.MouseButton1Click:Connect(function()
    switchTab(main2Tab, main2Content)
end)

creditsTab.MouseButton1Click:Connect(function()
    switchTab(creditsTab, creditsContent)
end)

-- Função Anti-Movement Melhorada
local function toggleAntiMovement()
    antiMovementEnabled = not antiMovementEnabled
    
    local character = LocalPlayer.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then
        return
    end
    
    local humanoidRootPart = character.HumanoidRootPart
    
    if antiMovementEnabled then
        antiMovementButton.Text = "Anti-Movement: ON"
        antiMovementButton.BackgroundColor3 = Color3.fromRGB(40, 167, 69)
        
        -- Salvar posição inicial
        originalCFrameValue = humanoidRootPart.CFrame
        lastServerPosition = humanoidRootPart.Position
        
        -- Sistema avançado de detecção de movimento forçado
        antiMovementConnections[#antiMovementConnections + 1] = humanoidRootPart:GetPropertyChangedSignal("CFrame"):Connect(function()
            if not antiMovementEnabled then return end
            
            local newCFrame = humanoidRootPart.CFrame
            local currentPosition = newCFrame.Position
            local distanceMoved = (currentPosition - lastServerPosition).Magnitude
            
            local character = LocalPlayer.Character
            local humanoid = character and character:FindFirstChild("Humanoid")
            
            -- Verificar se o movimento foi natural (jogador se movendo)
            local isPlayerMoving = humanoid and humanoid.MoveDirection.Magnitude > 0
            local isSpeedBoostActive = speedBoostEnabled
            local isNoClipActive = noClipEnabled
            
            -- Tolerância dinâmica baseada no estado do jogador
            local tolerance = antiMovementTolerance
            if isPlayerMoving then
                tolerance = tolerance * 2 -- Mais tolerância durante movimento
            end
            if isSpeedBoostActive then
                tolerance = tolerance * 3 -- Mais tolerância com speed boost
            end
            if isNoClipActive then
                tolerance = tolerance * 4 -- Mais tolerância com NoClip
            end
            
            -- Detectar teleportes forçados pelo servidor
            if distanceMoved > tolerance then
                -- Verificar se não é um teleporte legítimo (spawn, etc.)
                local timeSinceLastUpdate = tick() - (lastServerPosition.X ~= currentPosition.X and tick() or 0)
                
                if timeSinceLastUpdate > 0.1 then -- Não é um teleporte muito rápido
                    -- Reverter para posição segura
                    humanoidRootPart.CFrame = CFrame.new(lastServerPosition)
                    print("Anti-Movement: Bloqueado teleporte forçado! Distância: " .. math.floor(distanceMoved))
                    return
                end
            end
            
            -- Detectar mudanças súbitas de Y (anti-fly do servidor)
            local yDifference = math.abs(currentPosition.Y - lastServerPosition.Y)
            if yDifference > tolerance and not isPlayerMoving then
                humanoidRootPart.CFrame = CFrame.new(Vector3.new(currentPosition.X, lastServerPosition.Y, currentPosition.Z))
                print("Anti-Movement: Bloqueado mudança forçada de altura!")
                return
            end
            
            -- Atualizar posição segura apenas se o movimento foi natural
            if distanceMoved <= tolerance or isPlayerMoving then
                lastServerPosition = currentPosition
                originalCFrameValue = newCFrame
            end
        end)
        
        -- Proteção contra mudanças de velocidade
        antiMovementConnections[#antiMovementConnections + 1] = humanoidRootPart:GetPropertyChangedSignal("Velocity"):Connect(function()
            if not antiMovementEnabled then return end
            
            local velocity = humanoidRootPart.Velocity
            local character = LocalPlayer.Character
            local humanoid = character and character:FindFirstChild("Humanoid")
            
            -- Permitir velocidade normal durante movimento
            if humanoid and humanoid.MoveDirection.Magnitude > 0 then
                return
            end
            
            -- Detectar mudanças drásticas de velocidade (anti-speed do servidor)
            if velocity.Magnitude > 150 and not speedBoostEnabled then
                humanoidRootPart.Velocity = Vector3.new(0, velocity.Y, 0) -- Manter apenas velocidade Y
                print("Anti-Movement: Bloqueado mudança forçada de velocidade!")
            end
        end)
        
        -- Sistema de atualização contínua para movimento natural
        antiMovementConnections[#antiMovementConnections + 1] = RunService.Heartbeat:Connect(function()
            if not antiMovementEnabled then return end
            
            local character = LocalPlayer.Character
            if not character or not character:FindFirstChild("HumanoidRootPart") then return end
            
            local humanoid = character:FindFirstChild("Humanoid")
            local humanoidRootPart = character.HumanoidRootPart
            
            -- Atualizar posição segura durante movimento natural
            if humanoid and humanoid.MoveDirection.Magnitude > 0 then
                lastServerPosition = humanoidRootPart.Position
                originalCFrameValue = humanoidRootPart.CFrame
            end
            
            -- Proteção especial para NoClip
            if noClipEnabled then
                -- Verificar se o servidor está tentando "corrigir" a posição do NoClip
                local raycast = workspace:Raycast(humanoidRootPart.Position, Vector3.new(0, -5, 0))
                if raycast and raycast.Instance.CanCollide then
                    -- Se estamos dentro de um objeto sólido, manter posição
                    local insideObject = true
                    local directions = {
                        Vector3.new(1, 0, 0), Vector3.new(-1, 0, 0),
                        Vector3.new(0, 1, 0), Vector3.new(0, -1, 0),
                        Vector3.new(0, 0, 1), Vector3.new(0, 0, -1)
                    }
                    
                    for _, direction in pairs(directions) do
                        local ray = workspace:Raycast(humanoidRootPart.Position, direction * 3)
                        if not ray then
                            insideObject = false
                            break
                        end
                    end
                    
                    if insideObject then
                        lastServerPosition = humanoidRootPart.Position
                        originalCFrameValue = humanoidRootPart.CFrame
                    end
                end
            end
        end)
        
        print("Anti-Movement ativado! Protegendo contra teleportes e mudanças forçadas pelo servidor.")
        print("Compatível com Speed Boost e NoClip!")
        
    else
        antiMovementButton.Text = "Anti-Movement: OFF"
        antiMovementButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
        
        -- Desconectar todas as conexões
        for _, connection in pairs(antiMovementConnections) do
            connection:Disconnect()
        end
        antiMovementConnections = {}
        
        originalCFrameValue = nil
        lastServerPosition = nil
        print("Anti-Movement desativado!")
    end
end

antiMovementButton.MouseButton1Click:Connect(toggleAntiMovement)

-- Função NoClip Avançada
local function toggleNoClip()
    noClipEnabled = not noClipEnabled
    
    local character = LocalPlayer.Character
    if not character then return end
    
    if noClipEnabled then
        noClipButton.Text = "NoClip: ON"
        noClipButton.BackgroundColor3 = Color3.fromRGB(40, 167, 69)
        
        -- Função para desabilitar colisão de todas as partes
        local function disableCollision(obj)
            if obj:IsA("BasePart") then
                originalCanCollide[obj] = obj.CanCollide
                obj.CanCollide = false
            end
        end
        
        -- Função para habilitar colisão de todas as partes
        local function enableCollision(obj)
            if obj:IsA("BasePart") and originalCanCollide[obj] ~= nil then
                obj.CanCollide = originalCanCollide[obj]
                originalCanCollide[obj] = nil
            end
        end
        
        -- Desabilitar colisão inicial
        for _, part in pairs(character:GetChildren()) do
            disableCollision(part)
        end
        
        -- Monitorar novas partes adicionadas
        noClipConnections[#noClipConnections + 1] = character.ChildAdded:Connect(function(child)
            if noClipEnabled then
                wait(0.1)
                disableCollision(child)
            end
        end)
        
        -- Sistema de manutenção do NoClip
        noClipConnections[#noClipConnections + 1] = RunService.Heartbeat:Connect(function()
            if not noClipEnabled then return end
            
            local character = LocalPlayer.Character
            if not character then return end
            
            -- Verificar e manter colisão desabilitada
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") and part.CanCollide then
                    part.CanCollide = false
                end
            end
            
            -- Proteção contra anti-NoClip do servidor
            local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                -- Verificar se estamos presos em um objeto
                local raycast = workspace:Raycast(humanoidRootPart.Position, Vector3.new(0, -1, 0))
                if raycast and raycast.Instance.CanCollide then
                    -- Se o servidor tentar nos "ejetar", usar Anti-Movement para resistir
                    if antiMovementEnabled then
                        -- Anti-Movement vai ajudar a manter a posição
                        lastServerPosition = humanoidRootPart.Position
                    end
                end
            end
        end)
        
        -- Proteção contra detecção de NoClip
        noClipConnections[#noClipConnections + 1] = RunService.Stepped:Connect(function()
            if not noClipEnabled then return end
            
            local character = LocalPlayer.Character
            if not character then return end
            
            -- Simular física normal para enganar anti-cheats
            local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                local humanoid = character:FindFirstChild("Humanoid")
                if humanoid and humanoid.MoveDirection.Magnitude == 0 then
                    -- Aplicar "gravidade" falsa quando não estamos nos movendo
                    local raycast = workspace:Raycast(humanoidRootPart.Position, Vector3.new(0, -5, 0))
                    if not raycast then
                        -- Estamos no ar, simular queda lenta
                        humanoidRootPart.Velocity = Vector3.new(
                            humanoidRootPart.Velocity.X,
                            math.max(humanoidRootPart.Velocity.Y - 0.1, -10),
                            humanoidRootPart.Velocity.Z
                        )
                    end
                end
            end
        end)
        
        print("NoClip ativado! Atravesse paredes e objetos livremente!")
        print("Proteção contra anti-NoClip do servidor ativada!")
        
    else
        noClipButton.Text = "NoClip: OFF"
        noClipButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
        
        -- Desconectar todas as conexões
        for _, connection in pairs(noClipConnections) do
            connection:Disconnect()
        end
        noClipConnections = {}
        
        -- Restaurar colisão original
        local character = LocalPlayer.Character
        if character then
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") and originalCanCollide[part] ~= nil then
                    part.CanCollide = originalCanCollide[part]
                end
            end
        end
        
        originalCanCollide = {}
        print("NoClip desativado!")
    end
end

noClipButton.MouseButton1Click:Connect(toggleNoClip)

-- Função Skywalk melhorada
local skywalkConnection
local skywalkPlatform

skywalkButton.MouseButton1Click:Connect(function()
    skywalkEnabled = not skywalkEnabled
    
    if skywalkEnabled then
        skywalkButton.Text = "Skywalk: ON"
        skywalkButton.BackgroundColor3 = Color3.fromRGB(40, 167, 69)
        
        -- Criar plataforma completamente invisível
        local character = LocalPlayer.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            skywalkPlatform = Instance.new("Part")
            skywalkPlatform.Name = "SkywalkPlatform"
            skywalkPlatform.Size = Vector3.new(8, 0.5, 8)
            skywalkPlatform.Material = Enum.Material.Air
            skywalkPlatform.Transparency = 1
            skywalkPlatform.CanCollide = true
            skywalkPlatform.Anchored = true
            skywalkPlatform.Parent = workspace
            
            -- Tornar a plataforma totalmente invisível
            skywalkPlatform.TopSurface = Enum.SurfaceType.Smooth
            skywalkPlatform.BottomSurface = Enum.SurfaceType.Smooth
            
            skywalkConnection = RunService.Heartbeat:Connect(function()
                if character and character:FindFirstChild("HumanoidRootPart") then
                    local humanoidRootPart = character.HumanoidRootPart
                    local position = humanoidRootPart.Position
                    local velocity = humanoidRootPart.Velocity
                    
                    -- Posicionar plataforma ligeiramente abaixo do jogador
                    skywalkPlatform.Position = Vector3.new(position.X, position.Y - 3.5, position.Z)
                    
                    -- Se o jogador estiver caindo, ativar a plataforma
                    if velocity.Y < 0 then
                        skywalkPlatform.CanCollide = true
                    else
                        skywalkPlatform.CanCollide = false
                    end
                end
            end)
            end
    else
        skywalkButton.Text = "Skywalk: OFF"
        skywalkButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
        
        if skywalkConnection then
            skywalkConnection:Disconnect()
            skywalkConnection = nil
        end
        
        if skywalkPlatform then
            skywalkPlatform:Destroy()
            skywalkPlatform = nil
        end
        
        print("Skywalk desativado!")
    end
end)

-- Função ESP melhorada
local espConnections = {}
local espObjects = {}

local function createESP(player)
    if player == LocalPlayer then return end
    
    local character = player.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then return end
    
    -- Criar highlight para o jogador
    local highlight = Instance.new("Highlight")
    highlight.Name = "ESP_Highlight"
    highlight.Parent = character
    highlight.FillColor = Color3.fromRGB(255, 0, 0)
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0
    highlight.Adornee = character
    
    -- Criar BillboardGui para mostrar nome e distância
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP_Billboard"
    billboard.Parent = character:FindFirstChild("Head")
    billboard.Size = UDim2.new(0, 200, 0, 50)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Parent = billboard
    nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
    nameLabel.Position = UDim2.new(0, 0, 0, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameLabel.TextScaled = true
    nameLabel.Font = Enum.Font.SourceSansBold
    nameLabel.TextStrokeTransparency = 0
    nameLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    
    local distanceLabel = Instance.new("TextLabel")
    distanceLabel.Parent = billboard
    distanceLabel.Size = UDim2.new(1, 0, 0.5, 0)
    distanceLabel.Position = UDim2.new(0, 0, 0.5, 0)
    distanceLabel.BackgroundTransparency = 1
    distanceLabel.Text = "0 studs"
    distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
    distanceLabel.TextScaled = true
    distanceLabel.Font = Enum.Font.SourceSans
    distanceLabel.TextStrokeTransparency = 0
    distanceLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    
    -- Guardar referências
    espObjects[player] = {
        highlight = highlight,
        billboard = billboard,
        distanceLabel = distanceLabel
    }
    
    -- Atualizar distância
    local updateConnection = RunService.Heartbeat:Connect(function()
        if not espEnabled then return end
        
        local myCharacter = LocalPlayer.Character
        if not myCharacter or not myCharacter:FindFirstChild("HumanoidRootPart") then return end
        if not character or not character:FindFirstChild("HumanoidRootPart") then return end
        
        local distance = (myCharacter.HumanoidRootPart.Position - character.HumanoidRootPart.Position).Magnitude
        distanceLabel.Text = math.floor(distance) .. " studs"
        
        -- Mudar cor baseada na distância
        if distance <= 50 then
            highlight.FillColor = Color3.fromRGB(255, 0, 0) -- Vermelho - Muito perto
        elseif distance <= 100 then
            highlight.FillColor = Color3.fromRGB(255, 165, 0) -- Laranja - Perto
        else
            highlight.FillColor = Color3.fromRGB(0, 255, 0) -- Verde - Longe
        end
    end)
    
    espConnections[player] = updateConnection
end

local function removeESP(player)
    if espObjects[player] then
        if espObjects[player].highlight then
            espObjects[player].highlight:Destroy()
        end
        if espObjects[player].billboard then
            espObjects[player].billboard:Destroy()
        end
        espObjects[player] = nil
    end
    
    if espConnections[player] then
        espConnections[player]:Disconnect()
        espConnections[player] = nil
    end
end

espButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    
    if espEnabled then
        espButton.Text = "ESP: ON"
        espButton.BackgroundColor3 = Color3.fromRGB(40, 167, 69)
        
        -- Criar ESP para todos os jogadores atuais
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                createESP(player)
            end
        end
        
        -- Monitorar novos jogadores
        espConnections.playerAdded = Players.PlayerAdded:Connect(function(player)
            if espEnabled then
                player.CharacterAdded:Connect(function()
                    wait(1)
                    if espEnabled then
                        createESP(player)
                    end
                end)
            end
        end)
        
        -- Monitorar jogadores saindo
        espConnections.playerRemoving = Players.PlayerRemoving:Connect(function(player)
            removeESP(player)
        end)
        
        print("ESP ativado! Visualize todos os jogadores através das paredes!")
        
    else
        espButton.Text = "ESP: OFF"
        espButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
        
        -- Remover ESP de todos os jogadores
        for _, player in pairs(Players:GetPlayers()) do
            removeESP(player)
        end
        
        -- Desconectar conexões
        for _, connection in pairs(espConnections) do
            if connection then
                connection:Disconnect()
            end
        end
        espConnections = {}
        
        print("ESP desativado!")
    end
end)

-- Função Speed Boost melhorada
local speedConnection
local originalWalkSpeed = 16

speedButton.MouseButton1Click:Connect(function()
    speedBoostEnabled = not speedBoostEnabled
    
    local character = LocalPlayer.Character
    if not character then return end
    
    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    if speedBoostEnabled then
        speedButton.Text = "Speed Boost: ON"
        speedButton.BackgroundColor3 = Color3.fromRGB(40, 167, 69)
        
        -- Salvar velocidade original
        originalWalkSpeed = humanoid.WalkSpeed
        
        -- Aplicar speed boost
        humanoid.WalkSpeed = 50
        
        -- Monitorar mudanças de velocidade (anti-speed do servidor)
        speedConnection = humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
            if speedBoostEnabled and humanoid.WalkSpeed ~= 50 then
                humanoid.WalkSpeed = 50
            end
        end)
        
        print("Speed Boost ativado! Velocidade: 50")
        
    else
        speedButton.Text = "Speed Boost: OFF"
        speedButton.BackgroundColor3 = Color3.fromRGB(220, 53, 69)
        
        -- Restaurar velocidade original
        humanoid.WalkSpeed = originalWalkSpeed
        
        if speedConnection then
            speedConnection:Disconnect()
            speedConnection = nil
        end
        
        print("Speed Boost desativado!")
    end
end)

-- Gerenciar respawn do jogador
LocalPlayer.CharacterAdded:Connect(function(character)
    wait(1)
    
    -- Reativar funcionalidades se estiverem ligadas
    if speedBoostEnabled then
        speedButton.MouseButton1Click:Connect(function() end)
        speedButton.MouseButton1Click:Fire()
        speedButton.MouseButton1Click:Fire()
    end
    
    if espEnabled then
        espButton.MouseButton1Click:Connect(function() end)
        espButton.MouseButton1Click:Fire()
        espButton.MouseButton1Click:Fire()
    end
    
    if skywalkEnabled then
        skywalkButton.MouseButton1Click:Connect(function() end)
        skywalkButton.MouseButton1Click:Fire()
        skywalkButton.MouseButton1Click:Fire()
    end
    
    if antiMovementEnabled then
        antiMovementButton.MouseButton1Click:Connect(function() end)
        antiMovementButton.MouseButton1Click:Fire()
        antiMovementButton.MouseButton1Click:Fire()
    end
    
    if noClipEnabled then
        noClipButton.MouseButton1Click:Connect(function() end)
        noClipButton.MouseButton1Click:Fire()
        noClipButton.MouseButton1Click:Fire()
    end
end)

-- Botão de minimizar/maximizar
local minimizeButton = Instance.new("TextButton")
minimizeButton.Size = UDim2.new(0, 30, 0, 20)
minimizeButton.Position = UDim2.new(1, -35, 0, 5)
minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 193, 7)
minimizeButton.BorderSizePixel = 0
minimizeButton.Text = "-"
minimizeButton.TextColor3 = Color3.fromRGB(0, 0, 0)
minimizeButton.TextScaled = true
minimizeButton.Font = Enum.Font.SourceSansBold
minimizeButton.Parent = titleLabel

local minimizeCorner = Instance.new("UICorner")
minimizeCorner.CornerRadius = UDim.new(0, 5)
minimizeCorner.Parent = minimizeButton

local isMinimized = false
local originalSize = mainFrame.Size

minimizeButton.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    
    if isMinimized then
        mainFrame:TweenSize(UDim2.new(0, 250, 0, 50), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3)
        minimizeButton.Text = "+"
        tabFrame.Visible = false
        mainContent.Visible = false
        main2Content.Visible = false
        creditsContent.Visible = false
    else
        mainFrame:TweenSize(originalSize, Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3)
        minimizeButton.Text = "-"
        tabFrame.Visible = true
        -- Mostrar conteúdo da aba ativa
        if mainTab.BackgroundColor3 == Color3.fromRGB(70, 130, 180) then
            mainContent.Visible = true
        elseif main2Tab.BackgroundColor3 == Color3.fromRGB(70, 130, 180) then
            main2Content.Visible = true
        else
            creditsContent.Visible = true
        end
    end
end)

-- Tecla de atalho para abrir/fechar menu (Insert)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.Insert then
        mainFrame.Visible = not mainFrame.Visible
        if mainFrame.Visible then
            print("Menu aberto! Use Insert para fechar.")
        else
            print("Menu fechado! Use Insert para abrir.")
        end
    end
end)

-- Finalizar carregamento
wait(2)
loadingFrame.Visible = false
mainFrame.Visible = true

-- Animação de entrada suave
mainFrame.Size = UDim2.new(0, 0, 0, 0)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame:TweenSizeAndPosition(
    UDim2.new(0, 250, 0, 320),
    UDim2.new(0, 20, 0, 20),
    Enum.EasingDirection.Out,
    Enum.EasingStyle.Back,
    0.8
)
