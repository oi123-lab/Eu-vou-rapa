local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/tbao143/Library-ui/refs/heads/main/Redzhubui"))()

 

local Window = Library:MakeWindow({ 

Title = "MATRIX HUB V3.0 : BROOKHAVEN RP 🏡", 

SubTitle = "Por : TEAM MATRIX COMMUNITY", 

SaveFolder = "Shnmaxhub Folder" 

}) 

 

Window:AddMinimizeButton({ 

Button = { Image = "rbxassetid://122216401159246", BackgroundTransparency = 0 }, -- Coloque um ID válido 

Corner = { CornerRadius = UDim.new(0, 10) } 

}) 

 

local Tab1 = Window:MakeTab({"Home", "rbxassetid://138700382840270"}) 

 

local Section = Tab1:AddSection("Créditos: Shelby") 

 

Tab1:AddParagraph({"🔧 Interface Reformulada: Nova estrutura visual com foco em eficiência e organização."}) 

Tab1:AddParagraph({"⚙️ Funções Reestruturadas: Melhor desempenho e modularidade interna aplicadas."}) 

Tab1:AddParagraph({"✅ Estabilidade Aprimorada: Menor chance de falhas e resposta mais rápida do sistema."}) 

Tab1:AddParagraph({"🔒 Segurança Reforçada: Proteções adicionais aplicadas para usuários e dados."}) 

Tab1:AddParagraph({"🧩 Integrações Futuras: Base pronta para receber sistemas exclusivos do SHNMAXHUB."}) 

 

local playerName = game.Players.LocalPlayer.Name 

Tab1:AddParagraph({"Olá, " .. playerName .. "! ✅ Seja bem-vindo ao SHNMAXHUB. Aproveite com responsabilidade e bom uso do sistema."})


local Tab2 = Window:MakeTab({"Troll", "rbxassetid://10734934585"})

 

local Section = Tab2:AddSection({"Aba Troll"})

 

local Players = game:GetService("Players")

local TweenService = game:GetService("TweenService")

local LocalPlayer = Players.LocalPlayer

local CurrentCamera = workspace.CurrentCamera

local RunService = game:GetService("RunService")

 

local viewEnabled = false

local currentTarget = nil

local characterAddedConn = nil

local playerNames = {}

 

-- Função para atualizar a lista de jogadores

local function updateDropdown()

playerNames = {}

for _, player in ipairs(Players:GetPlayers()) do

if player ~= LocalPlayer then

table.insert(playerNames, player.Name)

end

end

if Dropdown then

Dropdown:Set(playerNames)

end

end

 

-- Função para tratar a adição de novos jogadores

local function onPlayerAdded(player)

if player ~= LocalPlayer then

table.insert(playerNames, player.Name)

if Dropdown then

Dropdown:Set(playerNames)

end

end

end

 

-- Função para tratar a remoção de jogadores

local function onPlayerRemoving(player)

for i, name in ipairs(playerNames) do

if name == player.Name then

table.remove(playerNames, i)

break

end

end

if Dropdown then

Dropdown:Set(playerNames)

end

 

if currentTarget == player then

stopViewing()

end

end

 

-- Atualizando dropdown com lista de jogadores ao iniciar

Players.PlayerAdded:Connect(onPlayerAdded)

Players.PlayerRemoving:Connect(onPlayerRemoving)

task.delay(1, updateDropdown)

 

-- Função para resetar a câmera do jogador local

local function resetCamera()

if LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait() then

CurrentCamera.CameraSubject = LocalPlayer.Character

end

end

 

-- Função para fazer a transição da câmera para o alvo

local function tweenToTargetPart(part)

local tweenInfo = TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

local targetCFrame = part.CFrame + part.CFrame.LookVector * -10 + Vector3.new(0, 5, 0)

local goal = {CFrame = CFrame.new(targetCFrame.Position, part.Position)}

