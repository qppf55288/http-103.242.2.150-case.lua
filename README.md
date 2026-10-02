



--lyy开源
--垃圾ai缝合脚本圈钱
--lyy泛滥




local Players  = game:GetService("Players")
local Tween    = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local LP       = Players.LocalPlayer
local Camera   = workspace.CurrentCamera

local C = {
	overlay = Color3.fromRGB(8, 8, 10),
	card    = Color3.fromRGB(19, 19, 22),
	field   = Color3.fromRGB(27, 27, 31),
	text    = Color3.fromRGB(236, 236, 240),
	dim     = Color3.fromRGB(118, 118, 126),
	faint   = Color3.fromRGB(58, 58, 64),
	accent  = Color3.fromRGB(236, 236, 240),
	ink     = Color3.fromRGB(18, 18, 21),
	bad     = Color3.fromRGB(198, 106, 106),
	good    = Color3.fromRGB(146, 200, 156),
}

local CACHE = "case_key.txt"

local function new(class, props, parent)
	local o = Instance.new(class)
	for k, v in pairs(props or {}) do o[k] = v end
	if parent then o.Parent = parent end
	return o
end

local function round(inst, r)
	if type(r) == "number" then r = UDim.new(0, r) end
	new("UICorner", { CornerRadius = r or UDim.new(0, 10) }, inst)
	return inst
end

local function tw(inst, time, props, style, delay)
	local info = TweenInfo.new(time, style or Enum.EasingStyle.Quad, Enum.EasingDirection.Out, 0, false, delay or 0)
	local t = Tween:Create(inst, info, props)
	t:Play()
	return t
end

local function getParent()
	if gethui then
		local ok, h = pcall(gethui)
		if ok and h then return h end
	end
	local ok, cg = pcall(function() return game:GetService("CoreGui") end)
	if ok and cg then return cg end
	return LP:WaitForChild("PlayerGui")
end

local function hasFileAPI()
	return type(writefile) == "function" and type(readfile) == "function" and type(isfile) == "function"
end

local function norm(s)
	return tostring(s or ""):gsub("%s+", ""):lower()
end

