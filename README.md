local NPCS = {}
    for i, v in pairs(game:GetService("Workspace").EnemyNPCs:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
            if not table.find(NPCS, tostring(v)) then
            table.insert(NPCS, tostring(v))
        end
    end
end
local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()
local X = Material.Load({
    Title = "Anime Lost Simulator",
    Style = 1,
    SizeX = 380,
    SizeY = 380,
    Theme = "Dark",
    ColorOverrides = {
        MainFrame = Color3.fromRGB(0,9,55),
    }
})
local Y = X.New({
    Title = "Main"
})
local ii = Y.Dropdown({
    Text = "Select Mobs!",
    Callback = function(Value)
        getgenv().mobname = Value
end,
    Options = NPCS
})
local function getNPC()
    local dist, thing = math.huge
    for i, v in pairs(game:GetService("Workspace").EnemyNPCs:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") and v.Name == mobname then
            local mag = (game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,5)).magnitude
            if mag < dist then
                dist = mag
                thing = v
            end
        end
    end
    return thing
end
Y.Button(
    {
        Text = "Refresh Mobs",
        Callback = function()
            table.clear(NPCS)
            for i, v in pairs(game:GetService("Workspace").EnemyNPCs:GetDescendants()) do
                if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
                    if not table.find(NPCS, tostring(v)) then
                        table.insert(NPCS, tostring(v))
                        ii:SetOptions(NPCS)
                    end
                end
            end
        end
    }
)
Y.Toggle({
    Text = "Auto Mob",
    Callback = function(Value)
        b = Value
        while b do task.wait()
                    pcall(function()
                    
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = getNPC().UpperTorso.CFrame
            end)
        end
	end,
    Enabled = false
})
