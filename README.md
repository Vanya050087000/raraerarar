local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local Lighting = game:GetService("Lighting")
local HttpService = game:GetService("HttpService")

local LocalPlayer = Players.LocalPlayer

local Fatalility = {}
Fatalility.__index = Fatalility

Fatalility.Themes = {
    Default = {
        Primary = Color3.fromRGB(20, 20, 20),
        Secondary = Color3.fromRGB(28, 28, 28),
        Tertiary = Color3.fromRGB(35, 35, 35),
        Accent = Color3.fromRGB(180, 40, 50),
        AccentSecondary = Color3.fromRGB(160, 30, 40),
        TextPrimary = Color3.fromRGB(240, 240, 240),
        TextSecondary = Color3.fromRGB(180, 180, 180),
        TextTertiary = Color3.fromRGB(120, 120, 120),
        Success = Color3.fromRGB(85, 170, 85),
        Warning = Color3.fromRGB(220, 160, 40),
        Error = Color3.fromRGB(220, 60, 60)
    },
    Dark = {
        Primary = Color3.fromRGB(15, 15, 15),
        Secondary = Color3.fromRGB(22, 22, 22),
        Tertiary = Color3.fromRGB(30, 30, 30),
        Accent = Color3.fromRGB(200, 50, 60),
        AccentSecondary = Color3.fromRGB(170, 40, 50),
        TextPrimary = Color3.fromRGB(230, 230, 230),
        TextSecondary = Color3.fromRGB(170, 170, 170),
        TextTertiary = Color3.fromRGB(110, 110, 110),
        Success = Color3.fromRGB(75, 160, 75),
        Warning = Color3.fromRGB(210, 150, 30),
        Error = Color3.fromRGB(210, 50, 50)
    },
    Blue = {
        Primary = Color3.fromRGB(20, 25, 35),
        Secondary = Color3.fromRGB(28, 35, 48),
        Tertiary = Color3.fromRGB(35, 45, 60),
        Accent = Color3.fromRGB(65, 140, 230),
        AccentSecondary = Color3.fromRGB(50, 120, 210),
        TextPrimary = Color3.fromRGB(240, 245, 255),
        TextSecondary = Color3.fromRGB(180, 190, 210),
        TextTertiary = Color3.fromRGB(120, 135, 160),
        Success = Color3.fromRGB(85, 200, 120),
        Warning = Color3.fromRGB(240, 180, 50),
        Error = Color3.fromRGB(240, 80, 80)
    },
    Purple = {
        Primary = Color3.fromRGB(25, 20, 35),
        Secondary = Color3.fromRGB(35, 28, 48),
        Tertiary = Color3.fromRGB(45, 35, 65),
        Accent = Color3.fromRGB(160, 80, 220),
        AccentSecondary = Color3.fromRGB(140, 60, 200),
        TextPrimary = Color3.fromRGB(245, 240, 255),
        TextSecondary = Color3.fromRGB(190, 180, 210),
        TextTertiary = Color3.fromRGB(135, 120, 160),
        Success = Color3.fromRGB(120, 200, 120),
        Warning = Color3.fromRGB(220, 160, 60),
        Error = Color3.fromRGB(220, 70, 90)
    }
}

