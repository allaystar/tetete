-- SCRIPT PARA [MEGUNA] SORCERER TOWER DEFENSE
-- SEM DEPENDÊNCIAS EXTERNAS - 100% AUTOSSUFICIENTE

local player = game:GetService("Players").LocalPlayer
local mouse = player:GetMouse()
local replicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

-- FUNÇÃO PARA ENCONTRAR O DINHEIRO/GEMAS
local function encontrarStats()
    local stats = {}
    
    -- Procura em leaderstats
    if player:FindFirstChild("leaderstats") then
        for _, obj in pairs(player.leaderstats:GetChildren()) do
            if obj:IsA("NumberValue") then
                table.insert(stats, obj)
            end
        end
    end
    
    -- Procura em outras pastas comuns
    local pastasComuns = {"Stats", "Data", "Currency", "Money", "Gems"}
    for _, nomePasta in pairs(pastasComuns) do
        if player:FindFirstChild(nomePasta) then
            for _, obj in pairs(player[nomePasta]:GetChildren()) do
                if obj:IsA("NumberValue") then
                    table.insert(stats, obj)
                end
            end
        end
    end
    
    return stats
end

-- CRIAR GUI SIMPLES
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.Name = "SorcererHack"
ScreenGui.ResetOnSpawn = false

-- FRAME PRINCIPAL
local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 400, 0, 500)
MainFrame.Active = true
MainFrame.Draggable = true

-- TÍTULO
local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
Title.BorderSizePixel = 0
Title.Size = UDim2.new(1, 0, 0, 35)
Title.Text = "🔮 SORCERER HACK [MEGUNA]"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18

-- BOTÃO FECHAR
local CloseBtn = Instance.new("TextButton")
CloseBtn.Parent = MainFrame
CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseBtn.BorderSizePixel = 0
CloseBtn.Position = UDim2.new(1, -35, 0, 0)
CloseBtn.Size = UDim2.new(0, 35, 0, 35)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 16
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- FUNÇÃO PARA CRIAR BOTÕES
local yOffset = 45
local function criarBotao(texto, cor, callback)
    local botao = Instance.new("TextButton")
    botao.Parent = MainFrame
    botao.BackgroundColor3 = cor
    botao.BorderSizePixel = 0
    botao.Position = UDim2.new(0.05, 0, 0, yOffset)
    botao.Size = UDim2.new(0.9, 0, 0, 40)
    botao.Text = texto
    botao.TextColor3 = Color3.fromRGB(255, 255, 255)
    botao.Font = Enum.Font.Gotham
    botao.TextSize = 14
    botao.MouseButton1Click:Connect(callback)
    yOffset = yOffset + 45
    return botao
end

-- FUNÇÃO PARA CRIAR TOGGLES (ON/OFF)
local function criarToggle(texto, cor, posY, callback)
    local frame = Instance.new("Frame")
    frame.Parent = MainFrame
    frame.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
    frame.BorderSizePixel = 0
    frame.Position = UDim2.new(0.05, 0, 0, posY)
    frame.Size = UDim2.new(0.9, 0, 0, 40)
    
    local label = Instance.new("TextLabel")
    label.Parent = frame
    label.BackgroundTransparency = 1
    label.Position = UDim2.new(0, 10, 0, 0)
    label.Size = UDim2.new(0.6, 0, 1, 0)
    label.Text = texto
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.Font = Enum.Font.Gotham
    label.TextSize = 14
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local toggle = Instance.new("TextButton")
    toggle.Parent = frame
    toggle.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    toggle.BorderSizePixel = 0
    toggle.Position = UDim2.new(0.7, 0, 0.05, 0)
    toggle.Size = UDim2.new(0.25, 0, 0.9, 0)
    toggle.Text = "OFF"
    toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggle.Font = Enum.Font.GothamBold
    toggle.TextSize = 12
    
    local ativo = false
    toggle.MouseButton1Click:Connect(function()
        ativo = not ativo
        if ativo then
            toggle.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
            toggle.Text = "ON"
        else
            toggle.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
            toggle.Text = "OFF"
        end
        callback(ativo)
    end)
    
    return frame
end

-- ==============================================
-- HACKS DE RECURSOS
-- ==============================================
local resLabel = Instance.new("TextLabel")
resLabel.Parent = MainFrame
resLabel.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
resLabel.BorderSizePixel = 0
resLabel.Position = UDim2.new(0, 0, 0, yOffset)
resLabel.Size = UDim2.new(1, 0, 0, 30)
resLabel.Text = "💰 RECURSOS"
resLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
resLabel.Font = Enum.Font.GothamBold
resLabel.TextSize = 16
yOffset = yOffset + 35

