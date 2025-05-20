-- GUI Script for KRNL with Toggles and Draggable GUI
local ScreenGui = Instance.new("ScreenGui")
local OpenButton = Instance.new("TextButton")
local MainFrame = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local NoclipButton = Instance.new("TextButton")
local FlyButton = Instance.new("TextButton")
local SpeedButton = Instance.new("TextButton")
local ESPButton = Instance.new("TextButton")
local Label = Instance.new("TextLabel")

-- States
local noclipOn = false
local speedOn = false
local espOn = false

-- Parent GUI
ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "CustomHackGUI"

-- Open Button (Draggable Square)
OpenButton.Size = UDim2.new(0, 30, 0, 30)
OpenButton.Position = UDim2.new(0, 10, 0, 10)
OpenButton.Text = "+"
OpenButton.Parent = ScreenGui
OpenButton.BackgroundColor3 = Color3.fromRGB(75, 75, 180)
OpenButton.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenButton.Font = Enum.Font.GothamBold
OpenButton.TextSize = 20
OpenButton.Active = true
OpenButton.Draggable = true

-- Main Frame (Draggable)
MainFrame.Size = UDim2.new(0, 280, 0, 280)
MainFrame.Position = UDim2.new(0.05, 0, 0.1, 0)
MainFrame.Visible = false
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 60)
MainFrame.BorderColor3 = Color3.fromRGB(0, 170, 255)
MainFrame.BorderSizePixel = 2
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui
UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = MainFrame

-- Toggle GUI
OpenButton.MouseButton1Click:Connect(function()
	MainFrame.Visible = not MainFrame.Visible
end)

-- Style function
local function styleButton(button)
	button.Size = UDim2.new(0, 220, 0, 40)
	button.BackgroundColor3 = Color3.fromRGB(60, 90, 150)
	button.TextColor3 = Color3.fromRGB(255, 255, 255)
	button.Font = Enum.Font.GothamBold
	button.TextSize = 18
	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = button
end

-- Noclip Toggle
NoclipButton.Position = UDim2.new(0, 30, 0, 20)
NoclipButton.Text = "Noclip: OFF"
NoclipButton.Parent = MainFrame
styleButton(NoclipButton)

local RunService = game:GetService("RunService")
local noclipConnection

NoclipButton.MouseButton1Click:Connect(function()
	noclipOn = not noclipOn
	NoclipButton.Text = "Noclip: " .. (noclipOn and "ON" or "OFF")
	
	if noclipOn then
		noclipConnection = RunService.Stepped:Connect(function()
			local char = game.Players.LocalPlayer.Character
			if char then
				for _, v in pairs(char:GetDescendants()) do
					if v:IsA("BasePart") then
						v.CanCollide = false
					end
				end
			end
		end)
	else
		if noclipConnection then
			noclipConnection:Disconnect()
		end
	end
end)

-- Fly (не твой, без выключателя)
FlyButton.Position = UDim2.new(0, 30, 0, 70)
FlyButton.Text = "Fly (V3)"
FlyButton.Parent = MainFrame
styleButton(FlyButton)

FlyButton.MouseButton1Click:Connect(function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
end)

-- SpeedHack Toggle
SpeedButton.Position = UDim2.new(0, 30, 0, 120)
SpeedButton.Text = "Speed: OFF"
SpeedButton.Parent = MainFrame
styleButton(SpeedButton)

SpeedButton.MouseButton1Click:Connect(function()
	speedOn = not speedOn
	SpeedButton.Text = "Speed: " .. (speedOn and "ON" or "OFF")
	
	local human = game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
	if human then
		human.WalkSpeed = speedOn and 100 or 16
	end
end)

-- ESP Toggle
ESPButton.Position = UDim2.new(0, 30, 0, 170)
ESPButton.Text = "ESP: OFF"
ESPButton.Parent = MainFrame
styleButton(ESPButton)

ESPButton.MouseButton1Click:Connect(function()
	espOn = not espOn
	ESPButton.Text = "ESP: " .. (espOn and "ON" or "OFF")

	for _, player in pairs(game.Players:GetPlayers()) do
		if player ~= game.Players.LocalPlayer and player.Character then
			local hrp = player.Character:FindFirstChild("HumanoidRootPart")
			if hrp then
				local existingESP = hrp:FindFirstChild("ESPBox")
				if existingESP then
					existingESP:Destroy()
				end
				if espOn then
					local box = Instance.new("BoxHandleAdornment")
					box.Name = "ESPBox"
					box.Size = Vector3.new(4, 6, 2)
					box.Color3 = Color3.fromRGB(255, 0, 0)
					box.Transparency = 0.5
					box.Adornee = hrp
					box.AlwaysOnTop = true
					box.ZIndex = 5
					box.Parent = hrp
				end
			end
		end
	end
end)

-- Label
Label.Size = UDim2.new(1, 0, 0, 30)
Label.Position = UDim2.new(0, 0, 1, -30)
Label.Text = "Made with я не знаю"
Label.Parent = MainFrame
Label.BackgroundTransparency = 1
Label.TextColor3 = Color3.fromRGB(180, 180, 180)
Label.Font = Enum.Font.GothamItalic
Label.TextSize = 14
Label.TextXAlignment = Enum.TextXAlignment.Center