Fatalility.Config = {
    CurrentTheme = "Default",
    MenuKey = Enum.KeyCode.RightShift,
    PanicKey = Enum.KeyCode.End,
    Watermark = true,
    Notifications = true,
    Rage = {
        Enabled = true,
        Auto = {
            Enabled = true,
            Slide = true,
            OnlyWhenLethal = false,
            Autoscope = true,
            FallbackMode = false,
            PreferFallback = false,
            ForceFallback = false,
            InAirTarget = true,
            IgnoreLimbsMoving = true,
            Silent = false,
            DoubleTap = false,
            Teleport = false,
            None = false
        },
        AntiAim = {
            Enabled = false,
            Pitch = "Default",
            Yaw = "Forward",
            YawOffset = 0,
            JitterRange = 0,
            BodyYaw = "Static",
            Freestanding = false,
            FakeLag = false,
            FakeDuck = false
        },
        Weapons = {
            Global = {
                Hitchance = 50,
                Mindamage = 20,
                HeadScale = 30,
                BodyScale = 60,
                Hitboxes = {"Head"},
                Multipoint = {"Head"},
                Autostop = "Slide"
            },
            Scout = {
                Hitchance = 83,
                Mindamage = 100,
                HeadScale = 30,
                BodyScale = 90,
                Hitboxes = {"Head", "Chest", "Arm"},
                Multipoint = {"Head", "Chest", "Arm"},
                Autostop = "Slide"
            },
            SWIP = {
                Hitchance = 80,
                Mindamage = 100,
                HeadScale = 30,
                BodyScale = 72,
                Hitboxes = {"Chest", "Arm", "Stomach"},
                Multipoint = {"Chest", "Arm", "Stomach"},
                Autostop = "Full"
            },
            HeavyPistols = {
                Hitchance = 81,
                Mindamage = 30,
                HeadScale = 30,
                BodyScale = 80,
                Hitboxes = {"Head", "Chest", "Stomach"},
                Multipoint = {"Head", "Chest", "Stomach"},
                Autostop = "Full"
            },
            Pistols = {
                Hitchance = 33,
                Mindamage = 20,
                HeadScale = 30,
                BodyScale = 67,
                Hitboxes = {"Head", "Chest", "Stomach"},
                Multipoint = {},
                Autostop = "None"
            },
            Other = {
                Hitchance = 34,
                Mindamage = 0,
                HeadScale = 0,
                BodyScale = 70,
                Hitboxes = {"Head", "Neck", "Chest"},
                Multipoint = {},
                Autostop = "None"
            }
        }
    },
    Visuals = {
        Enabled = true,
        ESP = {
            Players = true,
            Boxes = true,
            Names = true,
            Health = true,
            Weapon = true,
            Distance = true,
            Tracers = true,
            Skeletons = true,
            Chams = true,
            Glow = false,
            Radar = false
        },
        World = {
            Brightness = 1,
            Ambient = Color3.fromRGB(30, 30, 30),
            Fog = false,
            Skybox = "Default",
            NoFlash = false,
            NoSmoke = false,
            NoPostProcessing = false
        },
        Effects = {
            HitMarkers = true,
            KillEffect = true,
            BulletTracers = true,
            Impacts = true,
            Sound = true,
            CustomHitSound = "",
            CustomKillSound = ""
        }
    },
    Misc = {
        Enabled = true,
        Movement = {
            Bhop = true,
            AutoStrafe = true,
            Speed = 1.0,
            NoClip = false,
            Fly = false
        },
        Settings = {
            ClanTag = "",
            NameStealer = false,
            AutoAccept = false,
            RevealRanks = false,
            AutoBuy = false
        }
    },
    Inventory = {
        Enabled = true,
        SkinChanger = false,
        ProfileChanger = false,
        MusicKits = false
    },
    Legit = {
        Enabled = false,
        Aimbot = {
            Enabled = false,
            Keybind = Enum.KeyCode.MouseButton2,
            FOV = 3,
            Smoothness = 10,
            RecoilControl = true
        },
        Triggerbot = {
            Enabled = false,
            Delay = 50,
            Hitchance = 100
        }
    }
}

function Fatalility:CreateAnimation(object, properties, duration, easingStyle, easingDirection)
    local tweenInfo = TweenInfo.new(
        duration or 0.3,
        easingStyle or Enum.EasingStyle.Quad,
        easingDirection or Enum.EasingDirection.Out,
        0,
        false,
        0
    )
    local tween = TweenService:Create(object, tweenInfo, properties)
    return tween
end

function Fatalility:ApplyGlowEffect(frame, intensity, color)
    local glow = Instance.new("ImageLabel")
    glow.Name = "GlowEffect"
    glow.Size = UDim2.new(1, 20, 1, 20)
    glow.Position = UDim2.new(0, -10, 0, -10)
    glow.BackgroundTransparency = 1
    glow.Image = "rbxassetid://8992230675"
    glow.ImageColor3 = color or self.CurrentTheme.Accent
    glow.ImageTransparency = 1 - (intensity or 0.3)
    glow.ScaleType = Enum.ScaleType.Slice
    glow.SliceCenter = Rect.new(20, 20, 280, 280)
    glow.ZIndex = -1
    glow.Parent = frame
    return glow
end

