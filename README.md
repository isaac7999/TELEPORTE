local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui")
gui.Parent = player.PlayerGui
gui.Name = "PlayerListGUI"

-- Criando o painel
local panel = Instance.new("Frame")
panel.Parent = gui
panel.Size = UDim2.new(0, 400, 0, 300)
panel.Position = UDim2.new(0.5, -200, 0.5, -150)
panel.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
panel.BorderSizePixel = 0
panel.Draggable = true

-- Título do painel
local title = Instance.new("TextLabel")
title.Parent = panel
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Text = "Lista de Jogadores"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 18
title.TextXAlignment = Enum.TextXAlignment.Center

-- Botão de minimizar
local minimizeButton = Instance.new("TextButton")
minimizeButton.Parent = panel
minimizeButton.Size = UDim2.new(0, 40, 0, 40)
minimizeButton.Position = UDim2.new(1, -40, 0, 0)
minimizeButton.Text = "-"
minimizeButton.TextSize = 24
minimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
minimizeButton.BorderSizePixel = 0

local isMinimized = false
minimizeButton.MouseButton1Click:Connect(function()
    if isMinimized then
        panel.Size = UDim2.new(0, 400, 0, 300)
        isMinimized = false
    else
        panel.Size = UDim2.new(0, 400, 0, 40)
        isMinimized = true
    end
end)

-- Lista de jogadores
local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Parent = panel
scrollFrame.Size = UDim2.new(1, 0, 1, -40)
scrollFrame.Position = UDim2.new(0, 0, 0, 40)
scrollFrame.BackgroundTransparency = 1
scrollFrame.ScrollingDirection = Enum.ScrollingDirection.Y
scrollFrame.CanvasSize = UDim2.new(0, 0, 10, 0)

-- Criar os itens da lista de jogadores
local function updatePlayerList()
    -- Limpar a lista existente
    for _, child in pairs(scrollFrame:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end
    
    -- Adicionar novos itens para cada jogador
    local players = game.Players:GetPlayers()
    for i, plr in ipairs(players) do
        local button = Instance.new("TextButton")
        button.Parent = scrollFrame
        button.Size = UDim2.new(1, 0, 0, 40)
        button.Position = UDim2.new(0, 0, 0, (i - 1) * 40)
        button.Text = plr.Name
        button.TextColor3 = Color3.fromRGB(255, 255, 255)
        button.TextSize = 16
        button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        button.BorderSizePixel = 0
        button.MouseButton1Click:Connect(function()
            -- Teleportar para o jogador clicado
            if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                player.Character:SetPrimaryPartCFrame(plr.Character.HumanoidRootPart.CFrame)
            end
        end)
    end
end

-- Atualizar a lista de jogadores a cada vez que a lista mudar
game.Players.PlayerAdded:Connect(updatePlayerList)
game.Players.PlayerRemoving:Connect(updatePlayerList)
updatePlayerList()
