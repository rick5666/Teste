local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = playerGui
ScreenGui.Name = "ArcaneProHubModernUI"

-- Main Frame
local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0, 340, 0, 200)
Frame.Position = UDim2.new(0.5, -170, 0.5, -100)
Frame.BackgroundColor3 = Color3.fromRGB(26, 28, 34)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
local frameCorner = Instance.new("UICorner", Frame)
frameCorner.CornerRadius = UDim.new(0, 14)

-- Top Bar
local TopBar = Instance.new("Frame", Frame)
TopBar.Size = UDim2.new(1, 0, 0, 36)
TopBar.BackgroundColor3 = Color3.fromRGB(34, 37, 44)
TopBar.BorderSizePixel = 0
TopBar.ZIndex = 2
local topCorner = Instance.new("UICorner", TopBar)
topCorner.CornerRadius = UDim.new(0, 14)

local Title = Instance.new("TextLabel", TopBar)
Title.Text = "ArcanePro Hub"
Title.Size = UDim2.new(0, 170, 1, 0)
Title.Position = UDim2.new(0, 14, 0, 0)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(200, 200, 200)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18
Title.ZIndex = 3

-- Logic for toggles (moved up for close button logic)
local runningAutoDodge = false
local runningAutoQTE = false

-- Close Button
local CloseBtn = Instance.new("TextButton", TopBar)
CloseBtn.Text = "✕"
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(1, -34, 0, 4)
CloseBtn.BackgroundColor3 = Color3.fromRGB(192, 57, 43)
CloseBtn.TextColor3 = Color3.fromRGB(255,255,255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 16
CloseBtn.ZIndex = 3
local closeCorner = Instance.new("UICorner", CloseBtn)
closeCorner.CornerRadius = UDim.new(0,10)
CloseBtn.MouseButton1Click:Connect(function()
    -- DESATIVA AS FUNÇÕES AO FECHAR
    runningAutoDodge = false
    runningAutoQTE = false
    ScreenGui:Destroy()
end)

-- Tabs (left menu) - ONLY "Main"
local Tabs = Instance.new("Frame", Frame)
Tabs.Size = UDim2.new(0, 62, 1, -36)
Tabs.Position = UDim2.new(0, 0, 0, 36)
Tabs.BackgroundTransparency = 1

local function createTab(text, posY)
    local tabBtn = Instance.new("TextButton", Tabs)
    tabBtn.Text = text
    tabBtn.Size = UDim2.new(0, 54, 0, 28)
    tabBtn.Position = UDim2.new(0, 4, 0, posY)
    tabBtn.BackgroundColor3 = Color3.fromRGB(34, 37, 44)
    tabBtn.TextColor3 = Color3.fromRGB(200,200,200)
    tabBtn.Font = Enum.Font.GothamSemibold
    tabBtn.TextSize = 13
    tabBtn.ZIndex = 3
    local tabCorner = Instance.new("UICorner", tabBtn)
    tabCorner.CornerRadius = UDim.new(0,8)
    return tabBtn
end
local mainTab = createTab("Main", 8)
mainTab.BackgroundColor3 = Color3.fromRGB(46,204,113)

-- Pages (right)
local Pages = Instance.new("Frame", Frame)
Pages.Size = UDim2.new(1, -70, 1, -48)
Pages.Position = UDim2.new(0, 66, 0, 44)
Pages.BackgroundTransparency = 1
Pages.ZIndex = 3

local MainPage = Instance.new("Frame", Pages)
MainPage.Size = UDim2.new(1, 0, 1, 0)
MainPage.Position = UDim2.new(0,0,0,0)
MainPage.BackgroundTransparency = 1
MainPage.Visible = true

-- Modern Toggles for features
local function createToggle(parent, text, posY, callback)
    local tf = Instance.new("Frame", parent)
    tf.Size = UDim2.new(1, -24, 0, 36)
    tf.Position = UDim2.new(0, 12, 0, posY)
    tf.BackgroundTransparency = 1

    local label = Instance.new("TextLabel", tf)
    label.Text = text
    label.Size = UDim2.new(0.65, 0, 1, 0)
    label.Position = UDim2.new(0, 0, 0, 0)
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.fromRGB(170,170,180)
    label.Font = Enum.Font.Gotham
    label.TextSize = 15

    local toggle = Instance.new("TextButton", tf)
    toggle.Size = UDim2.new(0, 38, 0, 22)
    toggle.Position = UDim2.new(1, -44, 0.5, -11)
    toggle.BackgroundColor3 = Color3.fromRGB(36, 39, 46)
    toggle.Text = ""
    local toggleCorner = Instance.new("UICorner", toggle)
    toggleCorner.CornerRadius = UDim.new(1,0)
    local ball = Instance.new("Frame", toggle)
    ball.Size = UDim2.new(0, 14, 0, 14)
    ball.Position = UDim2.new(0, 3, 0.5, -7)
    ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
    ball.BorderSizePixel = 0
    local ballCorner = Instance.new("UICorner", ball)
    ballCorner.CornerRadius = UDim.new(1,0)

    local state = false
    toggle.MouseButton1Click:Connect(function()
        state = not state
        if state then
            toggle.BackgroundColor3 = Color3.fromRGB(46, 204, 113)
            ball.Position = UDim2.new(1, -17, 0.5, -7)
            ball.BackgroundColor3 = Color3.fromRGB(255,255,255)
        else
            toggle.BackgroundColor3 = Color3.fromRGB(36, 39, 46)
            ball.Position = UDim2.new(0, 3, 0.5, -7)
            ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
        end
        if callback then callback(state) end
    end)
    return tf
end

createToggle(MainPage, "Auto Dodge", 12, function(state)
    runningAutoDodge = state
end)
createToggle(MainPage, "Auto QTE", 54, function(state)
    runningAutoQTE = state
end)

-- Minimized Button (draggable)
local minimized = false
local minimizedBtn
local Minimize = Instance.new("TextButton", TopBar)
Minimize.Text = "—"
Minimize.Size = UDim2.new(0, 28, 0, 28)
Minimize.Position = UDim2.new(1, -68, 0, 4)
Minimize.BackgroundColor3 = Color3.fromRGB(55, 66, 88)
Minimize.TextColor3 = Color3.fromRGB(200, 200, 200)
Minimize.Font = Enum.Font.GothamBold
Minimize.TextSize = 18
Minimize.ZIndex = 3
local MinCorner = Instance.new("UICorner", Minimize)
MinCorner.CornerRadius = UDim.new(0,10)

Minimize.MouseButton1Click:Connect(function()
    minimized = true
    Frame.Visible = false
    minimizedBtn = Instance.new("TextButton", ScreenGui)
    minimizedBtn.Text = "ArcanePro Hub"
    minimizedBtn.Size = UDim2.new(0, 120, 0, 38)
    minimizedBtn.Position = UDim2.new(0, 22, 0, 22)
    minimizedBtn.BackgroundColor3 = Color3.fromRGB(26, 28, 34)
    minimizedBtn.TextColor3 = Color3.fromRGB(200,200,200)
    minimizedBtn.Font = Enum.Font.GothamBold
    minimizedBtn.TextSize = 16
    minimizedBtn.ZIndex = 10
    local minCorner = Instance.new("UICorner", minimizedBtn)
    minCorner.CornerRadius = UDim.new(0, 10)
    -- Fade-in Animation
    minimizedBtn.BackgroundTransparency = 1
    minimizedBtn.TextTransparency = 1
    for i = 1, 10 do
        minimizedBtn.BackgroundTransparency = 1 - i*0.1
        minimizedBtn.TextTransparency = 1 - i*0.1
        task.wait(0.01)
    end

    -- Draggable logic for minimizedBtn
    local dragging, dragInput, mousePos, btnPos
    local UserInputService = game:GetService("UserInputService")
    minimizedBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            mousePos = input.Position
            btnPos = minimizedBtn.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    minimizedBtn.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - mousePos
            minimizedBtn.Position = UDim2.new(btnPos.X.Scale, btnPos.X.Offset + delta.X, btnPos.Y.Scale, btnPos.Y.Offset + delta.Y)
        end
    end)

    minimizedBtn.MouseButton1Click:Connect(function()
        if not dragging then
            minimized = false
            Frame.Visible = true
            minimizedBtn:Destroy()
        end
    end)
end)

-- Coroutine Auto Dodge
coroutine.wrap(function()
    while true do
        if runningAutoDodge then
            game:GetService("ReplicatedStorage").Remotes.Information.RemoteFunction:FireServer({true, true}, "DodgeMinigame")
        end
        task.wait(0.001)
    end
end)()

-- Coroutine Auto QTE
coroutine.wrap(function()
    while true do
        if runningAutoQTE then
            local args = {true, "SwordQTE"}
            game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("Information"):WaitForChild("RemoteFunction"):FireServer(unpack(args))
        end
        task.wait(1.5)
    end
end)()

-- Responsive resize for mobile
playerGui:GetPropertyChangedSignal("AbsoluteSize"):Connect(function()
    local guiSize = playerGui.AbsoluteSize
    if guiSize.X < 500 then
        Frame.Size = UDim2.new(0, math.min(320, guiSize.X-20), 0, 180)
        Frame.Position = UDim2.new(0.5, -Frame.Size.X.Offset/2, 0.5, -Frame.Size.Y.Offset/2)
    else
        Frame.Size = UDim2.new(0, 340, 0, 200)
        Frame.Position = UDim2.new(0.5, -170, 0.5, -100)
    end
end)
