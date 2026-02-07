local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "เรียลคิง",
   LoadingTitle = "เรียลที่หล่อๆอ่ะ",
   LoadingSubtitle = "by Jadi59_z",
   ConfigurationSaving = {
      Enabled = false,
      FolderName = nil,
      FileName = "Example Hub"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink",
      RememberJoins = true
   },
   KeySystem = false -- <<< ปิดคีย์ตรงนี้
})

local MainTab = Window:CreateTab("🏠 หน้าหลัก", nil) -- Title, Image
local MainSection = MainTab:CreateSection("Main")

Rayfield:Notify({
   Title = "You executed the script",
   Content = "Very cool gui",
   Duration = 5,
   Image = 13047715178,
   Actions = { -- Notification Buttons
      Ignore = {
         Name = "Okay!",
         Callback = function()
         print("The user tapped Okay!")
      end
   },
},
})

local Button = MainTab:CreateButton({
   Name = "Infinite Jump Toggle",
   Callback = function()
       --Toggles the infinite jump between on or off on every script run
_G.infinjump = not _G.infinjump

if _G.infinJumpStarted == nil then
	--Ensures this only runs once to save resources
	_G.infinJumpStarted = true
	
	--Notifies readiness
	game.StarterGui:SetCore("SendNotification", {Title="Youtube Hub"; Text="Infinite Jump Activated!"; Duration=5;})

	--The actual infinite jump
	local plr = game:GetService('Players').LocalPlayer
	local m = plr:GetMouse()
	m.KeyDown:connect(function(k)
		if _G.infinjump then
			if k:byte() == 32 then
			humanoid = game:GetService'Players'.LocalPlayer.Character:FindFirstChildOfClass('Humanoid')
			humanoid:ChangeState('Jumping')
			wait()
			humanoid:ChangeState('Seated')
			end
		end
	end)
end
   end,
})

local Slider = MainTab:CreateSlider({
   Name = "วิ่งไว",
   Range = {1, 350},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "sliderws", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
   end,
})

local Slider = MainTab:CreateSlider({
   Name = "โดดสูง",
   Range = {1, 350},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "sliderjp", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = (Value)
   end,
})

local Dropdown = MainTab:CreateDropdown({
   Name = "Select Area",
   Options = {"Starter World","Pirate Island","Pineapple Paradise"},
   CurrentOption = {"Starter World"},
   MultipleOptions = false,
   Flag = "dropdownarea", -- A flag is the identifier for the configuration file, make sure every element has a different flag if you're using configuration saving to ensure no overlaps
   Callback = function(Option)
        print(Option)
   end,
})

local Input = MainTab:CreateInput({
   Name = "Walkspeed",
   PlaceholderText = "1-500",
   RemoveTextAfterFocusLost = true,
   Callback = function(Text)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Text)
   end,
})

local TPTab = Window:CreateTab("🏝 มองทะลุ", nil)

local ESPToggle1 = TPTab:CreateToggle({
   Name = "Team Check",
   CurrentValue = false,
   Flag = "esp_team",
   Callback = function(Value)
        TeamCheckEnabled = Value
   end,
})

local ESPToggle2 = TPTab:CreateToggle({
   Name = "Wall Check",
   CurrentValue = false,
   Flag = "esp_wall",
   Callback = function(Value)
        WallCheckEnabled = Value
   end,
})

local Button1 = TPTab:CreateButton({
   Name = "Starter Island",
   Callback = function()
        --Teleport1
   end,
})

local Button2 = TPTab:CreateButton({
   Name = "Pirate Island",
   Callback = function()
        --Teleport2
   end,
})

local Button3 = TPTab:CreateButton({
   Name = "Pineapple Paradise",
   Callback = function()
        --Teleport3
   end,
})

local TPTab = Window:CreateTab("🎲ล็อคหัว", nil) -- Title, Image
-- ========== SYSTEM CORE ==========

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local TeamCheckEnabled = false
local WallCheckEnabled = false

local function IsEnemy(p1, p2)
	if not p1.Team or not p2.Team then
		return true
	end
	return p1.Team ~= p2.Team
end

local function HasLineOfSight(fromPart, toPart)
	local params = RaycastParams.new()
	params.FilterType = Enum.RaycastFilterType.Blacklist
	params.FilterDescendantsInstances = {fromPart.Parent}

	local dir = toPart.Position - fromPart.Position
	local result = workspace:Raycast(fromPart.Position, dir, params)

	if result then
		return result.Instance:IsDescendantOf(toPart.Parent)
	end
	return false
end

RunService.RenderStepped:Connect(function()
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Head") then
			
			if TeamCheckEnabled and not IsEnemy(LocalPlayer, plr) then
				if plr.Character:FindFirstChild("Highlight") then
					plr.Character.Highlight:Destroy()
				end
				continue
			end
			
			if WallCheckEnabled then
				if not HasLineOfSight(
					LocalPlayer.Character.Head,
					plr.Character.Head
				) then
					if plr.Character:FindFirstChild("Highlight") then
						plr.Character.Highlight:Destroy()
					end
					continue
				end
			end
			
			if not plr.Character:FindFirstChild("Highlight") then
				local h = Instance.new("Highlight")
				h.FillColor = Color3.fromRGB(255,0,0)
				h.OutlineColor = Color3.fromRGB(255,255,255)
				h.Parent = plr.Character
			end
		end
	end
end)
