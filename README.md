local Players = game:GetService("Players")
local player = Players.LocalPlayer
local RunService = game:GetService("RunService")
local HttpService = game:GetService("HttpService")

-- Create GUI
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "CoordDisplay"

local coordLabel = Instance.new("TextLabel", screenGui)
coordLabel.Size = UDim2.new(0, 300, 0, 50)
coordLabel.Position = UDim2.new(0, 10, 0, 10)
coordLabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
coordLabel.TextColor3 = Color3.new(1, 1, 1)
coordLabel.TextScaled = true
coordLabel.Text = "Coordinates: Loading..."

local copyButton = Instance.new("TextButton", screenGui)
copyButton.Size = UDim2.new(0, 100, 0, 40)
copyButton.Position = UDim2.new(0, 10, 0, 70)
copyButton.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
copyButton.TextColor3 = Color3.new(1, 1, 1)
copyButton.TextScaled = true
copyButton.Text = "Copy"

-- Update position
RunService.RenderStepped:Connect(function()
	local char = player.Character
	if char and char:FindFirstChild("HumanoidRootPart") then
		local pos = char.HumanoidRootPart.Position
		coordLabel.Text = string.format("Coordinates: %.2f, %.2f, %.2f", pos.X, pos.Y, pos.Z)
	end
end)

-- Copy to clipboard (only works in Studio via `setclipboard`)
copyButton.MouseButton1Click:Connect(function()
	local char = player.Character
	if char and char:FindFirstChild("HumanoidRootPart") then
		local pos = char.HumanoidRootPart.Position
		local coordText = string.format("%.2f, %.2f, %.2f", pos.X, pos.Y, pos.Z)
		if setclipboard then
			setclipboard(coordText)
		else
			warn("Clipboard function not available in this environment.")
		end
	end
end)
