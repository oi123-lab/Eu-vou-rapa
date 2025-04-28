	-- Criando a GUI principal 

local ScreenGui = Instance.new("ScreenGui") 

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui") 

ScreenGui.Enabled = false -- Inicialmente o Frame estarÃƒÆ’Ã‚Â¡ invisÃƒÆ’Ã‚Â­vel 

 

-- Criando o frame 

local Frame = Instance.new("Frame") 

Frame.Parent = ScreenGui 

Frame.Size = UDim2.new(0, 400, 0, 200) 

Frame.Position = UDim2.new(0.5, -200, 0.5, -100) 

Frame.AnchorPoint = Vector2.new(0.5, 0.5) 

Frame.BackgroundColor3 = Color3.new(0, 0, 0) 

Frame.BackgroundTransparency = 0.3 

Frame.BorderSizePixel = 0 

 

-- Adicionando cantos arredondados ao Frame 

local UICornerFrame = Instance.new("UICorner") 

UICornerFrame.Parent = Frame 

UICornerFrame.CornerRadius = UDim.new(0, 15) 

 

-- Criando o texto explicativo 

local Label = Instance.new("TextLabel") 

Label.Parent = Frame 

Label.Size = UDim2.new(1, 0, 0.4, 0) 

Label.Position = UDim2.new(0, 0, 0, 0) 

Label.Text = "Put in the ID of a sound to play" 

Label.TextScaled = true 

Label.TextColor3 = Color3.new(1, 1, 1) 

Label.BackgroundTransparency = 1 

Label.Font = Enum.Font.SciFi 

 

-- Criando a TextBox 

local TextBox = Instance.new("TextBox") 

TextBox.Parent = Frame 

TextBox.Size = UDim2.new(0.8, 0, 0.2, 0) 

TextBox.Position = UDim2.new(0.1, 0, 0.5, 0) 

TextBox.PlaceholderText = "Enter ID" 

TextBox.Text = "" 

TextBox.TextScaled = true 

TextBox.TextColor3 = Color3.new(1, 1, 1) 

TextBox.BackgroundColor3 = Color3.new(0, 0, 0) 

TextBox.Font = Enum.Font.SciFi 

TextBox.BorderSizePixel = 0 

 

-- Adicionando cantos arredondados ÃƒÆ’ TextBox 

local UICornerTextBox = Instance.new("UICorner") 

UICornerTextBox.Parent = TextBox 

UICornerTextBox.CornerRadius = UDim.new(0, 8) 

 

-- Criando o botÃƒÆ’Ã‚Â£o 

local Button = Instance.new("TextButton") 

Button.Parent = Frame 

Button.Size = UDim2.new(0.4, 0, 0.2, 0) 

Button.Position = UDim2.new(0.3, 0, 0.75, 0) 

Button.Text = "Play" 

Button.TextScaled = true 

Button.TextColor3 = Color3.new(1, 1, 1) 

Button.BackgroundColor3 = Color3.new(0, 0, 0) 

Button.Font = Enum.Font.SciFi 

Button.BorderSizePixel = 0 

 

-- Adicionando cantos arredondados ao botÃƒÆ’Ã‚Â£o 

local UICornerButton = Instance.new("UICorner") 

UICornerButton.Parent = Button 

UICornerButton.CornerRadius = UDim.new(0, 8) 

 

-- VariÃƒÆ’Ã‚Â¡vel para armazenar o som em reproduÃƒÆ’Ã‚Â§ÃƒÆ’Ã‚Â£o 

local currentSound 

 

-- FunÃƒÆ’Ã‚Â§ÃƒÆ’Ã‚Â£o para tocar o som localmente 

local function playSoundLocally(audioID) 

local character = game.Players.LocalPlayer.Character 

if character and character:FindFirstChild("Head") then 

-- Verifica se jÃƒÆ’Ã‚Â¡ hÃƒÆ’Ã‚Â¡ um som sendo tocado e o interrompe 

if currentSound then 

currentSound:Stop() 

currentSound:Destroy() 

currentSound = nil 

end 

 

-- Cria e toca um novo som 

currentSound = Instance.new("Sound") 

currentSound.Parent = character.Head 

currentSound.SoundId = "rbxassetid://" .. audioID 

currentSound.Volume = 1 

currentSound:Play() 

 

-- Parar o som apÃƒÆ’Ã‚Â³s 3 segundos 

task.delay(3, function() 

if currentSound then 

currentSound:Stop() 

currentSound:Destroy() 

currentSound = nil 

end 

end) 

 

currentSound.Ended:Connect(function() 

if currentSound then 

currentSound:Destroy() 

currentSound = nil 

end 

end) 

else 

warn("NÃƒÆ’Ã‚Â£o foi possÃƒÆ’Ã‚Â­vel encontrar a cabeÃƒÆ’Ã‚Â§a do personagem para tocar o som!") 

end 

end 

 

-- FunÃƒÆ’Ã‚Â§ÃƒÆ’Ã‚Â£o para enviar o ÃƒÆ’Ã‚Â¡udio com o ID 

Button.MouseButton1Click:Connect(function() 

local audioID = tonumber(TextBox.Text) 

if audioID then 

-- Envia o som para o servidor 

local args = { 

[1] = game:GetService("Players").LocalPlayer.Character.Taser.Handle, 

[2] = audioID, 

[3] = 0.95 

} 

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Gu1nSound1s"):FireServer(unpack(args)) 

 

-- Toca o som localmente 

playSoundLocally(audioID) 

else 

warn("Por favor, insira um ID vÃƒÆ’Ã‚Â¡lido!") 

end

