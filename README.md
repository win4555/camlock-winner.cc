-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Variables
local Prediction = 0.15008720590445304
local Smoothness = 0.9937

local CamlockEnabled = false
local Target = nil

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "CamlockGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

local DragFrame = Instance.new("Frame")
DragFrame.Size = UDim2.new(0, 400, 0, 120) -- ขยายใหญ่ขึ้นจาก 300x90 เป็น 400x120
DragFrame.Position = UDim2.new(0, 20, 0, 100)
DragFrame.BackgroundTransparency = 1
DragFrame.BorderSizePixel = 0
DragFrame.Active = true
DragFrame.Selectable = true
DragFrame.Parent = ScreenGui

local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(1, -20, 1, -20) -- ขยายเต็ม Frame มีขอบเล็กน้อย
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.BackgroundTransparency = 1
ToggleButton.Text = "winner.cc off"
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 28 -- เพิ่มขนาดตัวอักษรให้ใหญ่ขึ้นด้วย
ToggleButton.TextWrapped = true
ToggleButton.Parent = DragFrame

-- Dragging logic (ลาก GUI โดยลากที่ข้อความ)
local dragging = false
local dragInput = nil
local dragStartPos = nil
local dragStartMousePos = nil

local function updatePosition(input)
	local delta = input.Position - dragStartMousePos
	DragFrame.Position = UDim2.new(
		dragStartPos.X.Scale,
		dragStartPos.X.Offset + delta.X,
		dragStartPos.Y.Scale,
		dragStartPos.Y.Offset + delta.Y
	)
end

ToggleButton.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragInput = input
		dragStartMousePos = input.Position
		dragStartPos = DragFrame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
				dragInput = nil
			end
		end)
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		updatePosition(input)
	end
end)

-- Get closest player to center
local function GetClosestToCenter()
	local closestPlayer = nil
	local shortestDistance = math.huge
	local screenCenter = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
			local rootPos = player.Character.HumanoidRootPart.Position
			local screenPos, onScreen = Camera:WorldToViewportPoint(rootPos)
			if onScreen then
				local dist = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
				if dist < shortestDistance then
					shortestDistance = dist
					closestPlayer = player
				end
			end
		end
	end
	return closestPlayer
end

-- Toggle Camlock
ToggleButton.MouseButton1Click:Connect(function()
	CamlockEnabled = not CamlockEnabled
	ToggleButton.Text = CamlockEnabled and "winner.cc on" or "winner.cc off"

	if CamlockEnabled then
		Target = GetClosestToCenter() -- ล็อกเป้าหมายครั้งเดียวตอนเปิด
	else
		Target = nil
	end
end)

-- Render camera lock
RunService.RenderStepped:Connect(function()
	if CamlockEnabled and Target and Target.Character and Target.Character:FindFirstChild("HumanoidRootPart") then
		local root = Target.Character.HumanoidRootPart
		local predictedPos = root.Position + root.Velocity * Prediction
		Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, predictedPos), Smoothness)
	end
end)-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Variables
local Prediction = 0.15008720590445304
local Smoothness = 0.9937

local CamlockEnabled = false
local Target = nil

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "CamlockGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

local DragFrame = Instance.new("Frame")
DragFrame.Size = UDim2.new(0, 400, 0, 120) -- ขยายใหญ่ขึ้นจาก 300x90 เป็น 400x120
DragFrame.Position = UDim2.new(0, 20, 0, 100)
DragFrame.BackgroundTransparency = 1
DragFrame.BorderSizePixel = 0
DragFrame.Active = true
DragFrame.Selectable = true
DragFrame.Parent = ScreenGui

local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(1, -20, 1, -20) -- ขยายเต็ม Frame มีขอบเล็กน้อย
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.BackgroundTransparency = 1
ToggleButton.Text = "winner.cc off"
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 28 -- เพิ่มขนาดตัวอักษรให้ใหญ่ขึ้นด้วย
ToggleButton.TextWrapped = true
ToggleButton.Parent = DragFrame

-- Dragging logic (ลาก GUI โดยลากที่ข้อความ)
local dragging = false
local dragInput = nil
local dragStartPos = nil
local dragStartMousePos = nil

local function updatePosition(input)
	local delta = input.Position - dragStartMousePos
	DragFrame.Position = UDim2.new(
		dragStartPos.X.Scale,
		dragStartPos.X.Offset + delta.X,
		dragStartPos.Y.Scale,
		dragStartPos.Y.Offset + delta.Y
	)
end

ToggleButton.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragInput = input
		dragStartMousePos = input.Position
		dragStartPos = DragFrame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
				dragInput = nil
			end
		end)
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		updatePosition(input)
	end
end)

-- Get closest player to center
local function GetClosestToCenter()
	local closestPlayer = nil
	local shortestDistance = math.huge
	local screenCenter = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
			local rootPos = player.Character.HumanoidRootPart.Position
			local screenPos, onScreen = Camera:WorldToViewportPoint(rootPos)
			if onScreen then
				local dist = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
				if dist < shortestDistance then
					shortestDistance = dist
					closestPlayer = player
				end
			end
		end
	end
	return closestPlayer
end

-- Toggle Camlock
ToggleButton.MouseButton1Click:Connect(function()
	CamlockEnabled = not CamlockEnabled
	ToggleButton.Text = CamlockEnabled and "winner.cc on" or "winner.cc off"

	if CamlockEnabled then
		Target = GetClosestToCenter() -- ล็อกเป้าหมายครั้งเดียวตอนเปิด
	else
		Target = nil
	end
end)

-- Render camera lock
RunService.RenderStepped:Connect(function()
	if CamlockEnabled and Target and Target.Character and Target.Character:FindFirstChild("HumanoidRootPart") then
		local root = Target.Character.HumanoidRootPart
		local predictedPos = root.Position + root.Velocity * Prediction
		Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, predictedPos), Smoothness)
	end
end)