-- Dinheiro Infinito (Toggle)
criarToggle("💰 Dinheiro Infinito", Color3.fromRGB(70, 130, 180), yOffset, function(estado)
    if estado then
        spawn(function()
            while estado do
                local stats = encontrarStats()
                for _, stat in pairs(stats) do
                    if stat.Name:lower():find("cash") or stat.Name:lower():find("money") or stat.Name:lower():find("yen") then
                        stat.Value = 999999999
                    end
                end
                wait(0.1)
            end
        end)
    end
end)
yOffset = yOffset + 45

-- Gemas Infinitas (Toggle)
criarToggle("💎 Gemas Infinitas", Color3.fromRGB(180, 70, 180), yOffset, function(estado)
    if estado then
        spawn(function()
            while estado do
                local stats = encontrarStats()
                for _, stat in pairs(stats) do
                    if stat.Name:lower():find("gem") or stat.Name:lower():find("diamond") or stat.Name:lower():find("crystal") then
                        stat.Value = 999999999
                    end
                end
                wait(0.1)
            end
        end)
    end
end)
yOffset = yOffset + 45

-- Botão: Adicionar Dinheiro
criarBotao("➕ Adicionar +10.000 DINHEIRO", Color3.fromRGB(0, 150, 0), function()
    local stats = encontrarStats()
    for _, stat in pairs(stats) do
        if stat.Name:lower():find("cash") or stat.Name:lower():find("money") then
            stat.Value = stat.Value + 10000
        end
    end
end)
yOffset = yOffset + 45

-- Botão: Adicionar Gemas
criarBotao("➕ Adicionar +1.000 GEMAS", Color3.fromRGB(150, 0, 150), function()
    local stats = encontrarStats()
    for _, stat in pairs(stats) do
        if stat.Name:lower():find("gem") or stat.Name:lower():find("diamond") then
            stat.Value = stat.Value + 1000
        end
    end
end)
yOffset = yOffset + 45

-- ==============================================
-- HACKS DE COMBATE
-- ==============================================
local combLabel = Instance.new("TextLabel")
combLabel.Parent = MainFrame
combLabel.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
combLabel.BorderSizePixel = 0
combLabel.Position = UDim2.new(0, 0, 0, yOffset)
combLabel.Size = UDim2.new(1, 0, 0, 30)
combLabel.Text = "⚔️ COMBATE"
combLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
combLabel.Font = Enum.Font.GothamBold
combLabel.TextSize = 16
yOffset = yOffset + 35

-- God Mode
local godMode = false
criarToggle("🛡️ God Mode (Invencível)", Color3.fromRGB(0, 200, 200), yOffset, function(estado)
    godMode = estado
    if estado then
        spawn(function()
            while godMode do
                if player.Character and player.Character:FindFirstChild("Humanoid") then
                    player.Character.Humanoid.MaxHealth = 9e9
                    player.Character.Humanoid.Health = 9e9
                end
                wait(0.1)
            end
        end)
    end
end)
yOffset = yOffset + 45

-- Kill Aura
local killAura = false
criarToggle("💀 Kill Aura (Auto Kill)", Color3.fromRGB(200, 0, 0), yOffset, function(estado)
    killAura = estado
    if estado then
        spawn(function()
            while killAura do
                local enemies = workspace:FindFirstChild("Enemies") or workspace:FindFirstChild("Mobs")
                if enemies then
                    for _, enemy in pairs(enemies:GetChildren()) do
                        if enemy:FindFirstChild("Humanoid") then
                            enemy.Humanoid.Health = 0
                        end
                    end
                end
                wait(0.5)
            end
        end)
    end
end)
yOffset = yOffset + 45

-- Matar todos agora
criarBotao("⚔️ MATAR TODOS INIMIGOS AGORA", Color3.fromRGB(200, 50, 50), function()
    local enemies = workspace:FindFirstChild("Enemies") or workspace:FindFirstChild("Mobs")
    if enemies then
        for _, enemy in pairs(enemies:GetChildren()) do
            if enemy:FindFirstChild("Humanoid") then
                enemy.Humanoid.Health = 0
            end
        end
    end
end)
yOffset = yOffset + 45

