local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

local Config = {
    Aimbot = {Enabled = true, FOV = 180},
    Visuals = {ESP = {Enabled = true}},
    Movement = {Bhop = true},
    Misc = {MenuKey = "RightShift", PanicKey = "End"}
}

local FatalilityUI = {}
FatalilityUI.Themes = {
    Dark = {
        Primary = Color3.fromRGB(15, 15, 15),
        Secondary = Color3.fromRGB(25, 25, 25),
        Tertiary = Color3.fromRGB(35, 35, 35),
        Accent = Color3.fromRGB(220, 20, 60),
        Text = Color3.fromRGB(240, 240, 240)
    }
}

FatalilityUI.CurrentTheme = "Dark"
FatalilityUI.Connections = {}

function FatalilityUI:CreateAnimation(object, properties, duration)
    local tweenInfo = TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(object, tweenInfo, properties)
    return tween
end

function FatalilityUI:GetConfigValue(path)
    local parts = string.split(path, ".")
    local current = Config
    for _, part in ipairs(parts) do
        current = current[part]
        if not current then return nil end
    end
    return current
end

function FatalilityUI:SetConfigValue(path, value)
    local parts = string.split(path, ".")
    local current = Config
    for i = 1, #parts - 1 do
        current = current[parts[i]]
        if not current then return end
    end
    current[parts[#parts]] = value
end

function FatalilityUI:CreateWindow(title)
    self.MainScreenGui = Instance.new("ScreenGui")
    self.MainScreenGui.Name = "FatalilityUI"
    self.MainScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    self.MainScreenGui.Parent = CoreGui

    self.MainFrame = Instance.new("Frame")
    self.MainFrame.Size = UDim2.new(0, 600, 0, 400)
    self.MainFrame.Position = UDim2.new(0.5, -300, 0.5, -200)
    self.MainFrame.BackgroundColor3 = self.Themes.Dark.Primary
    self.MainFrame.BorderSizePixel = 0
    self.MainFrame.Parent = self.MainScreenGui

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = self.MainFrame

    local titleBar = Instance.new("Frame")
    titleBar.Size = UDim2.new(1, 0, 0, 40)
    titleBar.BackgroundColor3 = self.Themes.Dark.Secondary
    titleBar.BorderSizePixel = 0
    titleBar.Parent = self.MainFrame

    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 8)
    titleCorner.Parent = titleBar

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(0.5, 0, 1, 0)
    titleLabel.Position = UDim2.new(0, 15, 0, 0)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = "FATALILITY MENU"
    titleLabel.TextColor3 = self.Themes.Dark.Text
    titleLabel.TextSize = 16
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = titleBar

    local closeButton = Instance.new("TextButton")
    closeButton.Size = UDim2.new(0, 30, 0, 30)
    closeButton.Position = UDim2.new(1, -35, 0.5, -15)
    closeButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
    closeButton.Text = "X"
    closeButton.TextColor3 = Color3.new(1, 1, 1)
    closeButton.TextSize = 14
    closeButton.Font = Enum.Font.GothamBold
    closeButton.Parent = titleBar

    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 4)
    closeCorner.Parent = closeButton

    self.TabContainer = Instance.new("Frame")
    self.TabContainer.Size = UDim2.new(0, 150, 1, -40)
    self.TabContainer.Position = UDim2.new(0, 0, 0, 40)
    self.TabContainer.BackgroundTransparency = 1
    self.TabContainer.Parent = self.MainFrame

    local tabLayout = Instance.new("UIListLayout")
    tabLayout.Padding = UDim.new(0, 5)
    tabLayout.Parent = self.TabContainer

    self.ContentContainer = Instance.new("Frame")
    self.ContentContainer.Size = UDim2.new(1, -160, 1, -60)
    self.ContentContainer.Position = UDim2.new(0, 160, 0, 40)
    self.ContentContainer.BackgroundTransparency = 1
    self.ContentContainer.Parent = self.MainFrame

    closeButton.MouseButton1Click:Connect(function()
        self.MainScreenGui:Destroy()
    end)

    local dragging = false
    local dragInput, dragStart, startPos

    titleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = self.MainFrame.Position
        end
    end)

    titleBar.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            self.MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)

    table.insert(self.Connections, UserInputService.InputBegan:Connect(function(input)
        if input.KeyCode == Enum.KeyCode[Config.Misc.MenuKey] then
            self.MainScreenGui.Enabled = not self.MainScreenGui.Enabled
        end
    end))

    return self