function Fatalility:CreateRippleEffect(button)
    local absoluteSize = button.AbsoluteSize
    local absolutePosition = button.AbsolutePosition
    
    button.MouseButton1Click:Connect(function()
        local ripple = Instance.new("Frame")
        ripple.Name = "Ripple"
        ripple.Size = UDim2.new(0, 0, 0, 0)
        ripple.Position = UDim2.new(0, 0, 0, 0)
        ripple.BackgroundColor3 = Color3.new(1, 1, 1)
        ripple.BackgroundTransparency = 0.8
        ripple.ZIndex = 10
        ripple.Parent = button
        
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(1, 0)
        corner.Parent = ripple
        
        local mouse = game:GetService("Players").LocalPlayer:GetMouse()
        local pos = Vector2.new(mouse.X - absolutePosition.X, mouse.Y - absolutePosition.Y)
        
        ripple.Position = UDim2.new(0, pos.X, 0, pos.Y)
        
        local tweenIn = self:CreateAnimation(ripple, {
            Size = UDim2.new(0, absoluteSize.X * 2, 0, absoluteSize.X * 2),
            Position = UDim2.new(0, pos.X - absoluteSize.X, 0, pos.Y - absoluteSize.X),
            BackgroundTransparency = 1
        }, 0.6, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
        
        tweenIn:Play()
        tweenIn.Completed:Connect(function()
            ripple:Destroy()
        end)
    end)
end

function Fatalility:CreateNotification(title, message, duration, type)
    if not Fatalility.Config.Notifications then return end
    
    local theme = self.CurrentTheme
    local notification = Instance.new("Frame")
    notification.Name = "Notification"
    notification.Size = UDim2.new(0, 320, 0, 70)
    notification.Position = UDim2.new(1, 350, 0.1, 0)
    notification.BackgroundColor3 = theme.Secondary
    notification.BorderSizePixel = 0
    notification.ZIndex = 100
    notification.Parent = self.ScreenGui
    
    local glow = self:ApplyGlowEffect(notification, 0.4)
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = notification
    
    local stroke = Instance.new("UIStroke")
    stroke.Color = theme.Accent
    stroke.Thickness = 1
    stroke.Parent = notification
    
    local icon = Instance.new("ImageLabel")
    icon.Size = UDim2.new(0, 24, 0, 24)
    icon.Position = UDim2.new(0, 15, 0, 15)
    icon.BackgroundTransparency = 1
    icon.ZIndex = 101
    icon.Parent = notification
    
    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, -50, 0, 20)
    titleLabel.Position = UDim2.new(0, 50, 0, 15)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = title
    titleLabel.TextColor3 = theme.TextPrimary
    titleLabel.TextSize = 14
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.ZIndex = 101
    titleLabel.Parent = notification
    
    local messageLabel = Instance.new("TextLabel")
    messageLabel.Size = UDim2.new(1, -50, 1, -40)
    messageLabel.Position = UDim2.new(0, 50, 0, 35)
    messageLabel.BackgroundTransparency = 1
    messageLabel.Text = message
    messageLabel.TextColor3 = theme.TextSecondary
    messageLabel.TextSize = 12
    messageLabel.Font = Enum.Font.Gotham
    messageLabel.TextXAlignment = Enum.TextXAlignment.Left
    messageLabel.TextYAlignment = Enum.TextYAlignment.Top
    messageLabel.TextWrapped = true
    messageLabel.ZIndex = 101
    messageLabel.Parent = notification
    
    local progressBar = Instance.new("Frame")
    progressBar.Size = UDim2.new(1, 0, 0, 3)
    progressBar.Position = UDim2.new(0, 0, 1, -3)
    progressBar.BackgroundColor3 = theme.Accent
    progressBar.BorderSizePixel = 0
    progressBar.ZIndex = 101
    progressBar.Parent = notification
    
    local progressCorner = Instance.new("UICorner")
    progressCorner.CornerRadius = UDim.new(0, 2)
    progressCorner.Parent = progressBar
    
    local iconMap = {
        Info = "rbxassetid://3926305904",
        Success = "rbxassetid://3926305904",
        Warning = "rbxassetid://3926305904",
        Error = "rbxassetid://3926305904"
    }
    
    local colorMap = {
        Info = theme.Accent,
        Success = theme.Success,
        Warning = theme.Warning,
        Error = theme.Error
    }
    
    icon.Image = iconMap[type or "Info"]
    icon.ImageColor3 = colorMap[type or "Info"]
    
    local slideIn = self:CreateAnimation(notification, {
        Position = UDim2.new(1, -350, 0.1, 0)
    }, 0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out)
    
    slideIn:Play()
    
    local progressTween = self:CreateAnimation(progressBar, {
        Size = UDim2.new(0, 0, 0, 3)
    }, duration or 5, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)
    
    progressTween:Play()
    
    delay(duration or 5, function()
        local slideOut = self:CreateAnimation(notification, {
            Position = UDim2.new(1, 350, 0.1, 0)
        }, 0.5, Enum.EasingStyle.Back, Enum.EasingDirection.In)
        
        slideOut:Play()
        slideOut.Completed:Connect(function()
            notification:Destroy()
        end)
    end)
end