-- ==============================================
-- SCAM TRADE
-- ==============================================
local scamLabel = Instance.new("TextLabel")
scamLabel.Parent = MainFrame
scamLabel.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
scamLabel.BorderSizePixel = 0
scamLabel.Position = UDim2.new(0, 0, 0, yOffset)
scamLabel.Size = UDim2.new(1, 0, 0, 30)
scamLabel.Text = "🤝 SCAM TRADE"
scamLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
scamLabel.Font = Enum.Font.GothamBold
scamLabel.TextSize = 16
yOffset = yOffset + 35

-- Caixa de texto para mensagem
local msgBox = Instance.new("TextBox")
msgBox.Parent = MainFrame
msgBox.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
msgBox.BorderSizePixel = 0
msgBox.Position = UDim2.new(0.05, 0, 0, yOffset)
msgBox.Size = UDim2.new(0.9, 0, 0, 35)
msgBox.PlaceholderText = "Digite sua mensagem de scam..."
msgBox.TextColor3 = Color3.fromRGB(255, 255, 255)
msgBox.Font = Enum.Font.Gotham
msgBox.TextSize = 14
msgBox.ClearTextOnFocus = false
yOffset = yOffset + 40

-- Botão enviar mensagem
criarBotao("📨 ENVIAR MENSAGEM", Color3.fromRGB(50, 150, 200), function()
    if msgBox.Text ~= "" then
        game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(msgBox.Text, "All")
    end
end)
yOffset = yOffset + 45

-- Lista de mensagens prontas
local mensagens = {
    "🎁 TROCA RÁPIDA: Dou 10k GEMAS por 1k DINHEIRO!",
    "⚠️ URGENTE: Preciso passar meus itens lendários!",
    "💰 GANHE 5x GEMAS - Só hoje!",
    "🆘 Alguém me ensina a trocar? Primeira vez aqui!",
    "🔄 DOUBLE GEMS TRADE - Só confiar!",
    "💎 FREE GEMS - Me add e /trade"
}

-- Botão enviar scam automático
local scamAuto = false
criarToggle("📢 SCAM AUTOMÁTICO", Color3.fromRGB(200, 100, 0), yOffset, function(estado)
    scamAuto = estado
    if estado then
        spawn(function()
            while scamAuto do
                local msg = mensagens[math.random(1, #mensagens)]
                game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(msg, "All")
                wait(8)
            end
        end)
    end
end)
yOffset = yOffset + 45

-- ==============================================
-- TELEPORTE E UTILITÁRIOS
-- ==============================================
local teleLabel = Instance.new("TextLabel")
teleLabel.Parent = MainFrame
teleLabel.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
teleLabel.BorderSizePixel = 0
teleLabel.Position = UDim2.new(0, 0, 0, yOffset)
teleLabel.Size = UDim2.new(1, 0, 0, 30)
teleLabel.Text = "🌍 UTILITÁRIOS"
teleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
teleLabel.Font = Enum.Font.GothamBold
teleLabel.TextSize = 16
yOffset = yOffset + 35

-- Speed Hack
criarBotao("🏃 SPEED HACK (10x)", Color3.fromRGB(0, 200, 0), function()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = 160
    end
end)
yOffset = yOffset + 45

-- Reset Speed
criarBotao("🔄 RESETAR VELOCIDADE", Color3.fromRGB(200, 200, 0), function()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = 16
    end
end)
yOffset = yOffset + 45

-- Botão para mostrar/esconder (F4)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.F4 then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

-- PRINT DE CONFIRMAÇÃO
print("="*50)
print("🔮 SORCERER HACK CARREGADO COM SUCESSO!")
print("📌 Pressione F4 para abrir/fechar o menu")
print("💰 Hacks de recursos disponíveis")
print("⚔️ God Mode e Kill Aura ativáveis")
print("🤝 Scam Trade incluso")
print("="*50)

-- AVISO
local aviso = Instance.new("TextLabel")
aviso.Parent = ScreenGui
aviso.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
aviso.BackgroundTransparency = 0.5
aviso.Position = UDim2.new(0, 10, 0, 10)
aviso.Size = UDim2.new(0, 200, 0, 30)
aviso.Text = "✅ HACK ATIVO | F4 = Menu"
aviso.TextColor3 = Color3.fromRGB(0, 255, 0)
aviso.Font = Enum.Font.GothamBold
aviso.TextSize = 14
