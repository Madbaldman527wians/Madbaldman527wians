-- Função para criar a interface de coletar animações
local function criarGuiColetarAnimacoes()
    -- Cria a ScreenGui com o nome "AnimationCollectorGui"
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "AnimationCollectorGui"
    screenGui.ResetOnSpawn = false -- Mantém o GUI ativo após o respawn
    screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

    -- Cria o Frame principal
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 500, 0, 400)
    frame.Position = UDim2.new(0.5, -250, 0.5, -200)
    frame.BackgroundColor3 = Color3.fromRGB(32, 34, 37) -- Cor escura moderna
    frame.BorderSizePixel = 0
    frame.Active = true
    frame.Draggable = true -- Frame arrastável
    frame.Parent = screenGui

    -- Adiciona cantos arredondados ao frame
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = frame

    -- Título do GUI
    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(0, 480, 0, 50)
    titleLabel.Position = UDim2.new(0, 10, 0, 10)
    titleLabel.Text = "Coletar Animações de Gears"
    titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 24
    titleLabel.TextScaled = true -- Texto redimensionável
    titleLabel.TextWrapped = true -- Quebra de linha automática
    titleLabel.Parent = frame

    -- Cria o botão para coletar as animações dos gears
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 200, 0, 50)
    button.Position = UDim2.new(0.5, -100, 1, -60)
    button.Text = "Coletar Animações"
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BackgroundColor3 = Color3.fromRGB(64, 68, 75)
    button.Font = Enum.Font.GothamBold
    button.TextSize = 20
    button.TextScaled = true -- Texto redimensionável
    button.TextWrapped = true -- Quebra de linha automática
    button.Parent = frame

    -- Adiciona cantos arredondados ao botão
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 8)
    buttonCorner.Parent = button

    -- Cria o scrolling frame para exibir as animações
    local scrollingFrame = Instance.new("ScrollingFrame")
    scrollingFrame.Size = UDim2.new(0, 480, 0, 250)
    scrollingFrame.Position = UDim2.new(0, 10, 0, 70)
    scrollingFrame.BackgroundColor3 = Color3.fromRGB(47, 49, 54)
    scrollingFrame.BorderSizePixel = 0
    scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 0) -- O tamanho será ajustado conforme os itens são adicionados
    scrollingFrame.ScrollBarThickness = 6
    scrollingFrame.Parent = frame

    -- Adiciona cantos arredondados ao scrolling frame
    local scrollingFrameCorner = Instance.new("UICorner")
    scrollingFrameCorner.CornerRadius = UDim.new(0, 8)
    scrollingFrameCorner.Parent = scrollingFrame

    -- Função para coletar e exibir as animações dos gears
    local function coletarAnimacoes()
        local animations = {}
        scrollingFrame:ClearAllChildren() -- Limpa o conteúdo anterior

        -- Obtém o jogador local e sua mochila
        local player = game.Players.LocalPlayer

        -- Verifica apenas gears (itens) na mochila
        for _, item in pairs(player.Backpack:GetChildren()) do
            -- Verifica animações associadas ao item (gear)
            for _, anim in pairs(item:GetDescendants()) do
                if anim:IsA("Animation") and anim.AnimationId and anim.AnimationId ~= "" then
                    -- Cria um botão para cada animação de gear
                    local animButton = Instance.new("TextButton")
                    animButton.Size = UDim2.new(0, 400, 0, 50)
                    animButton.Position = UDim2.new(0, 40, 0, #animations * 60)
                    animButton.Text = "Gear: " .. item.Name .. " | Animação: " .. anim.Name
                    animButton.BackgroundColor3 = Color3.fromRGB(64, 68, 75)
                    animButton.TextColor3 = Color3.fromRGB(255, 255, 255)
                    animButton.Font = Enum.Font.Gotham
                    animButton.TextSize = 18
                    animButton.TextScaled = true -- Ajusta o texto automaticamente
                    animButton.TextWrapped = true -- Quebra de linha automática
                    animButton.Parent = scrollingFrame

                    -- Ajusta o CanvasSize do scrolling frame conforme adiciona animações
                    scrollingFrame.CanvasSize = UDim2.new(0, 0, 0, #animations * 60 + 60)

                    -- Ao clicar, abre o GUI com o ID da animação
                    animButton.MouseButton1Click:Connect(function()
                        abrirGuiAnimacao(anim.Name, anim.AnimationId)
                    end)

                    table.insert(animations, animButton)
                end
            end
        end

        if #animations == 0 then
            -- Caso não encontre animações, exibe uma mensagem
            local noAnimsLabel = Instance.new("TextLabel")
            noAnimsLabel.Size = UDim2.new(0, 480, 0, 50)
            noAnimsLabel.Position = UDim2.new(0, 0, 0, 0)
            noAnimsLabel.Text = "Nenhuma animação encontrada nos gears."
            noAnimsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            noAnimsLabel.BackgroundTransparency = 1
            noAnimsLabel.Font = Enum.Font.Gotham
            noAnimsLabel.TextSize = 18
            noAnimsLabel.TextScaled = true -- Texto redimensionável
            noAnimsLabel.TextWrapped = true -- Quebra de linha automática
            noAnimsLabel.Parent = scrollingFrame
        end
    end

    -- Conecta a função ao clique do botão
    button.MouseButton1Click:Connect(coletarAnimacoes)
end

-- Função para abrir o GUI de exibir animação dentro do GUI principal
function abrirGuiAnimacao(nomeAnimacao, animationId)
    -- Remove qualquer GUI de animação aberto anteriormente
    local existingAnimGui = game.Players.LocalPlayer.PlayerGui:FindFirstChild("AnimationIDGui")
    if existingAnimGui then
        existingAnimGui:Destroy()
    end

    -- Cria o GUI para exibir o ID da animação
    local animationGui = Instance.new("Frame")
    animationGui.Name = "AnimationIDGui"
    animationGui.Size = UDim2.new(0, 400, 0, 200)
    animationGui.Position = UDim2.new(0.5, -200, 0.5, -100)
    animationGui.BackgroundColor3 = Color3.fromRGB(32, 34, 37)
    animationGui.Active = true
    animationGui.Draggable = true
    animationGui.Parent = game.Players.LocalPlayer.PlayerGui.AnimationCollectorGui

    -- Adiciona cantos arredondados ao frame de animação
    local animGuiCorner = Instance.new("UICorner")
    animGuiCorner.CornerRadius = UDim.new(0, 10)
    animGuiCorner.Parent = animationGui

   -- Título da animação
    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(0, 380, 0, 50)
    titleLabel.Position = UDim2.new(0, 10, 0, 10)
    titleLabel.Text = "Animação: " .. nomeAnimacao
    titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 24
    titleLabel.TextScaled = true -- Texto redimensionável
    titleLabel.TextWrapped = true -- Quebra de linha automática
    titleLabel.Parent = animationGui

    -- Exibir o ID da animação
    local idLabel = Instance.new("TextLabel")
    idLabel.Size = UDim2.new(0, 380, 0, 50)
    idLabel.Position = UDim2.new(0, 10, 0, 80)
    idLabel.Text = "ID da Animação: " .. animationId
    idLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    idLabel.BackgroundTransparency = 1
    idLabel.Font = Enum.Font.Gotham
    idLabel.TextSize = 18
    idLabel.TextScaled = true -- Texto redimensionável
    idLabel.TextWrapped = true -- Quebra de linha automática
    idLabel.Parent = animationGui

    -- Botão para copiar o ID da animação
    local copyButton = Instance.new("TextButton")
    copyButton.Size = UDim2.new(0, 200, 0, 50)
    copyButton.Position = UDim2.new(0.5, -100, 1, -70)
    copyButton.Text = "Copiar ID"
    copyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    copyButton.BackgroundColor3 = Color3.fromRGB(64, 68, 75)
    copyButton.Font = Enum.Font.GothamBold
    copyButton.TextSize = 20
    copyButton.TextScaled = true -- Texto redimensionável
    copyButton.TextWrapped = true -- Quebra de linha automática
    copyButton.Parent = animationGui

    -- Adiciona cantos arredondados ao botão de cópia
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 8)
    buttonCorner.Parent = copyButton

    -- Função para copiar o ID e enviar notificação
    copyButton.MouseButton1Click:Connect(function()
        -- Copiar o ID da animação para o clipboard
        if setclipboard then
            setclipboard(animationId) -- Copia para a área de transferência
        end
        
        -- Enviar notificação ao jogador
        game.StarterGui:SetCore("SendNotification", {
            Title = "Animação Copiada!";
            Text = "Copied!";
            Duration = 2; -- Duração de 2 segundos
        })
    end)
end

-- Inicializa o GUI principal
criarGuiColetarAnimacoes()
