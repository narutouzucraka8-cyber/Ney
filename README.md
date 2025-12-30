-- CONFIGURAÇÃO
local VELOCIDADE_INFINITA = 500 -- pode aumentar se quiser
local JUMP_POWER = 100

local player = game.Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- FUNÇÃO PARA AJUSTAR VELOCIDADE SEMPRE
local function aplicarVelocidade(humanoid)
    humanoid.WalkSpeed = VELOCIDADE_INFINITA
    humanoid.JumpPower = JUMP_POWER
end

-- QUANDO O PERSONAGEM SPAWNAR
local function onCharacterAdded(character)
    local humanoid = character:WaitForChild("Humanoid")

    aplicarVelocidade(humanoid)

    -- REAJUSTA SE O JOGO TENTAR MUDAR
    humanoid:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
        if humanoid.WalkSpeed ~= VELOCIDADE_INFINITA then
            humanoid.WalkSpeed = VELOCIDADE_INFINITA
        end
    end)
end

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
    onCharacterAdded(player.Character)
end

-- INFINITE JUMP
UserInputService.JumpRequest:Connect(function()
    if player.Character then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- GARANTE VELOCIDADE SEMPRE
RunService.RenderStepped:Connect(function()
    if player.Character then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if humanoid and humanoid.WalkSpeed ~= VELOCIDADE_INFINITA then
            humanoid.WalkSpeed = VELOCIDADE_INFINITA
        end
    end
end)