function Fatalility:CreateWatermark()
    if not Fatalility.Config.Watermark then return end
    
    local watermark = Instance.new("Frame")
    watermark.Name = "Watermark"
    watermark.Size = UDim2.new(0, 220, 0, 30)
    watermark.Position = UDim2.new(0, 10, 0, 10)
    watermark.BackgroundColor3 = self.CurrentTheme.Secondary
    watermark.BorderSizePixel = 0
    watermark.Parent = self.ScreenGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = watermark
    
    local stroke = Instance.new("UIStroke")
    stroke.Color = self.CurrentTheme.Accent
    stroke.Thickness = 1
    stroke.Parent = watermark
    
    local glow = self:ApplyGlowEffect(watermark, 0.3)
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -20, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = "FATALITY.CS2 | PREMIUM | ALPHA"
    label.TextColor3 = self.CurrentTheme.TextPrimary
    label.TextSize = 12
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = watermark
    
    local fpsLabel = Instance.new("TextLabel")
    fpsLabel.Size = UDim2.new(0, 60, 0, 30)
    fpsLabel.Position = UDim2.new(1, 70, 0, 10)
    fpsLabel.BackgroundColor3 = self.CurrentTheme.Secondary
    fpsLabel.BorderSizePixel = 0
    fpsLabel.Text = "FPS: 0"
    fpsLabel.TextColor3 = self.CurrentTheme.TextPrimary
    fpsLabel.TextSize = 12
    fpsLabel.Font = Enum.Font.GothamBold
    fpsLabel.Parent = self.ScreenGui
    
    local fpsCorner = Instance.new("UICorner")
    fpsCorner.CornerRadius = UDim.new(0, 6)
    fpsCorner.Parent = fpsLabel
    
    local fpsGlow = self:ApplyGlowEffect(fpsLabel, 0.3)
    
    local frameCount = 0
    local lastUpdate = tick()
    
    RunService.RenderStepped:Connect(function()
        frameCount = frameCount + 1
        
        if tick() - lastUpdate >= 1 then
            local fps = math.floor(frameCount / (tick() - lastUpdate))
            fpsLabel.Text = "FPS: " .. fps
            frameCount = 0
            lastUpdate = tick()
        end
    end)
end

function Fatalility:CreateWindow()
    self.ScreenGui = Instance.new("ScreenGui")
    self.ScreenGui.Name = "FatalilityCS2"
    self.ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    self.ScreenGui.ResetOnSpawn = false
    self.ScreenGui.Parent = CoreGui

    self.CurrentTheme = Fatalility.Themes[Fatalility.Config.CurrentTheme]
    
    self.MainFrame = Instance.new("Frame")
    self.MainFrame.Size = UDim2.new(0, 750, 0, 550)
    self.MainFrame.Position = UDim2.new(0.5, -375, 0.5, -275)
    self.MainFrame.BackgroundColor3 = self.CurrentTheme.Primary
    self.MainFrame.BorderSizePixel = 0
    self.MainFrame.ClipsDescendants = true
    self.MainFrame.Parent = self.ScreenGui

    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 12)
    mainCorner.Parent = self.MainFrame

    local mainStroke = Instance.new("UIStroke")
    mainStroke.Color = self.CurrentTheme.Secondary
    mainStroke.Thickness = 2
    mainStroke.Parent = self.MainFrame

    local mainGlow = self:ApplyGlowEffect(self.MainFrame, 0.4)

    self:CreateHeader()
    self:CreateTabSystem()
    self:CreateWatermark()
    self:SetupInput()
    self:CreateRageTab()
    
    self:CreateNotification("FATALITY.CS2", "Premium cheat loaded successfully!", 5, "Success")
end

function Fatalility:CreateHeader()
    local header = Instance.new("Frame")
    header.Size = UDim2.new(1, 0, 0, 40)
    header.BackgroundColor3 = self.CurrentTheme.Secondary
    header.BorderSizePixel = 0
    header.Parent = self.MainFrame

    local headerCorner = Instance.new("UICorner")
    headerCorner.CornerRadius = UDim.new(0, 12)
    headerCorner.Parent = header

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(0, 120, 1, 0)
    title.Position = UDim2.new(0, 20, 0, 0)
    title.BackgroundTransparency = 1
    title.Text = "FATALITY"
    title.TextColor3 = self.CurrentTheme.Accent
    title.TextSize = 20
    title.Font = Enum.Font.GothamBlack
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Parent = header

    local closeButton = Instance.new("TextButton")
    closeButton.Size = UDim2.new(0, 30, 0, 30)
    closeButton.Position = UDim2.new(1, -35, 0.5, -15)
    closeButton.BackgroundColor3 = self.CurrentTheme.Error
    closeButton.Text = "×"
    closeButton.TextColor3 = Color3.new(1, 1, 1)
    closeButton.TextSize = 18
    closeButton.Font = Enum.Font.GothamBold
    closeButton.Parent = header

    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 6)
    closeCorner.Parent = closeButton

    local minimizeButton = Instance.new("TextButton")
    minimizeButton.Size = UDim2.new(0, 30, 0, 30)
    minimizeButton.Position = UDim2.new(1, -70, 0.5, -15)
    minimizeButton.BackgroundColor3 = self.CurrentTheme.Warning
    minimizeButton.Text = "−"
    minimizeButton.TextColor3 = Color3.new(1, 1, 1)
    minimizeButton.TextSize = 18
    minimizeButton.Font = Enum.Font.GothamBold
    minimizeButton.Parent = header

    local minimizeCorner = Instance.new("UICorner")
    minimizeCorner.CornerRadius = UDim.new(0, 6)
    minimizeCorner.Parent = minimizeButton

    self:CreateRippleEffect(closeButton)
    self:CreateRippleEffect(minimizeButton)

    closeButton.MouseButton1Click:Connect(function()
        self.ScreenGui:Destroy()
    end)

    local minimized = false
    minimizeButton.MouseButton1Click:Connect(function()
        minimized = not minimized
        if minimized then
            self:CreateAnimation(self.MainFrame, {
                Size = UDim2.new(0, 750, 0, 40)
            }, 0.3):Play()
        else
            self:CreateAnimation(self.MainFrame, {
                Size = UDim2.new(0, 750, 0, 550)
            }, 0.3):Play()
        end
    end)
