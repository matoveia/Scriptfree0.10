local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = game.Workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()
local RayParams = RaycastParams.new()

-- Nome exato da arma
local ArmaPermitida = "Arisaka 7.7mm"

-- Configurações para raycast (ignorar obstáculos)
RayParams.IgnoreWater = true
RayParams.FilterType = Enum.RaycastFilterType.Blacklist
RayParams.FilterDescendantsInstances = {LocalPlayer.Character}  -- Ignorar o próprio personagem

-- Função para disparar um tiro que atravessa paredes
local function ShootRay()
    if TemArmaCerta() then
        local StartPosition = Camera.CFrame.Position
        local Direction = (Mouse.Hit.p - StartPosition).unit * 1000 -- 1000 é a distância máxima do raycast (ajuste conforme necessário)
        
        -- Raycast para detectar o alvo
        local RaycastResult = workspace:Raycast(StartPosition, Direction, RayParams)

        -- Verifica se o raio atingiu algo
        if RaycastResult then
            local HitPart = RaycastResult.Instance
            local HitPosition = RaycastResult.Position

            -- Verifica se atingiu um inimigo
            if HitPart and HitPart.Parent and HitPart.Parent:FindFirstChild("Humanoid") then
                local TargetPlayer = Players:GetPlayerFromCharacter(HitPart.Parent)
                if TargetPlayer and TargetPlayer.TeamColor == BrickColor.new("Bright red") then
                    -- Atingiu um inimigo, você pode adicionar a lógica do aimbot aqui
                    print("Inimigo atingido!")
                end
            end
        end
    end
end

-- Função para verificar se está segurando a arma certa
local function TemArmaCerta()
    local Character = LocalPlayer.Character
    if Character and Character:FindFirstChildOfClass("Tool") then
        local Arma = Character:FindFirstChildOfClass("Tool")
        return Arma.Name == ArmaPermitida
    end
    return false
end

-- Ao pressionar o botão esquerdo do mouse, disparar o raio
Mouse.Button1Down:Connect(function()
    ShootRay()
end)