local function onSuccess()
	print("[case] 卡密验证通过 ✔")
	-- ↓↓↓ 验证通过后加载你的脚本 ↓↓↓
	task.spawn(function()
		print("[case] 脚本开始执行")
pcall(function() setfpscap(360) end)

local Players = game:GetService("Players")
local LP = Players.LocalPlayer
local LocalPlayer = LP                       
local RS = game:GetService("ReplicatedStorage")
local WS = game:GetService("Workspace")
local UIS = game:GetService("UserInputService")
local TS = game:GetService("TweenService")
local Run = game:GetService("RunService")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local Lighting = game:GetService("Lighting")
local Camera = WS.CurrentCamera

local Theme = {
	Pink = Color3.fromRGB(255, 105, 180),
	Dark = Color3.fromRGB(25, 25, 35),
	Darker = Color3.fromRGB(15, 15, 20),
	White = Color3.fromRGB(240, 240, 245),
	Green = Color3.fromRGB(120, 255, 120),
	Red = Color3.fromRGB(255, 120, 120),
}

local State = {
	OnePunch = false,
	OP_Range = 35,
	OP_Delay = 0.7,
	OP_SkipShield = true,
	Aim = false,
	Aim_Part = "Head",
	Aim_FOV = 120,
	Aim_Distance = 500,
	Aim_Wallbang = false,
	Aim_Team = false,
	Aim_Smooth = true,
	Aim_Speed = 0.15,
	Aim_ShowFOV = true,
	Aim_ShowName = true,
	BulletTrack = false,
	BT_Part = "Head",
	BT_FOV = 200,
	BT_Distance = 1000,
	BT_Wallbang = false,
	BT_Team = false,
	Noclip = false,
	InfJump = false,
	WalkSpeed = 16,
	JumpPower = 50,
	Fullbright = false,
	Platform = false,
	TPWalk = false,
	TPWalkSpeed = 5,
	AutoRespawn = false,
	SelectedPlayer = nil,
	LoopTP = false,
	LoopTP_Delay = 1,
	PullPlayers = false,
	Pull_Distance = 10,
	Spectate = false,
	SavedCoords = {},
	AutoArmor = false,
	WhiteList = {},
	BlackList = {},
	WhiteList_Enabled = false,
	BlackList_Enabled = false,
}

local function getChar(p)
	p = p or LP
	return p.Character
end
local function getHRP(p)
	local c = getChar(p)
	return c and c:FindFirstChild("HumanoidRootPart")
end
local function getHum(p)
	local c = getChar(p)
	return c and c:FindFirstChildOfClass("Humanoid")
end
local function isAlive(p)
	return getHRP(p) and getHum(p) and getHum(p).Health > 0
end
local function getPlayers()
	local list = {}
	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= LP then table.insert(list, p) end
	end
	return list
end
local function getPlayerNameList()
	local t = {}
	for _, p in ipairs(getPlayers()) do
		table.insert(t, p.Name)
	end
	return t
end

local WindUI
do
	local t0 = tick()
	local CACHE = "case_windui_v1665.lua"
	local canFile = (type(readfile) == "function") and (type(writefile) == "function") and (type(isfile) == "function")

	local function build(src)
		local f, err = loadstring(src)
		if not f then return nil, err end
		local ok, w = pcall(f)
		if not ok or type(w) ~= "table" then return nil, tostring(w) end
		return w
	end

	if canFile then
		local ok, src = pcall(function()
			if isfile(CACHE) then return readfile(CACHE) end
		end)
		if ok and type(src) == "string" and #src > 10000 then
			local w, err = build(src)
			if w then
				WindUI = w
				print("[case] WindUI 缓存加载 " .. string.format("%.2f", tick() - t0) .. "s")
			else
				print("[case] 缓存损坏, 重新下载:", err)
			end
		end
	end

	if not WindUI then
		local urls = {
			"https://cdn.jsdelivr.net/gh/tnine-n9/n9@main/Wind3",
			"https://raw.githubusercontent.com/tnine-n9/n9/refs/heads/main/Wind3",
		}
		for i = 1, #urls do
			print("[case] 拉取 WindUI " .. i .. " ...")
			local ok, res = pcall(game.HttpGet, game, urls[i])
			if ok and type(res) == "string" and #res > 10000 then
				if canFile then pcall(writefile, CACHE, res) end
				local w, err = build(res)
				if w then
					WindUI = w
					print("[case] 下载完成 " .. string.format("%.2f", tick() - t0) .. "s | " .. #res .. " 字节 (已缓存)")
					break
				else
					warn("[case] 源码编译失败:", err)
				end
			else
				warn("[case] 失败:", tostring(res))
			end
		end
	end

	if not WindUI then error("[case] WindUI 加载失败: 无缓存且下载失败") end
end

local function notify(msg, dur)
	dur = dur or 3
	pcall(function()
		if WindUI and WindUI.Notify then
			WindUI:Notify({ Title = "case", Content = msg, Duration = dur })
		else
			local h = Instance.new("Hint", WS)
			h.Text = "case " .. msg
			task.delay(dur, function() h:Destroy() end)
		end
	end)
end

local Sig, v3, st, DEVV_OK = nil, nil, nil, false
local dvv = nil
local GUIDModule = nil
local v3item = nil
local FireServer = function() end
local InvokeServer = function() end
local items = {}
local targetPlayers = {}

local function syncTargetPlayers()
	targetPlayers = {}
	for _, n in ipairs(State.BlackList) do
		table.insert(targetPlayers, n)
	end
end

pcall(function()
	dvv = require(RS.devv)
	Sig = dvv.load("Signal")
	v3 = dvv.load("v3item")
	st = dvv.load("state")
	v3item = v3
	GUIDModule = dvv.load("GUID")
	DEVV_OK = true
end)

if Sig then
	FireServer = Sig.FireServer
	InvokeServer = Sig.InvokeServer
end

local function GUID()
	if GUIDModule then
		local ok, g = pcall(GUIDModule)
		if ok and g then return g end
	end
	return tostring(math.random(1, 1e9)) .. tostring(tick())
end

local function refreshItems()
	if v3 and v3.inventory and v3.inventory.items then
		items = v3.inventory.items
	end
end
refreshItems()

local function getGuid(name)
	for _, v in pairs(items) do
		if v.name == name then return v.guid end
	end
	return nil
end

local function getRoot(character)
	return character and (character:FindFirstChild("HumanoidRootPart") or character:FindFirstChildWhichIsA("BasePart"))
end

local function tableFind(t, v)
	return table.find(t, v) ~= nil
end

local function purchaseItem(itemName, loc)
	local root = getRoot(LP.Character)
	if not root then return false end
	local originalCF = root.CFrame
	if loc then root.CFrame = loc end
	task.wait(0.2)
	pcall(function() InvokeServer("attemptPurchase", itemName) end)
	task.wait(0.3)
	refreshItems()
	root.CFrame = originalCF
	return getGuid(itemName) ~= nil
end

local function isPlayerProtected(player)
	if not player then return true end
	if player == LP then return true end
	if State.WhiteList_Enabled and table.find(State.WhiteList, player.Name) then
		return true
	end
	if State.BlackList_Enabled and not table.find(State.BlackList, player.Name) then
		return true
	end
	return false
end

local function createTrace(targetPos)
	pcall(function()
		local char = LP.Character
		if not char or not char.PrimaryPart then return end
		local origin = char.PrimaryPart.Position
		local mag = (targetPos - origin).Magnitude
		if mag < 0.01 then return end
		local part = Instance.new("Part")
		part.Anchored = true
		part.CanCollide = false
		part.CastShadow = false
		part.Material = Enum.Material.Neon
		part.Color = Color3.fromHSV(tick() % 1, 0.8, 1)
		part.Size = Vector3.new(0.15, 0.15, mag)
		part.CFrame = CFrame.lookAt(origin, targetPos) * CFrame.new(0, 0, -mag / 2)
		part.Parent = WS
		TS:Create(part, TweenInfo.new(0.3), { Transparency = 1, Size = Vector3.new(0, 0, mag) }):Play()
		game:GetService("Debris"):AddItem(part, 0.3)
	end)
end

local wallParams = RaycastParams.new()
wallParams.FilterType = Enum.RaycastFilterType.Exclude
wallParams.IgnoreWater = true

local function canSee(part, ignoreWall)
	if ignoreWall then return true end
	local ignore = {}
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr.Character then table.insert(ignore, plr.Character) end
	end
	wallParams.FilterDescendantsInstances = ignore
	local origin = Camera.CFrame.Position
	local ok, hit = pcall(function()
		return WS:Raycast(origin, part.Position - origin, wallParams)
	end)
	if not ok then return true end
	return hit == nil
end

local OP_Threads = {}

local function ensureFists()
	if not DEVV_OK then return end
	pcall(function()
		local eq = v3.inventory.getEquipped()
		local nm = eq and eq.name or (st and st.equipped and st.equipped[LP.UserId])
		if nm ~= "Fists" then
			local fg = getGuid("Fists")
			if fg then
				Sig.FireServer("equip", fg)
			end
		end
	end)
end

local function getTargetsOP()
	local hrp = getHRP()
	if not hrp then return {} end
	local t = {}
	for _, p in ipairs(getPlayers()) do
		if isAlive(p) then
			local skip = false
			if State.OP_SkipShield then
				local c = getChar(p)
				if c and c:FindFirstChildOfClass("ForceField") then skip = true end
			end
			if not skip and State.WhiteList_Enabled then
				if table.find(State.WhiteList, p.Name) then skip = true end
			end
			if not skip and State.BlackList_Enabled then
				if not table.find(State.BlackList, p.Name) then skip = true end
			end
			if not skip then
				local phrp = getHRP(p)
				if phrp then
					local d = (hrp.Position - phrp.Position).Magnitude
					if d <= State.OP_Range then table.insert(t, { p = p, d = d }) end
				end
			end
		end
	end
	table.sort(t, function(a, b) return a.d < b.d end)
	return t
end

local function startOnePunch()
	if not DEVV_OK then return end
	table.insert(OP_Threads, task.spawn(function()
		while State.OnePunch do
			task.wait(1)
			ensureFists()
		end
	end))
	table.insert(OP_Threads, task.spawn(function()
		while State.OnePunch do
			task.wait(State.OP_Delay)
			if State.OnePunch then
				ensureFists()
				for _, v in ipairs(getTargetsOP()) do
					pcall(function()
						Sig.FireServer("attackMeleeHit", "player", {
							hitPlayerId = v.p.UserId,
							meleeType = "meleemegapunch",
						})
					end)
				end
			end
		end
	end))
	table.insert(OP_Threads, task.spawn(function()
		while State.OnePunch do
			task.wait(0.1)
			if State.OnePunch then
				for _, v in ipairs(getTargetsOP()) do
					local hum = getHum(v.p)
					if hum and hum.Health > 0 and hum.Health < 20 then
						pcall(function() Sig.FireServer("finish", v.p) end)
					end
				end
			end
		end
	end))
end

local function stopOnePunch()
	State.OnePunch = false
	for _, t in ipairs(OP_Threads) do pcall(function() task.cancel(t) end) end
	OP_Threads = {}
end

local Aim_Conn = nil
local FOV_Circle = nil
local NameTags = {}

local function getAimPart(p)
	local c = getChar(p)
	if not c then return nil end
	return c:FindFirstChild(State.Aim_Part) or c:FindFirstChild("Head") or getHRP(p)
end

local function isEnemy(p)
	if State.Aim_Team then return true end
	if p.Team and LP.Team and p.Team == LP.Team then return false end
	return true
end

local function getClosestTargetToCenter()
	local best, bestDist = nil, State.Aim_FOV
	local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
	local camPos = Camera.CFrame.Position
	for _, p in ipairs(getPlayers()) do
		if isAlive(p) and isEnemy(p) then
			local part = getAimPart(p)
			if part then
				local far = (camPos - part.Position).Magnitude > State.Aim_Distance
				if not far and canSee(part, State.Aim_Wallbang) then
					local screenPos, onScreen = Camera:WorldToScreenPoint(part.Position)
					if onScreen then
						local dist = (Vector2.new(screenPos.X, screenPos.Y) - center).Magnitude
						if dist < bestDist then
							bestDist = dist
							best = p
						end
					end
				end
			end
		end
	end
	return best
end

local function startAim()
	if Aim_Conn then Aim_Conn:Disconnect() end
	if State.Aim_ShowFOV and not FOV_Circle then
		FOV_Circle = Drawing.new("Circle")
		FOV_Circle.Visible = true
		FOV_Circle.Color = Theme.Pink
		FOV_Circle.Thickness = 1.5
		FOV_Circle.Radius = State.Aim_FOV
		FOV_Circle.NumSides = 64
		FOV_Circle.Filled = false
	end
	Aim_Conn = Run.RenderStepped:Connect(function()
		if State.Aim then
			if FOV_Circle then
				FOV_Circle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
				FOV_Circle.Radius = State.Aim_FOV
			end
			local target = getClosestTargetToCenter()
			if target then
				local part = getAimPart(target)
				if part then
					local desired = CFrame.new(Camera.CFrame.Position, part.Position)
					if State.Aim_Smooth then
						Camera.CFrame = Camera.CFrame:Lerp(desired, State.Aim_Speed)
					else
						Camera.CFrame = desired
					end
				end
			end
		end
	end)
end

local function stopAim()
	if Aim_Conn then Aim_Conn:Disconnect() Aim_Conn = nil end
	if FOV_Circle then FOV_Circle:Remove() FOV_Circle = nil end
end

local function updateNameTags()
	for _, tag in ipairs(NameTags) do
		pcall(function() tag:Destroy() end)
	end
	NameTags = {}
	if (State.Aim or State.BulletTrack) and State.Aim_ShowName then
		for _, p in ipairs(getPlayers()) do
			if isAlive(p) then
				local c = getChar(p)
				local head = c and c:FindFirstChild("Head")
				if head then
					local bill = Instance.new("BillboardGui")
					bill.Size = UDim2.new(0, 150, 0, 30)
					bill.StudsOffset = Vector3.new(0, 2.5, 0)
					bill.AlwaysOnTop = true
					bill.Parent = head
					local lbl = Instance.new("TextLabel")
					lbl.Size = UDim2.new(1, 0, 1, 0)
					lbl.BackgroundTransparency = 1
					lbl.Text = p.Name
					lbl.Font = Enum.Font.GothamBold
					lbl.TextSize = 12
					lbl.TextColor3 = Theme.Pink
					lbl.Parent = bill
					table.insert(NameTags, bill)
				end
			end
		end
	end
end

local BT_Conn = nil
local BT_TrackedRE = {}
local BT_HookInstalled = false

local function getBTPart(p)
	local c = getChar(p)
	if not c then return nil end
	return c:FindFirstChild(State.BT_Part) or c:FindFirstChild("Head") or getHRP(p)
end

local function isBTEnemy(p)
	local enemy = true
	if not State.BT_Team then
		pcall(function()
			if p.Team and LP.Team and p.Team == LP.Team then enemy = false end
		end)
	end
	return enemy
end

local function getBTTarget()
	local best, bestDist = nil, State.BT_FOV
	local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
	local camPos = Camera.CFrame.Position
	for _, p in ipairs(getPlayers()) do
		if isAlive(p) and isBTEnemy(p) then
			local part = getBTPart(p)
			if part then
				local far = (camPos - part.Position).Magnitude > State.BT_Distance
				if not far and canSee(part, State.BT_Wallbang) then
					local screenPos, onScreen = Camera:WorldToScreenPoint(part.Position)
					if onScreen then
						local dist = (Vector2.new(screenPos.X, screenPos.Y) - center).Magnitude
						if dist < bestDist then
							bestDist = dist
							best = p
						end
					end
				end
			end
		end
	end
	return best
end

local function scanGunRemotes()
	BT_TrackedRE = {}
	pcall(function()
		for _, v in ipairs(RS:GetDescendants()) do
			if v:IsA("RemoteEvent") then
				local n = string.lower(v.Name)
				if n:find("shoot") or n:find("gun") or n:find("bullet") or n:find("fire") or n:find("weapon") or n:find("hit") then
					BT_TrackedRE[v] = true
				end
			end
		end
	end)
	return BT_TrackedRE
end

local function installBTHook()
	if BT_HookInstalled then return end
	BT_HookInstalled = true
	scanGunRemotes()
	for re, _ in pairs(BT_TrackedRE) do
		pcall(function()
			local orig = re.FireServer
			re.FireServer = function(self, ...)
				if not State.BulletTrack then return orig(self, ...) end
				local target = getBTTarget()
				if not target then return orig(self, ...) end
				local args = { ... }
				for i, a in ipairs(args) do
					if typeof(a) == "Instance" and a:IsA("Player") then
						args[i] = target
					end
				end
				pcall(function()
					local part = getBTPart(target)
					if part then
						Camera.CFrame = CFrame.new(Camera.CFrame.Position, part.Position)
					end
				end)
				return orig(self, unpack(args))
			end
		end)
	end
end

local function startBulletTrack()
	if BT_Conn then BT_Conn:Disconnect() end
	local found = scanGunRemotes()
	installBTHook()
	local count = 0
	for _ in pairs(found) do count = count + 1 end
	warn("[case 子弹追踪] 疑似枪械 RemoteEvent:", count, "个")
	notify("子弹追踪已开启, 请开枪测试 (见输出栏)", 4)
	BT_Conn = Run.RenderStepped:Connect(function()
		if State.BulletTrack then
			local target = getBTTarget()
			if target then
				local part = getBTPart(target)
				if part then
					Camera.CFrame = CFrame.new(Camera.CFrame.Position, part.Position)
				end
			end
		end
	end)
end

local function stopBulletTrack()
	if BT_Conn then BT_Conn:Disconnect() BT_Conn = nil end
end

local AutoArmor_Conn

local function startAutoArmor()
	if AutoArmor_Conn then AutoArmor_Conn:Disconnect() end
	AutoArmor_Conn = Run.Heartbeat:Connect(function()
		if State.AutoArmor then
			pcall(function()
				refreshItems()
				local function findItemOnSale(name)
					local f = WS:FindFirstChild("ItemsOnSale")
					if not f then return nil end
					for _, v in next, f:GetChildren() do
						if v.Name == name then return v end
					end
					return nil
				end
				local function autoWearVest()
					local armor = LP:GetAttribute("armor")
					if armor and armor > 0 then return false end
					local lightGuid = getGuid("Light Vest")
					if not lightGuid then
						local root = getRoot(LP.Character)
						if not root then return false end
						local item = findItemOnSale("Light Vest")
						if not item then return false end
						local part = item:IsA("BasePart") and item or item.PrimaryPart or item:FindFirstChildWhichIsA("BasePart")
						if not part then return false end
						local originalCF = root.CFrame
						root.CFrame = part.CFrame
						task.wait(0.01)
						InvokeServer("attemptPurchase", "Light Vest")
						task.wait(0.01)
						refreshItems()
						root.CFrame = originalCF
						lightGuid = getGuid("Light Vest")
						if not lightGuid then return false end
					end
					FireServer("equip", lightGuid)
					FireServer("useConsumable", lightGuid)
					FireServer("removeItem", lightGuid)
					return true
				end
				autoWearVest()
			end)
		end
	end)
	notify("自动穿甲已开启", 3)
end

local function stopAutoArmor()
	State.AutoArmor = false
	if AutoArmor_Conn then AutoArmor_Conn:Disconnect() AutoArmor_Conn = nil end
end

local Noclip_Conn

local function setNoclip(s)
	if Noclip_Conn then Noclip_Conn:Disconnect() Noclip_Conn = nil end
	if not s then
		local c = getChar()
		if c then
			for _, p in ipairs(c:GetDescendants()) do
				if p:IsA("BasePart") then p.CanCollide = true end
			end
		end
		return
	end
	Noclip_Conn = Run.Stepped:Connect(function()
		local c = getChar()
		if c then
			for _, p in ipairs(c:GetDescendants()) do
				if p:IsA("BasePart") then p.CanCollide = false end
			end
		end
	end)
end

local FlyingEnabled = false
local SpinningEnabled = false
local FlightSpeed = 50
local SpinSpeed = 5
local CurrentAO, CurrentLV, CurrentMoverAttachment
local FlightConnection
local FlightControl = { F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0 }

FlightSpeed = 250
local FlyBlock = nil
local FlyBlockFresh = false
local FlyChar, FlyHRP, FlyHum = nil, nil, nil
local FlyConn = nil
local FlyCfg = {
	VERTICAL_MIN = 12,
	VERTICAL_MAX = 400,
	START_ANGLE = 0,
	MAX_ANGLE = 48,
	BLOCK_SIZE = Vector3.new(6, 1, 6),
	BLOCK_GAP = 0.05,
	RESYNC_DISTANCE = 8,
	COLLISION_CHECK = false,
}

local FlyExcludeMode
do
	local ok, mode = pcall(function() return Enum.RaycastFilterType.Exclude end)
	FlyExcludeMode = (ok and mode) or Enum.RaycastFilterType.Blacklist
end

local function flyBindCharacter(char)
	if not char then return end
	FlyChar = char
	FlyHRP = char:WaitForChild("HumanoidRootPart", 10)
	FlyHum = char:WaitForChild("Humanoid", 10)
end

if LP.Character then flyBindCharacter(LP.Character) end
LP.CharacterAdded:Connect(function(char)
	task.wait(0.35)
	flyBindCharacter(char)
end)

local function flyEnsureBlock()
	if FlyBlock and FlyBlock.Parent then return FlyBlock end
	local b = Instance.new("Part")
	b.Name = "FlyBlock"
	b.Size = FlyCfg.BLOCK_SIZE
	b.Anchored = true
	b.CanCollide = true
	b.CastShadow = false
	b.Transparency = 1
	b.Color = Color3.new(0, 0, 0)
	b.Material = Enum.Material.SmoothPlastic
	b.TopSurface = Enum.SurfaceType.Smooth
	b.BottomSurface = Enum.SurfaceType.Smooth
	pcall(function() b.CanTouch = false end)
	pcall(function() b.CanQuery = false end)
	b.Parent = WS
	FlyBlock = b
	FlyBlockFresh = true
	return b
end

local function flyRemoveBlock()
	if FlyBlock then pcall(function() FlyBlock:Destroy() end) end
	FlyBlock = nil
	FlyBlockFresh = false
end

local FLY_KEYS = {
	[Enum.KeyCode.W] = Vector3.new(0, 0, -1),
	[Enum.KeyCode.S] = Vector3.new(0, 0, 1),
	[Enum.KeyCode.A] = Vector3.new(-1, 0, 0),
	[Enum.KeyCode.D] = Vector3.new(1, 0, 0),
	[Enum.KeyCode.Up] = Vector3.new(0, 0, -1),
	[Enum.KeyCode.Down] = Vector3.new(0, 0, 1),
	[Enum.KeyCode.Left] = Vector3.new(-1, 0, 0),
	[Enum.KeyCode.Right] = Vector3.new(1, 0, 0),
}

local function flyMoveVector()
	if FlyHum then
		local md = FlyHum.MoveDirection
		local flat = Vector3.new(md.X, 0, md.Z)
		if flat.Magnitude > 0.05 then return flat.Unit end
	end
	local x, z = 0, 0
	for key, dir in pairs(FLY_KEYS) do
		if UIS:IsKeyDown(key) then
			x = x + dir.X
			z = z + dir.Z
		end
	end
	if x == 0 and z == 0 then return Vector3.zero end
	local raw = Vector3.new(x, 0, z).Unit
	local cam = WS.CurrentCamera
	if cam then
		local cf = cam.CFrame
		local forward = Vector3.new(cf.LookVector.X, 0, cf.LookVector.Z)
		local right = Vector3.new(cf.RightVector.X, 0, cf.RightVector.Z)
		forward = forward.Magnitude > 0.01 and forward.Unit or Vector3.new(0, 0, -1)
		right = right.Magnitude > 0.01 and right.Unit or Vector3.new(1, 0, 0)
		return forward * (-raw.Z) + right * raw.X
	end
	return raw
end

local function flyPitchDeg()
	local cam = WS.CurrentCamera
	if not cam then return 0 end
	return math.deg(math.asin(math.clamp(cam.CFrame.LookVector.Y, -1, 1)))
end

local function flySpeedFromAngle(pitchDeg)
	local a = math.abs(pitchDeg)
	if a <= FlyCfg.START_ANGLE then return 0 end
	if a >= FlyCfg.MAX_ANGLE then return FlyCfg.VERTICAL_MAX end
	local span = FlyCfg.MAX_ANGLE - FlyCfg.START_ANGLE
	if span <= 0 then return FlyCfg.VERTICAL_MAX end
	return FlyCfg.VERTICAL_MIN + (FlyCfg.VERTICAL_MAX - FlyCfg.VERTICAL_MIN) * ((a - FlyCfg.START_ANGLE) / span)
end

local function flyZeroVelocity(part)
	if not pcall(function() part.AssemblyLinearVelocity = Vector3.zero end) then
		pcall(function() part.Velocity = Vector3.zero end)
	end
end

local function flyRay(fromPos, dirVec, ignoreList)
	if dirVec.Magnitude < 1e-5 then return nil end
	local params = RaycastParams.new()
	params.FilterType = FlyExcludeMode
	params.FilterDescendantsInstances = ignoreList
	params.IgnoreWater = true
	return WS:Raycast(fromPos, dirVec, params)
end

local function flyClampVertical(origin, deltaY, ignoreList)
	if not FlyCfg.COLLISION_CHECK or math.abs(deltaY) < 1e-5 or not FlyHRP then return deltaY end
	local half = FlyHRP.Size.Y * 0.5
	local yOff = (deltaY > 0) and half or -half
	local hit = flyRay(origin + Vector3.new(0, yOff, 0), Vector3.new(0, deltaY, 0), ignoreList)
	if not hit then return deltaY end
	local allow = math.max(0, hit.Distance - 0.05)
	if deltaY > 0 then return math.min(deltaY, allow) end
	return -math.min(-deltaY, allow)
end

local function flyClampHorizontal(origin, horizDelta, ignoreList)
	if not FlyCfg.COLLISION_CHECK then return horizDelta end
	local mag = horizDelta.Magnitude
	if mag < 1e-5 then return horizDelta end
	local hi = flyRay(origin + Vector3.new(0, 2.0, 0), horizDelta, ignoreList)
	local lo = flyRay(origin + Vector3.new(0, -0.6, 0), horizDelta, ignoreList)
	if hi and lo then
		local d = math.min(hi.Distance, lo.Distance) - 1.0
		if d <= 0.1 then return Vector3.zero end
		return horizDelta.Unit * math.min(mag, d)
	end
	return horizDelta
end

local function startFlying()
	if FlyingEnabled then return end
	if not LP.Character then LP.CharacterAdded:Wait() end
	flyBindCharacter(LP.Character)
	if not FlyHRP then
		notify("飞行失败: 无法获取角色", 2)
		return
	end
	FlyingEnabled = true
	if FlyConn then FlyConn:Disconnect() end
	FlyConn = Run.RenderStepped:Connect(function(dt)
		if not FlyingEnabled then return end
		if not FlyHRP or not FlyHRP.Parent or not FlyHum or FlyHum.Health <= 0 then return end
		if FlyChar ~= LP.Character then flyBindCharacter(LP.Character) end
		if FlyHum.Sit then return end
		if not WS.CurrentCamera then return end

		local b = flyEnsureBlock()
		local origin = FlyHRP.Position
		local ignore = { FlyChar, b }

		local moveDir = flyMoveVector()
		local moving = moveDir.Magnitude > 0.05

		local pitch = flyPitchDeg()
		local vSpeed = 0
		if moving then
			vSpeed = flySpeedFromAngle(pitch)
			if pitch < 0 then vSpeed = -vSpeed end
		end

		local deltaY = flyClampVertical(origin, vSpeed * dt, ignore)
		local deltaH = flyClampHorizontal(origin, moveDir * (FlightSpeed * dt), ignore)
		local newPos = origin + Vector3.new(deltaH.X, deltaY, deltaH.Z)

		local halfY = FlyHRP.Size.Y * 0.5
		local feetNow = origin.Y - halfY
		local feetNew = newPos.Y - halfY
		local topNow = b.Position.Y + b.Size.Y * 0.5

		local wantBlockY
		if FlyBlockFresh or math.abs(topNow - feetNow) > FlyCfg.RESYNC_DISTANCE then
			wantBlockY = feetNow - FlyCfg.BLOCK_GAP - b.Size.Y * 0.5
			FlyBlockFresh = false
		elseif moving then
			wantBlockY = feetNew - FlyCfg.BLOCK_GAP - b.Size.Y * 0.5
		else
			wantBlockY = b.Position.Y
		end

		b.CFrame = CFrame.new(newPos.X, wantBlockY, newPos.Z)

		if moving then
			local rot = FlyHRP.CFrame - FlyHRP.CFrame.Position
			FlyHRP.CFrame = CFrame.new(newPos) * rot
			flyZeroVelocity(FlyHRP)
		end
	end)
	notify("飞行开启 (Tp_fly): 走动 + 抬头/低头升降", 3)
end

local function stopFlying()
	FlyingEnabled = false
	if FlyConn then FlyConn:Disconnect() FlyConn = nil end
	flyRemoveBlock()
	notify("飞行关闭", 2)
end

local function getControlModule()
	local PlayerModule = LP:WaitForChild("PlayerScripts"):WaitForChild("PlayerModule")
	return require(PlayerModule:WaitForChild("ControlModule"))
end

local function setupBodyMovers(character)
	local hrp = character:WaitForChild("HumanoidRootPart")
	local humanoid = character:WaitForChild("Humanoid")
	local moverParent = WS:FindFirstChildOfClass("Terrain") or WS
	local moverAttachment = Instance.new("Attachment", hrp)
	moverAttachment.Name = "FlightAttachment"
	local alignOrientation = Instance.new("AlignOrientation")
	alignOrientation.Mode = Enum.OrientationAlignmentMode.OneAttachment
	alignOrientation.RigidityEnabled = true
	alignOrientation.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
	alignOrientation.CFrame = hrp.CFrame
	alignOrientation.Attachment0 = moverAttachment
	alignOrientation.Parent = moverParent
	local linearVelocity = Instance.new("LinearVelocity")
	linearVelocity.VectorVectorVelocity = Vector3.new(0, 0, 0)
	linearVelocity.MaxForce = 9e9
	linearVelocity.Attachment0 = moverAttachment
	linearVelocity.Parent = moverParent
	return alignOrientation, linearVelocity, humanoid, moverAttachment
end

local function getFlightVector(controlModule)
	local moveVector = controlModule:GetMoveVector()
	local cam = WS.CurrentCamera
	FlightControl.F = -moveVector.Z
	FlightControl.B = moveVector.Z
	FlightControl.L = -moveVector.X
	FlightControl.R = moveVector.X
	FlightControl.Q = moveVector.Y
	FlightControl.E = -moveVector.Y
	if UIS:IsKeyDown(Enum.KeyCode.W) then FlightControl.F = 1 end
	if UIS:IsKeyDown(Enum.KeyCode.S) then FlightControl.B = 1 end
	if UIS:IsKeyDown(Enum.KeyCode.A) then FlightControl.L = 1 end
	if UIS:IsKeyDown(Enum.KeyCode.D) then FlightControl.R = 1 end
	if UIS:IsKeyDown(Enum.KeyCode.Space) then FlightControl.Q = 1 end
	if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then FlightControl.E = 1 end
	local flightVector = (cam.CFrame.LookVector * (FlightControl.F - FlightControl.B) +
		cam.CFrame.RightVector * (FlightControl.R - FlightControl.L) +
		Vector3.new(0, 1, 0) * (FlightControl.Q - FlightControl.E))
	return flightVector.Magnitude > 0 and flightVector.Unit or flightVector
end

local function startFlyingYttrium()
	if FlyingEnabled then return end
	local character = LP.Character or LP.CharacterAdded:Wait()
	if not character then
		notify("飞行失败: 无法获取角色", 2)
		return
	end
	FlyingEnabled = true
	if CurrentAO then CurrentAO:Destroy() end
	if CurrentLV then CurrentLV:Destroy() end
	if CurrentMoverAttachment then CurrentMoverAttachment:Destroy() end
	CurrentAO, CurrentLV, _, CurrentMoverAttachment = setupBodyMovers(character)
	notify("飞行开启, 速度: " .. FlightSpeed, 2)
	local controlModule = getControlModule()
	FlightConnection = Run.Heartbeat:Connect(function()
		if not FlyingEnabled or not CurrentLV or not CurrentAO then
			if FlightConnection then
				FlightConnection:Disconnect()
				FlightConnection = nil
			end
			return
		end
		local flightVector = getFlightVector(controlModule)
		if flightVector.Magnitude > 0 then
			CurrentLV.VelocityConstraintMode = Enum.VelocityConstraintMode.Vector
			CurrentLV.VectorVelocity = flightVector * FlightSpeed
		else
			CurrentLV.VectorVelocity = Vector3.new(0, 0, 0)
		end
		if SpinningEnabled then
			local targetPart = character.Humanoid.SeatPart or character.HumanoidRootPart
			local spinCFrame = targetPart.CFrame * CFrame.Angles(0, math.rad(SpinSpeed), 0)
			CurrentAO.CFrame = spinCFrame
		else
			CurrentAO.CFrame = WS.CurrentCamera.CFrame
		end
		if character.HumanoidRootPart then
			character.Humanoid.PlatformStand = true
		end
	end)
	character.AncestryChanged:Connect(function(_, parent)
		if not parent and FlyingEnabled then
			stopFlying()
		end
	end)
end

local function stopFlyingYttrium()
	if not FlyingEnabled then return end
	FlyingEnabled = false
	SpinningEnabled = false
	FlightControl = { F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0 }
	if FlightConnection then
		FlightConnection:Disconnect()
		FlightConnection = nil
	end
	local character = LP.Character
	if character and character:FindFirstChild("Humanoid") then
		character.Humanoid.PlatformStand = false
	end
	if CurrentAO then
		CurrentAO:Destroy()
		CurrentAO = nil
	end
	if CurrentLV then
		CurrentLV:Destroy()
		CurrentLV = nil
	end
	if CurrentMoverAttachment then
		CurrentMoverAttachment:Destroy()
		CurrentMoverAttachment = nil
	end
	notify("飞行关闭", 2)
end

local LoopTP_Conn = nil

local function getBehindCFrame(targetHRP)
	local cf = targetHRP.CFrame
	local behind = cf.Position - cf.LookVector * 3 + Vector3.new(0, 3, 0)
	return CFrame.new(behind, cf.Position)
end

local function startLoopTP()
	if LoopTP_Conn then LoopTP_Conn:Disconnect() end
	LoopTP_Conn = Run.Heartbeat:Connect(function()
		if State.LoopTP then
			local target = State.SelectedPlayer
			if target and isAlive(target) then
				local hrp = getHRP()
				local tHRP = getHRP(target)
				if hrp and tHRP then
					hrp.CFrame = getBehindCFrame(tHRP)
				end
			end
		end
	end)
	notify("循环传送已开启", 3)
end

local function stopLoopTP()
	State.LoopTP = false
	if LoopTP_Conn then LoopTP_Conn:Disconnect() LoopTP_Conn = nil end
end

local Pull_Conn = nil

local function startPull()
	if Pull_Conn then Pull_Conn:Disconnect() end
	Pull_Conn = Run.Heartbeat:Connect(function()
		if State.PullPlayers then
			local hrp = getHRP()
			if hrp then
				for _, p in ipairs(getPlayers()) do
					if isAlive(p) then
						local phrp = getHRP(p)
						if phrp then
							local d = (hrp.Position - phrp.Position).Magnitude
							if d < State.Pull_Distance then
								pcall(function()
									phrp.CFrame = phrp.CFrame:Lerp(hrp.CFrame, 0.3)
								end)
							end
						end
					end
				end
			end
		end
	end)
end

local function stopPull()
	State.PullPlayers = false
	if Pull_Conn then Pull_Conn:Disconnect() Pull_Conn = nil end
end

local Spectate_Conn = nil
local OldSubject = nil

local function startSpectate()
	local target = State.SelectedPlayer
	if not target then
		notify("请先选择玩家")
		return false
	end
	if Spectate_Conn then Spectate_Conn:Disconnect() Spectate_Conn = nil end
	OldSubject = Camera.CameraSubject
	Spectate_Conn = Run.RenderStepped:Connect(function()
		if not State.Spectate then return end
		local t = State.SelectedPlayer
		local hum = t and getHum(t)
		if hum then
			Camera.CameraSubject = hum
			Camera.CameraType = Enum.CameraType.Follow
		else
			State.Spectate = false
			notify("观战目标已失效")
		end
	end)
	notify("观战中: " .. target.Name)
	return true
end

local function stopSpectate()
	State.Spectate = false
	if Spectate_Conn then Spectate_Conn:Disconnect() Spectate_Conn = nil end
	local hum = getHum()
	if hum then
		Camera.CameraSubject = hum
		Camera.CameraType = Enum.CameraType.Custom
	elseif OldSubject then
		Camera.CameraSubject = OldSubject
	end
	notify("停止观战")
end

local function saveCoord(name)
	local hrp = getHRP()
	if not hrp then notify("无角色") return end
	table.insert(State.SavedCoords, { name = name, pos = hrp.Position })
	notify("已保存: " .. name)
end

local function tpCoord(index)
	local hrp = getHRP()
	if not hrp then return end
	local c = State.SavedCoords[index]
	if c then
		hrp.CFrame = CFrame.new(c.pos)
		notify("已传送到: " .. c.name)
	end
end

local function rejoin()
	pcall(function()
		TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, LP)
	end)
end

local function serverHop()
	pcall(function()
		local url = "https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"
		local resp = HttpService:JSONDecode(game:HttpGet(url))
		local servers = {}
		for _, srv in ipairs(resp.data or {}) do
			if srv.playing < srv.maxPlayers and srv.id ~= game.JobId then
				table.insert(servers, srv)
			end
		end
		table.sort(servers, function(a, b) return a.playing < b.playing end)
		if servers[1] then
			TeleportService:TeleportToPlaceInstance(game.PlaceId, servers[1].id, LP)
		else
			notify("无可用服务器")
		end
	end)
end

local Respawn_Conn = nil

local function startAutoRespawn()
	if Respawn_Conn then Respawn_Conn:Disconnect() end
	Respawn_Conn = LP.CharacterAdded:Connect(function(char)
		task.wait(0.5)
		notify("原地复活完成")
	end)
	if not getChar() or (getHum() and getHum().Health <= 0) then
		pcall(function() LP:LoadCharacter() end)
	end
end

local function stopAutoRespawn()
	if Respawn_Conn then Respawn_Conn:Disconnect() Respawn_Conn = nil end
end

local CustomScripts = {
	{
		name = "无限血量 (Local)",
		code = [[
local hum = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if hum then
	hum.HealthChanged:Connect(function()
		if hum.Health < hum.MaxHealth then
			hum.Health = hum.MaxHealth
		end
	end)
end
]],
	},
	{
		name = "清空背包",
		code = [[
local bp = game.Players.LocalPlayer:FindFirstChild("Backpack")
if bp then bp:ClearAllChildren() end
]],
	},
	{
		name = "显示 FPS",
		code = [[
local gui = Instance.new("ScreenGui", game.Players.LocalPlayer.PlayerGui)
local lbl = Instance.new("TextLabel", gui)
lbl.Size = UDim2.new(0, 100, 0, 30)
lbl.Position = UDim2.new(0, 10, 0, 10)
lbl.TextColor3 = Color3.fromRGB(255, 105, 180)
lbl.BackgroundTransparency = 1
lbl.Font = Enum.Font.GothamBold
local last = tick()
local frames = 0
game:GetService("RunService").RenderStepped:Connect(function()
	frames = frames + 1
	if tick() - last >= 1 then
		lbl.Text = "FPS: " .. frames
		frames = 0
		last = tick()
	end
end)
]],
	},
}

local function execCustomCode(code)
	if not code or code == "" then return end
	local func, err = loadstring(code)
	if func then
		local ok, e = pcall(func)
		if ok then
			notify("执行成功")
		else
			notify("运行错误: " .. tostring(e))
		end
	else
		notify("编译错误: " .. tostring(err))
	end
end

local function execCustom(index)
	local s = CustomScripts[index]
	if s then execCustomCode(s.code) end
end

local Window = WindUI:CreateWindow({
	Title = "case | Ohio",
	Icon = "drumstick",
	Author = "case",
	Folder = "case",
	Size = UDim2.fromOffset(620, 460),
	Transparent = false,
	Theme = "Dark",
	SideBarWidth = 180,
	NewElements = true,
	User = { Enabled = true, Anonymous = false },
})
Window:SetToggleKey(Enum.KeyCode.RightShift)
print("[case] 窗口已创建")

local Tabs = {
	Combat = Window:Tab({ Title = "战斗", Icon = "swords" }),
	Aim = Window:Tab({ Title = "自瞄", Icon = "crosshair" }),
	Bullet = Window:Tab({ Title = "子弹追踪", Icon = "target" }),
	Util = Window:Tab({ Title = "通用", Icon = "wrench" }),
	Players = Window:Tab({ Title = "玩家", Icon = "users" }),
	Teleport = Window:Tab({ Title = "传送", Icon = "map-pin" }),
	Server = Window:Tab({ Title = "服务器", Icon = "globe" }),
	Custom = Window:Tab({ Title = "脚本", Icon = "code" }),
}

local function refreshPlayerLists()
	local names = getPlayerNameList()
	pcall(function() if WhiteListDropdown then WhiteListDropdown:Refresh(names) end end)
	pcall(function() if BlackListDropdown then BlackListDropdown:Refresh(names) end end)
	pcall(function() if PlayerSelectDropdown then PlayerSelectDropdown:Refresh(names) end end)
end

local CombatList = Tabs.Combat:Section({ Title = "白名单 / 黑名单" })
local WhiteListDropdown
WhiteListDropdown = CombatList:Dropdown({
	Title = "加入白名单",
	Desc = "选择玩家加入白名单",
	Values = getPlayerNameList(),
	Multi = false,
	AllowNone = true,
	Value = nil,
	Callback = function(name)
		if name and name ~= "" then
			if not table.find(State.WhiteList, name) then
				table.insert(State.WhiteList, name)
				notify("已加入白名单: " .. name)
			end
		end
	end,
})
CombatList:Button({
	Title = "清空白名单",
	Desc = "清空当前白名单",
	Callback = function()
		table.clear(State.WhiteList)
		notify("白名单已清空")
	end,
})
CombatList:Button({
	Title = "刷新玩家列表",
	Desc = "刷新白名单/黑名单下拉框",
	Callback = function()
		refreshPlayerLists()
		notify("玩家列表已刷新")
	end,
})
CombatList:Toggle({
	Title = "白名单保护",
	Desc = "白名单内玩家不被攻击",
	Value = false,
	Callback = function(v)
		State.WhiteList_Enabled = v
	end,
})
local BlackListDropdown
BlackListDropdown = CombatList:Dropdown({
	Title = "加入黑名单",
	Desc = "选择玩家加入黑名单(RPG 只打黑名单里的人)",
	Values = getPlayerNameList(),
	Multi = false,
	AllowNone = true,
	Value = nil,
	Callback = function(name)
		if name and name ~= "" then
			if not table.find(State.BlackList, name) then
				table.insert(State.BlackList, name)
				syncTargetPlayers()
				notify("已加入黑名单: " .. name)
				if State.BlackList_Enabled then State.OnePunch = true end
			end
		end
	end,
})
CombatList:Button({
	Title = "清空黑名单",
	Desc = "清空黑名单并停止攻击",
	Callback = function()
		table.clear(State.BlackList)
		syncTargetPlayers()
		State.OnePunch = false
		notify("黑名单已清空")
	end,
})
CombatList:Toggle({
	Title = "黑名单复仇",
	Desc = "开启后自动追杀黑名单玩家",
	Value = false,
	Callback = function(v)
		State.BlackList_Enabled = v
		if v then
			State.OnePunch = true
			notify("黑名单已开启, 开始追杀")
		else
			State.OnePunch = false
			notify("黑名单已关闭")
		end
	end,
})

local CombatOP = Tabs.Combat:Section({ Title = "一拳超人" })
CombatOP:Toggle({
	Title = "一拳超人",
	Desc = "贴脸秒杀范围内玩家",
	Value = false,
	Callback = function(s)
		State.OnePunch = s
		if s then
			if not DEVV_OK then notify("RS.devv 未加载, 仅 Ohio 服可用") end
			startOnePunch()
		else
			stopOnePunch()
		end
	end,
})
CombatOP:Slider({
	Title = "攻击范围",
	Desc = "一拳超人攻击范围",
	Step = 1,
	Value = { Min = 4, Max = 40, Default = 35 },
	Callback = function(v) State.OP_Range = v end,
})
CombatOP:Slider({
	Title = "攻击间隔(ms)",
	Desc = "两次攻击间隔(毫秒)",
	Step = 10,
	Value = { Min = 100, Max = 2000, Default = 700 },
	Callback = function(v) State.OP_Delay = v / 1000 end,
})
CombatOP:Toggle({
	Title = "无视护盾",
	Desc = "跳过带 ForceField 的目标",
	Value = true,
	Callback = function(s) State.OP_SkipShield = s end,
})

local CombatArmor = Tabs.Combat:Section({ Title = "自动穿甲" })
CombatArmor:Toggle({
	Title = "自动穿轻型甲",
	Desc = "自动购买并穿戴 Light Vest",
	Value = false,
	Callback = function(s)
		State.AutoArmor = s
		if s then startAutoArmor() else stopAutoArmor() end
	end,
})

local AimSection = Tabs.Aim:Section({ Title = "自瞄" })
AimSection:Toggle({
	Title = "启用自瞄",
	Desc = "锁定视角对准敌人",
	Value = false,
	Callback = function(s)
		State.Aim = s
		if s then startAim() updateNameTags() else stopAim() updateNameTags() end
	end,
})
AimSection:Dropdown({
	Title = "瞄准部位",
	Desc = "选择自瞄锁定的部位",
	Values = { "Head", "UpperTorso", "HumanoidRootPart" },
	Multi = false,
	AllowNone = false,
	Value = "Head",
	Callback = function(v) State.Aim_Part = v end,
})
AimSection:Slider({
	Title = "FOV 范围",
	Desc = "自瞄搜索范围(像素)",
	Step = 1,
	Value = { Min = 10, Max = 300, Default = 120 },
	Callback = function(v)
		State.Aim_FOV = v
		if FOV_Circle then FOV_Circle.Radius = v end
	end,
})
AimSection:Slider({
	Title = "最大距离",
	Desc = "自瞄最远距离(超出不锁)",
	Step = 10,
	Value = { Min = 50, Max = 2000, Default = 500 },
	Callback = function(v) State.Aim_Distance = v end,
})
AimSection:Toggle({
	Title = "平滑锁定",
	Desc = "平滑移动视角而不是瞬移",
	Value = true,
	Callback = function(s) State.Aim_Smooth = s end,
})
AimSection:Slider({
	Title = "平滑速度",
	Desc = "锁定平滑程度",
	Step = 1,
	Value = { Min = 1, Max = 100, Default = 15 },
	Callback = function(v) State.Aim_Speed = v / 100 end,
})
AimSection:Toggle({
	Title = "显示 FOV 圈",
	Desc = "在屏幕中心画 FOV 圆",
	Value = true,
	Callback = function(s)
		State.Aim_ShowFOV = s
		if s and State.Aim and not FOV_Circle then
			startAim()
		elseif not s and FOV_Circle then
			FOV_Circle:Remove() FOV_Circle = nil
		end
	end,
})
AimSection:Toggle({
	Title = "显示名字",
	Desc = "在玩家头上显示名字标签",
	Value = true,
	Callback = function(s) State.Aim_ShowName = s updateNameTags() end,
})
AimSection:Toggle({
	Title = "穿透墙壁",
	Desc = "隔墙也能锁(默认关)",
	Value = false,
	Callback = function(s) State.Aim_Wallbang = s end,
})
AimSection:Toggle({
	Title = "包含队友",
	Desc = "自瞄也锁定队友",
	Value = false,
	Callback = function(s) State.Aim_Team = s end,
})

local BulletSection = Tabs.Bullet:Section({ Title = "子弹追踪" })
BulletSection:Toggle({
	Title = "启用子弹追踪",
	Desc = "拦截枪械 RemoteEvent 改写目标",
	Value = false,
	Callback = function(s)
		State.BulletTrack = s
		if s then startBulletTrack() else stopBulletTrack() end
	end,
})
BulletSection:Dropdown({
	Title = "追踪部位",
	Desc = "选择追踪的目标部位",
	Values = { "Head", "UpperTorso", "HumanoidRootPart" },
	Multi = false,
	AllowNone = false,
	Value = "Head",
	Callback = function(v) State.BT_Part = v end,
})
BulletSection:Slider({
	Title = "追踪 FOV",
	Desc = "追踪搜索范围(像素)",
	Step = 1,
	Value = { Min = 10, Max = 400, Default = 200 },
	Callback = function(v) State.BT_FOV = v end,
})
BulletSection:Slider({
	Title = "追踪距离",
	Desc = "追踪最远距离(超出不锁)",
	Step = 10,
	Value = { Min = 50, Max = 3000, Default = 1000 },
	Callback = function(v) State.BT_Distance = v end,
})
BulletSection:Toggle({
	Title = "穿透墙壁",
	Desc = "隔墙也能追踪(默认关)",
	Value = false,
	Callback = function(s) State.BT_Wallbang = s end,
})
BulletSection:Toggle({
	Title = "包含队友",
	Desc = "追踪也锁定队友",
	Value = false,
	Callback = function(s) State.BT_Team = s end,
})
BulletSection:Paragraph({
	Title = "说明",
	Desc = "拦截枪械 RemoteEvent 改写目标, 自动锁敌 + 转视角。开启后请开枪一次测试, 见输出栏日志。",
})
BulletSection:Button({
	Title = "重新扫描枪械事件",
	Desc = "重新扫描 RemoteEvent 列表",
	Callback = function()
		if State.BulletTrack then
			stopBulletTrack()
			BT_HookInstalled = false
		end
		startBulletTrack()
		notify("已重新扫描")
	end,
})

local UtilMove = Tabs.Util:Section({ Title = "移动" })
UtilMove:Toggle({
	Title = "飞行模式 (Tp_fly)",
	Desc = "由司空提供",
	Value = false,
	Callback = function(s)
		if s then
			startFlying()
		else
			stopFlying()
		end
	end,
})
UtilMove:Slider({
	Title = "飞行速度",
	Desc = "对应 HORIZONTAL_SPEED, 默认 250",
	Step = 5,
	Value = { Min = 1, Max = 600, Default = 250 },
	Callback = function(v)
		FlyCfg.HORIZONTAL_SPEED = v
		FlightSpeed = v
	end,
})
UtilMove:Slider({
	Title = "竖直最低速度",
	Desc = "对应 VERTICAL_MIN, 默认 12",
	Step = 1,
	Value = { Min = 1, Max = 200, Default = 12 },
	Callback = function(v) FlyCfg.VERTICAL_MIN = v end,
})
UtilMove:Slider({
	Title = "竖直最快速度",
	Desc = "对应 VERTICAL_MAX, 默认 400",
	Step = 10,
	Value = { Min = 20, Max = 1000, Default = 400 },
	Callback = function(v) FlyCfg.VERTICAL_MAX = v end,
})
UtilMove:Slider({
	Title = "启动角度",
	Desc = "对应 START_ANGLE, 默认 0",
	Step = 1,
	Value = { Min = 0, Max = 60, Default = 0 },
	Callback = function(v) FlyCfg.START_ANGLE = v end,
})
UtilMove:Slider({
	Title = "满速角度",
	Desc = "对应 MAX_ANGLE, 默认 48",
	Step = 1,
	Value = { Min = 5, Max = 89, Default = 48 },
	Callback = function(v) FlyCfg.MAX_ANGLE = v end,
})
UtilMove:Toggle({
	Title = "飞行碰撞检测",
	Desc = "开=会撞墙, 关=穿墙飞 (默认关)",
	Value = false,
	Callback = function(v) FlyCfg.COLLISION_CHECK = v end,
})
UtilMove:Toggle({
	Title = "穿墙 (Noclip)",
	Desc = "角色可穿过所有部件",
	Value = false,
	Callback = function(s) setNoclip(s) end,
})

local InfJump_Conn = nil
UtilMove:Toggle({
	Title = "无限跳跃",
	Desc = "可无限跳跃",
	Value = false,
	Callback = function(s)
		State.InfJump = s
		if InfJump_Conn then InfJump_Conn:Disconnect() InfJump_Conn = nil end
		if s then
			InfJump_Conn = UIS.JumpRequest:Connect(function()
				local hum = getHum()
				if hum then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
			end)
		end
	end,
})

local Fullbright_Conn = nil
UtilMove:Toggle({
	Title = "全亮 (Fullbright)",
	Desc = "照亮整个场景",
	Value = false,
	Callback = function(s)
		State.Fullbright = s
		if Fullbright_Conn then Fullbright_Conn:Disconnect() Fullbright_Conn = nil end
		if s then
			Fullbright_Conn = Run.RenderStepped:Connect(function()
				Lighting.Brightness = 2
				Lighting.ClockTime = 14
				Lighting.FogEnd = 100000
				Lighting.GlobalShadows = false
			end)
		else
			Lighting.Brightness = 1
			Lighting.FogEnd = 100000
			Lighting.GlobalShadows = true
		end
	end,
})

local Plat = nil
local Plat_Conn = nil
UtilMove:Toggle({
	Title = "移动平台(脚下)",
	Desc = "在脚下生成跟随移动的平台",
	Value = false,
	Callback = function(s)
		State.Platform = s
		if Plat_Conn then Plat_Conn:Disconnect() Plat_Conn = nil end
		if s then
			Plat_Conn = Run.Heartbeat:Connect(function()
				if not State.Platform then return end
				local root = getHRP()
				if root then
					if not Plat or not Plat.Parent then
						Plat = Instance.new("Part")
						Plat.Size = Vector3.new(6, 1, 6)
						Plat.Anchored = true
						Plat.CanCollide = true
						Plat.Transparency = 0.5
						Plat.Color = Theme.Pink
						Plat.Parent = WS
					end
					Plat.Position = root.Position - Vector3.new(0, 3.5, 0)
				end
			end)
		else
			if Plat then Plat:Destroy() Plat = nil end
		end
	end,
})

local TPWalk_Conn = nil
UtilMove:Toggle({
	Title = "传送行走",
	Desc = "以传送方式移动",
	Value = false,
	Callback = function(s)
		State.TPWalk = s
		if TPWalk_Conn then TPWalk_Conn:Disconnect() TPWalk_Conn = nil end
		if s then
			TPWalk_Conn = Run.Heartbeat:Connect(function()
				if not State.TPWalk then return end
				local root = getHRP()
				local hum = getHum()
				if root and hum and hum.MoveDirection.Magnitude > 0 then
					root.CFrame = root.CFrame + hum.MoveDirection.Unit * State.TPWalkSpeed
				end
			end)
		end
	end,
})
UtilMove:Slider({
	Title = "传送行走速度",
	Desc = "传送行走每步距离",
	Step = 1,
	Value = { Min = 1, Max = 20, Default = 5 },
	Callback = function(v) State.TPWalkSpeed = v end,
})

local UtilChar = Tabs.Util:Section({ Title = "角色" })
UtilChar:Slider({
	Title = "移动速度",
	Desc = "角色 WalkSpeed",
	Step = 1,
	Value = { Min = 16, Max = 200, Default = 16 },
	Callback = function(v)
		State.WalkSpeed = v
		pcall(function() getHum().WalkSpeed = v end)
	end,
})
UtilChar:Slider({
	Title = "跳跃高度",
	Desc = "角色 JumpPower",
	Step = 1,
	Value = { Min = 50, Max = 300, Default = 50 },
	Callback = function(v)
		State.JumpPower = v
		pcall(function() getHum().JumpPower = v end)
	end,
})
UtilChar:Toggle({
	Title = "自动复活",
	Desc = "死亡后自动重新加载角色",
	Value = false,
	Callback = function(s)
		if s then startAutoRespawn() else stopAutoRespawn() end
	end,
})

local UtilMisc = Tabs.Util:Section({ Title = "杂项" })
UtilMisc:Button({
	Title = "远程储物柜",
	Desc = "开启远程储物柜访问",
	Callback = function()
		pcall(function()
			local lp = Players.LocalPlayer
			local backpack = lp.PlayerGui:WaitForChild("Backpack", 5)
			if not backpack then notify("未找到 Backpack") return end
			local holder = backpack:WaitForChild("Holder", 5)
			if not holder then notify("未找到 Holder") return end
			local locker = holder:WaitForChild("Locker", 5)
			if not locker then notify("未找到 Locker") return end
			backpack:GetPropertyChangedSignal("Enabled"):Connect(function()
				if backpack.Enabled then
					locker.Visible = true
				end
			end)
			if backpack.Enabled then
				locker.Visible = true
			end
			notify("远程储物柜已启用")
		end)
	end,
})
UtilMisc:Button({
	Title = "解锁全皮肤",
	Desc = "点击一次解锁所有皮肤",
	Callback = function()
		pcall(function()
			local ReplicatedStorage = game:GetService('ReplicatedStorage')
			local skinsModule = require(ReplicatedStorage.devv.client.Helpers.ui.screens.CaseMenu.Skins)
			local load = require(ReplicatedStorage.devv).load
			local state = load("state")
			hookfunction(skinsModule.AttemptEquip, function(self, itemName, skinName)
				local skinToEquip = skinName
				if self:IsSkinEquipped(itemName, skinName) then
					skinToEquip = nil
				end
				state.data.equippedSkins[itemName] = skinToEquip
				load("v3item").inventory.unequipAll()
				load("v3item").inventory.skinUpdate(itemName, skinToEquip)
				self:_setEquipped(itemName, skinToEquip)
				return true
			end)
			local skins = load("skins")
			for skinName in pairs(skins.skinData) do
				for _, itemName in pairs(skins.compatabilities.Generic) do
					state.data.ownedSkins[itemName] = state.data.ownedSkins[itemName] or {}
					state.data.ownedSkins[itemName][skinName] = 1
				end
			end
			notify("全皮肤已解锁")
		end)
	end,
})

local PlayerSection = Tabs.Players:Section({ Title = "操作" })
local PlayerSelectDropdown
PlayerSelectDropdown = PlayerSection:Dropdown({
	Title = "选择玩家",
	Desc = "选择要操作的玩家",
	Values = getPlayerNameList(),
	Multi = false,
	AllowNone = true,
	Value = nil,
	Callback = function(name)
		if name and name ~= "" then
			local target = Players:FindFirstChild(name)
			if target and target ~= LP then
				State.SelectedPlayer = target
				notify("已选择: " .. name)
			end
		end
	end,
})
PlayerSection:Button({
	Title = "刷新玩家下拉框",
	Desc = "刷新玩家列表",
	Callback = function()
		refreshPlayerLists()
		notify("玩家列表已刷新")
	end,
})
PlayerSection:Button({
	Title = "传送到玩家",
	Desc = "传送到选中玩家身边",
	Callback = function()
		local t = State.SelectedPlayer
		if not t or not isAlive(t) then notify("请先选择玩家") return end
		local hrp = getHRP() local tHRP = getHRP(t)
		if hrp and tHRP then hrp.CFrame = tHRP.CFrame + Vector3.new(0, 3, 0) end
	end,
})
PlayerSection:Button({
	Title = "把玩家拉过来",
	Desc = "把选中玩家拉到自己身边",
	Callback = function()
		local t = State.SelectedPlayer
		if not t or not isAlive(t) then notify("请先选择玩家") return end
		local tHRP = getHRP(t)
		if tHRP then tHRP.CFrame = getHRP().CFrame + Vector3.new(0, 3, 0) end
	end,
})
PlayerSection:Button({
	Title = "到玩家身后",
	Desc = "传送到选中玩家身后",
	Callback = function()
		local t = State.SelectedPlayer
		if not t or not isAlive(t) then notify("请先选择玩家") return end
		local hrp = getHRP() local tHRP = getHRP(t)
		if hrp and tHRP then hrp.CFrame = getBehindCFrame(tHRP) end
	end,
})
PlayerSection:Toggle({
	Title = "循环传送(身后)",
	Desc = "持续跟随选中玩家",
	Value = false,
	Callback = function(s)
		State.LoopTP = s
		if s then startLoopTP() else stopLoopTP() end
	end,
})
PlayerSection:Slider({
	Title = "循环间隔(秒)",
	Desc = "循环传送间隔",
	Step = 1,
	Value = { Min = 1, Max = 10, Default = 1 },
	Callback = function(v) State.LoopTP_Delay = v end,
})
PlayerSection:Toggle({
	Title = "拉近范围内玩家",
	Desc = "把范围内玩家吸到自己身边",
	Value = false,
	Callback = function(s)
		if s then startPull() else stopPull() end
	end,
})
PlayerSection:Slider({
	Title = "拉近距离",
	Desc = "拉近判定半径",
	Step = 1,
	Value = { Min = 5, Max = 50, Default = 10 },
	Callback = function(v) State.Pull_Distance = v end,
})

PlayerSection:Toggle({
	Title = "观战选中玩家",
	Desc = "镜头跟住选中玩家(目标死了自动退出)",
	Value = false,
	Callback = function(s)
		if s then
			State.Spectate = true
			if not startSpectate() then State.Spectate = false end
		else
			stopSpectate()
		end
	end,
})

local TeleportSection = Tabs.Teleport:Section({ Title = "坐标保存" })
TeleportSection:Button({
	Title = "保存当前坐标",
	Desc = "保存当前角色位置",
	Callback = function()
		saveCoord("坐标" .. #State.SavedCoords + 1)
	end,
})
TeleportSection:Button({
	Title = "坐标1",
	Desc = "传送到已保存的坐标1",
	Callback = function() tpCoord(1) end,
})
TeleportSection:Button({
	Title = "坐标2",
	Desc = "传送到已保存的坐标2",
	Callback = function() tpCoord(2) end,
})
TeleportSection:Button({
	Title = "坐标3",
	Desc = "传送到已保存的坐标3",
	Callback = function() tpCoord(3) end,
})
TeleportSection:Button({
	Title = "清空坐标",
	Desc = "清空已保存的坐标",
	Callback = function()
		table.clear(State.SavedCoords)
		notify("已清空坐标")
	end,
})
TeleportSection:Button({
	Title = "回传出生点",
	Desc = "传送到地图出生点",
	Callback = function()
		pcall(function()
			local sp = WS.Spawns:FindFirstChildOfClass("SpawnLocation")
			if sp then getHRP().CFrame = sp.CFrame + Vector3.new(0, 3, 0) end
		end)
	end,
})

local ServerSection = Tabs.Server:Section({ Title = "服务器" })
ServerSection:Button({
	Title = "重新加入本服",
	Desc = "重新连接到当前服务器",
	Callback = function() rejoin() end,
})
ServerSection:Button({
	Title = "换服务器(低人数)",
	Desc = "切换到人少的公共服务器",
	Callback = function() serverHop() end,
})

local CustomSection = Tabs.Custom:Section({ Title = "预设脚本" })
CustomSection:Button({
	Title = "无限血量 (Local)",
	Desc = "本地锁定满血",
	Callback = function() execCustom(1) end,
})
CustomSection:Button({
	Title = "清空背包",
	Desc = "清空本地背包",
	Callback = function() execCustom(2) end,
})
CustomSection:Button({
	Title = "显示 FPS",
	Desc = "在屏幕左上角显示 FPS",
	Callback = function() execCustom(3) end,
})

LP.CharacterAdded:Connect(function()
	if FlyingEnabled then
		task.wait(0.5)
		stopFlying()
		task.wait(0.1)
		startFlying()
	end
end)

notify("case | Tp_fly  飞行已接入")
print("case | WindUI 版加载完成 (Yttrium 飞行已缝合)")

local rpgAttackEnabled = false
local rpgAttackDistance = 100
local rpgMinHealth = 0.3
local autoBuyRPGAmmo = false
local rpgBuyTimer = 0
local rpgBuyInterval = 10
local hitPart = "Head"
local rpgBuyLocation = CFrame.new(
	1145.82153, 25.5613174, -1322.12683,
	-0.173624277, 0, -0.984811902,
	0, 1, 0,
	0.984811902, 0, -0.173624277
)

local function ensureRPG()
	local hasRPG = false
	for _, v in pairs(items) do
		if v.name == "RPG" or v.name == "Trident" then
			hasRPG = true
			FireServer("equip", v.guid)
			break
		end
	end
	if not hasRPG then
		purchaseItem("RPG", rpgBuyLocation)
		task.wait(0.2)
		refreshItems()
		for _, v in pairs(items) do
			if v.name == "RPG" then
				FireServer("equip", v.guid)
				break
			end
		end
	end
end

local function startRPGAttack()
	task.spawn(function()
		ensureRPG()
		local same = { GUID() }
		while rpgAttackEnabled do
			if not LocalPlayer.Character or not getRoot(LocalPlayer.Character) then
				task.wait(0.5)
			else
				local equippedItem = v3item.inventory.getEquippedItem()
				if not equippedItem or (equippedItem.name ~= "RPG" and equippedItem.name ~= "Trident") then
					task.wait(0.1)
				else
					local equippedGUID = equippedItem.guid
					local friendIDs = {}
					for _, player in pairs(Players:GetPlayers()) do
						if player ~= LocalPlayer then
							local success, isFriend = pcall(function()
								return LocalPlayer:IsFriendsWith(player.UserId)
							end)
							if success and isFriend then
								table.insert(friendIDs, player.UserId)
							end
						end
					end
					local myRoot = getRoot(LocalPlayer.Character)
					if myRoot then
						for _, player in pairs(Players:GetPlayers()) do
							if not rpgAttackEnabled then break end
							if player ~= LocalPlayer and player.Character then
								local isTarget = false
								if #targetPlayers > 0 then
									isTarget = tableFind(targetPlayers, player.Name)
								else
									isTarget = true
								end
								if isTarget then
									local isFriend = false
									for _, friendID in pairs(friendIDs) do
										if player.UserId == friendID then
											isFriend = true
											break
										end
									end
									if not isFriend then
										local character = player.Character
										local humanoid = character:FindFirstChild("Humanoid")
										local targetPart = character:FindFirstChild(hitPart)
										local targetRoot = character:FindFirstChild("HumanoidRootPart")
										if humanoid and targetPart and targetRoot and humanoid.Health > rpgMinHealth then
											local distance = (myRoot.Position - targetRoot.Position).magnitude
											if distance <= rpgAttackDistance then
												local replicateArgs = { equippedGUID }
												local projectileData = { { same[1], targetPart.CFrame } }
												replicateArgs[2] = projectileData
												replicateArgs[3] = "semi"
												FireServer("replicateProjectiles", unpack(replicateArgs))
												local rocketArgs = { same[1], GUID(), targetPart.Position }
												for i = 1, 5 do
													FireServer("rocketHit", unpack(rocketArgs))
												end
												FireServer("reload", equippedGUID)
											end
										end
									end
								end
							end
						end
					end
				end
			end
			task.wait(0.1)
		end
	end)
end

task.spawn(function()
	while true do
		task.wait(1)
		if autoBuyRPGAmmo and rpgAttackEnabled then
			rpgBuyTimer = rpgBuyTimer + 1
			if rpgBuyTimer >= rpgBuyInterval then
				rpgBuyTimer = 0
				local equippedItem = v3item.inventory.getEquippedItem()
				if equippedItem and (equippedItem.name == "RPG" or equippedItem.name == "Trident") then
					local root = getRoot(LocalPlayer.Character)
					if root then
						local originalCF = root.CFrame
						root.CFrame = rpgBuyLocation
						local startTime = tick()
						while tick() - startTime < 3 do
							InvokeServer("attemptPurchaseAmmo", equippedItem.name)
							task.wait(0.03)
						end
						root.CFrame = originalCF
					end
				end
			end
		else
			rpgBuyTimer = 0
		end
	end
end)

local selectedWeapon = "Gun Kill"
local selectedGun = "Raygun"
local gunKillAutoTP = false
local forceEquipEnabled = false
local gunRapidFire = false
local autoBuyGunAmmo = false
local gunBuyTimer = 0
local gunBuyInterval = 10
local BUY_AMMO_COUNT = 10
local gunBuyLocations = {
	["Raygun"] = CFrame.new(147.022064, -98.0489502, -529.441406, 0, 0, 1, 0, 1, -0, -1, 0, 0),
	["M4A1"] = CFrame.new(603.467651, 25.6628113, -922.04425, 1, 0, 0, 0, 1, 0, 0, 0, 1),
	["AK47"] = CFrame.new(1628.71704, 6.15060806, -620.919617, 0.087131381, -0, -0.996196866, 0, 1, -0, 0.996196866, 0, 0.087131381),
}

local function prepareGunKillWeapon()
	local hasGun = false
	for _, v in pairs(items) do
		if v.name == selectedGun then
			hasGun = true
			FireServer("equip", v.guid)
			break
		end
	end
	if not hasGun then
		local buyLoc = gunBuyLocations[selectedGun]
		local root = getRoot(LocalPlayer.Character)
		if root and buyLoc then
			local originalCF = root.CFrame
			root.CFrame = buyLoc
			task.wait(0.5)
			InvokeServer("attemptPurchase", selectedGun)
			task.wait(0.3)
			refreshItems()
			for _, v in pairs(items) do
				if v.name == selectedGun then
					FireServer("equip", v.guid)
					break
				end
			end
			root.CFrame = originalCF
		end
	end
end

local function gunKillAttack(target)
	local targetPos = target.Position
	local player = Players:GetPlayerFromCharacter(target.Parent)
	if not player or player == LocalPlayer then return end
	if isPlayerProtected(player) then return end
	if forceEquipEnabled then
		local guid = getGuid(selectedGun)
		if guid then
			pcall(function()
				v3item.inventory.setEquipped(guid)
			end)
			task.wait(0.01)
		end
	end
	local item = v3item.inventory.getEquippedItem()
	if not item or item.type ~= "Gun" then return end
	if not gunRapidFire and item.ammoManager and item.ammoManager.ammo <= 0 then
		FireServer("reload", item.guid)
		return
	end
	local g = GUID()
	createTrace(targetPos)
	FireServer("replicateProjectiles", item.guid, {
		{ g, target.CFrame }
	}, item.firemode)
	FireServer("projectileHit", g, "player", {
		hitSize = target.Size,
		hitPart = target,
		pos = targetPos,
		hitPlayerId = player.UserId
	})
	if item.ammoManager then
		item.ammoManager.ammo = item.ammoManager.ammo - 1
	end
end

task.spawn(function()
	while true do
		task.wait(1)
		if autoBuyGunAmmo and selectedWeapon == "Gun Kill" and selectedGun ~= "Raygun" then
			gunBuyTimer = gunBuyTimer + 1
			if gunBuyTimer >= gunBuyInterval then
				gunBuyTimer = 0
				local item = v3item.inventory.getEquippedItem()
				if item and item.type == "Gun" and item.name == selectedGun then
					local buyLoc = gunBuyLocations[selectedGun]
					local root = getRoot(LocalPlayer.Character)
					if root and buyLoc then
						local originalCF = root.CFrame
						root.CFrame = buyLoc
						task.wait(0.2)
						for i = 1, BUY_AMMO_COUNT do
							InvokeServer("attemptPurchaseAmmo", item.name)
							task.wait(0.05)
						end
						root.CFrame = originalCF
					end
				end
			end
		else
			gunBuyTimer = 0
		end
	end
end)

local skinsec = "Sparkler"
local autoskin = false
local skinMap = {
	["烟火"] = "Sparkler", ["虚空"] = "Void", ["纯金"] = "Solid Gold", ["暗物质"] = "Dark Matter",
	["反物质"] = "Anti Matter", ["神秘"] = "Hystic", ["虚空神秘"] = "Void Mystic", ["战术"] = "Tactical",
	["纯金战术"] = "Solid Gold Tactical", ["白未来"] = "Future White", ["黑未来"] = "Future Black",
	["圣诞未来"] = "Christmas Future", ["礼物包装"] = "Gift Wrapped", ["猩红"] = "Crimson Blood",
	["收割者"] = "Reaper", ["虚空收割者"] = "Void Reaper", ["圣诞玩具"] = "Christmas Toy",
	["荒地"] = "Wasteland", ["隐形"] = "Invisible", ["像素"] = "Pixel", ["钻石像素"] = "Diamond Pixel",
	["黄金零下"] = "Frozen-Gold", ["绿水晶"] = "Atomic Nature", ["生物"] = "Biohazard",
	["樱花"] = "Sakura", ["精英"] = "Elite", ["黑樱花"] = "Death Blossom-Gold",
	["彩虹激光"] = "Rainbowlaser", ["蓝水晶"] = "Atomic Water", ["紫水晶"] = "Atomic Amethyst",
	["红水晶"] = "Atomic Flame", ["零下"] = "Sub-Zero", ["虚空射线"] = "Void-Ray",
	["冰冻钻石"] = "Frozen Diamond", ["虚空梦魇"] = "Void Nightmare", ["金雪"] = "Golden Snow",
	["爱国者"] = "Patriot", ["MM2"] = "MM2 Barrett", ["声望"] = "Prestige Barnett",
	["酷化"] = "Skin Walter", ["蒸汽"] = "Steampunk", ["海盗"] = "Pirate", ["玫瑰"] = "Rose",
	["黑玫瑰"] = "Black Rose", ["激光"] = "Hyperlaser", ["烟花"] = "Firework",
	["诅咒背瓜"] = "Cursed Pumpkin", ["大炮"] = "Cannon", ["财富"] = "Firework",
	["黄金大炮"] = "Gold Cannon", ["四叶草"] = "Lucky Clover", ["自由"] = "Freedom",
	["黑曜石"] = "Obsidian", ["赛博朋克"] = "Cyberpunk",
}

local function applySkinToGuns()
	if not autoskin then return end
	pcall(function()
		local it = require(RS.devv).load("v3item").inventory
		local b1 = require(RS.devv).load('v3item').inventory.items
		for i, item in next, b1 do
			if item.type == "Gun" then
				it.skinUpdate(item.name, skinsec)
			end
		end
	end)
end

local function enableBlackRoseBalloon()
	pcall(function()
		for _, v in pairs(getgc(true)) do
			if type(v) == "table" and rawget(v, "name") == "Balloon" and rawget(v, "holdableType") == "Balloon" then
				v.name, v.cost, v.unpurchasable, v.multiplier, v.movespeedAdd, v.cannotDiscard = "Black Rose", 200, true, 0.75, 12, true
				if v.TPSOffsets then v.TPSOffsets.hold = CFrame.new(0, 0.5, 0) end
				if v.viewportOffsets and v.viewportOffsets.hotbar then v.viewportOffsets.hotbar.dist = 3 end
				v.canDrop, v.dropCooldown, v.craft = nil
				break
			end
		end
		for _, item in pairs(require(RS.devv.client.Objects.v3item.modules.inventory).items) do
			if item.name == "Black Rose" then
				for _, btn in pairs({ item.button, item.backpackButton }) do
					if btn and btn.resetModelSkin then btn:resetModelSkin() end
				end
			end
		end
	end)
end

local function enableDollarBalloon()
	pcall(function()
		for _, v in pairs(getgc(true)) do
			if type(v) == "table" and rawget(v, "name") == "Balloon" and rawget(v, "holdableType") == "Balloon" then
				v.name, v.cost, v.unpurchasable, v.multiplier, v.movespeedAdd, v.cannotDiscard = "Dollar Balloon", 200, true, 0.8, 8, true
				if v.TPSOffsets then v.TPSOffsets.hold = CFrame.new(0, 0, 0) * CFrame.Angles(0, math.pi, 0) end
				if v.viewportOffsets and v.viewportOffsets.hotbar then v.viewportOffsets.hotbar.dist = 4 end
				v.canDrop, v.dropCooldown, v.craft = nil
				break
			end
		end
		for _, item in pairs(require(RS.devv.client.Objects.v3item.modules.inventory).items) do
			if item.name == "Dollar Balloon" then
				for _, btn in pairs({ item.button, item.backpackButton }) do
					if btn and btn.resetModelSkin then btn:resetModelSkin() end
				end
			end
		end
	end)
end

local function spawnSpiritKunai()
	pcall(function()
		local itemSystem = require(RS.devv).load("v3item")
		local inventory = itemSystem.inventory
		local spiritKunaiData = {
			name = "Spirit Kunai",
			guid = "spirit_kunai_" .. tostring(tick()),
			permanent = true,
			canDrop = true,
			dropCooldown = 120,
			holdableType = "Kunai",
			movespeedAdd = 12,
			TPSOffsets = { hold = CFrame.new(0, -0.3, 0) },
			viewportOffsets = {
				hotbar = { dist = 3, offset = CFrame.new(0, 0, 0), rotoffset = CFrame.Angles(0, 1.5707963267948966, 0) },
				ammoHUD = { dist = 2, offset = CFrame.new(-0.1, -0.2, 0), rotoffset = CFrame.Angles(0, -1.3744467859455345, 0) },
				slotButton = { dist = 1, offset = CFrame.new(-0.1, -0.2, 0), rotoffset = CFrame.Angles(0, -1.5707963267948966, 0) }
			},
			FPSOffsets = {}
		}
		if inventory.add then
			inventory.add(spiritKunaiData, false)
			if inventory.currentItemsData then table.insert(inventory.currentItemsData, spiritKunaiData) end
		end
		if inventory.rerender then inventory:rerender() end
	end)
end

spawn(function() applySkinToGuns() end)

local ExtraSec = Tabs.Custom:Section({ Title = "Ohio 隐藏功能" })

ExtraSec:Button({
	Title = "黑玫瑰气球",
	Desc = "把普通气球改写成 Black Rose(改的是内存里的道具数据)",
	Callback = function()
		enableBlackRoseBalloon()
		notify("黑玫瑰气球: 已执行(失败就不生效, 不会崩)", 4)
	end,
})
ExtraSec:Button({
	Title = "美元气球",
	Desc = "把普通气球改写成 Dollar Balloon",
	Callback = function()
		enableDollarBalloon()
		notify("美元气球: 已执行(失败就不生效, 不会崩)", 4)
	end,
})
ExtraSec:Button({
	Title = "生成精神苦无",
	Desc = "往背包塞一把 Spirit Kunai",
	Callback = function()
		spawnSpiritKunai()
		notify("精神苦无: 已执行(失败就不生效, 不会崩)", 4)
	end,
})

ExtraSec:Toggle({
	Title = "自动换皮肤",
	Desc = "给所有枪自动套用下面选的皮肤",
	Value = false,
	Callback = function(v)
		autoskin = v
		if v then
			applySkinToGuns()
			notify("自动换皮肤: " .. skinsec)
		else
			notify("自动换皮肤已关(已换上的不会自动还原)")
		end
	end,
})
ExtraSec:Dropdown({
	Title = "选择皮肤",
	Desc = "英文原名, 对应左边皮肤表",
	Values = { "Sparkler", "Void", "Solid Gold", "Dark Matter", "Anti Matter", "Hystic", "Void Mystic", "Tactical", "Solid Gold Tactical", "Future White", "Future Black", "Christmas Future", "Gift Wrapped", "Crimson Blood", "Reaper", "Void Reaper", "Christmas Toy", "Wasteland", "Invisible", "Pixel", "Diamond Pixel", "Frozen-Gold", "Atomic Nature", "Biohazard", "Sakura", "Elite", "Death Blossom-Gold", "Rainbowlaser", "Atomic Water", "Atomic Amethyst", "Atomic Flame", "Sub-Zero", "Void-Ray", "Frozen Diamond", "Void Nightmare", "Golden Snow", "Patriot", "MM2 Barrett", "Prestige Barnett", "Skin Walter", "Steampunk", "Pirate", "Rose", "Black Rose", "Hyperlaser", "Firework", "Cursed Pumpkin", "Cannon", "Gold Cannon", "Lucky Clover", "Freedom", "Obsidian", "Cyberpunk" },
	Multi = false,
	AllowNone = false,
	Value = "Sparkler",
	Callback = function(v)
		if v and v ~= "" then
			skinsec = v
			if autoskin then applySkinToGuns() end
		end
	end,
})

local RPGSec = Tabs.Combat:Section({ Title = "RPG / 枪械 (需 RS.devv)" })
RPGSec:Toggle({
	Title = "RPG 自动攻击",
	Desc = "手持 RPG/Trident 时自动开火(黑名单非空则只打黑名单)",
	Value = false,
	Callback = function(v)
		if v then
			if not DEVV_OK then
				notify("RS.devv 未加载, 仅 Ohio 服可用")
				return
			end
			rpgAttackEnabled = true
			startRPGAttack()
			notify("RPG 自动攻击已开启(会自动买 RPG)")
		else
			rpgAttackEnabled = false
			notify("RPG 自动攻击已关闭")
		end
	end,
})
RPGSec:Slider({
	Title = "RPG 攻击距离",
	Desc = "RPG 自动攻击最大距离",
	Step = 5,
	Value = { Min = 20, Max = 300, Default = 100 },
	Callback = function(v) rpgAttackDistance = v end,
})
RPGSec:Toggle({
	Title = "RPG 自动买弹",
	Desc = "每隔一段时间自动去购买 RPG 弹药",
	Value = false,
	Callback = function(v) autoBuyRPGAmmo = v end,
})
RPGSec:Dropdown({
	Title = "枪械自动买弹",
	Desc = "选一把枪, 手持它时自动补弹",
	Values = { "不补弹", "M4A1", "AK47" },
	Multi = false,
	AllowNone = false,
	Value = "不补弹",
	Callback = function(v)
		if v == "不补弹" then
			autoBuyGunAmmo = false
		else
			selectedGun = v
			autoBuyGunAmmo = true
			notify("自动补弹: " .. v)
		end
	end,
})
RPGSec:Slider({
	Title = "买弹间隔(秒)",
	Desc = "RPG / 枪械自动买弹的间隔",
	Step = 1,
	Value = { Min = 3, Max = 60, Default = 10 },
	Callback = function(v)
		rpgBuyInterval = v
		gunBuyInterval = v
	end,
})

print("[case] 全部加载完成 ✅")

local Farm = {}
Farm.s = {
	busy = false,
	mode = "Normal",
	idle = "TeTraX",
	maskBuying = false,
	gemRubbleCD = 0,
	gemRubbleTime = 30,
	itemAuraTimer = 0,
	itemAuraInterval = 0.1,
	rewardLoop = false,
	autoSellTask = nil,
	maskType = "黑色头巾",
	maskAutoBuy = true,
	gATM = false, gBank = false, gCashReg = false, gGemRubble = false,
	gTruckCash = false, gJewel = false, gSafe = false, gComponent = false,
	gSlot = false, gTreasure = false, gAirdrop = false, gWork = false,
	gStoreGems = false, gRentHouse = false, gClean = false,
	unlockAura = false, cashAura = false, itemAura = false,
	autoSell = false, autoRemove = false, autoConsume = false,
	autoCraft = false, autoClaim = false, autoStoreGems = false,
	autoVest = false, autoHeal = false, autoMask = false,
	antiSit = true, antiGrab = true, antiIdle = true,
	antiVoid = false, antiAdmin = false, bigFOV = false,
	findBlock = false, findPresent = false, findRareGem = false,
	findGem = false, findPrinter = false, findCard = false,
	pickRare = false, pickBalloon = false, stealBank = false,
	killEnabled = false, killTP = false, forceEquip = false, rapidFire = false,
}

do
	local S = Farm.s
	Farm.idleLocations = {
		["TeTraX"] = CFrame.new(1653.397216796875, -16.95315170288086, -530.3738403320312),
		["宿傩"] = CFrame.new(121.4214859008789, -42.42018508911133, -515.8087158203125),
		["位置1"] = CFrame.new(439.01190185546875, -25.120525360107422, -822.7509155273438),
		["位置2"] = CFrame.new(386.4916076660156, 3.1478753089904785, -1359.7310791015625),
		["位置3"] = CFrame.new(490.7265930175781, -22.4210262298584, -272.43170166015625),
		["位置4"] = CFrame.new(160.63153076171875, -33.42034912109375, -445.28424072265625),
		["位置5"] = CFrame.new(584.040283203125, -86.82018280029297, -724.7525634765625),
		["位置6"] = CFrame.new(1021.9706420898438, -21.59579086303711, 89.16853332519531),
		["位置7"] = CFrame.new(575.1665649414062, -40.00355911254883, -88.20584869384766),
		["位置8"] = CFrame.new(1410.9215087890625, -9.405234336853027, 714.591064453125),
		["位置9"] = CFrame.new(681.484619140625, -54.65692138671875, -337.3169860839844),
		["位置10"] = CFrame.new(-151048.375, 978.26171875, -88558.4609375),
		["位置11"] = CFrame.new(1740.39306640625, -48.72824478149414, -881.3480834960938),
	}
	Farm.getCurrentIdleCF = function()
		return Farm.idleLocations[S.idle] or Farm.idleLocations["TeTraX"]
	end

	Farm.lockpickBuyLocation = CFrame.new(659.280029, 5.50683689, -716.48999, -1.1920929e-07, 0, -1.00000012, 0, 1, 0, 1.00000012, 0, -1.1920929e-07)
	Farm.grenadeBuyLocation = CFrame.new(659.044739, 5.77163315, -706.697632, -1.1920929e-07, 0, -1.00000012, 0, 1, 0, 1.00000012, 0, -1.1920929e-07)
	Farm.bombThrowLocation = CFrame.new(1129.0994873046875, 14.843579292297363, -354.19488525390625)
	Farm.bombTargetPosition = Vector3.new(1124.0853271484, 5.3128666877747, -357.68710327148)
	Farm.afterExplosionWaitLocation = CFrame.new(1112.95142, 16.6149864, -331.99646, 0, 0, -1, 0, 1, 0, 1, 0, 0)
	Farm.bandageBuyLocation = CFrame.new(1168.04468, 25.0443974, -972.782654, 0, 0, -1, 0, 1, 0, 1, 0, 0)

	Farm.maskLocations = {
		["黑色头巾"] = CFrame.new(604.114014, 5.09485245, -1018.1275, 0, 0, 1, 0, 1, -0, -1, 0, 0),
		["红色头巾"] = CFrame.new(604.021545, 4.99485302, -1025.21191, 0, 0, 1, 0, 1, -0, -1, 0, 0),
		["蓝色头巾"] = CFrame.new(604.113892, 5.09485245, -1010.82751, 0, 0, 1, 0, 1, -0, -1, 0, 0),
		["外科医生口罩"] = CFrame.new(1160.00659, 4.65769672, -975.317871, 0, 0, -1, 0, 1, 0, 1, 0, 0),
		["面具"] = CFrame.new(1438.61926, 6.60790443, -145.005386, 1, 0, 0, 0, 1, 0, 0, 0, 1),
	}
	Farm.maskNames = {
		["黑色头巾"] = "Black Bandana",
		["红色头巾"] = "Red Bandana",
		["蓝色头巾"] = "Blue Bandana",
		["外科医生口罩"] = "Surgeon Mask",
		["面具"] = "Hockey Mask",
	}

	Farm.bankCashExists = function()
		local bank = WS:FindFirstChild("BankRobbery")
		if not bank then return false end
		local bc = bank:FindFirstChild("BankCash")
		if not bc then return false end
		local cash = bc:FindFirstChild("Cash")
		return cash and #cash:GetChildren() > 0
	end

	Farm.ensureItem = function(itemName, buyLocation)
		for _, v in pairs(items) do
			if v.name == itemName then return v.guid end
		end
		local root = getRoot(LP.Character)
		if root then
			local originalCF = root.CFrame
			root.CFrame = buyLocation
			local lockConn = Run.Heartbeat:Connect(function()
				if root and root.Parent then
					root.CFrame = buyLocation
					root.Velocity = Vector3.zero
					root.RotVelocity = Vector3.zero
				end
			end)
			task.wait(0.5)
			pcall(function() InvokeServer("attemptPurchase", itemName) end)
			task.wait(0.5)
			if lockConn then lockConn:Disconnect() end
			root.CFrame = originalCF
		end
		for _, v in pairs(items) do
			if v.name == itemName then return v.guid end
		end
		return nil
	end

	Farm.backOrIdle = function(root, originalCF)
		if not root then return end
		if S.mode == "AFK" and not S.gWork then
			root.CFrame = Farm.getCurrentIdleCF()
		elseif originalCF then
			root.CFrame = originalCF
		end
	end
end

do
	local S = Farm.s
	Farm.itemMap = {}
	local itemMap = Farm.itemMap

	local function updateItemCache()
		for k in pairs(itemMap) do itemMap[k] = nil end
		local folder = WS:FindFirstChild("Game")
		folder = folder and folder:FindFirstChild("Entities")
		folder = folder and folder:FindFirstChild("ItemPickup")
		if not folder then return end
		for _, model in pairs(folder:GetChildren()) do
			for _, v in pairs(model:GetChildren()) do
				if v:IsA("MeshPart") or v:IsA("Part") then
					local prompt = v:FindFirstChildOfClass("ProximityPrompt")
					if prompt and prompt.ObjectText then
						itemMap[prompt.ObjectText] = { part = v, prompt = prompt }
					end
				end
			end
		end
	end

	local cacheBound = false
	local function bindCache()
		if cacheBound then return true end
		local folder = WS:FindFirstChild("Game")
		folder = folder and folder:FindFirstChild("Entities")
		folder = folder and folder:FindFirstChild("ItemPickup")
		if not folder then return false end
		updateItemCache()
		folder.ChildAdded:Connect(updateItemCache)
		folder.ChildRemoved:Connect(updateItemCache)
		cacheBound = true
		return true
	end

	if not bindCache() then
		task.spawn(function()
			for _ = 1, 120 do
				task.wait(1)
				if bindCache() then break end
			end
		end)
	end

	Farm.Autoitem = function(itemName)
		local data = itemMap[itemName]
		if not data or not data.part or not data.part.Parent then return false end
		local root = getRoot(LP.Character)
		if not root then return false end
		root.CFrame = data.part.CFrame
		pcall(function()
			data.prompt.RequiresLineOfSight = false
			data.prompt.HoldDuration = 0
		end)
		pcall(function() fireproximityprompt(data.prompt) end)
		return true
	end

	Farm.buyMaskIfNeeded = function()
		if not S.maskAutoBuy or S.maskBuying then return end
		local char = LP.Character
		if not char then return end
		local maskName = Farm.maskNames[S.maskType]
		if not maskName then return end
		if char:FindFirstChild(maskName) then return end

		local existingGuid = getGuid(maskName)
		if existingGuid then
			FireServer("equip", existingGuid)
			FireServer("wearMask", existingGuid)
			return
		end

		S.maskBuying = true
		local root = getRoot(char)
		if root then
			local buyLoc = Farm.maskLocations[S.maskType]
			root.CFrame = buyLoc
			local lockConn = Run.Heartbeat:Connect(function()
				if root and root.Parent then
					root.CFrame = buyLoc
					root.Velocity = Vector3.zero
					root.RotVelocity = Vector3.zero
				end
			end)
			task.wait(0.05)
			pcall(function() InvokeServer("attemptPurchase", maskName) end)
			task.wait(0.05)
			if lockConn then lockConn:Disconnect() end
			for _, v in pairs(items) do
				if v.name == maskName then
					FireServer("equip", v.guid)
					FireServer("wearMask", v.guid)
					break
				end
			end
			if S.mode == "AFK" and not S.gWork then
				root.CFrame = Farm.getCurrentIdleCF()
			end
		end
		S.maskBuying = false
	end
end

do
	local S = Farm.s
	local unlockConn, cashConn, itemConn

	Farm.startUnlockAura = function()
		if unlockConn then unlockConn:Disconnect() end
		unlockConn = Run.Heartbeat:Connect(function()
			if not S.unlockAura then return end
			local root = getRoot(LP.Character)
			if not root then return end
			if not getGuid("Lockpick") then
				pcall(function() InvokeServer("attemptPurchase", "Lockpick") end)
			end
			local safeTypes = { "LargeSafe", "MediumSafe", "SmallSafe", "JewelSafe", "GoldJewelSafe" }
			for _, safeType in ipairs(safeTypes) do
				local game = WS:FindFirstChild("Game")
				local entities = game and game:FindFirstChild("Entities")
				local folder = entities and entities:FindFirstChild(safeType)
				if folder then
					for _, safe in pairs(folder:GetChildren()) do
						local prompt = safe:FindFirstChild("ProximityPrompt", true)
						if prompt then
							local ok, pos = pcall(function() return safe:GetPivot().Position end)
							if ok and (root.Position - pos).Magnitude <= 45 then
								pcall(function() fireproximityprompt(prompt) end)
							end
						end
					end
				end
			end
		end)
	end

	Farm.startCashAura = function()
		if cashConn then cashConn:Disconnect() end
		cashConn = Run.Heartbeat:Connect(function()
			if not S.cashAura then return end
			local root = getRoot(LP.Character)
			if not root then return end
			local game = WS:FindFirstChild("Game")
			local entities = game and game:FindFirstChild("Entities")
			local bundles = entities and entities:FindFirstChild("CashBundle")
			if not bundles then return end
			for _, cash in pairs(bundles:GetChildren()) do
				if not S.cashAura then break end
				local part = cash:FindFirstChildOfClass("Part") or cash:FindFirstChildWhichIsA("BasePart")
				if part and (root.Position - part.Position).Magnitude <= 30 then
					local cd = cash:FindFirstChildOfClass("ClickDetector") or part:FindFirstChildOfClass("ClickDetector")
					if cd then pcall(function() fireclickdetector(cd) end) end
				end
			end
		end)
	end

	Farm.valuableItems = {
		"Dark Matter Gem", "Void Gem", "Diamond Ring", "Diamond", "Rollie",
		"Watch", "Glock 18", "AR-15", "Amethyst", "Sapphire",
		"Ruby", "AK-47", "Glock",
		"Raygun", "Gold AK-47", "Gold Deagle", "AS Val", "AUG", "Acid Gun",
		"P90", "RPK", "Sawn Off", "Scar L", "Saiga 12", "Tommy Gun",
		"Double Barrel", "Deagle", "Dragunov", "Flamethrower", "M249 SAW",
		"MP7", "Minigun", "M4A1", "Barrett M107", "Gravity Gun",
		"Gold Lucky Block", "Orange Lucky Block", "Purple Lucky Block",
		"Green Lucky Block", "Red Lucky Block", "Blue Lucky Block",
		"Treasure Map", "Pearl Necklace", "Military Armory Keycard",
		"Police Armory Keycard", "Money Printer", "RPG", "Trident",
		"Gold Crown", "Gold Cup", "Heavy Vest", "Military Vest",
		"Electronics", "Weapon Parts",
	}

	Farm.startItemAura = function()
		if itemConn then itemConn:Disconnect() end
		S.itemAuraTimer = 0
		itemConn = Run.Heartbeat:Connect(function(dt)
			if not S.itemAura then return end
			S.itemAuraTimer = S.itemAuraTimer + dt
			if S.itemAuraTimer < S.itemAuraInterval then return end
			S.itemAuraTimer = 0
			local root = getRoot(LP.Character)
			if not root then return end
			local game = WS:FindFirstChild("Game")
			local entities = game and game:FindFirstChild("Entities")
			local pickups = entities and entities:FindFirstChild("ItemPickup")
			if not pickups then return end
			for _, item in pairs(pickups:GetChildren()) do
				if not S.itemAura then break end
				local mainPart = item:FindFirstChildOfClass("Part") or item:FindFirstChildWhichIsA("BasePart")
				if mainPart and (root.Position - mainPart.Position).Magnitude <= 27 then
					local itemName = item:GetAttribute("itemName") or mainPart:GetAttribute("itemName")
					if itemName and table.find(Farm.valuableItems, itemName) then
						local cd = item:FindFirstChildOfClass("ClickDetector") or mainPart:FindFirstChildOfClass("ClickDetector")
						if cd then pcall(function() fireclickdetector(cd) end) end
					end
				end
			end
		end)
	end
end

do
	local S = Farm.s

	Farm.runATMPhase = function()
		local atms = WS:FindFirstChild("ATMs")
		if not atms then return end
		local root = getRoot(LP.Character)
		if not root then return end
		local originalCF = root.CFrame
		local nearestATM, nearestDist = nil, math.huge
		for _, atm in ipairs(atms:GetChildren()) do
			if atm:IsA("Model") and (atm:GetAttribute("health") or 0) ~= 0 then
				local main = atm:FindFirstChild("Main")
				if main and main:IsA("BasePart") then
					local dist = (root.Position - main.Position).Magnitude
					if dist < nearestDist then
						nearestDist = dist
						nearestATM = atm
					end
				end
			end
		end
		if not nearestATM then return end
		local main = nearestATM:FindFirstChild("Main")
		local targetPos = main.Position + Vector3.new(0, -4, 0)
		local backDir = -main.CFrame.LookVector
		local yaw = math.atan2(backDir.X, backDir.Z)
		local targetCF = CFrame.new(targetPos) * CFrame.Angles(math.rad(90), 0, yaw)
		local lockConn = Run.Heartbeat:Connect(function()
			if root and root.Parent then
				root.CFrame = targetCF
				root.Velocity = Vector3.zero
				root.RotVelocity = Vector3.zero
			end
		end)
		task.wait(0.4)
		nearestATM:SetAttribute("health", 0)
		task.wait(0.9)
		if lockConn then lockConn:Disconnect() end
		Farm.backOrIdle(root, originalCF)
	end

	Farm.tryBankHeist = function()
		local bank = WS:FindFirstChild("BankRobbery")
		if not bank then return false end
		local cashFolder = bank:FindFirstChild("BankCash") and bank.BankCash:FindFirstChild("Cash")
		if not cashFolder or #cashFolder:GetChildren() == 0 then return false end

		local adjustedGrenadeLoc = CFrame.new(Farm.grenadeBuyLocation.Position + Vector3.new(0, -2, 0))
		local fragGuid = Farm.ensureItem("Frag", adjustedGrenadeLoc)
		if not fragGuid then return false end

		local root = getRoot(LP.Character)
		if not root then return false end
		local originalCF = root.CFrame

		root.CFrame = Farm.bombThrowLocation
		local throwLockConn = Run.Heartbeat:Connect(function()
			if root and root.Parent then
				root.CFrame = Farm.bombThrowLocation
				root.Velocity = Vector3.zero
				root.RotVelocity = Vector3.zero
			end
		end)

		task.wait(0.3)
		FireServer("equip", fragGuid)
		task.wait(0.1)
		local direction = (Farm.bombTargetPosition - root.Position).Unit
		FireServer("throwItem", fragGuid, direction, Farm.bombTargetPosition)
		FireServer("removeItem", fragGuid)

		task.wait(4)
		if throwLockConn then throwLockConn:Disconnect() end

		bank = WS:FindFirstChild("BankRobbery")
		if not bank then return false end
		local promptPart = bank:FindFirstChild("BankCash") and bank.BankCash:FindFirstChild("Main")
		local prompt = promptPart and promptPart:FindFirstChild("Attachment") and promptPart.Attachment:FindFirstChild("ProximityPrompt")
		if not prompt or not prompt.Enabled then return false end

		root = getRoot(LP.Character)
		if not root then return false end

		local lockPos = (Farm.afterExplosionWaitLocation * CFrame.new(0, 0, -4)).Position
		local lockCF = CFrame.new(lockPos) * CFrame.Angles(math.rad(90), 0, 0)
		root.CFrame = lockCF
		local lockConn = Run.Heartbeat:Connect(function()
			local cr = getRoot(LP.Character)
			if cr and cr.Parent then
				cr.CFrame = lockCF
				cr.Velocity = Vector3.zero
				cr.RotVelocity = Vector3.zero
			end
		end)
		task.wait(1.5)
		if lockConn then lockConn:Disconnect() end

		local collectCF = CFrame.new((bank.BankCash.Pallet.CFrame * CFrame.new(0, -2, 0)).Position) * CFrame.Angles(math.rad(90), 0, 0)
		root.CFrame = collectCF
		local collectLockConn = Run.Heartbeat:Connect(function()
			local cr = getRoot(LP.Character)
			if cr and cr.Parent then
				cr.CFrame = collectCF
				cr.Velocity = Vector3.zero
				cr.RotVelocity = Vector3.zero
			end
		end)

		local cashConnection
		cashConnection = Run.Heartbeat:Connect(function()
			if not cashFolder or not cashFolder.Parent or #cashFolder:GetChildren() == 0 then
				if cashConnection then cashConnection:Disconnect() end
				return
			end
			pcall(function() fireproximityprompt(prompt) end)
		end)

		repeat task.wait(0.2) until (not prompt.Enabled) or #cashFolder:GetChildren() == 0
		if cashConnection then cashConnection:Disconnect() end
		if collectLockConn then collectLockConn:Disconnect() end

		root = getRoot(LP.Character)
		Farm.backOrIdle(root, originalCF)
		return true
	end

	Farm.executeCashRegister = function()
		local gameRoot = WS:FindFirstChild("Game")
		local props = gameRoot and gameRoot:FindFirstChild("Props")
		local regFolder = props and props:FindFirstChild("CashRegister")
		if not regFolder then return false end
		local aliveRegs = {}
		for _, v in pairs(regFolder:GetChildren()) do
			if v:IsA("Model") and v:GetAttribute("state") ~= "destroyed" then
				table.insert(aliveRegs, v)
			end
		end
		if #aliveRegs == 0 then return false end

		local fistsGuid = getGuid("Fists")
		if not fistsGuid then return false end

		local root = getRoot(LP.Character)
		if not root then return false end

		for _, target in ipairs(aliveRegs) do
			if not S.gCashReg then break end
			local targetCFrame = target.WorldPivot * CFrame.new(0, -3, 0) * CFrame.Angles(math.rad(90), 0, 0)
			root.CFrame = targetCFrame
			FireServer("equip", fistsGuid)
			local lockConn = Run.Heartbeat:Connect(function()
				if root and root.Parent then
					root.CFrame = targetCFrame
					root.Velocity = Vector3.zero
					root.RotVelocity = Vector3.zero
				end
			end)
			local startTime = tick()
			while tick() - startTime < 1 do
				if not S.gCashReg then break end
				for _, reg in ipairs(aliveRegs) do
					local rGuid = reg:GetAttribute("guid")
					if rGuid and reg:GetAttribute("state") ~= "destroyed" and (root.Position - reg.WorldPivot.Position).Magnitude <= 30 then
						pcall(function()
							local attackName
							if Sig and Sig.FireServer then
								local ok, nameMap = pcall(function() return getupvalue(Sig.FireServer, 1) end)
								if ok and type(nameMap) == "table" then attackName = nameMap["attackMeleeHit"] end
							end
							local remoteStorage = RS.devv and RS.devv:FindFirstChild("remoteStorage")
							if not remoteStorage then return end
							local re
							if attackName and remoteStorage:FindFirstChild(tostring(attackName)) then
								re = remoteStorage[tostring(attackName)]
							else
								for _, v in pairs(remoteStorage:GetChildren()) do
									if v:IsA("RemoteEvent") and string.lower(v.Name):find("melee") then
										re = v
										break
									end
								end
							end
							if re then re:FireServer("prop", { meleeType = "meleepunch", guid = rGuid }) end
						end)
					end
				end
				task.wait(0.1)
			end
			if lockConn then lockConn:Disconnect() end
		end
		return true
	end

	Farm.tryGemRubble = function()
		local gem = WS:FindFirstChild("GemRobbery")
		local rubble = gem and gem:FindFirstChild("Rubble")
		if not rubble or not rubble:IsDescendantOf(WS) then return false end

		local tntGuid = getGuid("TNT")
		if not tntGuid then
			local root0 = getRoot(LP.Character)
			if not root0 then return false end
			local originalCF0 = root0.CFrame
			local targetCF = CFrame.new(Farm.grenadeBuyLocation.Position + Vector3.new(0, -2, 0))
			root0.CFrame = targetCF
			local lockConn0 = Run.Heartbeat:Connect(function()
				if root0 and root0.Parent then
					root0.CFrame = targetCF
					root0.Velocity = Vector3.zero
					root0.RotVelocity = Vector3.zero
				end
			end)
			task.wait(0.05)
			pcall(function() InvokeServer("attemptPurchase", "TNT") end)
			task.wait(0.05)
			if lockConn0 then lockConn0:Disconnect() end
			refreshItems()
			tntGuid = getGuid("TNT")
			root0.CFrame = originalCF0
			if not tntGuid then return false end
		end

		local root = getRoot(LP.Character)
		if not root then return false end
		local originalCF = root.CFrame

		local standCFrame = CFrame.new(1694, 22, -725)
		local throwTarget = Vector3.new(1700, 16, -721)

		root.CFrame = standCFrame
		FireServer("equip", tntGuid)
		task.wait(0.2)
		FireServer("throwItem", tntGuid, Vector3.new(5.2, 29.9, 79.3), throwTarget)
		task.wait(0.5)
		FireServer("removeItem", tntGuid)

		Farm.backOrIdle(root, originalCF)
		return true
	end
end

do
	local S = Farm.s

	Farm.runItemFindPhase = function()
		if Farm.bankCashExists() then return false end
		local did = false
		if S.findBlock then
			if Farm.Autoitem("Green Lucky Block") then did = true end
			if Farm.Autoitem("Orange Lucky Block") then did = true end
			if Farm.Autoitem("Purple Lucky Block") then did = true end
		end
		if S.findPresent then
			if Farm.Autoitem("Medium Present") then did = true end
			if Farm.Autoitem("Large Present") then did = true end
		end
		if S.findRareGem then
			if Farm.Autoitem("Diamond") then did = true end
			if Farm.Autoitem("Void Gem") then did = true end
			if Farm.Autoitem("Dark Matter Gem") then did = true end
			if Farm.Autoitem("Rollie") then did = true end
			if Farm.Autoitem("Gold Crown") then did = true end
			if Farm.Autoitem("Gold Cup") then did = true end
			if Farm.Autoitem("Pearl Necklace") then did = true end
		end
		if S.findGem then
			if Farm.Autoitem("Amethyst") then did = true end
			if Farm.Autoitem("Sapphire") then did = true end
			if Farm.Autoitem("Emerald") then did = true end
			if Farm.Autoitem("Topaz") then did = true end
			if Farm.Autoitem("Ruby") then did = true end
		end
		if S.findPrinter then
			if Farm.Autoitem("Money Printer") then did = true end
		end
		if S.findCard then
			local has = false
			for _, v in pairs(items) do
				if v.name == "Military Armory Keycard" then has = true break end
			end
			if not has then
				if Farm.Autoitem("Military Armory Keycard") then did = true end
			end
		end
		return did
	end

	Farm.runJewelPhase = function()
		local gemRobbery = WS:FindFirstChild("GemRobbery")
		if not gemRobbery then return false end
		local cases = gemRobbery:FindFirstChild("JewelryCases")
		if not cases then return false end

		local root = getRoot(LP.Character)
		if not root then return false end
		local originalCF = root.CFrame

		for _, descendant in pairs(cases:GetDescendants()) do
			if descendant:IsA("ProximityPrompt") and descendant.ActionText == "Steal" and descendant.Enabled then
				pcall(function()
					descendant.HoldDuration = 0
					descendant.RequiresLineOfSight = false
				end)
				local ok, targetPos = pcall(function() return descendant.Parent:GetPivot().Position end)
				if ok then
					root.CFrame = CFrame.new(targetPos)
					task.wait(0.1)
					pcall(function() fireproximityprompt(descendant) end)
					Farm.backOrIdle(root, originalCF)
					return true
				end
			end
		end
		return false
	end

	Farm.runSafePhase = function()
		if Farm.bankCashExists() then return false end
		local lockGuid = getGuid("Lockpick")
		if not lockGuid then
			local root0 = getRoot(LP.Character)
			if root0 then
				local originalCF0 = root0.CFrame
				local targetCF = CFrame.new(Farm.lockpickBuyLocation.Position + Vector3.new(0, -2, 0))
				root0.CFrame = targetCF
				local lockConn0 = Run.Heartbeat:Connect(function()
					if root0 and root0.Parent then
						root0.CFrame = targetCF
						root0.Velocity = Vector3.zero
						root0.RotVelocity = Vector3.zero
					end
				end)
				task.wait(0.05)
				pcall(function() InvokeServer("attemptPurchase", "Lockpick") end)
				task.wait(0.05)
				if lockConn0 then lockConn0:Disconnect() end
				refreshItems()
				root0.CFrame = originalCF0
			end
			return false
		end

		local chestTypes = { "SmallSafe", "MediumSafe", "LargeSafe", "JewelSafe", "GoldJewelSafe" }
		for _, ct in pairs(chestTypes) do
			local g = WS:FindFirstChild("Game")
			local entities = g and g:FindFirstChild("Entities")
			local folder = entities and entities:FindFirstChild(ct)
			if folder then
				for _, chest in pairs(folder:GetChildren()) do
					if chest.PrimaryPart then
						local prompt = chest:FindFirstChild("ProximityPrompt", true)
						if prompt and prompt.Enabled then
							local root = getRoot(LP.Character)
							if root then
								local originalCF = root.CFrame
								local lockCF = CFrame.new(chest.PrimaryPart.Position - Vector3.new(0, 3, 0)) * CFrame.Angles(math.rad(90), 0, 0)
								root.CFrame = lockCF
								local lockConn = Run.Heartbeat:Connect(function()
									if root and root.Parent then
										root.CFrame = lockCF
										root.Velocity = Vector3.zero
										root.RotVelocity = Vector3.zero
									end
								end)
								pcall(function() fireproximityprompt(prompt) end)
								task.wait(3.0)
								if lockConn then lockConn:Disconnect() end
								Farm.backOrIdle(root, originalCF)
								return true
							end
						end
					end
				end
			end
		end
		return false
	end
end

do
	local S = Farm.s

	Farm.autoSellItems = function()
		local sold = false
		for _, v in pairs(items) do
			if (v.type == "Holdable" and v.subtype == "gem" and (v.sellPrice or 0) < 5000) or
				(v.subtype == "valuable") or
				(v.type == "Gun" and (v.cost or 0) < 3999 and v.name ~= "Raygun") then
				FireServer("equip", v.guid)
				FireServer("sellItem", v.guid)
				sold = true
			end
		end
		return sold
	end

	Farm.performCrafting = function()
		if S.autoCraft then
			pcall(function() InvokeServer("beginCraft", "RollieCraft") end)
		end
		if S.autoClaim then
			pcall(function() InvokeServer("claimCraft", "RollieCraft") end)
		end
	end

	Farm.storeGems = function()
		if S.busy then return end
		local housingPlots = WS:FindFirstChild("HousingPlots")
		if not housingPlots then return end
		refreshItems()
		for _, v in pairs(housingPlots:GetDescendants()) do
			if v:IsA("ProximityPrompt") then
				local action = v.ActionText
				if action == "Add Gem" or action == "Equip a Gem" then
					local houseid = v.Parent.Parent.Name
					local hitid = v.Parent.Name
					for _, item in pairs(items) do
						if item.name == "Diamond" or item.name == "Rollie" or item.name == "Dark Matter Gem" or
							item.name == "Diamond Ring" or item.name == "Void Gem" then
							FireServer("equip", item.guid)
							FireServer("updateGemDisplay", houseid, hitid, item.guid)
						end
					end
				end
			end
		end
	end

	Farm.autoClaimRewards = function()
		for day = 1, 12 do
			pcall(function() InvokeServer("claimDailyReward", day) end)
			task.wait(0.1)
		end
		for tier = 1, 3 do
			for level = 1, 6 do
				pcall(function() InvokeServer("claimPlaytimeReward", tier, level) end)
				task.wait(0.1)
			end
		end
	end

	Farm.startAutoRewardLoop = function()
		S.rewardLoop = true
		task.spawn(function()
			while S.rewardLoop do
				Farm.autoClaimRewards()
				task.wait(5)
			end
		end)
	end

	Farm.stopAutoRewardLoop = function()
		S.rewardLoop = false
	end
end

do
	local S = Farm.s
	local idleRef = { conn = nil }
	local antiSitRef = { conn = nil }
	local antivoidRef = { conn = nil }

	Farm.updateIdleConnection = function()
		if idleRef.conn then idleRef.conn:Disconnect() idleRef.conn = nil end
		if S.mode == "AFK" and not S.gWork then
			local targetCF = Farm.getCurrentIdleCF()
			idleRef.conn = Run.Heartbeat:Connect(function()
				if S.busy or S.maskBuying then return end
				local root = getRoot(LP.Character)
				if root and root.Parent then
					root.CFrame = targetCF
					root.Velocity = Vector3.zero
					root.RotVelocity = Vector3.zero
				end
			end)
		end
	end

	Farm.toggleAntiVoid = function(state)
		if antivoidRef.conn then antivoidRef.conn:Disconnect() antivoidRef.conn = nil end
		if state then
			local destroyHeight = WS.FallenPartsDestroyHeight
			antivoidRef.conn = Run.Stepped:Connect(function()
				local root = getRoot(LP.Character)
				if root and root.Position.Y <= destroyHeight + 25 then
					root.Velocity = root.Velocity + Vector3.new(0, 250, 0)
				end
			end)
		end
	end

	local function antiSitApply(character)
		if not S.antiSit then return end
		local hum = character:WaitForChild("Humanoid", 10)
		if not hum then return end
		pcall(function() hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false) end)
		if antiSitRef.conn then antiSitRef.conn:Disconnect() end
		antiSitRef.conn = hum:GetPropertyChangedSignal("Sit"):Connect(function()
			if hum.Sit then hum.Sit = false end
		end)
		hum.Sit = false
	end
	Farm.antiSitApply = antiSitApply

	Farm.toggleAntiSit = function(state)
		S.antiSit = state
		if state then
			if LP.Character then antiSitApply(LP.Character) end
		else
			if antiSitRef.conn then antiSitRef.conn:Disconnect() antiSitRef.conn = nil end
			local hum = getHum()
			if hum then pcall(function() hum:SetStateEnabled(Enum.HumanoidStateType.Seated, true) end) end
		end
	end

	Farm.setupAntiGrab = function()
		pcall(function()
			local GrabHandler = require(RS.devv.client.Handlers.GrabHandler)
			local oldCheckValid = GrabHandler.CheckValid
			local oldGrab = GrabHandler.Grab
			GrabHandler.CheckValid = function(self, a, b)
				if S.antiGrab and a == LP then return false end
				return oldCheckValid(self, a, b)
			end
			GrabHandler.Grab = function(self, a)
				if S.antiGrab and a == LP then return end
				return oldGrab(self, a)
			end
		end)
	end
	Farm.setupAntiGrab()
end

do
	local S = Farm.s

	task.spawn(function()
		while true do
			if S.gATM and not S.busy then
				S.busy = true
				pcall(Farm.runATMPhase)
				S.busy = false
			end
			if S.gBank and not S.busy then
				S.busy = true
				pcall(Farm.tryBankHeist)
				S.busy = false
			end
			if S.gCashReg and not S.busy then
				S.busy = true
				pcall(Farm.executeCashRegister)
				S.busy = false
			end
			if S.gGemRubble and not S.busy and (tick() - S.gemRubbleCD >= S.gemRubbleTime) then
				S.busy = true
				pcall(Farm.tryGemRubble)
				S.gemRubbleCD = tick()
				S.busy = false
			end
			task.wait(0.5)
		end
	end)

	task.spawn(function()
		while true do
			if (S.findBlock or S.findPresent or S.findRareGem or S.findGem or S.findPrinter or S.findCard)
				and not S.busy and not Farm.bankCashExists() then
				S.busy = true
				while Farm.runItemFindPhase() do task.wait() end
				S.busy = false
			end
			if S.gSafe and not S.busy and not Farm.bankCashExists() then
				S.busy = true
				while S.gSafe do
					if not Farm.runSafePhase() then break end
					task.wait(0.1)
				end
				S.busy = false
			end
			if S.gJewel and not S.busy and not Farm.bankCashExists() then
				S.busy = true
				while S.gJewel and Farm.runJewelPhase() do task.wait(0.1) end
				S.busy = false
			end
			task.wait(0.1)
		end
	end)

	task.spawn(function()
		while true do
			task.wait(0.1)
			if S.gClean and not S.busy and not Farm.bankCashExists() then
				local g = WS:FindFirstChild("Game")
				local loc = g and g:FindFirstChild("Local")
				local rubbishFolder = loc and loc:FindFirstChild("Rubbish")
				if rubbishFolder then
					for _, v in pairs(rubbishFolder:GetChildren()) do
						local guid = v:GetAttribute("guid")
						if guid then
							pcall(function() FireServer("cleanRubbish", guid) end)
							pcall(function() v:Destroy() end)
						end
					end
				end
			end
			if S.autoCraft or S.autoClaim then
				pcall(Farm.performCrafting)
			end
		end
	end)

	task.spawn(function()
		while true do
			if S.autoSell then
				pcall(Farm.autoSellItems)
			end
			task.wait(0.1)
		end
	end)

	task.spawn(function()
		while true do
			task.wait(0.5)
			if S.gTreasure and not S.busy and not Farm.bankCashExists() then
				local root = getRoot(LP.Character)
				if root then
					local eq = v3item and v3item.inventory and v3item.inventory.getEquippedItem and v3item.inventory.getEquippedItem()
					if eq and eq.name == "Treasure Map" then
						local g = WS:FindFirstChild("Game")
						local loc = g and g:FindFirstChild("Local")
						local debris = loc and loc:FindFirstChild("Debris")
						if debris then
							for _, treasure in pairs(debris:GetChildren()) do
								if treasure.Name == "TreasureMarker" then
									local lockConn
									lockConn = Run.Heartbeat:Connect(function()
										if root and root.Parent then
											root.CFrame = treasure.CFrame
											root.Velocity = Vector3.zero
											root.RotVelocity = Vector3.zero
										end
									end)
									task.wait(0.1)
									local prompt = treasure:FindFirstChild("ProximityPrompt", true)
									if prompt then pcall(function() fireproximityprompt(prompt) end) end
									if lockConn then lockConn:Disconnect() end
									task.wait(0.2)
								end
							end
						end
					end
				end
			end
		end
	end)
end

do
	local S = Farm.s

	task.spawn(function()
		while true do
			task.wait(0.2)
			if S.gAirdrop and not S.busy and not Farm.bankCashExists() then
				local root = getRoot(LP.Character)
				if root then
					local g = WS:FindFirstChild("Game")
					local airdrops = g and g:FindFirstChild("Airdrops")
					if airdrops then
						for _, airdrop in pairs(airdrops:GetChildren()) do
							local main = airdrop:FindFirstChild("Airdrop")
							local prompt = main and main:FindFirstChild("ProximityPrompt")
							if prompt then
								pcall(function()
									prompt.RequiresLineOfSight = false
									prompt.HoldDuration = 0
								end)
								local lockConn
								lockConn = Run.Heartbeat:Connect(function()
									if root and root.Parent then
										root.CFrame = main.CFrame
										root.Velocity = Vector3.zero
										root.RotVelocity = Vector3.zero
									end
								end)
								task.wait(0.1)
								for _ = 1, 3 do
									pcall(function() fireproximityprompt(prompt) end)
									task.wait(0.02)
								end
								if lockConn then lockConn:Disconnect() end
								break
							end
						end
					end
				end
			end
		end
	end)

	task.spawn(function()
		while true do
			task.wait(0.1)
			if S.gComponent and not S.busy and not Farm.bankCashExists() then
				local root = getRoot(LP.Character)
				if root then
					local g = WS:FindFirstChild("Game")
					local entities = g and g:FindFirstChild("Entities")
					local pickups = entities and entities:FindFirstChild("ItemPickup")
					if pickups then
						local targets = { "Electronics", "Weapon Parts", "Component Box" }
						for _, folder in pairs(pickups:GetChildren()) do
							for _, part in pairs(folder:GetChildren()) do
								if part:IsA("MeshPart") or part:IsA("Part") then
									local prompt = part:FindFirstChildOfClass("ProximityPrompt")
									if prompt and table.find(targets, prompt.ObjectText) then
										local targetCF = part.CFrame + Vector3.new(0, 2, 0)
										local lockConn
										lockConn = Run.Heartbeat:Connect(function()
											if root and root.Parent then
												root.CFrame = targetCF
												root.Velocity = Vector3.zero
												root.RotVelocity = Vector3.zero
											end
										end)
										pcall(function()
											prompt.RequiresLineOfSight = false
											prompt.HoldDuration = 0
										end)
										task.wait(0.05)
										pcall(function() fireproximityprompt(prompt) end)
										if lockConn then lockConn:Disconnect() end
									end
								end
							end
						end
					end
				end
			end
		end
	end)

	task.spawn(function()
		while true do
			task.wait(0.8)
			if S.gWork and not S.busy and not Farm.bankCashExists() then
				local root = getRoot(LP.Character)
				if root then
					local targetCF = CFrame.new(1432.93445 + 0.5, 1.09644604, -367.879089 - 0.5)
						* CFrame.Angles(math.rad(90), math.rad(15), math.rad(90))
					local lockConn
					lockConn = Run.Heartbeat:Connect(function()
						if root and root.Parent then
							root.CFrame = targetCF
							root.Velocity = Vector3.zero
							root.RotVelocity = Vector3.zero
						end
					end)
					local jobs = WS:FindFirstChild("Jobs")
					local shoe = jobs and jobs:FindFirstChild("ShoeStore")
					local triggers = shoe and shoe:FindFirstChild("Triggers")
					local jobTrigger = triggers and triggers:FindFirstChild("BeginJobTrigger")
					local prox = jobTrigger and jobTrigger:FindFirstChild("ProximityPrompt")
					if prox then
						pcall(function()
							prox.RequiresLineOfSight = false
							prox.HoldDuration = 0
						end)
						pcall(function() fireproximityprompt(prox) end)
					end
					local g = WS:FindFirstChild("Game")
					local loc = g and g:FindFirstChild("Local")
					local rubbish = loc and loc:FindFirstChild("Rubbish")
					if rubbish then
						for _, v in pairs(rubbish:GetChildren()) do
							local guid = v:GetAttribute("guid")
							if guid then
								pcall(function() FireServer("cleanRubbish", guid) end)
								pcall(function() v:Destroy() end)
							end
						end
					end
					task.wait(0.05)
					if lockConn then lockConn:Disconnect() end
				end
			end
		end
	end)

	task.spawn(function()
		while true do
			task.wait(3)
			if S.gStoreGems and not S.busy and not Farm.bankCashExists() then
				local function hasGem()
					for _, item in pairs(items) do
						if item.name == "Diamond" or item.name == "Rollie" or item.name == "Dark Matter Gem" or
							item.name == "Diamond Ring" or item.name == "Void Gem" then
							return true
						end
					end
					return false
				end
				if hasGem() then
					local root = getRoot(LP.Character)
					if root then
						local originalCF = root.CFrame
						local homePlot
						local housingPlots = WS:FindFirstChild("HousingPlots")
						if housingPlots then
							for _, plot in pairs(housingPlots:GetChildren()) do
								if plot:GetAttribute("Owner") == LP.UserId then
									homePlot = plot
									break
								end
							end
						end
						if homePlot then
							local doorPart = homePlot:FindFirstChild("Main") or homePlot:FindFirstChild("Door") or homePlot:FindFirstChildWhichIsA("BasePart")
							if doorPart then
								root.CFrame = doorPart.CFrame * CFrame.new(0, 2, 0)
								task.wait(0.5)
								local startTime = tick()
								local failCount = 0
								while tick() - startTime < 8 do
									if not hasGem() then break end
									if not S.busy then Farm.storeGems() end
									task.wait(0.5)
									if not hasGem() then break end
									failCount = failCount + 1
									if failCount >= 3 then break end
								end
								Farm.backOrIdle(root, originalCF)
							end
						end
					end
				end
			end
		end
	end)

	task.spawn(function()
		while true do
			if S.gTruckCash then
				local root = getRoot(LP.Character)
				if root then
					local g = WS:FindFirstChild("Game")
					local vehicles = g and g:FindFirstChild("Vehicles")
					if vehicles then
						for _, vehicle in pairs(vehicles:GetChildren()) do
							if vehicle.Name == "Armored Truck" and vehicle:FindFirstChild("TruckCash") and vehicle.PrimaryPart then
								if (root.Position - vehicle.PrimaryPart.Position).Magnitude <= 100 then
									local orig = root.CFrame
									root.CFrame = vehicle.PrimaryPart.CFrame
									local attach = vehicle.TruckCash:FindFirstChild("Main")
									attach = attach and attach:FindFirstChild("Attachment")
									local prox = attach and attach:FindFirstChild("ProximityPrompt")
									if prox then
										pcall(function()
											prox.RequiresLineOfSight = false
											prox.HoldDuration = 0
										end)
										pcall(function() fireproximityprompt(prox) end)
										task.wait(0.5)
									end
									root.CFrame = orig
								end
							end
						end
					end
				end
			end
			task.wait(1)
		end
	end)

	task.spawn(function()
		while true do
			if S.gRentHouse then
				pcall(function()
					local housingPlots = WS:FindFirstChild("HousingPlots")
					if housingPlots then
						for _, plot in ipairs(housingPlots:GetChildren()) do
							if not plot:GetAttribute("Owner") then
								InvokeServer("rentHouse", plot)
							end
						end
					end
				end)
			end
			task.wait(2)
		end
	end)
end

do
	local S = Farm.s

	task.spawn(function()
		local slotCF = CFrame.new(846.239685, 0.435377538, -919.226746,
			-0.999359787, 0.0311656408, 0.0175692085,
			0.0265386514, 0.975102663, -0.220160127,
			-0.0239932127, -0.219552919, -0.975305498) * CFrame.new(10, -1.6, -5)
		while true do
			if S.gSlot then
				local serverFurniture = WS:FindFirstChild("ServerFurniture")
				local hasSlot = false
				if serverFurniture then
					for _, furniture in pairs(serverFurniture:GetDescendants()) do
						if furniture:GetAttribute("furnitureName") == "SlotMachine" then
							hasSlot = true
							break
						end
					end
				end
				local root = getRoot(LP.Character)
				if hasSlot and root then
					root.CFrame = slotCF
					local lockConn = Run.Heartbeat:Connect(function()
						if root and root.Parent then
							root.CFrame = slotCF
							root.Velocity = Vector3.zero
							root.RotVelocity = Vector3.zero
						end
					end)
					while S.gSlot and (LP:GetAttribute("slotSpins") or 0) > 0 do
						for _, furniture in pairs(serverFurniture:GetDescendants()) do
							if furniture:GetAttribute("furnitureName") == "SlotMachine" then
								local attach = furniture:FindFirstChild("Attachment", true)
								local prox = attach and attach:FindFirstChild("ProximityPrompt")
								if prox then
									pcall(function() prox.MaxActivationDistance = 40 end)
									pcall(function() fireproximityprompt(prox) end)
								end
								break
							end
						end
						task.wait(0.5)
					end
					if lockConn then lockConn:Disconnect() end
				end
			end
			task.wait(1)
		end
	end)

	local rareTargets = {
		"Dollar Balloon", "Heart Crossbow", "Void Gem", "Diamond",
		"Nuclear Missile Launcher", "NextBot Grenade", "Rollie",
		"Gold Crown", "Dark Matter Gem", "Diamond Glock",
		"Diamond Banana Peel", "Spirit Kunai", "Kunai",
		"Purple Lucky Block", "Snowflake Balloon", "Suitcase Nuke",
		"Nuke Launcher", "Easter Basket", "Gold Cup",
		"Pearl Necklace", "Treasure Map", "Spectral Scythe",
		"Bunny Balloon", "Ghost Balloon", "Clover Balloon",
		"Bat Balloon", "Gold Clover Balloon", "Golden Rose",
		"Black Rose", "Heart Balloon", "Skull Balloon",
		"Candy Cane", "Nuke Case", "Pulse Rifle", "Trident", "El Fuego",
	}
	local balloonTargets = {
		"Bat Balloon", "Bunny Balloon", "Clover Balloon",
		"Ghost Balloon", "Gold Clover Balloon", "Heart Balloon", "Skull Balloon",
	}

	local function pickLoop(getFlag, targetList)
		return task.spawn(function()
			while true do
				task.wait(0.05)
				if getFlag() then
					local char = LP.Character
					local root = char and char:FindFirstChild("HumanoidRootPart")
					if root then
						local g = WS:FindFirstChild("Game")
						local entities = g and g:FindFirstChild("Entities")
						local pickups = entities and entities:FindFirstChild("ItemPickup")
						if pickups then
							for _, l in pairs(pickups:GetChildren()) do
								if not getFlag() then break end
								for _, v in pairs(l:GetChildren()) do
									if v:IsA("MeshPart") or v:IsA("Part") then
										local prompt = v:FindFirstChildOfClass("ProximityPrompt")
										if prompt and table.find(targetList, prompt.ObjectText) then
											root.CFrame = v.CFrame + Vector3.new(0, 2, 0)
											pcall(function()
												prompt.RequiresLineOfSight = false
												prompt.HoldDuration = 0
											end)
											task.wait(0.03)
											pcall(function() fireproximityprompt(prompt) end)
											break
										end
									end
								end
							end
						end
					end
				end
			end
		end)
	end

	pickLoop(function() return S.pickRare end, rareTargets)
	pickLoop(function() return S.pickBalloon end, balloonTargets)

	local rubbishNames = { "Topaz", "Emerald Ring", "Topaz Ring", "Amethyst Ring", "Gold Bar", "Emerald" }
	local keepThrowables = { "Ninja Star", "Tomahawk", "Frag", "Banana Peel", "TNT" }
	task.spawn(function()
		while true do
			task.wait(0.15)
			pcall(function()
				if S.autoConsume then
					for _, v in pairs(items) do
						if v.type == "Consumable" and v.subtype ~= "vest" and v.subtype ~= "food" and v.name ~= "Lockpick" then
							FireServer("equip", v.guid)
							FireServer("useConsumable", v.guid)
							FireServer("removeItem", v.guid)
						end
					end
				end
				if S.autoRemove then
					for _, v in pairs(items) do
						local remove = false
						if v.type == "Consumable" and v.subtype == "food" then remove = true end
						if v.type == "Throwable" and not table.find(keepThrowables, v.name) then remove = true end
						if table.find(rubbishNames, v.name) then remove = true end
						if remove then FireServer("removeItem", v.guid) end
					end
				end
				if S.autoMask and S.maskAutoBuy and not S.maskBuying then
					local char = LP.Character
					if char and not char:FindFirstChild(Farm.maskNames[S.maskType] or "NONE") then
						task.spawn(Farm.buyMaskIfNeeded)
					end
				end
				if S.autoVest and not S.busy then
					local armor = LP:GetAttribute("armor")
					if not armor or armor <= 0 then
						local lightGuid = getGuid("Light Vest")
						if not lightGuid then
							local root = getRoot(LP.Character)
							if root then
								S.busy = true
								local originalCF = root.CFrame
								root.CFrame = Farm.lockpickBuyLocation
								task.wait(0.05)
								pcall(function() InvokeServer("attemptPurchase", "Light Vest") end)
								task.wait(0.05)
								refreshItems()
								root.CFrame = originalCF
								S.busy = false
							end
						else
							FireServer("equip", lightGuid)
							FireServer("useConsumable", lightGuid)
							FireServer("removeItem", lightGuid)
						end
					end
				end
				if S.autoHeal and not S.busy then
					local hum = getHum()
					if hum and hum.Health > 0 and hum.Health < hum.MaxHealth then
						local bandage = getGuid("Bandage")
						if not bandage then
							local root = getRoot(LP.Character)
							if root then
								S.busy = true
								local originalCF = root.CFrame
								root.CFrame = Farm.bandageBuyLocation
								task.wait(0.05)
								pcall(function() InvokeServer("attemptPurchase", "Bandage") end)
								task.wait(0.05)
								refreshItems()
								root.CFrame = originalCF
								S.busy = false
							end
						else
							FireServer("equip", bandage)
							FireServer("useConsumable", bandage)
							FireServer("removeItem", bandage)
						end
					end
				end
			end)
		end
	end)

	task.spawn(function()
		while true do
			task.wait(1)
			if S.antiAdmin then
				for _, p in ipairs(Players:GetPlayers()) do
					if p:GetAttribute("clanId") == "6557c057b60ffcc7226f532c" then
						pcall(function() LP:Kick("[Anti Admin] Admin UserName = " .. p.Name) end)
					end
				end
			end
		end
	end)

	local fovConn
	Farm.setBigFOV = function(state)
		if fovConn then fovConn:Disconnect() fovConn = nil end
		if state then
			fovConn = Run.Heartbeat:Connect(function()
				local cam = WS.CurrentCamera
				if cam then cam.FieldOfView = 120 end
			end)
		end
	end

	task.spawn(function()
		local remoteName = "stea" .. string.char(211, 143) .. "BankCash"
		while true do
			task.wait()
			if S.stealBank then
				local bank = WS:FindFirstChild("BankRobbery")
				local cash = bank and bank:FindFirstChild("BankCash")
				local main = cash and cash:FindFirstChild("Main")
				local root = getRoot(LP.Character)
				if main and root and (root.Position - main.Position).Magnitude <= 35 then
					pcall(function() FireServer(remoteName) end)
				end
			end
		end
	end)

	LP.Idled:Connect(function()
		if S.antiIdle then
			local VirtualUser = game:GetService("VirtualUser")
			pcall(function()
				VirtualUser:Button2Down(Vector2.new(0, 0), WS.CurrentCamera.CFrame)
				task.wait(1)
				VirtualUser:Button2Up(Vector2.new(0, 0), WS.CurrentCamera.CFrame)
			end)
		end
	end)

	local function onFarmCharacter(char)
		if S.autoMask then task.spawn(Farm.buyMaskIfNeeded) end
		if S.antiSit then Farm.antiSitApply(char) end
	end
	if LP.Character then onFarmCharacter(LP.Character) end
	LP.CharacterAdded:Connect(onFarmCharacter)
	Farm.toggleAntiSit(true)
end

do
	local S = Farm.s

	FlyCfg.HORIZONTAL_SPEED = FlyCfg.HORIZONTAL_SPEED or 250
	FlightSpeed = FlyCfg.HORIZONTAL_SPEED

	local FarmIdle = Tabs.Util:Section({ Title = "农场 · 挂机 / 模式" })
	FarmIdle:Dropdown({
		Title = "农场模式",
		Desc = "AFK = 干活完自动回挂机点",
		Values = { "Normal", "AFK" },
		Multi = false,
		AllowNone = false,
		Value = "Normal",
		Callback = function(v)
			S.mode = v
			Farm.updateIdleConnection()
		end,
	})
	FarmIdle:Dropdown({
		Title = "挂机位置",
		Desc = "AFK 模式回到的坐标",
		Values = { "TeTraX", "宿傩", "位置1", "位置2", "位置3", "位置4", "位置5", "位置6", "位置7", "位置8", "位置9", "位置10", "位置11" },
		Multi = false,
		AllowNone = false,
		Value = "TeTraX",
		Callback = function(v)
			S.idle = v
			Farm.updateIdleConnection()
		end,
	})
	FarmIdle:Toggle({
		Title = "自动工作(鞋店)",
		Desc = "持续传送到鞋店打工并清理垃圾",
		Value = false,
		Callback = function(v)
			S.gWork = v
			Farm.updateIdleConnection()
		end,
	})

	local FarmRob = Tabs.Util:Section({ Title = "农场 · 自动抢劫" })
	FarmRob:Toggle({ Title = "自动摧毁ATM", Desc = "自动跑去打爆最近的 ATM", Value = false, Callback = function(v) S.gATM = v end })
	FarmRob:Toggle({ Title = "自动偷盗银行", Desc = "买手雷炸开银行并捡钱", Value = false, Callback = function(v) S.gBank = v end })
	FarmRob:Toggle({ Title = "自动收银机", Desc = "依次打爆每个收银机, 每个停 1 秒", Value = false, Callback = function(v) S.gCashReg = v end })
	FarmRob:Toggle({ Title = "自动炸珠宝店", Desc = "用 TNT 炸开宝石废墟", Value = false, Callback = function(v) S.gGemRubble = v end })
	FarmRob:Slider({
		Title = "炸珠宝店间隔(秒)",
		Desc = "两次炸废墟之间的冷却",
		Step = 5,
		Value = { Min = 5, Max = 120, Default = 30 },
		Callback = function(v) S.gemRubbleTime = v end,
	})
	FarmRob:Toggle({ Title = "自动打开保险", Desc = "自动买开锁器并打开所有保险柜", Value = false, Callback = function(v) S.gSafe = v end })
	FarmRob:Toggle({ Title = "自动珠宝店", Desc = "自动偷珠宝展示柜", Value = false, Callback = function(v) S.gJewel = v end })
	FarmRob:Toggle({ Title = "自动装甲车现金", Desc = "自动收集附近装甲车上的现金", Value = false, Callback = function(v) S.gTruckCash = v end })
	FarmRob:Toggle({ Title = "自动老虎机", Desc = "有免费转数时自动去转老虎机", Value = false, Callback = function(v) S.gSlot = v end })
	FarmRob:Toggle({ Title = "自动挖海盗宝藏", Desc = "拿藏宝图时自动去挖宝藏", Value = false, Callback = function(v) S.gTreasure = v end })
	FarmRob:Toggle({ Title = "自动领取空投", Desc = "自动传送到空投并领取", Value = false, Callback = function(v) S.gAirdrop = v end })
	FarmRob:Toggle({ Title = "自动打扫", Desc = "自动清理附近的垃圾", Value = false, Callback = function(v) S.gClean = v end })
	FarmRob:Toggle({ Title = "自动租房", Desc = "自动租下没人住的房子", Value = false, Callback = function(v) S.gRentHouse = v end })
	FarmRob:Toggle({ Title = "银行现金秒偷", Desc = "靠近银行现金时持续触发偷钱(实验, 默认关)", Value = false, Callback = function(v) S.stealBank = v end })

	local FarmAura = Tabs.Util:Section({ Title = "农场 · 光环与拾取" })
	FarmAura:Toggle({
		Title = "开锁光环",
		Desc = "自动开附近所有保险柜",
		Value = false,
		Callback = function(v)
			S.unlockAura = v
			if v then Farm.startUnlockAura() end
		end,
	})
	FarmAura:Toggle({
		Title = "现金光环",
		Desc = "自动点附近掉落的现金",
		Value = false,
		Callback = function(v)
			S.cashAura = v
			if v then Farm.startCashAura() end
		end,
	})
	FarmAura:Toggle({
		Title = "物品光环",
		Desc = "自动捡附近值钱的东西",
		Value = false,
		Callback = function(v)
			S.itemAura = v
			if v then Farm.startItemAura() end
		end,
	})
	FarmAura:Toggle({ Title = "自动捡材料", Desc = "自动去捡 Electronics / Weapon Parts / Component Box", Value = false, Callback = function(v) S.gComponent = v end })
	FarmAura:Toggle({ Title = "自动捡最稀有物品", Desc = "看到稀有物品直接传过去捡", Value = false, Callback = function(v) S.pickRare = v end })
	FarmAura:Toggle({ Title = "自动捡气球", Desc = "自动捡各种气球", Value = false, Callback = function(v) S.pickBalloon = v end })

	local FarmFind = Tabs.Util:Section({ Title = "农场 · 找物品" })
	FarmFind:Toggle({ Title = "自动寻找幸运方块", Desc = "找绿/橙/紫幸运方块", Value = false, Callback = function(v) S.findBlock = v end })
	FarmFind:Toggle({ Title = "自动寻找礼物", Desc = "找中/大型礼物", Value = false, Callback = function(v) S.findPresent = v end })
	FarmFind:Toggle({ Title = "自动寻找稀有宝石", Desc = "找钻石/虚空宝石/暗物质宝石等", Value = false, Callback = function(v) S.findRareGem = v end })
	FarmFind:Toggle({ Title = "自动寻找普通宝石", Desc = "找紫晶/蓝宝石/绿宝石等", Value = false, Callback = function(v) S.findGem = v end })
	FarmFind:Toggle({ Title = "自动寻找印钞机", Desc = "找 Money Printer", Value = false, Callback = function(v) S.findPrinter = v end })
	FarmFind:Toggle({ Title = "自动寻找红卡", Desc = "找 Military Armory Keycard", Value = false, Callback = function(v) S.findCard = v end })

	local FarmBag = Tabs.Util:Section({ Title = "农场 · 背包" })
	FarmBag:Toggle({ Title = "自动售卖全部物品", Desc = "自动把不值钱的东西全卖掉", Value = false, Callback = function(v) S.autoSell = v end })
	FarmBag:Toggle({ Title = "自动移除垃圾", Desc = "自动丢弃食物/没用的投掷物/垃圾宝石", Value = false, Callback = function(v) S.autoRemove = v end })
	FarmBag:Toggle({ Title = "自动使用消耗品", Desc = "自动用掉背包里的消耗品", Value = false, Callback = function(v) S.autoConsume = v end })
	FarmBag:Toggle({ Title = "自动制作萝莉", Desc = "自动制作 Rollie", Value = false, Callback = function(v) S.autoCraft = v end })
	FarmBag:Toggle({ Title = "自动领取萝莉", Desc = "自动领取做好的 Rollie", Value = false, Callback = function(v) S.autoClaim = v end })
	FarmBag:Toggle({ Title = "自动存放珍贵宝石", Desc = "把宝石放进家里的展示柜", Value = false, Callback = function(v) S.autoStoreGems = v end })
	FarmBag:Toggle({
		Title = "自动传回家存宝石",
		Desc = "背包里有珍贵宝石时自动回家存",
		Value = false,
		Callback = function(v) S.gStoreGems = v end,
	})
	FarmBag:Toggle({
		Title = "自动领取奖励",
		Desc = "自动领每日奖励和游玩时间奖励",
		Value = false,
		Callback = function(v)
			if v then Farm.startAutoRewardLoop() else Farm.stopAutoRewardLoop() end
		end,
	})
end

do
	local S = Farm.s
	S.killDist = S.killDist or 300

	task.spawn(function()
		local prepared = false
		while true do
			task.wait(0.1)
			if S.killEnabled then
				pcall(function()
					if not prepared then
						prepareGunKillWeapon()
						prepared = true
					end
					local hrp = getHRP()
					if hrp then
						local best, bestD = nil, S.killDist
						for _, p in ipairs(getPlayers()) do
							if isAlive(p) and not isPlayerProtected(p) then
								local part = getBTPart(p)
								local phrp = getHRP(p)
								if part and phrp then
									local d = (hrp.Position - phrp.Position).Magnitude
									if d <= bestD then
										bestD = d
										best = part
									end
								end
							end
						end
						if best then
							if killTP then
								local owner = Players:GetPlayerFromCharacter(best.Parent)
								local phrp = owner and getHRP(owner)
								if phrp then hrp.CFrame = phrp.CFrame + Vector3.new(0, 3, 0) end
							end
							gunKillAttack(best)
						end
					end
				end)
			else
				prepared = false
			end
		end
	end)

	local KillSec = Tabs.Combat:Section({ Title = "枪械击杀" })
	KillSec:Toggle({
		Title = "启用枪械击杀",
		Desc = "自动用手上的枪打最近的人(自动买枪)",
		Value = false,
		Callback = function(v)
			S.killEnabled = v
			if v then notify("枪械击杀已开启") end
		end,
	})
	KillSec:Dropdown({
		Title = "选择枪械",
		Desc = "击杀用的枪(Raygun 不用买弹)",
		Values = { "Raygun", "M4A1", "AK47" },
		Multi = false,
		AllowNone = false,
		Value = "Raygun",
		Callback = function(v)
			selectedGun = v
			if v ~= "Raygun" then
				notify("已选择 " .. v .. " (可在下面开自动补弹)")
			end
		end,
	})
	KillSec:Slider({
		Title = "击杀距离",
		Desc = "超过这个距离不开枪",
		Step = 10,
		Value = { Min = 20, Max = 2000, Default = 300 },
		Callback = function(v) S.killDist = v end,
	})
	KillSec:Toggle({ Title = "传送到目标", Desc = "先传到目标身边再开枪", Value = false, Callback = function(v) killTP = v end })
	KillSec:Toggle({ Title = "强制持枪", Desc = "每次开枪前强制装备所选枪", Value = false, Callback = function(v) forceEquipEnabled = v end })
	KillSec:Toggle({ Title = "连发模式", Desc = "忽略弹量一直开火", Value = false, Callback = function(v) gunRapidFire = v end })
	KillSec:Paragraph({
		Title = "说明",
		Desc = "这个功能在原来的脚本里代码是全的, 但入口(UI)被删掉了, 现在补回来。",
	})

	local DefSec = Tabs.Combat:Section({ Title = "防护" })
	DefSec:Dropdown({
		Title = "口罩选择",
		Desc = "自动戴口罩用哪一种",
		Values = { "黑色头巾", "红色头巾", "蓝色头巾", "外科医生口罩", "面具" },
		Multi = false,
		AllowNone = false,
		Value = "黑色头巾",
		Callback = function(v)
			S.maskType = v
			S.maskAutoBuy = true
		end,
	})
	DefSec:Toggle({
		Title = "自动戴口罩",
		Desc = "没口罩就自动去买并戴上",
		Value = false,
		Callback = function(v)
			S.autoMask = v
			if v then task.spawn(Farm.buyMaskIfNeeded) end
		end,
	})
	DefSec:Toggle({ Title = "自动穿甲(补)", Desc = "没甲时自动买 Light Vest 穿上", Value = false, Callback = function(v) S.autoVest = v end })
	DefSec:Toggle({ Title = "自动回血", Desc = "血量不满时自动买绷带用", Value = false, Callback = function(v) S.autoHeal = v end })
	DefSec:Toggle({
		Title = "反坐下",
		Desc = "不会被别人按到座位上",
		Value = true,
		Callback = function(v) Farm.toggleAntiSit(v) end,
	})
	DefSec:Toggle({ Title = "防抓取", Desc = "别人抓不了你", Value = true, Callback = function(v) S.antiGrab = v end })
	DefSec:Toggle({ Title = "防挂机", Desc = "防止被系统判定挂机踢出", Value = true, Callback = function(v) S.antiIdle = v end })
	DefSec:Toggle({
		Title = "防虚空",
		Desc = "掉出地图时自动把自己顶回来",
		Value = false,
		Callback = function(v)
			S.antiVoid = v
			Farm.toggleAntiVoid(v)
		end,
	})
	DefSec:Toggle({ Title = "反管理", Desc = "发现管理员进服就把自己踢掉(保命用)", Value = false, Callback = function(v) S.antiAdmin = v end })
	DefSec:Toggle({
		Title = "扩大视野",
		Desc = "视野锁定 120",
		Value = false,
		Callback = function(v)
			S.bigFOV = v
			Farm.setBigFOV(v)
		end,
	})
	DefSec:Toggle({
		Title = "快速互动",
		Desc = "所有需要长按的互动秒完成",
		Value = false,
		Callback = function(v)
			S.fastInteract = v
		end,
	})
	DefSec:Button({
		Title = "不允许战斗中",
		Desc = "让游戏以为你永远不在战斗中",
		Callback = function()
			pcall(function()
				local ci = require(RS.devv.client.Helpers.ui.combatIndicator)
				hookfunction(ci.isInCombat, function() return false end)
				hookfunction(ci.enterCombat, function() end)
			end)
			notify("已解除战斗状态限制")
		end,
	})

	pcall(function()
		game:GetService("ProximityPromptService").PromptButtonHoldBegan:Connect(function(prompt)
			if S.fastInteract then
				pcall(function() prompt.HoldDuration = 0 end)
			end
		end)
	end)
end
	end)
	-- ↑↑↑ 验证通过后加载你的脚本 ↑↑↑

	local g = new("ScreenGui", {
		Name = "case_unlocked", IgnoreGuiInset = true,
		ResetOnSpawn = false, DisplayOrder = 9999, Parent = getParent(),
	})
	local pill = new("Frame", {
		Position = UDim2.fromOffset(20, 20), Size = UDim2.fromOffset(198, 38),
		BackgroundColor3 = C.card, BackgroundTransparency = 1, BorderSizePixel = 0, Parent = g,
	})
	round(pill, 10)
	local st = new("UIStroke", {
		Color = Color3.fromRGB(255, 255, 255), Transparency = 1, Thickness = 1,
		ApplyStrokeMode = Enum.ApplyStrokeMode.Border, Parent = pill,
	})
	local dot = new("Frame", {
		Position = UDim2.fromOffset(15, 15), Size = UDim2.fromOffset(8, 8),
		BackgroundColor3 = C.good, BackgroundTransparency = 1, BorderSizePixel = 0, Parent = pill,
	})
	round(dot, UDim.new(1, 0))
	local lbl = new("TextLabel", {
		Position = UDim2.fromOffset(32, 0), Size = UDim2.new(1, -44, 1, 0),
		BackgroundTransparency = 1, Font = Enum.Font.GothamBold, TextSize = 13,
		TextColor3 = C.text, TextXAlignment = Enum.TextXAlignment.Left,
		Text = "已解锁  ·  case", TextTransparency = 1, Parent = pill,
	})

	tw(pill, 0.3, { BackgroundTransparency = 0 })
	tw(st, 0.3, { Transparency = 0.86 })
	tw(dot, 0.3, { BackgroundTransparency = 0 })
	tw(lbl, 0.3, { TextTransparency = 0 })

	task.delay(2.6, function()
		tw(pill, 0.35, { BackgroundTransparency = 1 })
		tw(st, 0.35, { Transparency = 1 })
		tw(dot, 0.35, { BackgroundTransparency = 1 })
		tw(lbl, 0.35, { TextTransparency = 1 })
		task.wait(0.45)
		g:Destroy()
	end)