end

function Fatalility:CreateTabSystem()
    self.TabContainer = Instance.new("Frame")
    self.TabContainer.Size = UDim2.new(1, 0, 0, 30)
    self.TabContainer.Position = UDim2.new(0, 0, 0, 40)
    self.TabContainer.BackgroundColor3 = self.CurrentTheme.Secondary
    self.TabContainer.BorderSizePixel = 0
    self.TabContainer.Parent = self.MainFrame

    self.TabButtons = {}
    local tabNames = {"RAGE", "VISUALS", "MISC", "INVENTORY", "LEGIT"}
    
    for i, tabName in ipairs(tabNames) do
        local tabButton = Instance.new("TextButton")
        tabButton.Size = UDim2.new(0, 120, 1, 0)
        tabButton.Position = UDim2.new(0, (i-1)*120, 0, 0)
        tabButton.BackgroundColor3 = self.CurrentTheme.Secondary
        tabButton.BorderSizePixel = 0
        tabButton.Text = tabName
        tabButton.TextColor3 = self.CurrentTheme.TextSecondary
        tabButton.TextSize = 12
        tabButton.Font = Enum.Font.GothamBold
        tabButton.Parent = self.TabContainer
        
        local hoverEffect = Instance.new("Frame")
        hoverEffect.Size = UDim2.new(1, 0, 0, 2)
        hoverEffect.Position = UDim2.new(0, 0, 1, -2)
        hoverEffect.BackgroundColor3 = self.CurrentTheme.Accent
        hoverEffect.BorderSizePixel = 0
        hoverEffect.Visible = false
        hoverEffect.Parent = tabButton
        
        tabButton.MouseEnter:Connect(function()
            if not self.TabButtons[tabName] then
                self:CreateAnimation(tabButton, {
                    TextColor3 = self.CurrentTheme.TextPrimary
                }, 0.2):Play()
            end
        end)
        
        tabButton.MouseLeave:Connect(function()
            if not self.TabButtons[tabName] then
                self:CreateAnimation(tabButton, {
                    TextColor3 = self.CurrentTheme.TextSecondary
                }, 0.2):Play()
            end
        end)
        
        tabButton.MouseButton1Click:Connect(function()
            self:SwitchTab(tabName)
        end)
        
        self.TabButtons[tabName] = {Button = tabButton, Hover = hoverEffect}
        
        if i == 1 then
            self:SwitchTab(tabName)
        end
    end

    self.ContentContainer = Instance.new("Frame")
    self.ContentContainer.Size = UDim2.new(1, 0, 1, -70)
    self.ContentContainer.Position = UDim2.new(0, 0, 0, 70)
    self.ContentContainer.BackgroundTransparency = 1
    self.ContentContainer.ClipsDescendants = true
    self.ContentContainer.Parent = self.MainFrame
end

function Fatalility:SwitchTab(tabName)
    for name, tabData in pairs(self.TabButtons) do
        if name == tabName then
            tabData.Button.TextColor3 = self.CurrentTheme.Accent
            tabData.Hover.Visible = true
            self:CreateAnimation(tabData.Hover, {
                Size = UDim2.new(1, 0, 0, 2)
            }, 0.3):Play()
        else
            tabData.Button.TextColor3 = self.CurrentTheme.TextSecondary
            tabData.Hover.Visible = false
        end
    end
    
    for _, child in ipairs(self.ContentContainer:GetChildren()) do
        child.Visible = false
    end
    
    if self.ContentContainer:FindFirstChild(tabName .. "Content") then
        self.ContentContainer[tabName .. "Content"].Visible = true
    end
end

function Fatalility:CreateRageTab()
    local rageContent = Instance.new("Frame")
    rageContent.Name = "RAGEContent"
    rageContent.Size = UDim2.new(1, 0, 1, 0)
    rageContent.BackgroundTransparency = 1
    rageContent.Visible = true
    rageContent.Parent = self.ContentContainer

    local leftColumn = Instance.new("Frame")
    leftColumn.Size = UDim2.new(0.35, 0, 1, 0)
    leftColumn.BackgroundTransparency = 1
    leftColumn.Parent = rageContent

    local rightColumn = Instance.new("ScrollingFrame")
    rightColumn.Size = UDim2.new(0.65, 0, 1, 0)
    rightColumn.Position = UDim2.new(0.35, 0, 0, 0)
    rightColumn.BackgroundTransparency = 1
    rightColumn.BorderSizePixel = 0
    rightColumn.ScrollBarThickness = 3
    rightColumn.ScrollBarImageColor3 = self.CurrentTheme.Accent
    rightColumn.CanvasSize = UDim2.new(0, 0, 2.5, 0)
    rightColumn.Parent = rageContent

    self:CreateAimbotSection(leftColumn)
    self:CreateAntiAimSection(leftColumn)
    self:CreateWeaponSections(rightColumn)
    self:CreateConfigSection(rightColumn)
