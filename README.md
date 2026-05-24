-- TeleportHub.lua
-- Coloque este script em: StarterPlayerScripts (LocalScript)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- ========== GUI ==========

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TeleportHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = PlayerGui

-- Frame principal (altura aumentada para caber a search bar)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 220, 0, 340)
MainFrame.Position = UDim2.new(0, 16, 0.5, -170)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = MainFrame

local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(80, 80, 120)
UIStroke.Thickness = 1
UIStroke.Parent = MainFrame

-- Header
local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 44)
Header.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
Header.BorderSizePixel = 0
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 12)
HeaderCorner.Parent = Header

local HeaderFix = Instance.new("Frame")
HeaderFix.Size = UDim2.new(1, 0, 0, 12)
HeaderFix.Position = UDim2.new(0, 0, 1, -12)
HeaderFix.BackgroundColor3 = Color3.fromRGB(28, 28, 40)
HeaderFix.BorderSizePixel = 0
HeaderFix.Parent = Header

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Text = "⚡ Teleport Hub"
TitleLabel.Size = UDim2.new(1, -80, 1, 0)
TitleLabel.Position = UDim2.new(0, 14, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.TextColor3 = Color3.fromRGB(220, 220, 255)
TitleLabel.TextSize = 14
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = Header

-- Botão recarregar
local ReloadBtn = Instance.new("TextButton")
ReloadBtn.Text = "↺"
ReloadBtn.Size = UDim2.new(0, 30, 0, 30)
ReloadBtn.Position = UDim2.new(1, -72, 0, 7)
ReloadBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 90)
ReloadBtn.TextColor3 = Color3.fromRGB(180, 180, 255)
ReloadBtn.TextSize = 18
ReloadBtn.Font = Enum.Font.GothamBold
ReloadBtn.BorderSizePixel = 0
ReloadBtn.Parent = Header

local ReloadCorner = Instance.new("UICorner")
ReloadCorner.CornerRadius = UDim.new(0, 8)
ReloadCorner.Parent = ReloadBtn

-- Botão fechar (vermelho)
local CloseBtn = Instance.new("TextButton")
CloseBtn.Text = "✕"
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -38, 0, 7)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
CloseBtn.TextColor3 = Color3.fromRGB(255, 220, 220)
CloseBtn.TextSize = 14
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.BorderSizePixel = 0
CloseBtn.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 8)
CloseCorner.Parent = CloseBtn

-- ===== BARRA DE PESQUISA =====
local SearchBox = Instance.new("TextBox")
SearchBox.PlaceholderText = "🔍  Pesquisar jogador..."
SearchBox.Text = ""
SearchBox.Size = UDim2.new(1, -16, 0, 30)
SearchBox.Position = UDim2.new(0, 8, 0, 50)
SearchBox.BackgroundColor3 = Color3.fromRGB(30, 30, 46)
SearchBox.TextColor3 = Color3.fromRGB(210, 210, 255)
SearchBox.PlaceholderColor3 = Color3.fromRGB(90, 90, 130)
SearchBox.TextSize = 12
SearchBox.Font = Enum.Font.Gotham
SearchBox.BorderSizePixel = 0
SearchBox.ClearTextOnFocus = false
SearchBox.Parent = MainFrame

local SearchCorner = Instance.new("UICorner")
SearchCorner.CornerRadius = UDim.new(0, 8)
SearchCorner.Parent = SearchBox

local SearchStroke = Instance.new("UIStroke")
SearchStroke.Color = Color3.fromRGB(70, 70, 110)
SearchStroke.Thickness = 1
SearchStroke.Parent = SearchBox

local SearchPadding = Instance.new("UIPadding")
SearchPadding.PaddingLeft = UDim.new(0, 10)
SearchPadding.Parent = SearchBox

-- Info de quantidade
local CountLabel = Instance.new("TextLabel")
CountLabel.Text = "0 jogadores"
CountLabel.Size = UDim2.new(1, -16, 0, 18)
CountLabel.Position = UDim2.new(0, 10, 0, 86)
CountLabel.BackgroundTransparency = 1
CountLabel.TextColor3 = Color3.fromRGB(100, 100, 140)
CountLabel.TextSize = 10
CountLabel.Font = Enum.Font.Gotham
CountLabel.TextXAlignment = Enum.TextXAlignment.Left
CountLabel.Parent = MainFrame

-- ScrollingFrame
local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Size = UDim2.new(1, -12, 1, -152)
ScrollFrame.Position = UDim2.new(0, 6, 0, 108)
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.BorderSizePixel = 0
ScrollFrame.ScrollBarThickness = 3
ScrollFrame.ScrollBarImageColor3 = Color3.fromRGB(80, 80, 120)
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollFrame.Parent = MainFrame

local ListLayout = Instance.new("UIListLayout")
ListLayout.SortOrder = Enum.SortOrder.LayoutOrder
ListLayout.Padding = UDim.new(0, 4)
ListLayout.Parent = ScrollFrame

-- Linha divisória
local Divider = Instance.new("Frame")
Divider.Size = UDim2.new(1, -20, 0, 1)
Divider.Position = UDim2.new(0, 10, 1, -42)
Divider.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
Divider.BorderSizePixel = 0
Divider.Parent = MainFrame

-- Footer
local FooterLabel = Instance.new("TextLabel")
FooterLabel.Text = "Clique no nome para teleportar"
FooterLabel.Size = UDim2.new(1, 0, 0, 32)
FooterLabel.Position = UDim2.new(0, 0, 1, -38)
FooterLabel.BackgroundTransparency = 1
FooterLabel.TextColor3 = Color3.fromRGB(80, 80, 110)
FooterLabel.TextSize = 10
FooterLabel.Font = Enum.Font.Gotham
FooterLabel.TextXAlignment = Enum.TextXAlignment.Center
FooterLabel.Parent = MainFrame