end

function FatalilityUI:CreateTab(name)
    local tabButton = Instance.new("TextButton")
    tabButton.Name = name .. "Tab"
    tabButton.Size = UDim2.new(0.9, 0, 0, 40)
    tabButton.Position = UDim2.new(0.05, 0, 0, #self.TabContainer:GetChildren() * 45)
    tabButton.BackgroundColor3 = self.Themes.Dark.Secondary
    tabButton.Text = name
    tabButton.TextColor3 = self.Themes.Dark.Text
    tabButton.TextSize = 14
    tabButton.Font = Enum.Font.GothamSemibold
    tabButton.Parent = self.TabContainer

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = tabButton

    local tabContent = Instance.new("ScrollingFrame")
    tabContent.Name = name .. "Content"
    tabContent.Size = UDim2.new(1, 0, 1, 0)
    tabContent.BackgroundTransparency = 1
    tabContent.BorderSizePixel = 0
    tabContent.ScrollBarThickness = 3
    tabContent.ScrollBarImageColor3 = self.Themes.Dark.Accent
    tabContent.Visible = false
    tabContent.Parent = self.ContentContainer

    local contentLayout = Instance.new("UIListLayout")
    contentLayout.Padding = UDim.new(0, 10)
    contentLayout.Parent = tabContent

    tabButton.MouseButton1Click:Connect(function()
        for _, child in ipairs(self.ContentContainer:GetChildren()) do
            if child:IsA("ScrollingFrame") then
                child.Visible = false
            end
        end
        tabContent.Visible = true
        
        for _, btn in ipairs(self.TabContainer:GetChildren()) do
            if btn:IsA("TextButton") then
                btn.BackgroundColor3 = self.Themes.Dark.Secondary
            end
        end
        tabButton.BackgroundColor3 = self.Themes.Dark.Accent
    end)

    if #self.TabContainer:GetChildren() == 1 then
        tabButton.BackgroundColor3 = self.Themes.Dark.Accent
        tabContent.Visible = true
    end

    return tabContent
end

function FatalilityUI:CreateSection(title, parent)
    local section = Instance.new("Frame")
    section.Name = title .. "Section"
    section.Size = UDim2.new(1, -20, 0, 0)
    section.Position = UDim2.new(0, 10, 0, 0)
    section.BackgroundColor3 = self.Themes.Dark.Secondary
    section.BorderSizePixel = 0
    section.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = section

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 30)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = " " .. title
    titleLabel.TextColor3 = self.Themes.Dark.Text
    titleLabel.TextSize = 14
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = section

    local content = Instance.new("Frame")
    content.Name = "Content"
    content.Size = UDim2.new(1, 0, 0, 0)
    content.Position = UDim2.new(0, 0, 0, 30)
    content.BackgroundTransparency = 1
    content.Parent = section

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 5)
    layout.Parent = content

    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        content.Size = UDim2.new(1, 0, 0, layout.AbsoluteContentSize.Y)
        section.Size = UDim2.new(1, -20, 0, layout.AbsoluteContentSize.Y + 35)
    end)

    return content
end

function FatalilityUI:CreateToggle(configPath, displayName, parent)
    local toggleFrame = Instance.new("Frame")
    toggleFrame.Name = displayName .. "Toggle"
    toggleFrame.Size = UDim2.new(1, 0, 0, 30)
    toggleFrame.BackgroundTransparency = 1
    toggleFrame.Parent = parent

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0.7, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = displayName
    label.TextColor3 = self.Themes.Dark.Text
    label.TextSize = 14
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = toggleFrame

    local toggleButton = Instance.new("TextButton")
    toggleButton.Name = "Toggle"
    toggleButton.Size = UDim2.new(0, 50, 0, 25)
    toggleButton.Position = UDim2.new(0.8, 0, 0.5, -12.5)
    toggleButton.BackgroundColor3 = self.Themes.Dark.Tertiary
    toggleButton.Text = ""
    toggleButton.Parent = toggleFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = toggleButton

    local dot = Instance.new("Frame")
    dot.Name = "Dot"
    dot.Size = UDim2.new(0, 21, 0, 21)
    dot.Position = UDim2.new(0, 2, 0.5, -10.5)
    dot.BackgroundColor3 = Color3.new(1, 1, 1)
    dot.Parent = toggleButton

    local dotCorner = Instance.new("UICorner")
    dotCorner.CornerRadius = UDim.new(1, 0)
    dotCorner.Parent = dot

    local currentValue = self:GetConfigValue(configPath)

    if currentValue then
        dot.Position = UDim2.new(1, -23, 0.5, -10.5)
        toggleButton.BackgroundColor3 = self.Themes.Dark.Accent
    end

    toggleButton.MouseButton1Click:Connect(function()
        local newValue = not self:GetConfigValue(configPath)
        self:SetConfigValue(configPath, newValue)
        
        if newValue then
            self:CreateAnimation(dot, {Position = UDim2.new(1, -23, 0.5, -10.5)}, 0.2):Play()
            self:CreateAnimation(toggleButton, {BackgroundColor3 = self.Themes.Dark.Accent}, 0.2):Play()
        else
            self:CreateAnimation(dot, {Position = UDim2.new(0, 2, 0.5, -10.5)}, 0.2):Play()
            self:CreateAnimation(toggleButton, {BackgroundColor3 = self.Themes.Dark.Tertiary}, 0.2):Play()
        end
    end)

    return toggleFrame
