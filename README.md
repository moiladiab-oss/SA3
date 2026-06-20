-- SA3 | Purple & White Pulse Script
-- Dev: moila933 & خالد

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local RS = game:GetService("ReplicatedStorage")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SA3"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("CoreGui")

local guiOpen = true

-- ═══════════════════════════════════════
-- ✅ دائرة خارجية قابلة للسحب (تشغيل/إيقاف)
-- ═══════════════════════════════════════
local ExtBtn = Instance.new("TextButton")
ExtBtn.Size = UDim2.new(0, 48, 0, 48)
ExtBtn.Position = UDim2.new(0, 20, 0.5, -24)
ExtBtn.BackgroundColor3 = Color3.fromRGB(100, 30, 180)
ExtBtn.BorderSizePixel = 0
ExtBtn.Text = "SA3"
ExtBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ExtBtn.TextSize = 11
ExtBtn.Font = Enum.Font.GothamBold
ExtBtn.Parent = ScreenGui
Instance.new("UICorner", ExtBtn).CornerRadius = UDim.new(1, 0)
local ExtStroke = Instance.new("UIStroke", ExtBtn)
ExtStroke.Color = Color3.fromRGB(255, 255, 255)
ExtStroke.Thickness = 2

-- سحب الدائرة الخارجية
local extDragging, extDragStart, extStartPos
ExtBtn.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        extDragging = true
        extDragStart = i.Position
        extStartPos = ExtBtn.Position
    end
end)
ExtBtn.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        extDragging = false
    end
end)
UIS.InputChanged:Connect(function(i)
    if extDragging and i.UserInputType == Enum.UserInputType.MouseMovement then
        local d = i.Position - extDragStart
        ExtBtn.Position = UDim2.new(
            extStartPos.X.Scale, extStartPos.X.Offset + d.X,
            extStartPos.Y.Scale, extStartPos.Y.Offset + d.Y
        )
    end
end)

-- ═══════════════════════════════════════
-- Main Frame
-- ═══════════════════════════════════════
local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 300, 0, 100)
Main.Position = UDim2.new(0.5, -150, 0.5, -260)
Main.BackgroundColor3 = Color3.fromRGB(18, 8, 32)
Main.BorderSizePixel = 0
Main.Parent = ScreenGui
Main.ClipsDescendants = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 14)

local MainStroke = Instance.new("UIStroke", Main)
MainStroke.Color = Color3.fromRGB(255, 255, 255)
MainStroke.Thickness = 2

-- Title Bar
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 44)
TitleBar.BackgroundColor3 = Color3.fromRGB(80, 30, 150)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = Main
Instance.new("UICorner", TitleBar).CornerRadius = UDim.new(0, 14)

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -50, 1, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "SA3"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 22
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.Parent = TitleBar

local TitleStroke = Instance.new("UIStroke", TitleLabel)
TitleStroke.Color = Color3.fromRGB(255, 255, 255)
TitleStroke.Thickness = 1.5
TitleStroke.Transparency = 0.4

-- زر طي الواجهة (−/+) داخل التايتل
local CollapseBtn = Instance.new("TextButton")
CollapseBtn.Size = UDim2.new(0, 28, 0, 28)
CollapseBtn.Position = UDim2.new(1, -36, 0.5, -14)
CollapseBtn.BackgroundColor3 = Color3.fromRGB(200, 80, 255)
CollapseBtn.BorderSizePixel = 0
CollapseBtn.Text = "−"
CollapseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CollapseBtn.TextSize = 18
CollapseBtn.Font = Enum.Font.GothamBold
CollapseBtn.Parent = TitleBar
Instance.new("UICorner", CollapseBtn).CornerRadius = UDim.new(1, 0)
local CollapseStroke = Instance.new("UIStroke", CollapseBtn)
CollapseStroke.Color = Color3.fromRGB(255, 255, 255)
CollapseStroke.Thickness = 1.5