-- ========== FUNÇÕES ==========

local function getTeleportTarget(player)
	local character = player.Character
	if character then
		local hrp = character:FindFirstChild("HumanoidRootPart")
		if hrp then return hrp.CFrame end
	end
	return nil
end

local function teleportTo(player)
	local target = getTeleportTarget(player)
	if not target then
		FooterLabel.Text = "❌ Jogador não encontrado!"
		FooterLabel.TextColor3 = Color3.fromRGB(220, 80, 80)
		task.delay(2, function()
			FooterLabel.Text = "Clique no nome para teleportar"
			FooterLabel.TextColor3 = Color3.fromRGB(80, 80, 110)
		end)
		return
	end
	local myCharacter = LocalPlayer.Character
	if myCharacter then
		local myHRP = myCharacter:FindFirstChild("HumanoidRootPart")
		if myHRP then
			myHRP.CFrame = target * CFrame.new(2, 0, 2)
			FooterLabel.Text = "✔ Teleportado para " .. player.Name
			FooterLabel.TextColor3 = Color3.fromRGB(80, 220, 120)
			task.delay(2, function()
				FooterLabel.Text = "Clique no nome para teleportar"
				FooterLabel.TextColor3 = Color3.fromRGB(80, 80, 110)
			end)
		end
	end
end

local function createPlayerButton(player)
	if player == LocalPlayer then return end

	local Btn = Instance.new("TextButton")
	Btn.Name = player.Name
	Btn.Size = UDim2.new(1, 0, 0, 36)
	Btn.BackgroundColor3 = Color3.fromRGB(30, 30, 46)
	Btn.BorderSizePixel = 0
	Btn.TextColor3 = Color3.fromRGB(200, 200, 240)
	Btn.TextSize = 13
	Btn.Font = Enum.Font.Gotham
	Btn.TextXAlignment = Enum.TextXAlignment.Left
	Btn.AutoButtonColor = false
	Btn.Parent = ScrollFrame

	local BtnCorner = Instance.new("UICorner")
	BtnCorner.CornerRadius = UDim.new(0, 8)
	BtnCorner.Parent = Btn

	local BtnPadding = Instance.new("UIPadding")
	BtnPadding.PaddingLeft = UDim.new(0, 10)
	BtnPadding.Parent = Btn

	Btn.Text = "👤  " .. player.DisplayName

	Btn.MouseEnter:Connect(function()
		Btn.BackgroundColor3 = Color3.fromRGB(55, 55, 85)
	end)
	Btn.MouseLeave:Connect(function()
		Btn.BackgroundColor3 = Color3.fromRGB(30, 30, 46)
	end)
	Btn.Activated:Connect(function()
		Btn.BackgroundColor3 = Color3.fromRGB(70, 70, 110)
		task.delay(0.15, function()
			Btn.BackgroundColor3 = Color3.fromRGB(30, 30, 46)
		end)
		teleportTo(player)
	end)

	return Btn
end

-- Filtra os botões visíveis pelo texto da pesquisa
local function applyFilter(query)
	local q = query:lower()
	local visible = 0
	for _, btn in ipairs(ScrollFrame:GetChildren()) do
		if btn:IsA("TextButton") then
			local match = q == "" or btn.Name:lower():find(q, 1, true) ~= nil
			btn.Visible = match
			if match then visible += 1 end
		end
	end
	ListLayout:ApplyLayout()
	ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, ListLayout.AbsoluteContentSize.Y + 4)

	if q ~= "" then
		CountLabel.Text = visible .. " resultado(s) para \"" .. query .. "\""
	else
		local total = #Players:GetPlayers() - 1
		CountLabel.Text = total .. (total == 1 and " jogador no servidor" or " jogadores no servidor")
	end
end

local function refreshList()
	for _, child in ipairs(ScrollFrame:GetChildren()) do
		if child:IsA("TextButton") then child:Destroy() end
	end

	local count = 0
	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= LocalPlayer then
			createPlayerButton(player)
			count += 1
		end
	end

	ListLayout:ApplyLayout()
	ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, ListLayout.AbsoluteContentSize.Y + 4)
	CountLabel.Text = count .. (count == 1 and " jogador no servidor" or " jogadores no servidor")

	-- Reaplicar filtro se houver texto na busca
	if SearchBox.Text ~= "" then
		applyFilter(SearchBox.Text)
	end
end

-- ========== EVENTOS ==========

-- Pesquisa em tempo real ao digitar
SearchBox:GetPropertyChangedSignal("Text"):Connect(function()
	applyFilter(SearchBox.Text)
end)

-- Highlight da search bar ao focar
SearchBox.Focused:Connect(function()
	SearchStroke.Color = Color3.fromRGB(120, 120, 200)
end)
SearchBox.FocusLost:Connect(function()
	SearchStroke.Color = Color3.fromRGB(70, 70, 110)
end)

CloseBtn.MouseEnter:Connect(function()
	CloseBtn.BackgroundColor3 = Color3.fromRGB(220, 50, 50)
end)
CloseBtn.MouseLeave:Connect(function()
	CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
end)
CloseBtn.Activated:Connect(function()
	MainFrame.Visible = false
	ScreenGui.Enabled = false
	pcall(function() ScreenGui:Destroy() end)
end)

ReloadBtn.Activated:Connect(function()
	ReloadBtn.Text = "..."
	task.wait(0.3)
	refreshList()
	ReloadBtn.Text = "↺"
end)

Players.PlayerAdded:Connect(function()
	task.wait(1)
	refreshList()
end)

Players.PlayerRemoving:Connect(function()
	task.wait(0.2)
	refreshList()
end)

-- Carga inicial
task.wait(1)
refreshList()
