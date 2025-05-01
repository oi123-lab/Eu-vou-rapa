local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "MATRIX HUB TESTE VERSION", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})

--[[
Name = <string> - The name of the UI.
HidePremium = <bool> - Whether or not the user details shows Premium status or not.
SaveConfig = <bool> - Toggles the config saving in the UI.
ConfigFolder = <string> - The name of the folder where the configs are saved.
IntroEnabled = <bool> - Whether or not to show the intro animation.
IntroText = <string> - Text to show in the intro animation.
IntroIcon = <string> - URL to the image you want to use in the intro animation.
Icon = <string> - URL to the image you want displayed on the window.
CloseCallback = <function> - Function to execute when the window is closed.
]]

local Tab = Window:MakeTab({
	Name = "TESTE DE ILUMINAR O MAPA",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

--[[
Name = <string> - The name of the tab.
Icon = <string> - The icon of the tab.
PremiumOnly = <bool> - Makes the tab accessible to Sirus Premium users only.


Tab:AddButton({
	Name = "Iluminar mapa",
	Callback = function()
 -- SERVIÇOS
local cleartoolremote = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clea1rTool1s") 
local picktoolremote = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l")

-- CONTROLE DE PARADA
local stopProcess = false

-- FUNÇÃO DE RESET
local function resetCharacter() 
    local player = game.Players.LocalPlayer 
    if player.Character then 
        player.Character:BreakJoints()
    end 
end

-- FUNÇÃO PRINCIPAL
local function createPaintRollerTornado(height, radius, rotationSpeed, verticalSpacing)
    stopProcess = false
    local nametools = "PaintRoller Tornado"
    local oldcframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame    

    if game.Players.LocalPlayer.Character.Humanoid.Sit then       
        task.wait()
        game.Players.LocalPlayer.Character.Humanoid.Sit = false   
    end    

    task.wait()
    cleartoolremote:FireServer("ClearAllTools")

    if workspace:FindFirstChild("Camera") then       
        workspace.Camera:Destroy()   
    end    

    for _ = 1, 2 do
        task.wait()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(999999999, -490, 999999999)   
    end    

    task.wait()
    game.Players.LocalPlayer.Character.HumanoidRootPart.Anchored = true
    task.wait()

    for i = 1, height do
        if stopProcess then
            game.Players.LocalPlayer.Character.HumanoidRootPart.Anchored = false
            resetCharacter()
            return
        end

        picktoolremote:InvokeServer("PickingTools", "Iphone")
        local tool = game.Players.LocalPlayer.Backpack:WaitForChild("Iphone")
        tool.Parent = game.Players.LocalPlayer.Character

        if stopProcess then
            game.Players.LocalPlayer.Character.HumanoidRootPart.Anchored = false
            resetCharacter()
            return
        end

        task.wait()
        tool.Handle.Name = "H⁥a⁥n⁥d⁥l⁥e"
        tool.Parent = game.Players.LocalPlayer.Backpack
        tool.Parent = game.Players.LocalPlayer.Character

        repeat
            if workspace:FindFirstChild("Camera") then
                workspace.Camera:Destroy()
            end
            task.wait()
        until not game.Players.LocalPlayer.Character:FindFirstChild("Iphone")
    end

    game.Players.LocalPlayer.Character.HumanoidRootPart.Anchored = false
    repeat task.wait() until not game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    repeat task.wait() until game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

    -- POSICIONAMENTO ALEATÓRIO
    local tools = game.Players.LocalPlayer.Backpack:GetChildren()
    for i = 1, #tools do
        if tools[i]:IsA("Tool") then
            local x = math.random(-radius, radius)
            local y = math.random(10, 200)
            local z = math.random(-radius, radius)

            tools[i].Grip = CFrame.new(Vector3.new(x, y, z))
            tools[i].Name = nametools
        end
    end

    -- VOLTA PRA POSIÇÃO ORIGINAL
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = oldcframe
    wait(0.5)

    -- APLICA AS FERRAMENTAS
    for _, tool in ipairs(game.Players.LocalPlayer.Backpack:GetChildren()) do       
        if tool:IsA("Tool") and tool.Name == nametools then
            tool.Parent = game.Players.LocalPlayer.Character
        end
    end
end

-- CHAMADA DIRETA (sem GUI)
createPaintRollerTornado(400, 1000, 20, 3)
      		print("button pressed")
  	end    
})

--[[
Name = <string> - The name of the button.
Callback = <function> - The function of the button.
]]