end

function Fatalility:CreateAimbotSection(parent)
    local section = self:CreateSection("AIMBOT", 200, parent, 5)
    
    local autoSection = self:CreateSubSection("Auto", 25, section)
    
    local autoOptions = {
        "Slide", "Only when lethal", "Autoscope", "Fallback mode", 
        "Prefer fallback", "Force fallback", "In air target, Lei!", 
        "Ignore limbs when target moving", "Silent", "Double top", "Teleport", "NONE"
    }

    for i, option in ipairs(autoOptions) do
        local toggle = self:CreateToggle("Rage.Auto." .. option, option, autoSection, 5 + (i-1)*22, true)
    end
end

function Fatalility:CreateAntiAimSection(parent)
    local section = self:CreateSection("ANTI-AIM", 150, parent, 210)
    
    local antiAimOptions = {
        "Enabled", "Pitch", "Yaw", "Yaw offset", "Jitter range", 
        "Body yaw", "Freestanding", "Fake lag", "Fake duck"
    }

    for i, option in ipairs(antiAimOptions) do
        local yPos = 25 + (i-1)*25
        if option == "Enabled" then
            self:CreateToggle("Rage.AntiAim." .. option, option, section, yPos, true)
        else
            self:CreateSetting(option, "", section, yPos)
        end
    end
end

function Fatalility:CreateWeaponSections(parent)
    local weapons = {
        {Name = "Scout", Data = Fatalility.Config.Rage.Weapons.Scout},
        {Name = "SWIP", Data = Fatalility.Config.Rage.Weapons.SWIP},
        {Name = "Heavy pistols", Data = Fatalility.Config.Rage.Weapons.HeavyPistols},
        {Name = "Pistols", Data = Fatalility.Config.Rage.Weapons.Pistols},
        {Name = "Other", Data = Fatalility.Config.Rage.Weapons.Other}
    }

    for i, weapon in ipairs(weapons) do
        local sectionHeight = 120
        if weapon.Name == "Scout" then sectionHeight = 190
        elseif weapon.Name == "SWIP" or weapon.Name == "Heavy pistols" then sectionHeight = 170
        end
        
        local section = self:CreateSection(weapon.Name, sectionHeight, parent, (i-1)*125)
        
        self:CreateWeaponSetting("Hitchance", weapon.Data.Hitchance .. " %", section, 25)
        self:CreateWeaponSetting("Mindamage", weapon.Data.Mindamage .. " dmg", section, 45)
        self:CreateWeaponSetting("Head scale", "+" .. weapon.Data.HeadScale .. " %", section, 65)
        self:CreateWeaponSetting("Body scale", "+" .. weapon.Data.BodyScale .. " %", section, 85)

        if weapon.Name == "Scout" then
            self:CreateWeaponSetting("Hibboxes", "Head, Chest, Arm", section, 105)
            self:CreateWeaponSetting("Multipoint", "Head, Chest, Arm", section, 125)
            self:CreateWeaponSetting("Autostop", "Slide", section, 145)
            self:CreateWeaponSetting("", "NONE", section, 165)
        elseif weapon.Name == "SWIP" then
            self:CreateWeaponSetting("Hibboxes", "Chest, Arm, Sion", section, 105)
            self:CreateWeaponSetting("Multipoint", "Chest, Arm, Sion", section, 125)
            self:CreateWeaponSetting("Autostop", "", section, 145)
        elseif weapon.Name == "Heavy pistols" then
            self:CreateWeaponSetting("Hibboxes", "Head, Chest, Sion", section, 105)
            self:CreateWeaponSetting("Multipoint", "Head, Chest, Sion", section, 125)
            self:CreateWeaponSetting("Autostop", "", section, 145)
        elseif weapon.Name == "Pistols" then
            self:CreateWeaponSetting("Hibboxes", "Head, Chest, Sion", section, 105)
        elseif weapon.Name == "Other" then
            self:CreateWeaponSetting("Hibboxes", "Head, Neck, Che", section, 105)
        end
    end
end

