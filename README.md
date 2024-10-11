-- Variável para controlar o estado do aimbot
local aimbotAtivo = false

-- Função para encontrar o jogador alvo mais próximo
function encontrarAlvo()
    local alvoMaisProximo = nil
    local menorDistancia = math.huge -- Inicia com uma distância muito alta

    for _, jogador in pairs(game.Players:GetPlayers()) do
        if jogador ~= game.Players.LocalPlayer and jogador.Character and jogador.Character:FindFirstChild("HumanoidRootPart") then
            local distancia = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - jogador.Character.HumanoidRootPart.Position).Magnitude
            if distancia < menorDistancia then
                menorDistancia = distancia
                alvoMaisProximo = jogador
            end
        end
    end
    
    return alvoMaisProximo
end

-- Função para ajustar a mira da câmera do jogador
function aimbotar()
    local alvo = encontrarAlvo()
    
    if alvo and alvo.Character and alvo.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = alvo.Character.HumanoidRootPart.Position
        local jogador = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
        
        -- Calcula a nova direção para a mira
        local novaCFrame = CFrame.new(jogador, humanoidRootPart)

        -- Ajusta a CFrame da câmera do jogador
        game.Workspace.CurrentCamera.CFrame = novaCFrame
    end
end

-- Função para alternar o estado do aimbot
function alternarAimbot()
    aimbotAtivo = not aimbotAtivo -- Inverte o estado
    if aimbotAtivo then
        print("Aimbot ativado")
    else
        print("Aimbot desativado")
    end
end

-- Conectar a função de alternar à tecla "E"
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessedEvent)
    if input.KeyCode == Enum.KeyCode.E and not gameProcessedEvent then
        alternarAimbot() -- Chama a função de alternar
    end
end)

-- Loop de aimbot
while true do
    if aimbotAtivo then
        aimbotar()
    end
    wait(0.1) -- Ajuste a frequência de ajuste de mira
end
