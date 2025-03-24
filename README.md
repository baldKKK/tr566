-- 🟢 AVISO: Este script deve ser executado no Xeno Executor

-- Criando um GUI para ativar os comandos
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui

local TextButton = Instance.new("TextButton")
TextButton.Parent = ScreenGui
TextButton.Size = UDim2.new(0, 200, 0, 50)
TextButton.Position = UDim2.new(0.5, -100, 0.1, 0)
TextButton.Text = "Ativar Voo"
TextButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

-- 🔹 Sistema de voo
local flying = false
TextButton.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if not character then return end
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end

    if flying then
        -- Desativar voo
        if humanoidRootPart:FindFirstChild("BodyVelocity") then
            humanoidRootPart.BodyVelocity:Destroy()
        end
        flying = false
        TextButton.Text = "Ativar Voo"
        TextButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    else
        -- Ativar voo
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, 50, 0) -- Faz o jogador subir
        bodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
        bodyVelocity.Parent = humanoidRootPart
        flying = true
        TextButton.Text = "Desativar Voo"
        TextButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)

-- 🔹 Comando de matar todos
local function killAll()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character then
            local humanoid = player.Character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.Health = 0
            end
        end
    end
end

-- 🔹 Comando de puxar todos os jogadores
local function pullAll()
    local player = game.Players.LocalPlayer
    if not player.Character then return end
    local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end

    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character then
            local targetRoot = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
            if targetRoot then
                targetRoot.CFrame = humanoidRootPart.CFrame + Vector3.new(0, 3, 0)
            end
        end
    end
end

-- 🔹 Monitorar o Chat para Comandos
game.Players.LocalPlayer.Chatted:Connect(function(msg)
    if msg == "!killall" then
        killAll()
    elseif msg == "!pullall" then
        pullAll()
    end
end)