function Fatalility:CreateConfigSection(parent)
    local section = self:CreateSection("Config", 80, parent, 625)
    
    self:CreateWeaponSetting("Slot", "Slot 1", section, 25)
    
    local loadBtn = self:CreateButton("Load config", UDim2.new(0.4, 0, 0, 25), UDim2.new(0.05, 0, 0, 50), section)
    local saveBtn = self:CreateButton("Save config", UDim2.new(0.4, 0, 0, 25), UDim2.new(0.55, 0, 0, 50), section)
    
    loadBtn.MouseButton1Click:Connect(function()
        self:CreateNotification("Config", "Configuration loaded successfully!", 3, "Success")
    end)
    
    saveBtn.MouseButton1Click:Connect(function()
        self:CreateNotification("Config", "Configuration saved successfully!", 3, "Success")
    end)
end

function Fatalility:CreateSection(title, height, parent, yPosition)
    local section = Instance.new("Frame")
    section.Size = UDim2.new(1, -10, 0, height)
    section.Position = UDim2.new(0, 5, 0, yPosition)
    section.BackgroundColor3 = self.CurrentTheme.Section
    section.BorderSizePixel = 0
    section.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = section

    local stroke = Instance.new("UIStroke")
    stroke.Color = self.CurrentTheme.Tertiary
    stroke.Thickness = 1
    stroke.Parent = section

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 25)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = title
    titleLabel.TextColor3 = self.CurrentTheme.TextPrimary
    titleLabel.TextSize = 14
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = section

    return section
end

function Fatalility:CreateSubSection(title, height, parent)
    local subSection = Instance.new("Frame")
    subSection.Size = UDim2.new(1, -10, 0, height)
    subSection.Position = UDim2.new(0, 5, 0, 25)
    subSection.BackgroundTransparency = 1
    subSection.Parent = parent

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 20)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = title
    titleLabel.TextColor3 = self.CurrentTheme.TextSecondary
    titleLabel.TextSize = 12
    titleLabel.Font = Enum.Font.Gotham
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = subSection

    return subSection
end

function Fatalility:CreateToggle(configPath, displayName, parent, yPosition, small)
    local toggleFrame = Instance.new("Frame")
    toggleFrame.Size = UDim2.new(1, -20, 0, small and 20 or 30)
    toggleFrame.Position = UDim2.new(0, 10, 0, yPosition)
    toggleFrame.BackgroundTransparency = 1
    toggleFrame.Parent = parent

    local toggleButton = Instance.new("TextButton")
    toggleButton.Size = UDim2.new(1, 0, 1, 0)
    toggleButton.BackgroundColor3 = self.CurrentTheme.Tertiary
    toggleButton.BorderSizePixel = 0
    toggleButton.Text = "  " .. displayName
    toggleButton.TextColor3 = self.CurrentTheme.TextSecondary
    toggleButton.TextSize = small and 11 or 12
    toggleButton.Font = Enum.Font.Gotham
    toggleButton.TextXAlignment = Enum.TextXAlignment.Left
    toggleButton.Parent = toggleFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 4)
    corner.Parent = toggleButton

    local currentValue = self:GetConfigValue(configPath)
    
    if currentValue then
        toggleButton.BackgroundColor3 = self.CurrentTheme.Accent
        toggleButton.TextColor3 = self.CurrentTheme.TextPrimary
    end

    toggleButton.MouseButton1Click:Connect(function()
        local newValue = not self:GetConfigValue(configPath)
        self:SetConfigValue(configPath, newValue)
        
        if newValue then
            self:CreateAnimation(toggleButton, {
                BackgroundColor3 = self.CurrentTheme.Accent,
                TextColor3 = self.CurrentTheme.TextPrimary
            }, 0.2):Play()
        else
            self:CreateAnimation(toggleButton, {
                BackgroundColor3 = self.CurrentTheme.Tertiary,
                TextColor3 = self.CurrentTheme.TextSecondary
            }, 0.2):Play()
        end
        
        self:CreateRippleEffect(toggleButton)
    end)

    toggleButton.MouseEnter:Connect(function()
        if not self:GetConfigValue(configPath) then
            self:CreateAnimation(toggleButton, {
                BackgroundColor3 = self.CurrentTheme.Secondary
            }, 0.2):Play()
        end
    end)

    toggleButton.MouseLeave:Connect(function()
        if not self:GetConfigValue(configPath) then
            self:CreateAnimation(toggleButton, {
                BackgroundColor3 = self.CurrentTheme.Tertiary
            }, 0.2):Play()
        end
    end)

    return toggleFrame
end

