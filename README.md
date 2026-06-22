-- =============================================================
-- PAINEL DE REGISTROS - BATE-PAPO RÁPIDO
-- LocalScript → StarterPlayerScripts
-- =============================================================

local Players             = game:GetService("Players")
local UserInputService    = game:GetService("UserInputService")
local ReplicatedStorage   = game:GetService("ReplicatedStorage")
local TextChatService     = game:GetService("TextChatService")

local localPlayer = Players.LocalPlayer
local playerGui   = localPlayer:WaitForChild("PlayerGui")

-- ─────────────────────────────────────────────────────────────
-- GUI
-- ─────────────────────────────────────────────────────────────

local screenGui = Instance.new("ScreenGui")
screenGui.Name           = "PainelRegistros"
screenGui.ResetOnSpawn   = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.DisplayOrder   = 10
screenGui.Parent         = playerGui

local mainFrame = Instance.new("Frame")
mainFrame.Name            = "MainFrame"
mainFrame.Size            = UDim2.new(0, 520, 0, 420)
mainFrame.Position        = UDim2.new(0.5, -260, 0.5, -210)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Active          = true
mainFrame.Draggable       = true
mainFrame.Parent          = screenGui
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 10)
local ms = Instance.new("UIStroke", mainFrame)
ms.Color = Color3.fromRGB(60, 60, 70)
ms.Thickness = 0.5

-- ── HEADER ───────────────────────────────────────────────────

local header = Instance.new("Frame")
header.Name              = "Header"
header.Size              = UDim2.new(1, 0, 0, 42)
header.BackgroundColor3  = Color3.fromRGB(26, 26, 35)
header.BorderSizePixel   = 0
header.Parent            = mainFrame
Instance.new("UICorner", header).CornerRadius = UDim.new(0, 10)

local headerFix = Instance.new("Frame")
headerFix.Size             = UDim2.new(1, 0, 0, 10)
headerFix.Position         = UDim2.new(0, 0, 1, -10)
headerFix.BackgroundColor3 = Color3.fromRGB(26, 26, 35)
headerFix.BorderSizePixel  = 0
headerFix.Parent           = header

