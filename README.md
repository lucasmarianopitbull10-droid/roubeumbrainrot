-- Serviços Roblox
local ServerStorage = game:GetService("ServerStorage")
local Workspace = game:GetService("Workspace")

-- Nomes dos modelos em ServerStorage
local modelNames = {"BrainrotGarama", "Madundung"}

-- Quantidade de cada entidade a spawnar
local countEach = 10

-- Função para spawnar múltiplos clones em uma região ao redor de um ponto
local function spawnEntities(modelName, count, centerPosition, radius)
    local modelTemplate = ServerStorage:FindFirstChild(modelName)
    if not modelTemplate then
        warn(("Modelo '%s' não encontrado no ServerStorage"):format(modelName))
        return
    end

    for i = 1, count do
        local clone = modelTemplate:Clone()
        clone.Parent = Workspace

        -- Posição aleatória no entorno do ponto central
        local offset = Vector3.new(
            math.random(-radius, radius),
            0,
            math.random(-radius, radius)
        )
        local pos = centerPosition + offset

        if clone.PrimaryPart then
            clone:SetPrimaryPartCFrame(CFrame.new(pos))
        else
            clone:MoveTo(pos)
        end
    end
end

-- Execução do spawn
local function executeSpawn()
    local center = Vector3.new(0, 10, 0) -- posição central (ajuste conforme desejar)
    local radius = 20 -- raio de dispersão (ajuste conforme o tamanho do mapa)

    for _, name in ipairs(modelNames) do
        spawnEntities(name, countEach, center, radius)
    end
end

-- Chama a função após 5 segundos (tempo para carregamento do mapa)
delay(5, executeSpawn)
