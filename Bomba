-- Verifica se o script está sendo executado no Delta Executor V2.645
if not _G.DeltaVersion or _G.DeltaVersion ~= "2.645" then
    return print("Este script só pode ser executado no Delta Executor V2.645.")
end

-- Script principal com menu GUI

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local screenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 200, 0, 300)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
frame.BorderSizePixel = 0

local titleLabel = Instance.new("TextLabel", frame)
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.Text = "Menu de Funções"
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.BackgroundTransparency = 1
titleLabel.Font = Enum.Font.SourceSans
titleLabel.TextSize = 24

local function createButton(name, position, callback)
    local button = Instance.new("TextButton", frame)
    button.Size = UDim2.new(1, -20, 0, 50)
    button.Position = UDim2.new(0, 10, 0, position)
    button.Text = name
    button.TextColor3 = Color3.new(1, 1, 1)
    button.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 18
    button.MouseButton1Click:Connect(callback)
end

local dangerMode = false
local invisibilityDistance = 5
local escapeDistance = 10
local isInvisible = false

-- Função para verificar a distância entre dois pontos
local function getDistance(pos1, pos2)
    return (pos1 - pos2).Magnitude
end

-- Função para encontrar o assassino
local function findMurderer()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            -- Aqui você precisa de uma maneira de identificar o assassino
            -- Isso pode variar dependendo de como o jogo armazena essa informação
            local role = "Murderer" -- Exemplo: substituir com a lógica correta
            if role == "Murderer" then
                return player
            end
        end
    end
    return nil
end

-- Função para tornar o jogador invisível
local function setInvisibility(state)
    local character = LocalPlayer.Character
    if character then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.Transparency = state and 1 or 0
                part.CanCollide = not state
            end
        end
        isInvisible = state
    end
end

-- Função para fugir do assassino
local function runFromMurderer()
    local murderer = findMurderer()
    if murderer and murderer.Character and murderer.Character:FindFirstChild("HumanoidRootPart") then
        local murdererPosition = murderer.Character.HumanoidRootPart.Position
        local playerPosition = LocalPlayer.Character.HumanoidRootPart.Position
        if getDistance(playerPosition, murdererPosition) < escapeDistance then
            local direction = (playerPosition - murdererPosition).unit
            LocalPlayer.Character.Humanoid:MoveTo(playerPosition + direction * escapeDistance)
        end
    end
end

-- Função para atirar no assassino
local function shootMurderer()
    local murderer = findMurderer()
    if murderer and murderer.Character and murderer.Character:FindFirstChild("HumanoidRootPart") then
        local gun = LocalPlayer.Backpack:FindFirstChild("Gun") or LocalPlayer.Character:FindFirstChild("Gun")
        if gun then
            LocalPlayer.Character.HumanoidRootPart.CFrame = murderer.Character.HumanoidRootPart.CFrame
            gun:Activate()
        end
    end
end

-- Função para teletransportar para a arma do xerife
local function teleportToSheriffGun()
    for _, item in pairs(Workspace:GetChildren()) do
        if item:IsA("Tool") and item.Name == "SheriffGun" then
            LocalPlayer.Character.HumanoidRootPart.CFrame = item.Handle.CFrame
            break
        end
    end
end

-- Função para adicionar ESP a um objeto
local function addESP(object)
    local espBox = Instance.new("BoxHandleAdornment")
    espBox.Size = object.Size
    espBox.Adornee = object
    espBox.Color3 = Color3.new(1, 0, 0) -- Cor vermelha
    espBox.AlwaysOnTop = true
    espBox.ZIndex = 10
    espBox.Transparency = 0.5
    espBox.Parent = object
end

-- Função para atualizar a contagem de jogadores restantes
local function updatePlayerCount()
    local remainingPlayers = #Players:GetPlayers()
    print("Jogadores restantes: " .. remainingPlayers)
end

-- Função para ativar/desativar o modo perigo
local function toggleDangerMode()
    dangerMode = not dangerMode
    if dangerMode then
        print("Modo perigo ativado!")
    else
        print("Modo perigo desativado!")
    end
end

-- Botões do menu
createButton("Ativar/Desativar Modo Perigo", 60, toggleDangerMode)
createButton("Teletransportar para Arma do Xerife", 120, teleportToSheriffGun)
createButton("Atirar no Assassino", 180, shootMurderer)

-- Loop para verificar a proximidade do assassino e fugir se necessário
RunService.RenderStepped:Connect(function()
    if dangerMode then
        runFromMurderer()
    end
    local murderer = findMurderer()
    if murderer and murderer.Character and murderer.Character:FindFirstChild("HumanoidRootPart") then
        local murdererPosition = murderer.Character.HumanoidRootPart.Position
        local playerPosition = LocalPlayer.Character.HumanoidRootPart.Position
        if getDistance(playerPosition, murdererPosition) < invisibilityDistance then
            if not isInvisible then
                setInvisibility(true)
            end
        else
            if isInvisible then
                setInvisibility(false)
            end
        end
    end
end)

-- Inicializa a contagem de jogadores no início do jogo
updatePlayerCount()