local titleLabel = Instance.new("TextLabel")
titleLabel.Size            = UDim2.new(0, 240, 1, 0)
titleLabel.Position        = UDim2.new(0, 12, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text            = "⚡ Registros — Bate-Papo Rápido"
titleLabel.TextColor3      = Color3.fromRGB(220, 220, 230)
titleLabel.TextSize        = 13
titleLabel.Font            = Enum.Font.GothamBold
titleLabel.TextXAlignment  = Enum.TextXAlignment.Left
titleLabel.Parent          = header

local liveBadge = Instance.new("TextLabel")
liveBadge.Size             = UDim2.new(0, 60, 0, 18)
liveBadge.Position         = UDim2.new(0, 255, 0.5, -9)
liveBadge.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
liveBadge.Text             = "AO VIVO"
liveBadge.TextColor3       = Color3.fromRGB(255, 255, 255)
liveBadge.TextSize         = 10
liveBadge.Font             = Enum.Font.GothamBold
liveBadge.Parent           = header
Instance.new("UICorner", liveBadge).CornerRadius = UDim.new(0, 4)

-- Keybind: botão clicável
local keybindBtn = Instance.new("TextButton")
keybindBtn.Name            = "KeybindBtn"
keybindBtn.Size            = UDim2.new(0, 120, 0, 22)
keybindBtn.Position        = UDim2.new(1, -128, 0.5, -11)
keybindBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
keybindBtn.Text            = "Ocultar: [clique]"
keybindBtn.TextColor3      = Color3.fromRGB(160, 160, 190)
keybindBtn.TextSize        = 10
keybindBtn.Font            = Enum.Font.GothamBold
keybindBtn.BorderSizePixel = 0
keybindBtn.Parent          = header
Instance.new("UICorner", keybindBtn).CornerRadius = UDim.new(0, 5)
local ks = Instance.new("UIStroke", keybindBtn)
ks.Color = Color3.fromRGB(80, 80, 110)
ks.Thickness = 0.5

-- ── ABAS ─────────────────────────────────────────────────────

local tabBar = Instance.new("Frame")
tabBar.Name              = "TabBar"
tabBar.Size              = UDim2.new(1, 0, 0, 34)
tabBar.Position          = UDim2.new(0, 0, 0, 42)
tabBar.BackgroundColor3  = Color3.fromRGB(23, 23, 30)
tabBar.BorderSizePixel   = 0
tabBar.Parent            = mainFrame

local tabLayout = Instance.new("UIListLayout")
tabLayout.FillDirection  = Enum.FillDirection.Horizontal
tabLayout.SortOrder      = Enum.SortOrder.LayoutOrder
tabLayout.Parent         = tabBar

local function makeTab(label, icon, order)
	local btn = Instance.new("TextButton")
	btn.Name             = "Tab_" .. label
	btn.Size             = UDim2.new(0, 160, 1, 0)
	btn.BackgroundColor3 = Color3.fromRGB(23, 23, 30)
	btn.BorderSizePixel  = 0
	btn.Text             = icon .. "  " .. label .. "  (0)"
	btn.TextColor3       = Color3.fromRGB(100, 100, 120)
	btn.TextSize         = 12
	btn.Font             = Enum.Font.GothamBold
	btn.LayoutOrder      = order
	btn.Parent           = tabBar

	local underline = Instance.new("Frame")
	underline.Name             = "Underline"
	underline.Size             = UDim2.new(1, 0, 0, 2)
	underline.Position         = UDim2.new(0, 0, 1, -2)
	underline.BackgroundColor3 = Color3.fromRGB(40, 120, 220)
	underline.BorderSizePixel  = 0
	underline.Visible          = false
	underline.Parent           = btn
	return btn
end

local tabTodos   = makeTab("Todos",   "📋", 1)
local tabBoxs    = makeTab("Boxs",    "🏁", 2)
local tabLargada = makeTab("Largada", "🚀", 3)

local separator = Instance.new("Frame")
separator.Size             = UDim2.new(1, 0, 0, 1)
separator.Position         = UDim2.new(0, 0, 0, 76)
separator.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
separator.BorderSizePixel  = 0
separator.Parent           = mainFrame

-- ── SCROLL ───────────────────────────────────────────────────

local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Name                = "LogScroll"
scrollFrame.Size                = UDim2.new(1, -8, 1, -84)
scrollFrame.Position            = UDim2.new(0, 4, 0, 77)
scrollFrame.BackgroundTransparency = 1
scrollFrame.BorderSizePixel     = 0
scrollFrame.ScrollBarThickness  = 4
scrollFrame.ScrollBarImageColor3 = Color3.fromRGB(70, 70, 90)
scrollFrame.CanvasSize          = UDim2.new(0, 0, 0, 0)
scrollFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
scrollFrame.Parent              = mainFrame

local listLayout = Instance.new("UIListLayout")
listLayout.SortOrder   = Enum.SortOrder.LayoutOrder
listLayout.Padding     = UDim.new(0, 4)
listLayout.Parent      = scrollFrame

local listPadding = Instance.new("UIPadding")
listPadding.PaddingTop    = UDim.new(0, 6)
listPadding.PaddingLeft   = UDim.new(0, 6)
listPadding.PaddingRight  = UDim.new(0, 6)
listPadding.PaddingBottom = UDim.new(0, 6)
listPadding.Parent        = scrollFrame

local emptyLabel = Instance.new("TextLabel")
emptyLabel.Name              = "EmptyLabel"
emptyLabel.Size              = UDim2.new(1, 0, 0, 60)
emptyLabel.BackgroundTransparency = 1
emptyLabel.Text              = "Nenhum bate-papo rápido detectado ainda..."
emptyLabel.TextColor3        = Color3.fromRGB(80, 80, 100)
emptyLabel.TextSize          = 12
emptyLabel.Font              = Enum.Font.Gotham
emptyLabel.LayoutOrder       = 9999
emptyLabel.Parent            = scrollFrame

-- ─────────────────────────────────────────────────────────────
-- ESTADO
-- ─────────────────────────────────────────────────────────────

local entries      = {}
local entryCount   = 0
local currentTab   = "Todos"
local hudVisible   = true
local keybind      = nil   -- Enum.KeyCode ou nil
local recording    = false

-- ─────────────────────────────────────────────────────────────
-- HELPERS
-- ─────────────────────────────────────────────────────────────

local function getTime()
	local t = os.date("*t")
	return string.format("%02d:%02d:%02d", t.hour, t.min, t.sec)
end

local function updateTabLabels()
	local tots, boxs, largs = #entries, 0, 0
	for _, e in ipairs(entries) do
		if e.tag == "Boxs"    then boxs  += 1 end
		if e.tag == "Largada" then largs += 1 end
	end
	tabTodos.Text   = "📋  Todos  (" .. tots .. ")"
	tabBoxs.Text    = "🏁  Boxs  (" .. boxs .. ")"
	tabLargada.Text = "🚀  Largada  (" .. largs .. ")"
end

local function refreshVisibility()
	local any = false
	for _, e in ipairs(entries) do
		if e.frame then
			local show = (currentTab == "Todos")
				or (currentTab == "Boxs"    and e.tag == "Boxs")
				or (currentTab == "Largada" and e.tag == "Largada")
			e.frame.Visible = show
			if show then any = true end
		end
	end
	emptyLabel.Visible = not any
end

local function setActiveTab(tab)
	currentTab = tab
	for _, t in ipairs({tabTodos, tabBoxs, tabLargada}) do
		local active = t.Name == "Tab_" .. tab
		t.TextColor3          = active and Color3.fromRGB(220,220,240) or Color3.fromRGB(100,100,120)
		t.Underline.Visible   = active
	end
	refreshVisibility()
end

-- ─────────────────────────────────────────────────────────────
-- CRIAR LINHA DE REGISTRO
-- ─────────────────────────────────────────────────────────────

local function makeEntryFrame(entry)
	local frame = Instance.new("Frame")
	frame.Name             = "Entry_" .. entry.id
	frame.Size             = UDim2.new(1, 0, 0, 56)
	frame.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
	frame.BorderSizePixel  = 0
	frame.LayoutOrder      = -entry.id
	frame.Parent           = scrollFrame
	Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 7)
	local fs = Instance.new("UIStroke", frame)
	fs.Color = Color3.fromRGB(50, 50, 65)
	fs.Thickness = 0.5

	-- Ícone
	local icon = Instance.new("TextLabel")
	icon.Size              = UDim2.new(0, 32, 0, 32)
	icon.Position          = UDim2.new(0, 8, 0.5, -16)
	icon.BackgroundColor3  = Color3.fromRGB(35, 60, 100)
	icon.Text              = "💬"
	icon.TextSize          = 16
	icon.Font              = Enum.Font.Gotham
	icon.BorderSizePixel   = 0
	icon.Parent            = frame
	Instance.new("UICorner", icon).CornerRadius = UDim.new(1, 0)

	-- Texto principal
	local mainText = Instance.new("TextLabel")
	mainText.Size           = UDim2.new(1, -175, 0, 20)
	mainText.Position       = UDim2.new(0, 48, 0, 8)
	mainText.BackgroundTransparency = 1
	mainText.RichText       = true
	mainText.Text           = "O player <b>" .. entry.player .. "</b> utilizou o bate-papo rápido às <b>" .. entry.time .. "</b>"
	mainText.TextColor3     = Color3.fromRGB(190, 190, 210)
	mainText.TextSize       = 11
	mainText.Font           = Enum.Font.Gotham
	mainText.TextXAlignment = Enum.TextXAlignment.Left
	mainText.TextWrapped    = true
	mainText.Parent         = frame

	local subText = Instance.new("TextLabel")
	subText.Size            = UDim2.new(1, -175, 0, 16)
	subText.Position        = UDim2.new(0, 48, 0, 31)
	subText.BackgroundTransparency = 1
	subText.Text            = '"' .. entry.message .. '"'
	subText.TextColor3      = Color3.fromRGB(100, 100, 130)
	subText.TextSize        = 10
	subText.Font            = Enum.Font.Gotham
	subText.TextXAlignment  = Enum.TextXAlignment.Left
	subText.Parent          = frame

	-- Tag label
	local tagLabel = Instance.new("TextLabel")
	tagLabel.Name           = "TagLabel"
	tagLabel.Size           = UDim2.new(0, 60, 0, 18)
	tagLabel.Position       = UDim2.new(1, -165, 0.5, -9)
	tagLabel.BackgroundTransparency = 1
	tagLabel.Text           = ""
	tagLabel.TextSize       = 10
	tagLabel.Font           = Enum.Font.GothamBold
	tagLabel.BorderSizePixel = 0
	tagLabel.Parent         = frame
	Instance.new("UICorner", tagLabel).CornerRadius = UDim.new(0, 4)

	-- Botão B
	local btnB = Instance.new("TextButton")
	btnB.Name              = "BtnBoxs"
	btnB.Size              = UDim2.new(0, 38, 0, 24)
	btnB.Position          = UDim2.new(1, -100, 0.5, -12)
	btnB.BackgroundColor3  = Color3.fromRGB(160, 110, 20)
	btnB.Text              = "B"
	btnB.TextColor3        = Color3.fromRGB(255, 235, 180)
	btnB.TextSize          = 12
	btnB.Font              = Enum.Font.GothamBold
	btnB.BorderSizePixel   = 0
	btnB.Parent            = frame
	Instance.new("UICorner", btnB).CornerRadius = UDim.new(0, 5)

	-- Tooltip B
	local tipB = Instance.new("TextLabel")
	tipB.Size              = UDim2.new(0, 38, 0, 10)
	tipB.Position          = UDim2.new(0, 0, 1, 2)
	tipB.BackgroundTransparency = 1
	tipB.Text              = "Boxs"
	tipB.TextColor3        = Color3.fromRGB(160, 130, 60)
	tipB.TextSize          = 8
	tipB.Font              = Enum.Font.Gotham
	tipB.Parent            = btnB

	-- Botão L
	local btnL = Instance.new("TextButton")
	btnL.Name              = "BtnLargada"
	btnL.Size              = UDim2.new(0, 38, 0, 24)
	btnL.Position          = UDim2.new(1, -52, 0.5, -12)
	btnL.BackgroundColor3  = Color3.fromRGB(30, 120, 55)
	btnL.Text              = "L"
	btnL.TextColor3        = Color3.fromRGB(180, 255, 200)
	btnL.TextSize          = 12
	btnL.Font              = Enum.Font.GothamBold
	btnL.BorderSizePixel   = 0
	btnL.Parent            = frame
	Instance.new("UICorner", btnL).CornerRadius = UDim.new(0, 5)

	local tipL = Instance.new("TextLabel")
	tipL.Size              = UDim2.new(0, 38, 0, 10)
	tipL.Position          = UDim2.new(0, 0, 1, 2)
	tipL.BackgroundTransparency = 1
	tipL.Text              = "Largada"
	tipL.TextColor3        = Color3.fromRGB(80, 180, 100)
	tipL.TextSize          = 8
	tipL.Font              = Enum.Font.Gotham
	tipL.Parent            = btnL

	local function applyTag(tag)
		if entry.tag then return end
		entry.tag = tag
		-- desativa botões
		btnB.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
		btnB.TextColor3       = Color3.fromRGB(70, 70, 90)
		btnL.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
		btnL.TextColor3       = Color3.fromRGB(70, 70, 90)
		-- mostra tag
		if tag == "Boxs" then
			tagLabel.BackgroundColor3 = Color3.fromRGB(80, 55, 10)
			tagLabel.TextColor3       = Color3.fromRGB(255, 215, 100)
			tagLabel.Text             = " Boxs "
		else
			tagLabel.BackgroundColor3 = Color3.fromRGB(15, 65, 30)
			tagLabel.TextColor3       = Color3.fromRGB(130, 255, 170)
			tagLabel.Text             = " Largada "
		end
		tagLabel.BackgroundTransparency = 0
		updateTabLabels()
		refreshVisibility()
	end

	btnB.MouseButton1Click:Connect(function() applyTag("Boxs")    end)
	btnL.MouseButton1Click:Connect(function() applyTag("Largada") end)

	return frame