function Fatalility:CreateWeaponSetting(name, value, parent, yPosition)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 20)
    frame.Position = UDim2.new(0, 0, 0, yPosition)
    frame.BackgroundTransparency = 1
    frame.Parent = parent

    if name ~= "" then
        local nameLabel = Instance.new("TextLabel")
        nameLabel.Size = UDim2.new(0.4, 0, 1, 0)
        nameLabel.Position = UDim2.new(0, 10, 0, 0)
        nameLabel.BackgroundTransparency = 1
        nameLabel.Text = name
        nameLabel.TextColor3 = self.CurrentTheme.TextSecondary
        nameLabel.TextSize = 11
        nameLabel.Font = Enum.Font.Gotham
        nameLabel.TextXAlignment = Enum.TextXAlignment.Left
        nameLabel.Parent = frame
    end

    local valueLabel = Instance.new("TextLabel")
    valueLabel.Size = UDim2.new(0.5, 0, 1, 0)
    valueLabel.Position = UDim2.new(0.4, 0, 0, 0)
    valueLabel.BackgroundTransparency = 1
    valueLabel.Text = value
    valueLabel.TextColor3 = self.CurrentTheme.TextPrimary
    valueLabel.TextSize = 11
    valueLabel.Font = Enum.Font.Gotham
    valueLabel.TextXAlignment = Enum.TextXAlignment.Left
    valueLabel.Parent = frame

    return frame
end

function Fatalility:CreateSetting(name, value, parent, yPosition)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -20, 0, 20)
    frame.Position = UDim2.new(0, 10, 0, yPosition)
    frame.BackgroundTransparency = 1
    frame.Parent = parent

    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(0.6, 0, 1, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.Text = name
    nameLabel.TextColor3 = self.CurrentTheme.TextSecondary
    nameLabel.TextSize = 11
    nameLabel.Font = Enum.Font.Gotham
    nameLabel.TextXAlignment = Enum.TextXAlignment.Left
    nameLabel.Parent = frame

    local valueLabel = Instance.new("TextLabel")
    valueLabel.Size = UDim2.new(0.4, 0, 1, 0)
    valueLabel.Position = UDim2.new(0.6, 0, 0, 0)
    valueLabel.BackgroundTransparency = 1
    valueLabel.Text = value
    valueLabel.TextColor3 = self.CurrentTheme.TextPrimary
    valueLabel.TextSize = 11
    valueLabel.Font = Enum.Font.Gotham
    valueLabel.TextXAlignment = Enum.TextXAlignment.Right
    valueLabel.Parent = frame

    return frame
end

function Fatalility:CreateButton(text, size, position, parent)
    local button = Instance.new("TextButton")
    button.Size = size
    button.Position = position
    button.BackgroundColor3 = self.CurrentTheme.Tertiary
    button.BorderSizePixel = 0
    button.Text = text
    button.TextColor3 = self.CurrentTheme.TextPrimary
    button.TextSize = 12
    button.Font = Enum.Font.Gotham
    button.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = button

    self:CreateRippleEffect(button)

    button.MouseEnter:Connect(function()
        self:CreateAnimation(button, {
            BackgroundColor3 = self.CurrentTheme.Secondary
        }, 0.2):Play()
    end)

    button.MouseLeave:Connect(function()
        self:CreateAnimation(button, {
            BackgroundColor3 = self.CurrentTheme.Tertiary
        }, 0.2):Play()
    end)

    return button
end

function Fatalility:GetConfigValue(path)
    local parts = string.split(path, ".")
    local current = Fatalility.Config
    for _, part in ipairs(parts) do
        current = current[part]
        if not current then return nil end
    end
    return current
end

function Fatalility:SetConfigValue(path, value)
    local parts = string.split(path, ".")
    local current = Fatalility.Config
    for i = 1, #parts - 1 do
        current = current[parts[i]]
        if not current then return end
    end
    current[parts[#parts]] = value
end

function Fatalility:SetupInput()
    UserInputService.InputBegan:Connect(function(input)
        if input.KeyCode == Fatalility.Config.MenuKey then
            self.ScreenGui.Enabled = not self.ScreenGui.Enabled
        elseif input.KeyCode == Fatalility.Config.PanicKey then
            self:Panic()
        end
    end)

    local dragging = false
    local dragInput, dragStart, startPos

    self.MainFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = self.MainFrame.Position
        end
    end)

    self.MainFrame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            self.MainFrame.Position = UDim2.new(
                startPos.X.Scale, 
                startPos.X.Offset + delta.X,
                startPos.Y.Scale, 
                startPos.Y.Offset + delta.Y
            )
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
end

function Fatalility:Panic()
    self:CreateNotification("PANIC MODE", "All features disabled!", 3, "Error")
    
    for _, connection in ipairs(self.Connections or {}) do
        connection:Disconnect()
    end
    
    if self.ScreenGui then
        self.ScreenGui:Destroy()
    end
    
    Fatalility.Config.Rage.Enabled = false
    Fatalility.Config.Visuals.Enabled = false
    Fatalility.Config.Misc.Enabled = false
end

Fatalility:CreateWindow()

warn("FATALITY.CS2 Premium loaded successfully!")
warn("Menu Key: " .. tostring(Fatalility.Config.MenuKey))
warn("Panic Key: " .. tostring(Fatalility.Config.PanicKey))
