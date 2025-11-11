--[[ 
    SlayerLibrary.lua
    Criado por você 😎
    Um sistema de UI modular, estilo Rayfield/Xeter.
]]

local SlayerLibrary = {}

-- // Criar janela principal
function SlayerLibrary:CreateWindow(settings)
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "SlayerHubUI"
    ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

    -- Fundo principal
    local Main = Instance.new("Frame")
    Main.Size = UDim2.new(0, 650, 0, 420)
    Main.Position = UDim2.new(0.5, -325, 0.5, -210)
    Main.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    Main.BorderSizePixel = 0
    Main.Active = true
    Main.Draggable = true
    Main.Parent = ScreenGui

    local UICorner = Instance.new("UICorner", Main)
    UICorner.CornerRadius = UDim.new(0, 12)

    local Title = Instance.new("TextLabel")
    Title.Size = UDim2.new(1, -20, 0, 40)
    Title.Position = UDim2.new(0, 10, 0, 5)
    Title.BackgroundTransparency = 1
    Title.Text = settings.Name or "Slayer Hub"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 22
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = Main

    -- Divisor vertical (menu lateral)
    local SideMenu = Instance.new("Frame")
    SideMenu.Size = UDim2.new(0, 180, 1, -50)
    SideMenu.Position = UDim2.new(0, 0, 0, 50)
    SideMenu.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    SideMenu.BorderSizePixel = 0
    SideMenu.Parent = Main

    local UICorner2 = Instance.new("UICorner", SideMenu)
    UICorner2.CornerRadius = UDim.new(0, 10)

    local TabList = Instance.new("Frame", SideMenu)
    TabList.BackgroundTransparency = 1
    TabList.Size = UDim2.new(1, 0, 1, 0)

    local UIListLayout = Instance.new("UIListLayout", TabList)
    UIListLayout.Padding = UDim.new(0, 6)
    UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

    -- Container da aba ativa
    local ContentFrame = Instance.new("Frame")
    ContentFrame.Size = UDim2.new(1, -190, 1, -60)
    ContentFrame.Position = UDim2.new(0, 190, 0, 60)
    ContentFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    ContentFrame.BorderSizePixel = 0
    ContentFrame.Parent = Main

    local UICorner3 = Instance.new("UICorner", ContentFrame)
    UICorner3.CornerRadius = UDim.new(0, 10)

    local window = {}
    window.Tabs = {}

    -- Criar nova aba
    function window:CreateTab(name, icon)
        local TabButton = Instance.new("TextButton")
        TabButton.Size = UDim2.new(1, -10, 0, 35)
        TabButton.Position = UDim2.new(0, 5, 0, 0)
        TabButton.Text = "  " .. name
        TabButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        TabButton.Font = Enum.Font.Gotham
        TabButton.TextSize = 14
        TabButton.BorderSizePixel = 0
        TabButton.AutoButtonColor = false
        TabButton.Parent = TabList

        local UIC = Instance.new("UICorner", TabButton)
        UIC.CornerRadius = UDim.new(0, 8)

        local TabContent = Instance.new("ScrollingFrame")
        TabContent.Visible = false
        TabContent.Size = UDim2.new(1, -20, 1, -20)
        TabContent.Position = UDim2.new(0, 10, 0, 10)
        TabContent.CanvasSize = UDim2.new(0, 0, 0, 0)
        TabContent.ScrollBarThickness = 4
        TabContent.BackgroundTransparency = 1
        TabContent.Parent = ContentFrame

        local layout = Instance.new("UIListLayout", TabContent)
        layout.Padding = UDim.new(0, 8)
        layout.SortOrder = Enum.SortOrder.LayoutOrder

        local tab = {}
        tab.Frame = TabContent

        -- Botão de seleção
        TabButton.MouseButton1Click:Connect(function()
            for _, v in pairs(ContentFrame:GetChildren()) do
                if v:IsA("ScrollingFrame") then v.Visible = false end
            end
            for _, v in pairs(TabList:GetChildren()) do
                if v:IsA("TextButton") then
                    v.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
                end
            end
            TabButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            TabContent.Visible = true
        end)

        -- Criar botão
        function tab:CreateButton(data)
            local Btn = Instance.new("TextButton")
            Btn.Size = UDim2.new(1, 0, 0, 40)
            Btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
            Btn.Text = data.Name or "Button"
            Btn.Font = Enum.Font.Gotham
            Btn.TextSize = 16
            Btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            Btn.BorderSizePixel = 0
            Btn.Parent = TabContent

            local corner = Instance.new("UICorner", Btn)
            corner.CornerRadius = UDim.new(0, 8)

            Btn.MouseButton1Click:Connect(function()
                if data.Callback then
                    pcall(data.Callback)
                end
            end)
        end

        window.Tabs[name] = tab
        return tab
    end

    return window
end

return SlayerLibrary