end

-- ─────────────────────────────────────────────────────────────
-- ADICIONAR ENTRADA
-- ─────────────────────────────────────────────────────────────

-- Anti-duplicata: evita registrar a mesma msg do mesmo player 2x em <2s
local recentKeys = {}

local function addEntry(playerName, message)
	local key = playerName .. "|" .. message
	if recentKeys[key] then return end
	recentKeys[key] = true
	task.delay(2, function() recentKeys[key] = nil end)

	entryCount += 1
	local entry = {
		id      = entryCount,
		player  = playerName,
		message = message,
		time    = getTime(),
		tag     = nil,
		frame   = nil,
	}
	table.insert(entries, 1, entry)
	entry.frame = makeEntryFrame(entry)
	emptyLabel.Visible = false
	updateTabLabels()
	refreshVisibility()
	scrollFrame.CanvasPosition = Vector2.new(0, 0)
end

-- ─────────────────────────────────────────────────────────────
-- DETECÇÃO (método principal: TextChatService)
-- ─────────────────────────────────────────────────────────────
-- O Roblox envia bate-papos rápidos com o prefixo
-- "[Bate-papo rápido]" visível no chat. Capturamos isso.
-- ─────────────────────────────────────────────────────────────

TextChatService.MessageReceived:Connect(function(msg)
	local text = msg.Text or ""

	-- O Roblox adiciona rich-text ao prefixo; limpamos tags HTML
	local clean = text:gsub("<[^>]+>", "")

	local playerName = nil
	local message    = nil

	-- Padrão 1: "[Bate-papo rápido] NomePlayer: Mensagem"
	local n1, m1 = clean:match("%[Bate%-papo r%ápido%]%s+(.-):%s+(.+)")
	if n1 then playerName, message = n1, m1 end

	-- Padrão 2 (inglês/fallback): "[Quick Chat] NomePlayer: Mensagem"
	if not playerName then
		local n2, m2 = clean:match("%[Quick Chat%]%s+(.-):%s+(.+)")
		if n2 then playerName, message = n2, m2 end
	end

	-- Padrão 3: a fonte da mensagem marcada como QuickChat nos metadados
	if not playerName then
		local meta = msg.Metadata or ""
		if meta:find("[Qq]uick") or meta:find("[Rr]%ápido") then
			local src = msg.TextSource
			if src then
				local p = Players:GetPlayerByUserId(src.UserId)
				if p then
					playerName = p.DisplayName ~= "" and p.DisplayName or p.Name
					message    = clean:match("^.+:%s*(.+)$") or clean
				end
			end
		end
	end

	-- Padrão 4: verifica se o TextSource tem IsQuickChat (novo atributo)
	if not playerName then
		local src = msg.TextSource
		if src and src:FindFirstChild("IsQuickChat") and src.IsQuickChat.Value then
			local p = Players:GetPlayerByUserId(src.UserId)
			if p then
				playerName = p.DisplayName ~= "" and p.DisplayName or p.Name
				message    = clean:match("^.+:%s*(.+)$") or clean
			end
		end
	end

	if playerName and message then
		-- Limpa espaços extras
		playerName = playerName:match("^%s*(.-)%s*$")
		message    = message:match("^%s*(.-)%s*$")
		addEntry(playerName, message)
	end
end)