-- سحب الواجهة الرئيسية
local dragging, dragStart, startPos
TitleBar.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true; dragStart = i.Position; startPos = Main.Position
    end
end)
TitleBar.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
end)
UIS.InputChanged:Connect(function(i)
    if dragging and i.UserInputType == Enum.UserInputType.MouseMovement then
        local d = i.Position - dragStart
        Main.Position = UDim2.new(
            startPos.X.Scale, startPos.X.Offset + d.X,
            startPos.Y.Scale, startPos.Y.Offset + d.Y
        )
    end
end)

-- Body
local Body = Instance.new("Frame")
Body.Size = UDim2.new(1, 0, 1, -44)
Body.Position = UDim2.new(0, 0, 0, 44)
Body.BackgroundTransparency = 1
Body.BorderSizePixel = 0
Body.Parent = Main

-- Players label
local PLbl = Instance.new("TextLabel")
PLbl.Size = UDim2.new(1, -12, 0, 18)
PLbl.Position = UDim2.new(0, 8, 0, 8)
PLbl.BackgroundTransparency = 1
PLbl.Text = "Players in Map:"
PLbl.TextColor3 = Color3.fromRGB(200, 160, 255)
PLbl.TextSize = 11
PLbl.Font = Enum.Font.GothamBold
PLbl.TextXAlignment = Enum.TextXAlignment.Left
PLbl.Parent = Body

-- Player List
local ListFrame = Instance.new("ScrollingFrame")
ListFrame.Size = UDim2.new(1, -12, 0, 130)
ListFrame.Position = UDim2.new(0, 6, 0, 28)
ListFrame.BackgroundColor3 = Color3.fromRGB(28, 12, 50)
ListFrame.BorderSizePixel = 0
ListFrame.ScrollBarThickness = 4
ListFrame.ScrollBarImageColor3 = Color3.fromRGB(160, 80, 255)
ListFrame.Parent = Body
Instance.new("UICorner", ListFrame).CornerRadius = UDim.new(0, 8)
local ListStroke = Instance.new("UIStroke", ListFrame)
ListStroke.Color = Color3.fromRGB(200, 150, 255)
ListStroke.Thickness = 1
ListStroke.Transparency = 0.6

local ListLayout = Instance.new("UIListLayout", ListFrame)
ListLayout.Padding = UDim.new(0, 3)
Instance.new("UIPadding", ListFrame).PaddingTop = UDim.new(0, 4)

local selectedPlayer = ""

local function refreshPlayers()
    for _, c in pairs(ListFrame:GetChildren()) do
        if c:IsA("TextButton") then c:Destroy() end
    end
    for _, p in pairs(Players:GetPlayers()) do
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1, -8, 0, 26)
        btn.BackgroundColor3 = Color3.fromRGB(50, 20, 85)
        btn.BorderSizePixel = 0
        btn.Text = p.Name
        btn.TextColor3 = Color3.fromRGB(220, 190, 255)
        btn.TextSize = 12
        btn.Font = Enum.Font.Gotham
        btn.Parent = ListFrame
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
        btn.MouseButton1Click:Connect(function()
            selectedPlayer = p.Name
            for _, b in pairs(ListFrame:GetChildren()) do
                if b:IsA("TextButton") then
                    b.BackgroundColor3 = Color3.fromRGB(50, 20, 85)
                    b.TextColor3 = Color3.fromRGB(220, 190, 255)
                end
            end
            btn.BackgroundColor3 = Color3.fromRGB(120, 55, 210)
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        end)
    end
    ListFrame.CanvasSize = UDim2.new(0, 0, 0, ListLayout.AbsoluteContentSize.Y + 8)
end

refreshPlayers()
Players.PlayerAdded:Connect(refreshPlayers)
Players.PlayerRemoving:Connect(refreshPlayers)