end

function FatalilityUI:CreateSlider(configPath, displayName, min, max, defaultValue, parent)
    local sliderFrame = Instance.new("Frame")
    sliderFrame.Name = displayName .. "Slider"
    sliderFrame.Size = UDim2.new(1, 0, 0, 50)
    sliderFrame.BackgroundTransparency = 1
    sliderFrame.Parent = parent

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 20)
    label.BackgroundTransparency = 1
    label.Text = displayName .. ": " .. tostring(defaultValue)
    label.TextColor3 = self.Themes.Dark.Text
    label.TextSize = 14
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = sliderFrame

    local track = Instance.new("Frame")
    track.Size = UDim2.new(1, 0, 0, 5)
    track.Position = UDim2.new(0, 0, 0, 30)
    track.BackgroundColor3 = self.Themes.Dark.Tertiary
    track.Parent = sliderFrame

    local trackCorner = Instance.new("UICorner")
    trackCorner.CornerRadius = UDim.new(1, 0)
    trackCorner.Parent = track

    local fill = Instance.new("Frame")
    fill.Size = UDim2.new((defaultValue - min) / (max - min), 0, 1, 0)
    fill.BackgroundColor3 = self.Themes.Dark.Accent
    fill.Parent = track

    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(1, 0)
    fillCorner.Parent = fill

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 20, 0, 20)
    button.Position = UDim2.new((defaultValue - min) / (max - min), -10, 0.5, -10)
    button.BackgroundColor3 = Color3.new(1, 1, 1)
    button.Text = ""
    button.Parent = track

    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(1, 0)
    buttonCorner.Parent = button

    local dragging = false

    local function updateSlider(input)
        local relativeX = (input.Position.X - track.AbsolutePosition.X) / track.AbsoluteSize.X
        local value = math.floor(min + (max - min) * math.clamp(relativeX, 0, 1))
        
        self:SetConfigValue(configPath, value)
        label.Text = displayName .. ": " .. value
        
        local fillSize = (value - min) / (max - min)
        fill.Size = UDim2.new(fillSize, 0, 1, 0)
        button.Position = UDim2.new(fillSize, -10, 0.5, -10)
    end

    button.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
        end
    end)

    button.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            updateSlider(input)
        end
    end)

    track.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            updateSlider(input)
        end
    end)

    return sliderFrame
end

function FatalilityUI:Init()
    self:CreateWindow("FATALILITY")
    
    local aimbotTab = self:CreateTab("AIMBOT")
    local visualsTab = self:CreateTab("VISUALS")
    local movementTab = self:CreateTab("MOVEMENT")
    
    local aimbotMain = self:CreateSection("AIMBOT SETTINGS", aimbotTab)
    self:CreateToggle("Aimbot.Enabled", "Enable Aimbot", aimbotMain)
    self:CreateSlider("Aimbot.FOV", "Aimbot FOV", 1, 360, 180, aimbotMain)
    
    local visualsMain = self:CreateSection("ESP SETTINGS", visualsTab)
    self:CreateToggle("Visuals.ESP.Enabled", "Enable ESP", visualsMain)
    
    local movementMain = self:CreateSection("MOVEMENT SETTINGS", movementTab)
    self:CreateToggle("Movement.Bhop", "Bunny Hop", movementMain)
    
    warn("Fatalility Menu Loaded! Press " .. Config.Misc.MenuKey .. " to toggle menu")
end

FatalilityUI:Init()
