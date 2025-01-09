--// Configuração Inicial - Lighting e Tecnologia (Focado em Realismo no PC) \\--
local Lighting = game:GetService("Lighting")

-- Tecnologia de Iluminação
Lighting.Technology = Enum.Technology.Future
Lighting.GlobalShadows = true

-- Suavizando o Ambiente
Lighting.Brightness = 1.8         -- Brilho geral, mantido abaixo do exagero
Lighting.ExposureCompensation = 0 -- Sem compensação extra de exposição
Lighting.ClockTime = 14           -- Simulando meio da tarde
Lighting.OutdoorAmbient = Color3.fromRGB(100, 100, 110)
Lighting.Ambient = Color3.fromRGB(70, 75, 80)

-- Sombras e Distância de Visão
Lighting.FogColor = Color3.fromRGB(145, 155, 160)
Lighting.FogStart = 30
Lighting.FogEnd = 900
Lighting.EnvironmentDiffuseScale = 1
Lighting.EnvironmentSpecularScale = 1
Lighting.ShadowSoftness = 0.2

--// Atmosfera (Sem Texturas do Céu) \\--
local atmosphere = Instance.new("Atmosphere")
atmosphere.Density = 0.4            -- Mantém um leve toque de névoa
atmosphere.Offset = 0.15
atmosphere.Glare = 0.1             -- Reflexos suaves
atmosphere.Haze = 1.8              -- Névoa no horizonte
atmosphere.Color = Color3.fromRGB(200, 210, 220)
atmosphere.Decay = Color3.fromRGB(130, 140, 150)
atmosphere.Parent = Lighting

--// Reflexos nos Objetos \\--
local function applyReflectance(reflectValue)
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            obj.Reflectance = reflectValue
            obj.CastShadow = true
        end
    end
end
applyReflectance(0.2) -- Reflexo moderado para realismo sutil

--// Pós-Processamento para Realismo \\--
-- Bloom
local bloom = Instance.new("BloomEffect")
bloom.Intensity = 0.35  -- Mantém intensidade controlada
bloom.Threshold = 0.6   -- Limiar para o brilho
bloom.Size = 50         -- Suavidade do bloom
bloom.Parent = Lighting

-- Correção de Cor
local colorCorrection = Instance.new("ColorCorrectionEffect")
colorCorrection.Brightness = -0.05 -- Leve escurecimento
colorCorrection.Contrast = 0.4     -- Contraste equilibrado
colorCorrection.Saturation = -0.1  -- Saturação sutilmente reduzida
colorCorrection.TintColor = Color3.fromRGB(180, 190, 200)
colorCorrection.Parent = Lighting

-- Efeito de Raios Solares
local sunRays = Instance.new("SunRaysEffect")
sunRays.Intensity = 0.02 -- Bem discreto
sunRays.Spread = 1.1
sunRays.Parent = Lighting

-- Profundidade de Campo
local dof = Instance.new("DepthOfFieldEffect")
dof.InFocusRadius = 12    -- Área de foco levemente maior
dof.FocusDistance = 30    -- Distância de foco
dof.NearIntensity = 0.5
dof.FarIntensity = 0.3
dof.Parent = Lighting

-- Leve Blur
local blur = Instance.new("BlurEffect")
blur.Size = 2 -- Pequena suavidade global
blur.Parent = Lighting

--// Luzes Dinâmicas \\--
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        local dynamicLight = Instance.new("PointLight")
        dynamicLight.Brightness = 2.5  -- Intensidade sutil para o personagem
        dynamicLight.Range = 12        -- Alcance moderado
        dynamicLight.Shadows = true    -- Sombras realistas
        dynamicLight.Color = Color3.fromRGB(220, 220, 230)
        dynamicLight.Parent = character:WaitForChild("HumanoidRootPart")
    end)
end)

-- Exemplo de luz focal em algum objeto (poste, lâmpada, etc.)
local postLight = Instance.new("SpotLight")
postLight.Brightness = 4
postLight.Angle = 45
postLight.Range = 18
postLight.Shadows = true
postLight.Color = Color3.fromRGB(210, 215, 220)
postLight.Parent = workspace:WaitForChild("Post") -- Substitua "Post" pelo nome do seu objeto

--// Chuva Realista e Som Ambiente \\--
local rainPart = Instance.new("Part")
rainPart.Name = "RainPart"
rainPart.Size = Vector3.new(500, 1, 500)
rainPart.Position = Vector3.new(0, 100, 0)
rainPart.Anchored = true
rainPart.CanCollide = false
rainPart.Transparency = 1
rainPart.Parent = workspace

local rainEmitter = Instance.new("ParticleEmitter")
rainEmitter.Texture = "rbxassetid://341689523"
rainEmitter.Rate = 1000
rainEmitter.Speed = NumberRange.new(70, 85)
rainEmitter.Lifetime = NumberRange.new(2, 3)
rainEmitter.Transparency = NumberSequence.new(0.6)
rainEmitter.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2), NumberSequenceKeypoint.new(1, 0.4)})
rainEmitter.Rotation = NumberRange.new(0, 360)
rainEmitter.Parent = rainPart

local rainSound = Instance.new("Sound")
rainSound.SoundId = "rbxassetid://1845743147"
rainSound.Looped = true
rainSound.Volume = 0.4
rainSound.Playing = true
rainSound.Parent = rainPart

--// Fim \\--