-- ─────────────────────────────────────────────────────────────
-- REMOTEVENT DO SERVIDOR (redundância garantida)
-- ─────────────────────────────────────────────────────────────

task.spawn(function()
	local folder = ReplicatedStorage:WaitForChild("PainelRegistrosEvents", 15)
	if not folder then return end
	local ev = folder:WaitForChild("QuickChatDetected", 15)
	if not ev then return end
	ev.OnClientEvent:Connect(function(playerName, message)
		addEntry(playerName, message)
	end)
end)

-- ─────────────────────────────────────────────────────────────
-- ABAS
-- ─────────────────────────────────────────────────────────────

tabTodos.MouseButton1Click:Connect(function()   setActiveTab("Todos")   end)
tabBoxs.MouseButton1Click:Connect(function()    setActiveTab("Boxs")    end)
tabLargada.MouseButton1Click:Connect(function() setActiveTab("Largada") end)
setActiveTab("Todos")

-- ─────────────────────────────────────────────────────────────
-- KEYBIND SELECIONÁVEL
-- ─────────────────────────────────────────────────────────────

keybindBtn.MouseButton1Click:Connect(function()
	if recording then return end
	recording = true
	keybindBtn.Text      = "Pressione uma tecla..."
	keybindBtn.TextColor3 = Color3.fromRGB(220, 180, 60)

	local conn
	conn = UserInputService.InputBegan:Connect(function(input, gpe)
		-- Ignora modificadores sozinhos
		if input.UserInputType ~= Enum.UserInputType.Keyboard then return end
		local ignore = {
			[Enum.KeyCode.LeftShift]=true, [Enum.KeyCode.RightShift]=true,
			[Enum.KeyCode.LeftControl]=true,[Enum.KeyCode.RightControl]=true,
			[Enum.KeyCode.LeftAlt]=true,   [Enum.KeyCode.RightAlt]=true,
			[Enum.KeyCode.LeftMeta]=true,  [Enum.KeyCode.RightMeta]=true,
		}
		if ignore[input.KeyCode] then return end

		keybind = input.KeyCode
		keybindBtn.Text       = "Ocultar: [" .. input.KeyCode.Name .. "]"
		keybindBtn.TextColor3 = Color3.fromRGB(160, 160, 190)
		recording = false
		conn:Disconnect()
	end)
end)

-- Escuta o keybind para ocultar/mostrar
UserInputService.InputBegan:Connect(function(input, gpe)
	if gpe then return end
	if recording then return end
	if keybind and input.KeyCode == keybind then
		hudVisible = not hudVisible
		mainFrame.Visible = hudVisible
	end
end)
