if game.Players.LocalPlayer:FindFirstChild("PlayerGui"):FindFirstChild("BladeBallGUI") then
    game.Players.LocalPlayer:FindFirstChild("PlayerGui"):FindFirstChild("BladeBallGUI"):Destroy()
end

-- Criação da GUI
local gui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "BladeBallGUI"
gui.ResetOnSpawn = false

-- Frame azul
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 160, 0, 120)
frame.Position = UDim2.new(0.05, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 102, 204)
frame.Active = true
frame.Draggable = true

local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 12)

-- Layout vertical
local layout = Instance.new("UIListLayout", frame)
layout.Padding = UDim.new(0, 10)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.VerticalAlignment = Enum.VerticalAlignment.Center

-- Variáveis de controle
local autoParry = false
local autoExpand = false

-- Referência à bola
local Ball = workspace:WaitForChild("Ball", 10)

-- Função para detectar distância e parry
function StartAutoParry()
    while autoParry and task.wait() do
        if Ball and Ball:FindFirstChild("Velocity") and Ball:FindFirstChild("Position") then
            local player = game.Players.LocalPlayer
            local character = player.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                local dist = (Ball.Position - character.HumanoidRootPart.Position).Magnitude
                if dist < 25 then
                    -- Tentar parry
                    local vim = game:GetService("VirtualInputManager")
                    vim:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                    task.wait(0.1)
                    vim:SendKeyEvent(false, Enum.KeyCode.E, false, game)
                end
            end
        end
    end
end

-- Função para usar expansão em intervalo
function StartAutoExpand()
    while autoExpand and task.wait(2.5) do
        local vim = game:GetService("VirtualInputManager")
        vim:SendKeyEvent(true, Enum.KeyCode.Q, false, game)
        task.wait(0.1)
        vim:SendKeyEvent(false, Enum.KeyCode.Q, false, game)
    end
end

-- Função botão Auto Parry
local parryBtn = Instance.new("TextButton", frame)
parryBtn.Size = UDim2.new(0, 140, 0, 40)
parryBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
parryBtn.Text = "Auto Parry [OFF]"
parryBtn.TextColor3 = Color3.new(1, 1, 1)
parryBtn.Font = Enum.Font.SourceSansBold
parryBtn.TextSize = 18

parryBtn.MouseButton1Click:Connect(function()
    autoParry = not autoParry
    parryBtn.Text = autoParry and "Auto Parry [ON]" or "Auto Parry [OFF]"
    if autoParry then
        task.spawn(StartAutoParry)
    end
end)

-- Função botão Auto Expansion
local expandBtn = Instance.new("TextButton", frame)
expandBtn.Size = UDim2.new(0, 140, 0, 40)
expandBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
expandBtn.Text = "Auto Expansion [OFF]"
expandBtn.TextColor3 = Color3.new(1, 1, 1)
expandBtn.Font = Enum.Font.SourceSansBold
expandBtn.TextSize = 18

expandBtn.MouseButton1Click:Connect(function()
    autoExpand = not autoExpand
    expandBtn.Text = autoExpand and "Auto Expansion [ON]" or "Auto Expansion [OFF]"
    if autoExpand then
        task.spawn(StartAutoExpand)
    end
end)