local tween = TweenService:Create(CurrentCamera, tweenInfo, goal)

tween:Play()

end

 

-- Função para configurar a visualização do alvo

function setViewTarget(targetName)

local targetPlayer = Players:FindFirstChild(targetName)

if not targetPlayer then

warn("[VIEW] Jogador não encontrado: " .. targetName)

return

end

 

currentTarget = targetPlayer

 

-- Desconectar qualquer conexão anterior

if characterAddedConn then

characterAddedConn:Disconnect()

end




characterAddedConn = targetPlayer.CharacterAdded:Connect(function(char)

task.wait(0.1)

if viewEnabled and currentTarget == targetPlayer then

local hrp = char:FindFirstChild("HumanoidRootPart")

if hrp then

tweenToTargetPart(hrp)

end

pcall(function()

CurrentCamera.CameraSubject = char

end)

end

end)

 

-- Verificar o personagem já existente

if targetPlayer.Character then

local hrp = targetPlayer.Character:FindFirstChild("HumanoidRootPart")

if hrp then

tweenToTargetPart(hrp)

end

pcall(function()

CurrentCamera.CameraSubject = targetPlayer.Character

end)

end

end

 

-- Função para parar a visualização

function stopViewing()

viewEnabled = false

currentTarget = nil

if characterAddedConn then

characterAddedConn:Disconnect()

characterAddedConn = nil

end

resetCamera()

end

 

-- Render loop para atualizar a visualização

RunService.RenderStepped:Connect(function()

if viewEnabled and currentTarget then

if not currentTarget:IsDescendantOf(game) then

stopViewing()

return

end

if currentTarget.Character and CurrentCamera.CameraSubject ~= currentTarget.Character then

pcall(function()

CurrentCamera.CameraSubject = currentTarget.Character

end)

end

end

end)

 

-- Adicionando o Dropdown para seleção de jogador

Dropdown = Tab2:AddDropdown({

Name = "Selecione o Jogador.",

Options = playerNames,

Default = {},

MultiSelect = false,

Callback = function(Value)

if typeof(Value) == "string" and Players:FindFirstChild(Value) then

getgenv().Target = Value

if viewEnabled then

setViewTarget(Value)

end

end

end

})

 

-- Toggle para ativar/desativar a visualização

Tab2:AddToggle({

Name = "View",

Default = false,

Callback = function(state)

viewEnabled = state

if state and getgenv().Target then

setViewTarget(getgenv().Target)

else

stopViewing()

end

end

})

 
-- Atualizando o Dropdown quando um jogador entra ou sai

Players.PlayerAdded:Connect(onPlayerAdded)

Players.PlayerRemoving:Connect(onPlayerRemoving)

task.delay(1, updateDropdown)

 

local TweenService = game:GetService("TweenService")

local Players = game:GetService("Players")

local LocalPlayer = Players.LocalPlayer

 

Tab2:AddButton({

Name = "Goto",

Callback = function()

local success, err = pcall(function()

local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

local hrp = character:FindFirstChild("HumanoidRootPart")

 

if not hrp then return end

 

local targetName = getgenv().Target

if not targetName then

warn("[GOTO] Nenhum alvo definido.")

return

end

 

local targetPlayer = Players:FindFirstChild(targetName)

if not targetPlayer or not targetPlayer.Character then

warn("[GOTO] Alvo inválido ou não encontrado.")

return

end

 

local targetHRP = targetPlayer.Character:FindFirstChild("HumanoidRootPart")

if not targetHRP then

warn("[GOTO] Alvo sem HumanoidRootPart.")

return

end

 

-- Tween suave de teleporte

local goal = {CFrame = targetHRP.CFrame + Vector3.new(0, 5, 0)}

local tween = TweenService:Create(hrp, TweenInfo.new(0.6, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), goal)

tween:Play()

end)

 

if not success then

warn("[GOTO] Erro ao tentar teleportar:", err)

end

end

})