end

local function startKeyCheck(done)
	local gui = new("ScreenGui", {
		Name = "case_verify", IgnoreGuiInset = true, ResetOnSpawn = false,
		DisplayOrder = 9999, ZIndexBehavior = Enum.ZIndexBehavior.Sibling, Parent = getParent(),
	})

	local overlay = new("Frame", {
		Name = "Overlay", Size = UDim2.fromScale(1, 1),
		BackgroundColor3 = C.overlay, BackgroundTransparency = 1,
		BorderSizePixel = 0, ZIndex = 1, Parent = gui,
	})

	local scaleObj = new("UIScale", { Scale = 1, Parent = overlay })
	local function updateScale()
		scaleObj.Scale = math.clamp(Camera.ViewportSize.X / 440, 0.7, 1)
	end
	updateScale()
	Camera:GetPropertyChangedSignal("ViewportSize"):Connect(updateScale)

	local card = new("Frame", {
		Name = "Card", AnchorPoint = Vector2.new(0.5, 0.5),
		Position = UDim2.fromScale(0.5, 0.5), Size = UDim2.fromOffset(380, 292),
		BackgroundColor3 = C.card, BackgroundTransparency = 1,
		BorderSizePixel = 0, ZIndex = 2, Parent = overlay,
	})
	round(card, 16)
	local cardStroke = new("UIStroke", {
		Color = Color3.fromRGB(255, 255, 255), Thickness = 1, Transparency = 1,
		ApplyStrokeMode = Enum.ApplyStrokeMode.Border, Parent = card,
	})

	local pad = new("Frame", {
		Size = UDim2.new(1, -56, 1, -52), Position = UDim2.fromOffset(28, 26),
		BackgroundTransparency = 1, ZIndex = 3, Parent = card,
	})

	local mark = new("Frame", { Size = UDim2.fromOffset(18, 18), BackgroundTransparency = 1, ZIndex = 3, Parent = pad })
	local bars = {}
	for _, ang in ipairs({ 0, 60, 120 }) do
		local bar = new("Frame", {
			AnchorPoint = Vector2.new(0.5, 0.5), Position = UDim2.fromScale(0.5, 0.5),
			Size = UDim2.fromOffset(18, 2), BackgroundColor3 = C.dim, BackgroundTransparency = 1,
			BorderSizePixel = 0, Rotation = 0, ZIndex = 3, Parent = mark,
		})
		round(bar, UDim.new(1, 0))
		table.insert(bars, { bar = bar, ang = ang })
	end

	local kicker = new("TextLabel", {
		Position = UDim2.fromOffset(28, 0), Size = UDim2.new(1, -28, 0, 18),
		BackgroundTransparency = 1, Font = Enum.Font.GothamBold, TextSize = 11,
		TextColor3 = C.dim, TextXAlignment = Enum.TextXAlignment.Left,
		Text = CONFIG.KICKER, TextTransparency = 1, ZIndex = 3, Parent = pad,
	})

	local title = new("TextLabel", {
		Position = UDim2.fromOffset(0, 34), Size = UDim2.new(1, 0, 0, 30),
		BackgroundTransparency = 1, Font = Enum.Font.GothamBold, TextSize = 24,
		TextColor3 = C.text, TextXAlignment = Enum.TextXAlignment.Left,
		Text = CONFIG.TITLE, TextTransparency = 1, ZIndex = 3, Parent = pad,
	})

	local desc = new("TextLabel", {
		Position = UDim2.fromOffset(0, 66), Size = UDim2.new(1, 0, 0, 18),
		BackgroundTransparency = 1, Font = Enum.Font.Gotham, TextSize = 13,
		TextColor3 = C.dim, TextXAlignment = Enum.TextXAlignment.Left,
		Text = CONFIG.DESC, TextTransparency = 1, ZIndex = 3, Parent = pad,
	})

	local div = new("Frame", {
		Position = UDim2.fromOffset(0, 98), Size = UDim2.new(1, 0, 0, 1),
		BackgroundColor3 = Color3.fromRGB(255, 255, 255), BackgroundTransparency = 1,
		BorderSizePixel = 0, ZIndex = 3, Parent = pad,
	})

	local field = new("Frame", {
		Position = UDim2.fromOffset(0, 114), Size = UDim2.new(1, 0, 0, 46),
		BackgroundColor3 = C.field, BackgroundTransparency = 1,
		BorderSizePixel = 0, ZIndex = 3, Parent = pad,
	})
	round(field, 10)
	local fieldStroke = new("UIStroke", {
		Color = Color3.fromRGB(255, 255, 255), Thickness = 1, Transparency = 1,
		ApplyStrokeMode = Enum.ApplyStrokeMode.Border, Parent = field,
	})

	local box = new("TextBox", {
		Position = UDim2.fromOffset(15, 0), Size = UDim2.new(1, -30, 1, 0),
		BackgroundTransparency = 1, Font = Enum.Font.Gotham, TextSize = 15,
		TextColor3 = C.text, Text = "", PlaceholderText = CONFIG.PLACEHOLDER,
		PlaceholderColor3 = C.faint, ClearTextOnFocus = false,
		TextXAlignment = Enum.TextXAlignment.Left, TextTransparency = 1,
		ZIndex = 4, Parent = field,
	})

	local btnWrap = new("Frame", {
		Position = UDim2.fromOffset(0, 174), Size = UDim2.new(1, 0, 0, 44),
		BackgroundTransparency = 1, ZIndex = 3, Parent = pad,
	})
	local btn = new("TextButton", {
		AnchorPoint = Vector2.new(0.5, 0.5), Position = UDim2.fromScale(0.5, 0.5),
		Size = UDim2.new(1, 0, 0, 44), BackgroundColor3 = C.accent, BackgroundTransparency = 1,
		Text = CONFIG.BTN, Font = Enum.Font.GothamBold, TextSize = 14,
		TextColor3 = C.ink, TextTransparency = 1, AutoButtonColor = false,
		ZIndex = 4, Parent = btnWrap,
	})
	local btnCorner = new("UICorner", { CornerRadius = UDim.new(0, 10) }, btn)
	local btnScale = new("UIScale", { Scale = 1, Parent = btn })

	local check = new("TextLabel", {
		Size = UDim2.fromScale(1, 1), BackgroundTransparency = 1,
		Font = Enum.Font.GothamBold, TextSize = 24, Text = "✓",
		TextColor3 = C.ink, TextTransparency = 1, ZIndex = 7, Parent = btn,
	})
	local checkScale = new("UIScale", { Scale = 0.7, Parent = check })

	local spin = new("Frame", {
		AnchorPoint = Vector2.new(0.5, 0.5), Position = UDim2.fromScale(0.5, 0.5),
		Size = UDim2.fromOffset(0, 0), BackgroundTransparency = 1,
		ZIndex = 6, Parent = btnWrap,
	})

	local status = new("TextLabel", {
		Position = UDim2.fromOffset(0, 228), Size = UDim2.new(1, 0, 0, 16),
		BackgroundTransparency = 1, Font = Enum.Font.Gotham, TextSize = 12,
		TextColor3 = C.dim, TextXAlignment = Enum.TextXAlignment.Left,
		Text = "等待输入…", TextTransparency = 1, ZIndex = 3, Parent = pad,
	})

	local hint = new("TextLabel", {
		Position = UDim2.fromOffset(0, 228), Size = UDim2.new(1, 0, 0, 16),
		BackgroundTransparency = 1, Font = Enum.Font.GothamBold, TextSize = 11,
		TextColor3 = C.faint, TextXAlignment = Enum.TextXAlignment.Right,
		Text = "ENTER ", TextTransparency = 1, ZIndex = 3, Parent = pad,
	})

	local blur
	if CONFIG.BLUR then
		blur = new("BlurEffect", { Size = 0, Parent = Lighting })
		tw(blur, 0.5, { Size = 14 })
	end

	tw(overlay, 0.35, { BackgroundTransparency = 0.45 })
	tw(card, 0.5, { BackgroundTransparency = 0 }, Enum.EasingStyle.Quint)
	tw(cardStroke, 0.5, { Transparency = 0.86 })
	for i, b in ipairs(bars) do
		tw(b.bar, 0.5, { Rotation = b.ang, BackgroundTransparency = 0.45 }, Enum.EasingStyle.Quint, i * 0.06)
	end
	task.delay(0.12, function()
		tw(kicker, 0.4, { TextTransparency = 0.25 })
		tw(title, 0.45, { TextTransparency = 0 })
		tw(desc, 0.45, { TextTransparency = 0 }, nil, 0.06)
		tw(div, 0.45, { BackgroundTransparency = 0.9 }, nil, 0.1)
		tw(field, 0.45, { BackgroundTransparency = 0 })
		tw(fieldStroke, 0.45, { Transparency = 0.9 })
		tw(box, 0.45, { TextTransparency = 0 })
		tw(btn, 0.45, { BackgroundTransparency = 0 }, nil, 0.08)
		tw(btn, 0.45, { TextTransparency = 0 }, nil, 0.08)
		tw(status, 0.45, { TextTransparency = 0.1 }, nil, 0.14)
		tw(hint, 0.45, { TextTransparency = 0.5 }, nil, 0.14)
	end)

	local busy = false

	local function setStatus(text, color)
		status.Text = text
		status.TextColor3 = color or C.dim
	end

	local shaking = false
	local function shake()
		if shaking then return end
		shaking = true
		local seq = { 0.013, -0.013, 0.009, -0.009, 0.005, 0 }
		for i, dx in ipairs(seq) do
			task.delay(i * 0.045, function()
				tw(card, 0.06, { Position = UDim2.new(0.5 + dx, 0, 0.5, 0) }, Enum.EasingStyle.Linear)
			end)
		end
		task.delay(#seq * 0.045 + 0.12, function() shaking = false end)
	end

	local flashToken = 0
	local function flash(color)
		flashToken = flashToken + 1
		local mine = flashToken
		fieldStroke.Color = color
		fieldStroke.Transparency = 0.35
		task.delay(0.9, function()
			if mine ~= flashToken then return end
			fieldStroke.Color = Color3.fromRGB(255, 255, 255)
			tw(fieldStroke, 0.4, { Transparency = 0.9 })
		end)
	end

	local spinActive = false
	local spinTween = nil

	local function stopSpin()
		spinActive = false
		if spinTween then
			pcall(function() spinTween:Cancel() end)
			spinTween = nil
		end
	end

	local function startSpin()
		stopSpin()
		spinActive = true
		task.spawn(function()
			while spinActive do
				if not spin.Parent then break end

				local cur = spin.Rotation % 360
				spin.Rotation = cur - 360
				local okT, t = pcall(function()
					return tw(spin, CONFIG.SPIN, { Rotation = cur }, Enum.EasingStyle.Linear)
				end)
				if not okT or not t then break end
				spinTween = t
				local okW = pcall(function() t.Completed:Wait() end)
				spinTween = nil
				if not okW then break end
			end
			spinActive = false
		end)
	end

	local function clearDots()
		for _, d in ipairs(spin:GetChildren()) do
			if d:IsA("Frame") then d:Destroy() end
		end
	end

	local function fadeOut()
		stopSpin()
		for _, d in ipairs(pad:GetDescendants()) do
			if d:IsA("TextLabel") or d:IsA("TextBox") then
				tw(d, 0.3, { TextTransparency = 1 })
			elseif d:IsA("Frame") then
				tw(d, 0.3, { BackgroundTransparency = 1 })
			elseif d:IsA("UIStroke") then
				tw(d, 0.3, { Transparency = 1 })
			end
		end
		tw(card, 0.4, { BackgroundTransparency = 1 })
		tw(cardStroke, 0.4, { Transparency = 1 })
		tw(overlay, 0.4, { BackgroundTransparency = 1 })
		if blur then tw(blur, 0.4, { Size = 0 }) end
		task.wait(0.45)
		if blur then blur:Destroy() end
		gui:Destroy()
	end

	local function playMorph(success)
		busy = true
		box.TextEditable = false
		stopSpin()
		clearDots()
		spin.Rotation = 0
		check.Text = success and "✓" or "X"
		check.TextTransparency = 1
		checkScale.Scale = 0.7
		setStatus("验证中…", C.dim)

		tw(btn, 0.14, { TextTransparency = 1 }, Enum.EasingStyle.Sine)
		task.wait(0.16)

		tw(btn, 0.40, { Size = UDim2.new(0, 44, 0, 44) }, Enum.EasingStyle.Quint)
		tw(btnCorner, 0.40, { CornerRadius = UDim.new(1, 0) }, Enum.EasingStyle.Quint)
		task.wait(0.42)

		tw(btnScale, 0.15, { Scale = 1.14 }, Enum.EasingStyle.Sine)
		task.wait(0.15)
		tw(btnScale, 0.26, { Scale = 1 }, Enum.EasingStyle.Sine)

		local dots = {}
		for i = 1, CONFIG.DOTS do
			local rad = math.rad((i - 1) * (360 / CONFIG.DOTS))
			local dot = new("Frame", {
				AnchorPoint = Vector2.new(0.5, 0.5),
				Position = UDim2.fromOffset(math.cos(rad) * CONFIG.RADIUS, math.sin(rad) * CONFIG.RADIUS),
				Size = UDim2.fromOffset(8, 8), BackgroundColor3 = C.accent,
				BackgroundTransparency = 0, BorderSizePixel = 0,
				ZIndex = 6, Parent = spin,
			})
			round(dot, UDim.new(1, 0))
			local s = new("UIScale", { Scale = 0, Parent = dot })
			tw(s, 0.28, { Scale = 1 }, Enum.EasingStyle.Back, i * 0.05)
			table.insert(dots, dot)
		end
		startSpin()

		task.wait(CONFIG.SPIN_TIME)

		if success then
			stopSpin()
			for _, d in ipairs(dots) do
				tw(d, 0.32, {
					Position = UDim2.fromOffset(0, 0),
					Size = UDim2.fromOffset(0, 0),
					BackgroundColor3 = C.good,
				}, Enum.EasingStyle.Quint)
			end
			tw(btn, 0.32, { BackgroundColor3 = C.good })
			task.wait(0.18)
			tw(check, 0.26, { TextTransparency = 0 }, Enum.EasingStyle.Back)
			tw(checkScale, 0.32, { Scale = 1 }, Enum.EasingStyle.Back)
			setStatus("验证通过", C.good)
			if CONFIG.REMEMBER and hasFileAPI() then
				pcall(writefile, CACHE, CONFIG.KEY)
			end
			task.wait(0.75)
			fadeOut()
			if done then done() end
		else
			for _, d in ipairs(dots) do
				tw(d, 0.24, { BackgroundColor3 = C.bad, Size = UDim2.fromOffset(11, 11) })
			end
			tw(btn, 0.22, { BackgroundColor3 = C.bad })
			task.wait(0.2)
			tw(check, 0.22, { TextTransparency = 0 }, Enum.EasingStyle.Back)
			tw(checkScale, 0.26, { Scale = 1 }, Enum.EasingStyle.Back)
			setStatus("密钥无效", C.bad)
			task.wait(0.55)

			tw(check, 0.16, { TextTransparency = 1 })
			tw(checkScale, 0.16, { Scale = 0.6 })
			for _, d in ipairs(dots) do
				local p = d.Position
				tw(d, 0.32, {
					Position = UDim2.fromOffset(p.X.Offset * 1.9, p.Y.Offset * 1.9),
					Size = UDim2.fromOffset(0, 0),
					BackgroundTransparency = 1,
				}, Enum.EasingStyle.Quint)
			end
			task.wait(0.3)
			stopSpin()
			clearDots()
			spin.Rotation = 0

			tw(btn, 0.36, { BackgroundColor3 = C.accent, Size = UDim2.new(1, 0, 0, 44) }, Enum.EasingStyle.Quint)
			tw(btnCorner, 0.36, { CornerRadius = UDim.new(0, 10) }, Enum.EasingStyle.Quint)
			task.wait(0.38)
			tw(btn, 0.3, { TextTransparency = 0 }, Enum.EasingStyle.Sine)
			flash(C.bad)
			shake()
			setStatus("密钥无效 · 请重试", C.bad)
			box.TextEditable = true
			busy = false
		end
	end

	local verify

	box.Focused:Connect(function()
		tw(fieldStroke, 0.2, { Transparency = 0.7 })
	end)
	box.FocusLost:Connect(function(enter)
		tw(fieldStroke, 0.2, { Transparency = 0.9 })
		if enter and verify then verify() end
	end)

	btn.MouseEnter:Connect(function()
		if not busy then tw(btn, 0.16, { BackgroundColor3 = Color3.fromRGB(255, 255, 255) }) end
	end)
	btn.MouseLeave:Connect(function()
		if not busy then tw(btn, 0.16, { BackgroundColor3 = C.accent }) end
	end)
	btn.MouseButton1Down:Connect(function()
		if not busy then tw(btn, 0.08, { BackgroundColor3 = Color3.fromRGB(206, 206, 212) }) end
	end)
	btn.MouseButton1Up:Connect(function()
		if not busy then tw(btn, 0.12, { BackgroundColor3 = C.accent }) end
	end)
	btn.MouseButton1Click:Connect(function()
		if verify then verify() end
	end)

	verify = function()
		if busy then return end
		local v = norm(box.Text)
		if v == "" then
			setStatus("请先输入密钥", C.dim)
			shake()
			flash(C.bad)
			return
		end
		playMorph(v == norm(CONFIG.KEY))
	end

end
-- 卡密系统已移除，直接加载脚本
print("[case] 跳过卡密验证，直接加载脚本")
onSuccess()
