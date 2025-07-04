local Players = game:GetService("Players")
local lp = Players.LocalPlayer

local function setupCharacter(char)
    local torso = char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
    if not torso then return end

    -- Remover roupas e acessórios
    for _, item in ipairs(char:GetChildren()) do
        if item:IsA("Accessory") or item:IsA("Hat") or item:IsA("Shirt") or item:IsA("Pants") or item:IsA("ShirtGraphic") then
            item:Destroy()
        end
    end
    local bodyColors = char:FindFirstChild("BodyColors") or char:FindFirstChild("Body Colors")
    if bodyColors then
        bodyColors:Destroy()
    end

    local corPele = torso.BrickColor

    -- Criar cápsula (bola esticada no peito)
    local capsula = Instance.new("Part")
    capsula.Size = Vector3.new(1.6, 1.2, 1.2)
    capsula.Material = Enum.Material.Neon
    capsula.BrickColor = corPele
    capsula.Anchored = false
    capsula.CanCollide = false
    capsula.Parent = char

    local mesh = Instance.new("SpecialMesh")
    mesh.MeshType = Enum.MeshType.Sphere
    mesh.Scale = Vector3.new(1.6, 1.2, 1.2)
    mesh.Parent = capsula

    local weld = Instance.new("Weld")
    weld.Part0 = torso
    weld.Part1 = capsula
    weld.C0 = CFrame.new(0, 0.3, -0.6)
    weld.Parent = capsula

    -- Criar as duas bolas grudadas na cintura
    local bolaTamanho = 2
    local separacao = 0.95
    local altura = -1

    for i = -0.5, 0.5, 1 do
        local bola = Instance.new("Part")
        bola.Shape = Enum.PartType.Ball
        bola.Size = Vector3.new(bolaTamanho, bolaTamanho, bolaTamanho)
        bola.BrickColor = corPele
        bola.Material = Enum.Material.Neon
        bola.Anchored = false
        bola.CanCollide = false
        bola.Parent = char

        local weldBola = Instance.new("Weld")
        weldBola.Part0 = torso
        weldBola.Part1 = bola
        weldBola.C0 = CFrame.new(i * separacao, altura, 1)
        weldBola.Parent = bola
    end
end

if lp.Character then
    setupCharacter(lp.Character)
end
lp.CharacterAdded:Connect(setupCharacter)
