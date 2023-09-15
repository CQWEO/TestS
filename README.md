local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer or Players:GetPropertyChangedSignal("LocalPlayer"):Wait()
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
local Mouse = LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local CurrentTarget = nil
 
local Connections = {
	CharacterAdded = {}
}
 
table.insert(Connections.CharacterAdded, LocalPlayer.CharacterAdded:Connect(function(Char)
	Character = Char
	Humanoid = Char:WaitForChild("Humanoid")
	HumanoidRootPart = Char:WaitForChild("HumanoidRootPart")
end))

local Esp = {}; do
    Instance.new("ScreenGui",game.CoreGui).Name = "Kaoru"
    local ChamsFolder = Instance.new("Folder")
    ChamsFolder.Name = "ChamsFolder"
    for _,v in next, game.CoreGui:GetChildren() do
        if v:IsA'ScreenGui' and v.Name == 'Kaoru' then
            ChamsFolder.Parent = v
        end
    end
    Players.PlayerRemoving:Connect(function(plr)
        if ChamsFolder:FindFirstChild(plr.Name) then
            ChamsFolder[plr.Name]:Destroy()
        end
    end)
    local Loops = {RenderStepped = {}, Heartbeat = {}, Stepped = {}}
    function Esp:BindToRenderStepped(id, callback)
        if not Loops.RenderStepped[id] then
            Loops.RenderStepped[id] = RunService.RenderStepped:Connect(callback)
        end
    end
    function Esp:UnbindFromRenderStepped(id)
        if Loops.RenderStepped[id] then
            Loops.RenderStepped[id]:Disconnect()
            Loops.RenderStepped[id] = nil
        end
    end
    function Esp:TeamCheck(Player, Toggle)
        if Toggle then
            return Player.Team ~= LocalPlayer.Team
        else
            return true
        end
    end
    function Esp:Update()
        for _, Player in next, Players:GetChildren() do
            if ChamsFolder:FindFirstChild(Player.Name) then
                Chams = ChamsFolder[Player.Name]
                Chams.Enabled = false
                Chams.FillColor = Color3.fromRGB(255, 255, 255)
                Chams.OutlineColor = Color3.fromHSV(tick()%5/5,1,1)
            end
            if Player ~= LocalPlayer and Player.Character then
                if ChamsFolder:FindFirstChild(Player.Name) == nil then
                    local chamfolder = Instance.new("Highlight")
                    chamfolder.Name = Player.Name
                    chamfolder.Parent = ChamsFolder
                    Chams = chamfolder
                end
                Chams.Enabled = true
                Chams.Adornee = Player.Character
                Chams.OutlineTransparency = 0
                Chams.DepthMode = Enum.HighlightDepthMode[(true and "AlwaysOnTop" or "Occluded")]
                Chams.FillTransparency = 1
            end
        end
    end
    function Esp:Toggle(boolean)
        if boolean then
            self:BindToRenderStepped("Esp", function()
                self:Update()
            end)
        else
            self:UnbindFromRenderStepped("Esp")
            ChamsFolder:ClearAllChildren()
        end
    end
local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Rayfield/main/source'))()
local Window = Rayfield:CreateWindow({
	Name = "FPS LOADING",
	LoadingTitle = "FPS FOR GAME",
	LoadingSubtitle = "BY RECHEDMCVN",
	ConfigurationSaving = {
		Enabled = true,
		FolderName = "FPS",
		FileName = "FPS"
}}
)
Main:CreateSection("Esp")
 
Main:CreateToggle({
    Name = "Esp",
    CurrentValue = false,
    Callback = function(EspToggle)
        Esp:Toggle(EspToggle)
    end,
})