-- Prefix label
local PrefLbl = Instance.new("TextLabel")
PrefLbl.Size = UDim2.new(0, 120, 0, 16)
PrefLbl.Position = UDim2.new(0, 8, 0, 168)
PrefLbl.BackgroundTransparency = 1
PrefLbl.Text = "الامر Prefix:"
PrefLbl.TextColor3 = Color3.fromRGB(190, 150, 255)
PrefLbl.TextSize = 11
PrefLbl.Font = Enum.Font.GothamBold
PrefLbl.TextXAlignment = Enum.TextXAlignment.Left
PrefLbl.Parent = Body

-- Prefix box
local PrefixBox = Instance.new("TextBox")
PrefixBox.Size = UDim2.new(1, -16, 0, 28)
PrefixBox.Position = UDim2.new(0, 8, 0, 186)
PrefixBox.BackgroundColor3 = Color3.fromRGB(38, 15, 70)
PrefixBox.BorderSizePixel = 0
PrefixBox.Text = ";"
PrefixBox.TextColor3 = Color3.fromRGB(255, 255, 255)
PrefixBox.TextSize = 16
PrefixBox.Font = Enum.Font.GothamBold
PrefixBox.ClearTextOnFocus = false
PrefixBox.Parent = Body
Instance.new("UICorner", PrefixBox).CornerRadius = UDim.new(0, 8)
local PrefStroke = Instance.new("UIStroke", PrefixBox)
PrefStroke.Color = Color3.fromRGB(200, 140, 255)
PrefStroke.Thickness = 1.5

local function getP()
    return PrefixBox.Text ~= "" and PrefixBox.Text or ";"
end

-- Send via DataService + HD Admin
local function sendChat(msg)
    pcall(function()
        RS.RemoteEvents.DataService:FireServer(msg)
    end)
    pcall(function()
        local args = { [1] = msg }
        RS.HDAdminHDClient.Signals.RequestCommandModification:InvokeServer(unpack(args))
    end)
end

-- Build command string
local function buildSpamMsg(cmds)
    local p = getP()
    local n = selectedPlayer
    local parts = {}
    for _, cmd in ipairs(cmds) do
        local full = p .. cmd .. (n ~= "" and (" " .. n) or "")
        table.insert(parts, full)
    end
    return table.concat(parts, " ")
end

-- Divider
local Div = Instance.new("Frame")
Div.Size = UDim2.new(1, -16, 0, 1)
Div.Position = UDim2.new(0, 8, 0, 224)
Div.BackgroundColor3 = Color3.fromRGB(160, 80, 255)
Div.BackgroundTransparency = 0.5
Div.BorderSizePixel = 0
Div.Parent = Body

local activeSpams = {}
local btnY = 234

local function addBtn(label, bgColor, cmds)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -12, 0, 40)
    btn.Position = UDim2.new(0, 6, 0, btnY)
    btn.BackgroundColor3 = bgColor
    btn.BorderSizePixel = 0
    btn.Text = label
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 13
    btn.Font = Enum.Font.GothamBold
    btn.Parent = Body
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)
    local bs = Instance.new("UIStroke", btn)
    bs.Color = Color3.fromRGB(255, 255, 255)
    bs.Thickness = 1.5
    bs.Transparency = 0.55

    local isRunning = false

    btn.MouseButton1Click:Connect(function()
        if not isRunning then
            isRunning = true
            btn.Text = "🔴 إيقاف | " .. label
            btn.BackgroundColor3 = Color3.fromRGB(180, 30, 30)
            local thread = task.spawn(function()
                while isRunning do
                    local msg = buildSpamMsg(cmds)
                    sendChat(msg)
                    task.wait(0.30)
                end
            end)
            activeSpams[btn] = thread
        else
            isRunning = false
            activeSpams[btn] = nil
            btn.Text = label
            btn.BackgroundColor3 = bgColor
        end
    end)

    btnY = btnY + 46
    return btn
