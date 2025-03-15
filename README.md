local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local GuiService = game:GetService("GuiService")

local localPlayer = Players.LocalPlayer
local camera = workspace.CurrentCamera
local ENABLED = false

local SMOOTHNESS = 1
local BUTTON_SIZE = UDim2.new(0.15, 0, 0.15, 0)
local BUTTON_CORNER_RADIUS = 0.5

local screenGui = Instance.new("ScreenGui")
screenGui.Parent = localPlayer:WaitForChild("PlayerGui")

local toggleButton = Instance.new("TextButton")
toggleButton.Size = BUTTON_SIZE
toggleButton.Position = UDim2.new(0.75, 0, 0.8, 0)
toggleButton.Text = "🔒"
toggleButton.TextScaled = true
toggleButton.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
toggleButton.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(BUTTON_CORNER_RADIUS, 0)
corner.Parent = toggleButton

local isDragging = false
local dragOffset = Vector2.new(0, 0)

toggleButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        isDragging = true
        local touchPosition = input.Position
        local buttonPosition = toggleButton.AbsolutePosition
        dragOffset = Vector2.new(buttonPosition.X - touchPosition.X, buttonPosition.Y - touchPosition.Y)
    end
end)

toggleButton.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch then
        isDragging = false
    end
end)

UserInputService.TouchMoved:Connect(function(input)
    if isDragging then
        local touchPosition = input.Position
        local newX = touchPosition.X + dragOffset.X
        local newY = touchPosition.Y + dragOffset.Y

end)

toggleButton.Activated:Connect(function()
    ENABLED = not ENABLED
    toggleButton.Text = ENABLED and "🔫" or "🔒"
    toggleButton.BackgroundColor3 = ENABLED and Color3.fromRGB(60, 255, 60) or Color3.fromRGB(255, 60, 60)
end)

function getClosestHead()
    local closestHead = nil
    local closestDistance = math.huge -- Infinite range
    local localChar = localPlayer.Character
    
end

RunService.RenderStepped:Connect(function()
    if ENABLED then
        local targetHead = getClosestHead()
        if targetHead then
            local currentCF = camera.CFrame
            local targetCF = CFrame.new(currentCF.Position, targetHead.Position)
            camera.CFrame = currentCF:Lerp(targetCF, SMOOTHNESS)
        end
    end
end)