end

addBtn("📋  عادي", Color3.fromRGB(100, 45, 180),
    {"jc","ice","re","nv","kill","char miri","logs","cmdbar","res"})

addBtn("🔄  re", Color3.fromRGB(75, 30, 140),
    {"re","re","re","re","re","re","re","re","re","re","re","re","re","re","re","re","re","re","re","re"})

addBtn("🌑  غامض", Color3.fromRGB(45, 15, 90),
    {"explode","nv","cmdbar","loopkill","loopwarp","char miri"})

addBtn("📜  Logs", Color3.fromRGB(110, 55, 195),
    {"logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs","logs"})

addBtn("⚡  NV", Color3.fromRGB(130, 65, 215),
    {"nv","nv","nv","nv","nv","nv","nv","nv","nv"})

-- Credits
local CredLbl = Instance.new("TextLabel")
CredLbl.Size = UDim2.new(1, -12, 0, 20)
CredLbl.Position = UDim2.new(0, 6, 0, btnY + 4)
CredLbl.BackgroundTransparency = 1
CredLbl.Text = "Dev: moila933 & خالد"
CredLbl.TextColor3 = Color3.fromRGB(160, 100, 220)
CredLbl.TextSize = 10
CredLbl.Font = Enum.Font.GothamBold
CredLbl.Parent = Body

local totalBodyHeight = btnY + 30
local fullHeight = totalBodyHeight + 44
Main.Size = UDim2.new(0, 300, 0, fullHeight)

-- ✅ زر الطي (−/+) يخفي/يظهر المحتوى فقط
local bodyCollapsed = false
CollapseBtn.MouseButton1Click:Connect(function()
    bodyCollapsed = not bodyCollapsed
    if bodyCollapsed then
        Main.Size = UDim2.new(0, 300, 0, 44)
        CollapseBtn.Text = "+"
        Body.Visible = false
    else
        Main.Size = UDim2.new(0, 300, 0, fullHeight)
        CollapseBtn.Text = "−"
        Body.Visible = true
    end
end)

-- ✅ الدائرة الخارجية تخفي/تظهر الواجهة كاملة
ExtBtn.MouseButton1Click:Connect(function()
    guiOpen = not guiOpen
    Main.Visible = guiOpen
    ExtBtn.Text = guiOpen and "SA3" or "▶"
    ExtBtn.BackgroundColor3 = guiOpen
        and Color3.fromRGB(100, 30, 180)
        or Color3.fromRGB(40, 10, 80)
end)

-- Pulse animation
RunService.Heartbeat:Connect(function()
    local t = tick()
    local p = (math.sin(t * 2.5) + 1) / 2

    MainStroke.Thickness = 1.5 + p * 1.5
    MainStroke.Transparency = 0.05 + (1 - p) * 0.45

    TitleBar.BackgroundColor3 = Color3.fromRGB(
        math.floor(55 + p * 65),
        math.floor(20 + p * 20),
        math.floor(120 + p * 70)
    )

    TitleLabel.TextColor3 = Color3.fromRGB(255, math.floor(200 + p * 55), 255)
    TitleStroke.Transparency = 0.2 + (1 - p) * 0.5

    ListStroke.Color = Color3.fromRGB(math.floor(150 + p * 80), math.floor(60 + p * 40), 255)
    ListStroke.Transparency = 0.3 + (1 - p) * 0.5

    PrefStroke.Color = Color3.fromRGB(math.floor(180 + p * 75), math.floor(120 + p * 50), 255)

    CollapseBtn.BackgroundColor3 = Color3.fromRGB(
        math.floor(160 + p * 60),
        math.floor(40 + p * 20),
        math.floor(220 + p * 35)
    )

    -- نبضة الدائرة الخارجية (فقط الحواف، بدون تغيير لون الخلفية)
    ExtStroke.Thickness = 1.5 + p * 2
end)
