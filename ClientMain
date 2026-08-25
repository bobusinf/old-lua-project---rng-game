--VARIABLES--

--SERVICES
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local MarketPlaceService = game:GetService("MarketplaceService")
local TextChatService = game:GetService("TextChatService")
local BadgeService = game:GetService("BadgeService")

--REPLICATEDSTORAGE
local modules = ReplicatedStorage:WaitForChild("Modules")
local remotes = ReplicatedStorage.Remotes
local DisplayPlace = ReplicatedStorage.DisplayPlace
local morphs  = ReplicatedStorage.Morphs

--MODULESCRIPTS
local cutscenes = require(modules.Cutscenes)
local auras = require(modules.Auras)
local items = require(modules.Items)

--REMOTEEVENTS
local UseItemEvent = remotes.UseItem
local UpdateSettingsEvent = remotes.UpdateSettings
local EquipAuraEvent = remotes.EquipAura
local KeepItemEvent = remotes.KeepItem
local AddRollEvent = remotes.AddRoll
local GetLuckEvent = remotes.GetLuckStC
local AddCraftingEvent = remotes.AddCrafting
local CloseCraftingEvent = remotes.ChangeCraftingToFalse
local InteractGauntletEvent = remotes.InteractGauntlet
local AdminRollEvent = remotes.AdminRoll
local RareItemEvent = remotes.PulledRareItem
local UsedCodeEvent = remotes.AddUsedCode
local SendLuckEvent = remotes.SendLuck

--REMOTEFUNCTIONS
local GetInventoryFunction = remotes.GetInventory
local GetCraftingSuppliesFunction = remotes.GetCraftingSupplies
local GetEquippedGauntletsFunction = remotes.GetEquippedGauntlets
local GetUsedCodes = remotes.GetUsedCodes
local GetRemainingLuck = remotes.GetRemaining

--PLAYER
local plr = game.Players.LocalPlayer

--STARTERGUI:{
local sg = script.Parent

--FOLDERS
local uiButtons = sg.UIButtons
local uiFrames = sg.UIFrames

--MAINBUTTONS
local rollButton = sg.Roll
local autorollButton = sg.AutoRoll
local quickrollButton = sg.QuickRoll
local keepButton = sg.Keep
local leaveButton = sg.Leave
local stopButton = sg.Stop

--MAINFRAMES
local cutsceneFrame = sg.Cutscene
local lucksFrame = sg.Lucks
local rollFrame = sg.RollFrame
local EGFrame = sg.EquippedGauntlets

--SIDEBUTTONS
local potionsButton = uiButtons.RealInv
local ShopButton = uiButtons.Shop
local inventoryButton = uiButtons.Inventory
local settingsButton = uiButtons.Settings
local codesButton = uiButtons.Codes

--SIDEBUTTONFRAMES
local potionsFrame = uiFrames.RealInv
local ShopFrame = uiFrames.Shop
local inventoryFrame = uiFrames.Inventory
local settingsFrame = uiFrames.Settings
local CraftingFrame = uiFrames.Crafting
local CodesFrame = uiFrames.Codes

--CraftingUi
local CraftingStats = CraftingFrame.CraftingStats
local CraftingTab = CraftingFrame.SelectTab.SelectTabFrame

local CraftingItemName = CraftingStats.ItemName
local CraftingItemDescription = CraftingStats.ItemDescription
local CraftingItemBalance = CraftingStats.Balance

local OpenMetalButton = CraftingTab.Metal
local OpenGauntletButton = CraftingTab.Gauntlets
local OpenMagicPotionButton = CraftingTab.MagicPotion
local OpenLuckPotionButton = CraftingTab.LuckPotion

local CloseButton = CraftingFrame.CloseButton

local CraftButton = CraftingStats.Craft
local UnequipButton = CraftingStats.Unequip

local MetalFolder = script.Crafting.Metal
local GauntletsFolder = script.Crafting.Gauntlets
local LuckFolder = script.Crafting.LuckPotion
local MagicFolder = script.Crafting.MagicPotion

local InGauntletsFolder = GauntletsFolder.Gauntlets
local AddThanos = GauntletsFolder.AddThanos
local AddClockwork = GauntletsFolder.AddClockwork
local AddRHL = GauntletsFolder.AddRHL
local AddGrenade = GauntletsFolder.AddGrenade
local InAddFolders = {AddThanos, AddClockwork, AddRHL, AddGrenade}

local ThanosGauntletButton = InGauntletsFolder.Thanos
local ClockworkGauntletButton = InGauntletsFolder.Clockwork
local RHLGauntletButton = InGauntletsFolder.RHL
local GrenadeGauntletButton = InGauntletsFolder.Grenade

local GauntletButtonFolder = {ThanosGauntletButton, ClockworkGauntletButton, RHLGauntletButton, GrenadeGauntletButton}

--SettingsUi
local SettingsA = settingsFrame.SettingsFrame
local AutoKeep = SettingsA.AutoKeep
local AutoDelete = SettingsA.AutoDelete

local AutoKeepButton = AutoKeep.TextButton
local AutoDeleteButton = AutoDelete.TextButton

local AutoKeepTextBox = AutoKeep.TextBox
local AutoDeleteTextBox = AutoDelete.TextBox

--TEXTLABELS
local errorLabel = sg.Error

local leftLabel = inventoryFrame.OutOf
--}
--Values Handler
local Values = plr:WaitForChild("Values")

--Values
local Crafting = Values.CurrentlyCrafting

--DEBOUNCE VALUES / LATER IDENTIFIED VARIABLES

local ableToViewCutscene = false
local gotAuras = false
local FireServerDB = false
local autoroll = false
local autorolling = false
local quickrolling = false
local equipped = false
local viewinggauntlet = false
local equippingGauntlet = false
local hasMultipleGauntletGamepass = false
local equippingGauntlet2 = false
local et1 = false
local et2 = false
local et3 = false
local errordb = false
local once = false
local twice = false
local on2 = false
local on3 = false
local hasPremium = false
local premiumDB = false
local friendsDB = false
local filtering = false

local AutoKeeping = 0
local AutoDeleting = 0

local LuckPCountdown = 0
local MagicPCountdown = 0

local friends = 0

local currentlyOpenCraftingTab
local currentlyOpenUi
local currentlyViewingAura
local currentlySelectedAura
local currentCutsceneOrder
local currentlyViewingGauntlet

local canRoll = true
local Rolling = true

--Luck
local weight = 0

--Luck variables (Gauntlets)
local ThanosE = false
local ClockworkE = false
local RHLE = false

--CURRENTCAMERA
local camera  = workspace.CurrentCamera

--CountdownUi
local CooldownUI = rollButton.Cooldown

--Buttons
local deleteButton = inventoryFrame.Delete
local equipButton = inventoryFrame.Equip

local useButtonP = potionsFrame.Use
local deleteButtonP = potionsFrame.Delete

--Colors
local green = Color3.fromRGB(153, 214, 128)
local red = Color3.fromRGB(190, 45, 60)

--Text
local check = "✓"

--Other folders
local SelectedItems = inventoryFrame.SelectedItems
local potionsSelectedItems = potionsFrame.SelectedItems

--Tables
local SelectedObjects = {}
local potionsSelectedObjects = {}

stopButton.Visible = false

local function getAuras()
	gotAuras = true
	local auras = GetInventoryFunction:InvokeServer()
	return auras
end


local function addNew(reVal)
	local item = script.Item:Clone()
	item.Name = reVal.Name
	item.Parent = inventoryFrame.Auras.Auras2
	item.NameOfItem.Text = reVal.Name
	item.NameOfItem.TextColor3 = reVal.Color
	item.BackgroundColor3 = reVal.Color
	item.ChanceOfItem.Text = "1 in "..reVal.OneIn.." ("..reVal.Rarity..")"
	item.ItemScript.Enabled = true
	item.Rarity.Value = reVal.OneIn
	item.NameOfRarity.Value = reVal.Rarity
	item.LayoutOrder = -table.find(auras, reVal)
end

local function addPotion(potion)
	if potion == "Luck" then
		local item = script.Item:Clone()
		item.Parent = potionsFrame.Items.ItemsHolder
		item.TypeNonInventory.Value = "Potion"
		item.NameOfItem.Text = "Luck potion"
		item.NameOfItem.TextColor3 = Color3.fromRGB(0,185,30)
		item.ItemScript.Enabled = true
		item.NameOfRarity.Value = "This potion gives you x1.2 luck for 1 minute"
		item.Rarity.Value = 0
		item.ChanceOfItem.Text = ""
		item.Name = "LuckPotion"
	elseif potion == "Magic" then
		local item = script.Item:Clone()
		item.Parent = potionsFrame.Items.ItemsHolder
		item.TypeNonInventory.Value = "Potion"
		item.NameOfItem.Text = "Magic potion"
		item.NameOfItem.TextColor3 = Color3.fromRGB(100,20,175)
		item.ItemScript.Enabled = true
		item.NameOfRarity.Value = "This potion gives you x1.5 luck for 1 minute"
		item.Rarity.Value = 0
		item.ChanceOfItem.Text = ""
		item.Name = "MagicPotion"
	elseif potion == "Exotic" then
		local item = script.Item:Clone()
		item.Parent = potionsFrame.Items.ItemsHolder
		item.TypeNonInventory.Value = "Potion"
		item.NameOfItem.Text = "Exotic potion"
		item.NameOfItem.TextColor3 = Color3.fromRGB(60,60,190)
		item.ItemScript.Enabled = true
		item.NameOfRarity.Value = "This potion gives you x2 luck for 1 minute"
		item.Rarity.Value = 0
		item.ChanceOfItem.Text = ""
		item.Name = "ExoticPotion"
	end
end

local GottenAuras = getAuras()

if GottenAuras == nil then GottenAuras = {}
else 
	for i, v in GottenAuras do
		local localAura
		for i1, v1 in auras do
			if v1.Name == v then
				localAura = v1
				break
			end
		end
		if localAura then
			addNew(localAura)
		end
	end
end

leftLabel.Text = #GottenAuras.." / 100"

----------------------------------------

local CraftingSupplies = GetCraftingSuppliesFunction:InvokeServer()

if not CraftingSupplies then CraftingSupplies = {
	["Metal"] = 0,
	["Gauntlets"] = {
		["Thanos"] = false,
		["Clockwork"] = false,
		["RHL"] = false,
		["Grenade"] = false
	},
	["LuckPotions"] = 0,
	["MagicPotions"] = 0,
	["ExoticPotions"] = 0
	} end

-----------------------------------------
local EquippedGauntlets = GetEquippedGauntletsFunction:InvokeServer()
if not EquippedGauntlets then EquippedGauntlets = {
	["Thanos"] = false,
	["Clockwork"] = false,
	["RHL"] = false,
	["Grenade"] = false
}end

--------------------------------------------

if not CraftingSupplies["ExoticPotions"] then CraftingSupplies["ExoticPotions"] = 0 end

for i = 1, CraftingSupplies["LuckPotions"] do
	addPotion("Luck")
end

for i = 1, CraftingSupplies["MagicPotions"] do
	addPotion("Magic")
end

for i = 1, CraftingSupplies["ExoticPotions"] do
	addPotion("Exotic")
end

--------------------------------------------
local UsedCodes = GetUsedCodes:InvokeServer()
if not UsedCodes then UsedCodes = {
	["1k"] = false
}end
--------------------------------------------
local remainingLuck = GetRemainingLuck:InvokeServer()
script.L.Value = remainingLuck[1]
script.M.Value = remainingLuck[2]
script.E.Value = remainingLuck[3]
--------------------------------------------

local function doError(text, timeA, colorA)
	if not errordb then
		errordb = true
		errorLabel.Visible = true
		errorLabel.Position = UDim2.new(0.5, 0,0.755, 0)
		errorLabel.Text = text
		errorLabel.TextTransparency = .3
		errorLabel.TextColor3 = colorA or Color3.fromRGB(170, 0, 0)
		local totween = {
			Position = UDim2.new(.5,0,.5,0),
			TextTransparency = 1
		}
		local twinfo = TweenInfo.new(timeA or 4.5, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)
		local errortween = TweenService:Create(errorLabel, twinfo, totween)
		errortween:Play()
		errortween.Completed:Wait()
		errorLabel.Visible = false
		errordb = false
	end
end

local function Roll()
	local chosen
	local function roll()
		for i = #auras, 0, -1 do
			local v = auras[math.random(1, #auras)]
			local fraction
			if table.find(auras, v) >= 11 then
				local fraction1 = (1/v.OneIn) 
				fraction = fraction1 + (fraction1 * weight)
			else
				fraction = (1/v.OneIn) 
			end
			if math.random() <= fraction then
				chosen = table.find(auras, v)
				break
			end
		end
		if not chosen then 
			repeat
				chosen = roll()
			until chosen
			return chosen
		else return chosen end
	end
	roll()
	return chosen
end

local function onRoll(a)
	if #GottenAuras < 100 and canRoll then
		if autorolling then autoroll = false end
		if not canRoll then return end
		canRoll = false
		AddRollEvent:FireServer()
		local targetPosition = {
			Position = UDim2.new(0.499, 0,0.528, 0)
		}
		local tweeninfo 
		
		if quickrolling then
			tweeninfo = TweenInfo.new(0.2,Enum.EasingStyle.Quad,Enum.EasingDirection.InOut)
		else
			tweeninfo = TweenInfo.new(0.4,Enum.EasingStyle.Quad,Enum.EasingDirection.InOut)
		end
		
		local targetSizeCooldown = {
			Size = UDim2.new(1,0,0,0)
		}
		local tweeninfoCooldown
		if quickrolling then
			tweeninfoCooldown = TweenInfo.new(.3,Enum.EasingStyle.Linear,Enum.EasingDirection.InOut)
		else
			tweeninfoCooldown = TweenInfo.new(.6,Enum.EasingStyle.Linear,Enum.EasingDirection.InOut)
		end
		
		local CooldownTween = TweenService:Create(CooldownUI, tweeninfoCooldown, targetSizeCooldown)
		
		rollFrame.Visible = true
		local rolledElement
		for i = 4, 0, -1 do
			rolledElement = auras[a] or auras[Roll()]
			rollFrame.ShowRoll.Position = UDim2.new(0.499, 0, 0.481, 0)
			rollFrame.ShowRoll.Text = rolledElement.Name
			rollFrame.ShowRoll.RollChance.Text = "1 in "..rolledElement.OneIn.." ("..rolledElement.Rarity..")"
			rollFrame.ShowRoll.TextColor3 = rolledElement.Color
			local tween = TweenService:Create(rollFrame.ShowRoll,tweeninfo, targetPosition)
			tween:Play()
			if i > 0 then
				if quickrolling then
					task.wait(.2)
				else
					task.wait(.4) 
				end
			end
		end
		if rolledElement.OneIn >= 10000 then
			cutscenes[math.random(1,6)](plr, rolledElement.Color)
		end
		if quickrolling then
			task.wait(.3)
		else
			task.wait(.55)
		end
		if AutoKeeping == 0 or AutoKeeping > rolledElement.OneIn then 
			if AutoDeleting == 0 or AutoDeleting < rolledElement.OneIn then 
				keepButton.Visible = true
				leaveButton.Visible = true
				keepButton.MouseButton1Click:Connect(function()
					if not a and rolledElement.OneIn >= 10000 then
						local text1 = "<font color='#"..rolledElement.Color:ToHex().."'>"..plr.Name.." has rolled "..rolledElement.Name.." (1 in "..rolledElement.OneIn.." ("..rolledElement.Rarity.."))</font>"
						RareItemEvent:FireServer(rolledElement.OneIn, text1)
						if rolledElement.Rarity == "The One." then
							BadgeService:AwardBadge(plr.UserId, 30432410670119)
						end
					end
					if not FireServerDB then
						FireServerDB = true
						table.insert(GottenAuras, tostring(rolledElement.Name))
						addNew(rolledElement)				
						keepButton.Visible = false
						leaveButton.Visible = false
						
						KeepItemEvent:FireServer(GottenAuras)
						if Rolling then
							CooldownUI.Visible = true
							
							CooldownUI.Size = UDim2.new(1,0,1,0)
							CooldownTween:Play()
						end
						
						rollFrame.Visible = false
						
						leftLabel.Text = #GottenAuras.." / 100"
						if quickrolling then
							task.wait(.25)
						else
							task.wait(.5)
						end
						
							canRoll = true
							CooldownUI.Visible = false

						FireServerDB = false
						
						if autorolling then autoroll = true end
					end			
				end)
					
				leaveButton.MouseButton1Click:Connect(function()
					keepButton.Visible = false
					leaveButton.Visible = false
					if Rolling then
						CooldownUI.Visible = true
						CooldownUI.Size = UDim2.new(1,0,1,0)
						CooldownTween:Play()
					end
					rollFrame.Visible = false
					if autorolling then
						if quickrolling then
							task.wait(.75)
							canRoll = true
						else
							task.wait(1.25)
							canRoll = true
						end
						autoroll = true
					else
						canRoll = true
					end
					if Rolling then
						CooldownUI.Visible = false
					end
					if autorolling then autoroll = true end
				end)
			else
				task.spawn(function() doError("Autodeleted", 1, Color3.fromRGB(20,185,80)) end)
				if Rolling then
					CooldownUI.Visible = true
					CooldownUI.Size = UDim2.new(1,0,1,0)
					CooldownTween:Play()
				end
				rollFrame.Visible = false
				if autorolling then
					if quickrolling then
						task.wait(.5)
						canRoll = true
					else
						task.wait(1)
						canRoll = true
					end
				else
					canRoll = true
				end
				if Rolling then
					CooldownUI.Visible = false
				end
				if autorolling then autoroll = true end
			end
		else
			if not a and rolledElement.OneIn >= 10000 then
				local text1 = "<font color='#"..rolledElement.Color:ToHex().."'>"..plr.Name.." has rolled "..rolledElement.Name.." (1 in "..rolledElement.OneIn.." ("..rolledElement.Rarity.."))</font>"
				RareItemEvent:FireServer(rolledElement.OneIn, text1)
				if rolledElement.Rarity == "The One." then
					BadgeService:AwardBadge(plr.UserId, 30432410670119)
				end
			end
			task.spawn(function() doError("Autokeeped", 1, Color3.fromRGB(20,185,80)) end)
			if not FireServerDB then
				FireServerDB = true
				table.insert(GottenAuras, tostring(rolledElement.Name))
				addNew(rolledElement)

				KeepItemEvent:FireServer(GottenAuras)
				if Rolling then
					CooldownUI.Visible = true

					CooldownUI.Size = UDim2.new(1,0,1,0)
					CooldownTween:Play()
				end

				rollFrame.Visible = false

				leftLabel.Text = #GottenAuras.." / 100"
				if quickrolling then
					task.wait(.5)
				else
					task.wait(1)
				end

				canRoll = true
				CooldownUI.Visible = false

				FireServerDB = false

				if autorolling then autoroll = true end
			end	
		end
	elseif #GottenAuras == 100 then
		autorolling = false
		canRoll = false
		autoroll = false
		rollFrame.Visible = true
		rollFrame.ShowRoll.Text = "Max auras reached; Delete some auras."
		rollFrame.ShowRoll.RollChance.Text = ""
		if quickrolling then
			task.wait(.5)
		else
			task.wait(1)
		end
		rollFrame.Visible = false
		if autorolling then
			if quickrolling then
				task.wait(.75)
				canRoll = true
			else
				task.wait(1.25)
				canRoll = true
			end
			autoroll = true
		else
			canRoll = true
		end	
		StopAutoroll()
	end
	return
end

local function delete()
	if inventoryButton.Visible then
		if #SelectedObjects == 0 and inventoryFrame.Interacted.Value ~= nil	then 
			inventoryFrame.AreYouSure.Visible = true
			inventoryFrame.AreYouSure.TextLabel.Text = "Are you sure you want to delete "..tostring(inventoryFrame.Interacted.Value).."?"
			inventoryFrame.AreYouSure.Yes.MouseButton1Click:Connect(function()
				local todelete = inventoryFrame.Interacted.Value
				local instancetodelete = inventoryFrame.Interacted.Value
				table.remove(GottenAuras, table.find(GottenAuras, tostring(instancetodelete)))
				pcall(function()
					instancetodelete.Parent = nil
				end)
				KeepItemEvent:FireServer(GottenAuras)					
				inventoryFrame.Interacted.Value = nil
				inventoryFrame.AreYouSure.Visible = false
			end)
			
			inventoryFrame.AreYouSure.No.MouseButton1Click:Connect(function()
				inventoryFrame.AreYouSure.Visible = false
			end)
		elseif #SelectedObjects >= 1 then
			inventoryFrame.AreYouSure.Visible = true
			inventoryFrame.AreYouSure.TextLabel.Text = "Are you sure you want to delete every single one of these items? (this process cannot be undone.)"
			inventoryFrame.AreYouSure.Yes.MouseButton1Click:Connect(function()
				for i, v in SelectedObjects do 
					for i1, v1 in SelectedItems:GetChildren() do
						if v1.Value == v then v1:Destroy() end
					end
					table.remove(GottenAuras, table.find(GottenAuras, tostring(v)))
					v:Destroy()
				end
				SelectedObjects = {}
				inventoryFrame.Interacted.Value = nil
				inventoryFrame.AreYouSure.Visible = false
				task.wait(.02)
				KeepItemEvent:FireServer(GottenAuras)				
			end)

			inventoryFrame.AreYouSure.No.MouseButton1Click:Connect(function()
				inventoryFrame.AreYouSure.Visible = false
			end)
		end
	end
end

local function changeEquip()
	inventoryFrame.AreYouSure.Visible = false
end

local function Autoroll() 
	rollButton.Visible = false
	stopButton.Visible = true
	autorolling = true
	autoroll = true
	Rolling = false
end

function StopAutoroll()
	rollButton.Visible = true
	stopButton.Visible = false
	if canRoll then
		autorolling = false
		autoroll = false
		Rolling = true
	else
		autorolling = false
		autoroll = false
		Rolling = true
	end
end

local function QuickRoll()
	if plr.leaderstats.Rolls.Value >= 1000 then
		if not quickrolling then
			quickrollButton.Text = "QuickRoll: On"
			quickrolling = true
		else
			quickrollButton.Text = "QuickRoll: Off"
			quickrolling = false
		end
	end
end

local function equipMorph()
	if inventoryFrame.Interacted.Value and not equipped then
		EquipAuraEvent:FireServer("Equip", tostring(inventoryFrame.Interacted.Value), "<font color='#".. tostring(inventoryFrame.Interacted.Value.NameOfItem.TextColor3:ToHex()) .."'>"..inventoryFrame.Interacted.Value.ChanceOfItem.Text.."</font>")
		equipped = true
		equipButton.Text = "Unequip"
		equipButton.BackgroundColor3 = red
		equipButton.TextColor3 = Color3.fromRGB(184,0, 0)
	elseif equipped then
		equipped = false
		EquipAuraEvent:FireServer("Unequip")
		equipButton.Text = "Equip"
		equipButton.BackgroundColor3 = green
		equipButton.TextColor3 = Color3.fromRGB(0,184,0)
	end
end

local function SelectEnd()
	equipButton.Visible = true
	inventoryFrame.AuraName.Text = "Select something"
	inventoryFrame.OneIn.Text = "Chance"
end

local function SelectAdd(Item)	
	table.insert(SelectedObjects, Item.Value)
	if #SelectedObjects >= 1 then
		equipButton.Visible = false
		inventoryFrame.AuraName.Text = "Selecting"
		inventoryFrame.OneIn.Text = ""
	end
end

local function SelectRemove(Item)
	table.remove(SelectedObjects, table.find(SelectedObjects, Item.Value))
	if #SelectedObjects == 0 then
		SelectEnd()
	end
end

local function pSelectEnd()
	useButtonP.Visible = true
	potionsFrame.ItemName.Text = "Select something"
	potionsFrame.Info.Text = "Description"
end

local function pSelectAdd(Item)
	table.insert(potionsSelectedObjects, Item.Value)
	if #potionsSelectedObjects >= 1 then
		useButtonP.Visible = false
		potionsFrame.ItemName.Text = "Selecting"
		potionsFrame.Info.Text = ""
	end
end

local function pSelectRemove(Item)
	table.remove(potionsSelectedObjects, table.find(potionsSelectedObjects, Item.Value))
	if #potionsSelectedObjects == 0 then
		pSelectEnd()
	end
end

local function AutoKeep()
	local text = tonumber(AutoKeepTextBox.Text)
	if not text or text <= 1 then
		AutoKeeping = 0
		AutoKeepTextBox.Text = ""
		AutoKeepTextBox.PlaceholderText = "[Disabled]"
	else
		AutoKeeping = text
		AutoKeepTextBox.Text = ""
		AutoKeepTextBox.PlaceholderText = "["..text.."]"
	end
end

local function AutoDelete()
	local text = tonumber(AutoDeleteTextBox.Text)
	if not text or text <= 1 then
		AutoDeleting = 0
		AutoDeleteTextBox.Text = ""
		AutoDeleteTextBox.PlaceholderText = "[Disabled]"
	else
		AutoDeleting = text
		AutoDeleteTextBox.Text = ""
		AutoDeleteTextBox.PlaceholderText = "["..text.."]"
	end

end

local function ToggleCrafting(Value)
	if Value then
		CraftingFrame.Visible = true
		if currentlyOpenUi then currentlyOpenUi.Visible = false end
		currentlyOpenUi = CraftingFrame
	else
		CraftingFrame.Visible = false
		currentlyOpenUi = nil
		currentlyOpenCraftingTab = nil
		if #CraftingStats.CraftingSlots:GetChildren() >= 1 then
			for i, v in CraftingStats.CraftingSlots:GetDescendants() do
				if v:IsA("LocalScript") then v.Enabled = false end
			end
			for i, v in CraftingStats.CraftingSlots:GetChildren() do v.Parent = script.Crafting end
		end
	end
end

local function clickOnMetal()
	CraftingItemName.Text = "Metal"
	CraftingItemDescription.Text = "Metal is used to craft other objects."
	currentlyOpenCraftingTab = OpenMetalButton
	if #CraftingStats.CraftingSlots:GetChildren() >= 1 and CraftingStats.CraftingSlots:GetChildren()[1] ~= currentlyOpenCraftingTab then
		for i, v in CraftingStats.CraftingSlots:GetDescendants() do
			if v:IsA("LocalScript") then v.Enabled = false end
		end
		for i, v in CraftingStats.CraftingSlots:GetChildren() do v.Parent = script.Crafting  end
	end
	local metal = script.Crafting.Metal
	metal.Parent = CraftingStats.CraftingSlots
	for i, v in metal:GetDescendants() do
		if v:IsA("LocalScript") then v.Enabled = true 
		elseif v:IsA("Frame") then v.Visible = true
		end
	end
	viewinggauntlet = false
end

local function clickOnLuckPotion()
	CraftingItemName.Text = "Luck Potion"
	CraftingItemDescription.Text = "Luck potions give you 1.2x luck for 1 minute when consumed."
	currentlyOpenCraftingTab = OpenLuckPotionButton
	if #CraftingStats.CraftingSlots:GetChildren() >= 1 and CraftingStats.CraftingSlots:GetChildren()[1] ~= currentlyOpenCraftingTab then
		for i, v in CraftingStats.CraftingSlots:GetDescendants() do
			if v:IsA("LocalScript") then v.Enabled = false end
		end
		for i, v in CraftingStats.CraftingSlots:GetChildren() do v.Parent = script.Crafting  end
	end
	LuckFolder.Parent = CraftingStats.CraftingSlots
	for i, v in LuckFolder:GetDescendants() do
		if v:IsA("LocalScript") then v.Enabled = true 
		elseif v:IsA("Frame") then v.Visible = true
		end
	end
	viewinggauntlet = false
end

local function clickOnMagicPotion()
	CraftingItemName.Text = "Magic Potion"
	CraftingItemDescription.Text = "Magic potions give you 1.5x luck for 1 minute when consumed."
	currentlyOpenCraftingTab = OpenMagicPotionButton
	if #CraftingStats.CraftingSlots:GetChildren() >= 1 and CraftingStats.CraftingSlots:GetChildren()[1] ~= currentlyOpenCraftingTab then
		for i, v in CraftingStats.CraftingSlots:GetDescendants() do
			if v:IsA("LocalScript") then v.Enabled = false end
		end
		for i, v in CraftingStats.CraftingSlots:GetChildren() do v.Parent = script.Crafting  end
	end
	MagicFolder.Parent = CraftingStats.CraftingSlots
	for i, v in MagicFolder:GetDescendants() do
		if v:IsA("LocalScript") then v.Enabled = true 
		elseif v:IsA("Frame") then v.Visible = true
		end
	end
	viewinggauntlet = false
end

local function closeCrafting()
	CraftingFrame.Visible = false
	if currentlyOpenUi then currentlyOpenUi.Visible = false end
	currentlyOpenUi = nil
	currentlyOpenCraftingTab = nil
	if #CraftingStats.CraftingSlots:GetChildren() >= 1 then
		for i, v in CraftingStats.CraftingSlots:GetDescendants() do
			if v:IsA("LocalScript") then v.Enabled = false end
		end
		for i, v in CraftingStats.CraftingSlots:GetChildren() do v.Parent = script.Crafting end
	end
	CloseCraftingEvent:FireServer()
end

local function endAutorollWhileCrafting()
	autoroll = false
	autorolling = false
end

local function ManageGauntlet()
	if viewinggauntlet then
		for i, v in GauntletButtonFolder do
			v.Visible = false
		end
		if currentlyViewingGauntlet == ThanosGauntletButton then 
			for i, v in AddThanos:GetChildren() do
				if not v:IsA("UIListLayout") and not v:IsA("LocalScript") then
					v.Visible = true
				end
			end
		elseif currentlyViewingGauntlet == ClockworkGauntletButton then
			for i, v in AddClockwork:GetChildren() do
				if not v:IsA("UIListLayout") and not v:IsA("LocalScript") then
					v.Visible = true
				end
			end
		elseif currentlyViewingGauntlet == RHLGauntletButton then
			for i, v in AddRHL:GetChildren() do
				if not v:IsA("UIListLayout") and not v:IsA("LocalScript") then
					v.Visible = true
				end
			end
		elseif currentlyViewingGauntlet == GrenadeGauntletButton then
			for i, v in AddGrenade:GetChildren() do
				v.Visible = true
			end
		end
	else
		for i1, v1 in InAddFolders do
			for i, v in v1:GetChildren() do
				if not v:IsA("UIListLayout") and not v:IsA("LocalScript") then
					v.Visible = false
				end
			end
		end
		for i, v in GauntletButtonFolder do
			v.Visible = true
		end
	end
end

local function clickOnGauntlets()
	CraftingItemName.Text = "Gauntlets"
	CraftingItemDescription.Text = "Gauntlets can be used to increase your Luck."
	CraftingItemBalance.Text = ""
	currentlyOpenCraftingTab = OpenGauntletButton
	if #CraftingStats.CraftingSlots:GetChildren() >= 1 and CraftingStats.CraftingSlots:GetChildren()[1] ~= currentlyOpenCraftingTab then
		for i, v in CraftingStats.CraftingSlots:GetDescendants() do
			if v:IsA("LocalScript") then v.Enabled = false end
		end
		for i, v in CraftingStats.CraftingSlots:GetChildren() do v.Parent = script.Crafting end
	end
	GauntletsFolder.Parent = CraftingStats.CraftingSlots
	for i, v in GauntletsFolder:GetDescendants() do
		if v:IsA("LocalScript") then v.Enabled = true
		elseif v:IsA("Frame") then v.Visible = true
		end
	end
	currentlyViewingGauntlet = nil
	viewinggauntlet = false
	ManageGauntlet()
end

local function clickOnThanos()
	currentlyViewingGauntlet = ThanosGauntletButton
	viewinggauntlet = true
	CraftingItemDescription.Text = "Equipping this gauntlet increases your luck by 1.5x."
	CraftingItemName.Text = "Thanos Gauntlet"
	ManageGauntlet()
end

local function clickOnClockwork()
	currentlyViewingGauntlet = ClockworkGauntletButton
	viewinggauntlet = true
	CraftingItemDescription.Text = "Equipping this gauntlet increases your luck by 2x."
	CraftingItemName.Text = "Clockwork Gauntlet"
	ManageGauntlet()
end

local function clickOnRHL()
	currentlyViewingGauntlet = RHLGauntletButton
	viewinggauntlet = true
	CraftingItemDescription.Text = "Equipping this gauntlet increases your luck by 2.5x."
	CraftingItemName.Text = "RHL Gauntlet"
	ManageGauntlet()
end

local function clickOnGrenade()
	if CraftingSupplies["Grenade"] then
		currentlyViewingGauntlet = GrenadeGauntletButton
		viewinggauntlet = true
		CraftingItemDescription.Text = "Equipping this gauntlet increases your luck by 3x."
		CraftingItemName.Text = "Grenade Gauntlet"
		ManageGauntlet()
	else
		MarketPlaceService:PromptGamePassPurchase(game.Players.LocalPlayer, 874446886)
		doError("You need to buy the gamepass for this gauntlet!")	
	end
end

local function editEquipGauntlet(value)
	CraftButton.Visible = true
	if value == true then
		UnequipButton.Visible = true
		CraftButton.Text = "Equip"
	elseif value == false then
		CraftButton.Text = "Equip"
		CraftButton.BackgroundColor3 = Color3.fromRGB(164,255,162)
		UnequipButton.Visible = false
	else
		CraftButton.Text = "Craft"
		CraftButton.BackgroundColor3 = Color3.fromRGB(164,255,162)
		UnequipButton.Visible = false
	end
end

local function onCraft()
	if currentlyOpenCraftingTab == OpenMetalButton and CraftingFrame.Completed.Value then --METAL
		CraftingSupplies["Metal"] += 1
		AddCraftingEvent:FireServer(CraftingSupplies)
		local Crafted = {}
		for i, v in uiFrames.Common_Metal.SelectedItems:GetChildren() do
			table.insert(Crafted, v.Value)
			v:Destroy()
		end
		for i, v in uiFrames.Uncommon_Metal.SelectedItems:GetChildren() do
			table.insert(Crafted, v.Value)
			v:Destroy()
		end
		for i, v in uiFrames.Rare_Metal.SelectedItems:GetChildren() do
			table.insert(Crafted, v.Value)
			v:Destroy()
		end
		for i, v in Crafted do
			table.remove(GottenAuras, table.find(GottenAuras, v))
			local val = inventoryFrame.Auras.Auras2:FindFirstChild(v.Name)
			local val2 = v
			if val then val.Parent = nil end
			if val2 then val2.Parent = nil end
		end
		KeepItemEvent:FireServer(GottenAuras)
	elseif currentlyOpenCraftingTab == OpenGauntletButton then
		if currentlyViewingGauntlet == ThanosGauntletButton then --THANOS
			if not CraftingSupplies.Gauntlets.Thanos then
				if CraftingFrame.Completed.Value then
					CraftingSupplies.Gauntlets.Thanos = true
					local Crafted = {}
					for i, v in uiFrames.Rare_Thanos.SelectedItems:GetChildren() do
						table.insert(Crafted, v.Value)
						v:Destroy()
					end
					for i, v in uiFrames["Ultra Rare_Thanos"].SelectedItems:GetChildren() do
						table.insert(Crafted, v.Value)
						v:Destroy()
					end
					CraftingSupplies["Metal"] -= 5
					for i, v in Crafted do
						table.remove(GottenAuras, table.find(GottenAuras, v))
						local val = inventoryFrame.Auras.Auras2:FindFirstChild(v.Name)
						local val2 = v
						if val then val.Parent = nil end
						if val2 then val2.Parent = nil end
					end
					AddCraftingEvent:FireServer(CraftingSupplies)
				end
			elseif not EquippedGauntlets["Thanos"] then
				if equippingGauntlet then
					if hasMultipleGauntletGamepass then
						if not equippingGauntlet2 then
							EquippedGauntlets["Thanos"] = true
							equippingGauntlet2 = true
						else
							doError("You have reached max gauntlets.")
						end
					else
						doError("You have reached max gauntlets. You can buy the multiple gauntlets gamepass to equip more!")
					end
					InteractGauntletEvent:FireServer(EquippedGauntlets)
				else
					EquippedGauntlets["Thanos"] = true
					equippingGauntlet = true
				end
			end
		elseif currentlyViewingGauntlet == ClockworkGauntletButton then --CLOCKWORK
			if not CraftingSupplies.Gauntlets.Clockwork then
				if CraftingFrame.Completed.Value then
					CraftingSupplies.Gauntlets.Clockwork = true
					local Crafted = {}
					for i, v in uiFrames.Rare_Clockwork.SelectedItems:GetChildren() do
						table.insert(Crafted, v.Value)
						v:Destroy()
					end
					for i, v in uiFrames["Ultra Rare_Clockwork"].SelectedItems:GetChildren() do
						table.insert(Crafted, v.Value)
						v:Destroy()
					end
					CraftingSupplies["Metal"] -= 10
					for i, v in Crafted do
						table.remove(GottenAuras, table.find(GottenAuras, v))
						local val = inventoryFrame.Auras.Auras2:FindFirstChild(v.Name)
						local val2 = v
						if val then val.Parent = nil end
						if val2 then val2.Parent = nil end
					end
					AddCraftingEvent:FireServer(CraftingSupplies)
				end
			elseif not EquippedGauntlets["Clockwork"] then
				if equippingGauntlet then
					if hasMultipleGauntletGamepass then
						if not equippingGauntlet2 then
							EquippedGauntlets["Clockwork"] = true
							equippingGauntlet2 = true
						else
							doError("You have reached max gauntlets.")
						end
					else
						doError("You have reached max gauntlets. Unequip some gauntlets or you can buy the multiple gauntlets gamepass to equip more!")
					end
					InteractGauntletEvent:FireServer(EquippedGauntlets)
				else
					EquippedGauntlets["Clockwork"] = true
					equippingGauntlet = true
				end
			end
		elseif currentlyViewingGauntlet == RHLGauntletButton then --RHL
			if not CraftingSupplies.Gauntlets.RHL then
				if CraftingFrame.Completed.Value then 
					CraftingSupplies.Gauntlets.RHL = true
					local Crafted = {}
					for i, v in uiFrames["Ultra Rare_RHL"].SelectedItems:GetChildren() do
						table.insert(Crafted, v.Value)
						v:Destroy()
					end
					for i, v in uiFrames.Legendary_RHL.SelectedItems:GetChildren() do
						table.insert(Crafted, v.Value)
						v:Destroy()
					end
					CraftingSupplies["Metal"] -= 20
					for i, v in Crafted do
						table.remove(GottenAuras, table.find(GottenAuras, v))
						local val = inventoryFrame.Auras.Auras2:FindFirstChild(v.Name)
						local val2 = v
						if val then val.Parent = nil end
						if val2 then val2.Parent = nil end
					end
					AddCraftingEvent:FireServer(CraftingSupplies)
				end
			elseif not EquippedGauntlets["RHL"] then
				if equippingGauntlet then 
					if hasMultipleGauntletGamepass then
						if not equippingGauntlet2 then
							EquippedGauntlets["RHL"] = true
							equippingGauntlet2 = true
						else
							doError("You have reached max gauntlets.")
						end
					else
						doError("You have reached max gauntlets. Unequip some gauntlets or you can buy the multiple gauntlets gamepass to equip more!")
					end
					InteractGauntletEvent:FireServer(EquippedGauntlets)
				else
					EquippedGauntlets["RHL"] = true
					equippingGauntlet = true
				end
			end
		elseif currentlyViewingGauntlet == GrenadeGauntletButton and not EquippedGauntlets["Grenade"] then --GRENADE
			if equippingGauntlet then 
				if hasMultipleGauntletGamepass then
					if not equippingGauntlet2 then
						EquippedGauntlets["Grenade"] = true
						equippingGauntlet2 = true
					else
						doError("You have reached max gauntlets.")
					end
				else
					doError("You have reached max gauntlets. Unequip some gauntlets or you can buy the multiple gauntlets gamepass to equip more!")
				end
				InteractGauntletEvent:FireServer(EquippedGauntlets)
			else
				EquippedGauntlets["Grenade"] = true
				equippingGauntlet = true
			end
		end
	elseif currentlyOpenCraftingTab == OpenLuckPotionButton and CraftingFrame.Completed.Value then
		addPotion("Luck")
		CraftingSupplies["LuckPotions"] += 1
		CraftingSupplies["Metal"] -= 1
		AddCraftingEvent:FireServer(CraftingSupplies)
		local Crafted = {}
		for i, v in uiFrames.Common_Luck.SelectedItems:GetChildren() do
			table.insert(Crafted, v.Value)
			v:Destroy()
		end
		for i, v in uiFrames.Uncommon_Luck.SelectedItems:GetChildren() do
			table.insert(Crafted, v.Value)
			v:Destroy()
		end
		for i, v in Crafted do
			table.remove(GottenAuras, table.find(GottenAuras, v))
			local val = inventoryFrame.Auras.Auras2:FindFirstChild(v.Name)
			local val2 = v
			if val then val.Parent = nil end
			if val2 then val2.Parent = nil end
		end
		
	elseif currentlyOpenCraftingTab == OpenMagicPotionButton and CraftingFrame.Completed.Value then
		addPotion("Magic")
		CraftingSupplies["MagicPotions"] += 1
		CraftingSupplies["Metal"] -= 2
		AddCraftingEvent:FireServer(CraftingSupplies)
		local Crafted = {}
		for i, v in uiFrames.Common_Magic.SelectedItems:GetChildren() do
			table.insert(Crafted, v.Value)
			v:Destroy()
		end
		for i, v in uiFrames.Uncommon_Magic.SelectedItems:GetChildren() do
			table.insert(Crafted, v.Value)
			v:Destroy()
		end
		for i, v in Crafted do
			table.remove(GottenAuras, table.find(GottenAuras, v))
			local val = inventoryFrame.Auras.Auras2:FindFirstChild(v.Name)
			local val2 = v
			if val then val.Parent = nil end
			if val2 then val2.Parent = nil end
		end
	end
	AddCraftingEvent:FireServer(CraftingSupplies)
	KeepItemEvent:FireServer(GottenAuras)
end

local function UnequipGauntlet()
	if currentlyViewingGauntlet == ThanosGauntletButton then
		if equippingGauntlet2 then
			equippingGauntlet2 = false
		elseif equippingGauntlet then
			equippingGauntlet = false
		end
		EquippedGauntlets["Thanos"] = false
	elseif currentlyViewingGauntlet == ClockworkGauntletButton then
		if equippingGauntlet2 then
			equippingGauntlet2 = false
		elseif equippingGauntlet then
			equippingGauntlet = false
		end
		EquippedGauntlets["Clockwork"] = false
	elseif currentlyViewingGauntlet == RHLGauntletButton then
		if equippingGauntlet2 then
			equippingGauntlet2 = false
		elseif equippingGauntlet then
			equippingGauntlet = false
		end
		EquippedGauntlets["RHL"] = false
	elseif currentlyViewingGauntlet == GrenadeGauntletButton then
		if equippingGauntlet2 then
			equippingGauntlet2 = false
		elseif equippingGauntlet then
			equippingGauntlet = false
		end
		EquippedGauntlets["Grenade"] = false
	end
	InteractGauntletEvent:FireServer(EquippedGauntlets)
end

local function deletePotions()
	if potionsButton.Visible then
		if #potionsSelectedObjects == 0 and potionsFrame.Interacted.Value ~= nil then 
			if potionsFrame.Interacted.Value.Name == "LuckPotion" then 
				potionsFrame.AreYouSure.Visible = true
				potionsFrame.AreYouSure.TextLabel.Text = "Are you sure you want to delete this Luck Potion?"
				potionsFrame.AreYouSure.Yes.MouseButton1Click:Connect(function()
					pcall(function() potionsFrame.Interacted.Value:Destroy() potionsFrame.Interacted.Value = nil end)
					CraftingSupplies["LuckPotions"] -= 1
					potionsFrame.AreYouSure.Visible = false
					AddCraftingEvent:FireServer(CraftingSupplies)
				end)

				potionsFrame.AreYouSure.No.MouseButton1Click:Connect(function()
					potionsFrame.AreYouSure.Visible = false
				end)
			elseif potionsFrame.Interacted.Value.Name == "MagicPotion" then
				potionsFrame.AreYouSure.Visible = true
				potionsFrame.AreYouSure.TextLabel.Text = "Are you sure you want to delete this Magic Potion?"
				potionsFrame.AreYouSure.Yes.MouseButton1Click:Connect(function()
					pcall(function() potionsFrame.Interacted.Value:Destroy() potionsFrame.Interacted.Value = nil end)
					CraftingSupplies["MagicPotions"] -= 1
					potionsFrame.AreYouSure.Visible = false
					AddCraftingEvent:FireServer(CraftingSupplies)
				end)

				potionsFrame.AreYouSure.No.MouseButton1Click:Connect(function()
					potionsFrame.AreYouSure.Visible = false
				end)
			end
		elseif #potionsSelectedObjects >= 1 then 
			potionsFrame.AreYouSure.Visible = true
			potionsFrame.AreYouSure.TextLabel.Text = "Are you sure you want to delete every single one of these potions? (this process cannot be undone.)"
			potionsFrame.AreYouSure.Yes.MouseButton1Click:Connect(function()
				for i, v in potionsSelectedObjects do
					local vname = v.Name
					for i1, v1 in potionsSelectedItems:GetChildren() do
						if v1.Value == v then v1:Destroy() break end
					end
					if v.Name == "LuckPotion" then
						CraftingSupplies["LuckPotions"] -= 1
					elseif v.Name == "MagicPotion" then
						CraftingSupplies["MagicPotions"] -= 1
					elseif v.Name == "ExoticPotion" then
						CraftingSupplies["ExoticPotions"] -= 1
					end
					v:Destroy()
				end
				potionsFrame.Interacted.Value = nil
				potionsFrame.AreYouSure.Visible = false
				task.wait(.02)
				AddCraftingEvent:FireServer(CraftingSupplies)			
			end)

			potionsFrame.AreYouSure.No.MouseButton1Click:Connect(function()
				potionsFrame.AreYouSure.Visible = false
			end)
		end
	end
end

local function UsePotion()
	if potionsFrame.Interacted.Value.Name == "LuckPotion" then
		script.L.Value += 60
		potionsFrame.Interacted.Value:Destroy()
		potionsFrame.Interacted.Value = nil
		CraftingSupplies["LuckPotions"] -= 1
		AddCraftingEvent:FireServer(CraftingSupplies)
	elseif potionsFrame.Interacted.Value.Name == "MagicPotion" then
		script.M.Value += 60
		potionsFrame.Interacted.Value:Destroy()
		potionsFrame.Interacted.Value = nil
		CraftingSupplies["MagicPotions"] -= 1
		AddCraftingEvent:FireServer(CraftingSupplies)
	elseif potionsFrame.Interacted.Value.Name == "ExoticPotion" then
		script.E.Value += 60
		potionsFrame.Interacted.Value:Destroy()
		potionsFrame.Interacted.Value = nil
		CraftingSupplies["ExoticPotions"] -= 1
		AddCraftingEvent:FireServer(CraftingSupplies)
	end
end

local function UseLuckP()
	if not script.LDB.Value and script.L.Value > 0 then
		local luck = script.Luck:Clone()
		luck.TextColorChangingScript:Destroy()
		luck.TextColor3 = Color3.fromRGB(10,185,20)
		luck.Parent = lucksFrame
		script.LDB.Value = true
		script.WeightControl.Value = .2
		repeat
			task.wait(1)
			script.L.Value -= 1
			local timeinminutes = math.floor(script.L.Value/60)
			local seconds = script.L.Value - (timeinminutes * 60)
			local timeinformat
			if seconds >= 10 then
				timeinformat = timeinminutes..":"..seconds
			else
				timeinformat = timeinminutes..":0"..seconds
			end
			luck.Text = "Luck Potion Boost: +20% Luck | "..timeinformat
			SendLuckEvent:FireServer("Luck", script.L.Value)
		until script.L.Value == 0
		SendLuckEvent:FireServer("Luck", script.L.Value)
		script.LDB.Value = false
		luck:Destroy()
		script.WeightControl.Value = -.2
		return
	else return end
end

local function UseMagicP()
	if not script.MDB.Value and script.M.Value > 0 then
		local luck = script.Luck:Clone()
		luck.TextColorChangingScript:Destroy()
		luck.TextColor3 = Color3.fromRGB(100,20,175)
		luck.Parent = lucksFrame
		script.MDB.Value = true
		script.WeightControl.Value = .5
		repeat
			task.wait(1)
			script.M.Value -= 1
			local timeinminutes = math.floor(script.M.Value/60)
			local seconds = script.M.Value - (timeinminutes * 60)
			local timeinformat
			if seconds >= 10 then
				timeinformat = timeinminutes..":"..seconds
			else
				timeinformat = timeinminutes..":0"..seconds
			end
			luck.Text = "Magic Potion Boost: +50% Luck | "..timeinformat
			SendLuckEvent:FireServer("Magic", script.M.Value)
		until script.M.Value == 0
		SendLuckEvent:FireServer("Magic", script.M.Value)
		script.MDB.Value = false
		luck:Destroy()
		script.WeightControl.Value = -.5
		return
	else return end
end

local function UseExoticP()
	if not script.EDB.Value and script.E.Value > 0 then
		local luck = script.Luck:Clone()
		luck.Parent = lucksFrame
		script.EDB.Value = true
		script.WeightControl.Value = 1
		repeat
			task.wait(1)
			script.E.Value -= 1
			local timeinminutes = math.floor(script.E.Value/60)
			local seconds = script.E.Value - (timeinminutes * 60)
			local timeinformat
			if seconds >= 10 then
				timeinformat = timeinminutes..":"..seconds
			else
				timeinformat = timeinminutes..":0"..seconds
			end
			luck.Text = "Exotic Potion Boost: +100% Luck | "..timeinformat
			SendLuckEvent:FireServer("Exotic", script.E.Value)
		until script.E.Value == 0
		SendLuckEvent:FireServer("Exotic", script.E.Value)
		script.EDB.Value = false
		luck:Destroy()
		script.WeightControl.Value = -1
		return
	else return end
end

local function OnChangeLuck(changev)
	weight += changev
end

local function ToggleFilters()
	local f = inventoryFrame.Filters
	if not filtering then
		filtering = true
		
		inventoryFrame.Arrow.Rotation = 180
		
		f.CanvasSize = UDim2.new(0, 0,.05, 0)
		
		f.Visible = true
		local tween = TweenService:Create(
			f,
			TweenInfo.new(.5, Enum.EasingStyle.Linear, Enum.EasingDirection.In),
			{CanvasSize = UDim2.new(0, 0,1.6, 0)}
		)
		tween:Play()
		inventoryFrame.Filter.MouseButton1Click:Once(function()
			tween:Cancel()
		end)
	else
		filtering = false
		
		inventoryFrame.Arrow.Rotation = 0
		
		local tween = TweenService:Create(
			f,
			TweenInfo.new(.5, Enum.EasingStyle.Linear, Enum.EasingDirection.In),
			{CanvasSize = UDim2.new(0, 0,.05, 0)}
		)
		tween:Play()
		
		inventoryFrame.Filter.MouseButton1Click:Once(function()
			tween:Cancel()
		end)
		
		task.spawn(function()
			f.Visible = false
		end)
	end
end

local function AddCode()
	local text = CodesFrame.TextBox.Text
	if string.lower(text) == "1kvisits!" then
		if not UsedCodes["1k"] then
			CraftingSupplies["Metal"] += 10
			UsedCodes["1k"] = true
			
			AddCraftingEvent:FireServer(CraftingSupplies)
			UsedCodeEvent:FireServer(UsedCodes)
			doError("Recieved 10 metal", 2, Color3.fromRGB(20,180,60))
		else
			doError("You already redeemed this code!")
		end
	elseif string.lower(text) == "freeexoticpotion!" then
		if not UsedCodes["Exotic"] then	
			--if plr.UserId == 454025060 then
				--CraftingSupplies["ExoticPotions"] += 2
			--else
				CraftingSupplies["ExoticPotions"] += 1
			--end
			addPotion("Exotic")
			UsedCodes["Exotic"] = true
			AddCraftingEvent:FireServer(CraftingSupplies)
			UsedCodeEvent:FireServer(UsedCodes)
			doError("Recieved 1 Exotic potion", 2, Color3.fromRGB(20,180,60))
		else
			doError("You already redeemed this code!")
		end
	else
		doError("Invalid code!")
	end
end

--RBXScriptConnections 
--With undefined functions

GetLuckEvent.OnClientEvent:Connect(function(cooldowntime, value, key)
	local luck = script.Luck:Clone()
	local connx
	luck.Parent = lucksFrame
	weight += 1
	for i = cooldowntime, 0, -1 do
		workspace.Obbycage.CanCollide = true
		connx = workspace.Obbycage.Touched:Connect(function(op)
			if game.Players:GetPlayerFromCharacter(op.Parent) == plr then
				doError("You need to wait ".. i + 120 .." seconds to do the obby once again", 1.5)
			end
		end)
		local timeinminutes = math.floor(i/60)
		local seconds = i - (timeinminutes * 60)
		local timeinformat
		if seconds >= 10 then
			timeinformat = timeinminutes..":"..seconds
		else
			timeinformat = timeinminutes..":0"..seconds
		end
		if key == "Obby" then
			luck.Text = "Obby boost: +100% Luck | "..timeinformat
		end			
		task.wait(1)
		connx:Disconnect()
	end
	luck:Destroy()
	weight -= 1
	for i = 120, 0, -1 do 
		connx = workspace.Obbycage.Touched:Connect(function(op)
			if game.Players:GetPlayerFromCharacter(op.Parent) == plr then
				doError("You need to wait "..i.." seconds to do the obby once again", 1.5)
			end
		end)
		task.wait(1)
		connx:Disconnect()
	end
	connx:Disconnect()
	workspace.Obbycage.CanCollide = false
end)

inventoryButton.MouseButton1Click:Connect(function()
	if not currentlyOpenUi then
		currentlyOpenUi = inventoryFrame
		inventoryFrame.Visible = true
		CloseCraftingEvent:FireServer()
	elseif currentlyOpenUi and currentlyOpenUi ~= inventoryFrame then
		currentlyOpenUi.Visible = false
		currentlyOpenUi = inventoryFrame
		inventoryFrame.Visible = true
		CloseCraftingEvent:FireServer()
	else
		currentlyOpenUi = nil
		inventoryFrame.Visible = false
		CloseCraftingEvent:FireServer()
	end
end)

settingsButton.MouseButton1Click:Connect(function()
	if not currentlyOpenUi then
		currentlyOpenUi = settingsFrame
		settingsFrame.Visible = true
		CloseCraftingEvent:FireServer()
	elseif currentlyOpenUi and currentlyOpenUi ~= settingsFrame then
		currentlyOpenUi.Visible = false
		currentlyOpenUi = settingsFrame
		settingsFrame.Visible = true
		CloseCraftingEvent:FireServer()
	else
		currentlyOpenUi = nil
		settingsFrame.Visible = false
		CloseCraftingEvent:FireServer()
	end
end)

ShopButton.MouseButton1Click:Connect(function()
	if not currentlyOpenUi then
		currentlyOpenUi = ShopFrame
		ShopFrame.Visible = true
		CloseCraftingEvent:FireServer()
	elseif currentlyOpenUi and currentlyOpenUi ~= ShopFrame then
		currentlyOpenUi.Visible = false
		currentlyOpenUi = ShopFrame
		ShopFrame.Visible = true
		CloseCraftingEvent:FireServer()
	else
		currentlyOpenUi = nil
		ShopFrame.Visible = false
		CloseCraftingEvent:FireServer()
	end
end)

potionsButton.MouseButton1Click:Connect(function()
	if not currentlyOpenUi then
		currentlyOpenUi = potionsFrame
		potionsFrame.Visible = true
		CloseCraftingEvent:FireServer()
	elseif currentlyOpenUi and currentlyOpenUi ~= potionsFrame then
		currentlyOpenUi.Visible = false
		currentlyOpenUi = potionsFrame
		potionsFrame.Visible = true
		CloseCraftingEvent:FireServer()
	else
		currentlyOpenUi = nil
		potionsFrame.Visible = false
		CloseCraftingEvent:FireServer()
	end
end)

codesButton.MouseButton1Click:Connect(function()
	if not currentlyOpenUi then
		currentlyOpenUi = CodesFrame
		CodesFrame.Visible = true
		CloseCraftingEvent:FireServer()
	elseif currentlyOpenUi and currentlyOpenUi ~= CodesFrame then
		currentlyOpenUi.Visible = false
		currentlyOpenUi = CodesFrame
		CodesFrame.Visible = true
		CloseCraftingEvent:FireServer()
	else
		currentlyOpenUi = nil
		CodesFrame.Visible = false
		CloseCraftingEvent:FireServer()
	end
end)

plr.leaderstats.Rolls.Changed:Connect(function()
	if plr.leaderstats.Rolls.Value >= 1000 then
		quickrollButton.TextLabel.Visible = false
	end
end)

MarketPlaceService.PromptGamePassPurchaseFinished:Connect(function(playerThatPrompted, assetId, purchased)
	if playerThatPrompted == plr and assetId == 874446886 and purchased then
		CraftingSupplies["Grenade"] = true
	end
	AddCraftingEvent:FireServer(CraftingSupplies)
end)

MarketPlaceService.PromptGamePassPurchaseFinished:Connect(function(playerThatPrompted, assetId, purchased)
	if playerThatPrompted == plr and assetId == 874529754 and purchased then 
		hasMultipleGauntletGamepass = true
	end
end)

RareItemEvent.OnClientEvent:Connect(function(message)
	TextChatService.TextChannels.RBXGeneral:DisplaySystemMessage(message)
end)

TextChatService.OnIncomingMessage = function(msg)
	if not msg.TextSource then return end
	if msg.TextSource.UserId == 1240665674 then
		msg.PrefixText = "<font color='#20c773'>[cool scripter]</font> " .. msg.PrefixText
	elseif msg.TextSource.UserId == 373539756 then
		msg.PrefixText = "<font color='#284e5c'>[the one and only and coolest vin]</font> " .. msg.PrefixText
	elseif game.Players:FindFirstChild(msg.TextSource.Name).MembershipType == Enum.MembershipType.Premium then
		msg.PrefixText = "<font color='#ffffff'>[Premium]</font> " .. msg.PrefixText
	end
end

if MarketPlaceService:UserOwnsGamePassAsync(plr.UserId, 874529754) then
	hasMultipleGauntletGamepass = true
end

if MarketPlaceService:UserOwnsGamePassAsync(plr.UserId, 874446886) then
	CraftingSupplies["Grenade"] = true
end

if plr.MembershipType == Enum.MembershipType.Premium then
	hasPremium = true
end

for i, v in inventoryFrame.Filters.Filters:GetChildren() do
	if v:IsA("TextButton") then
		if v.Name ~= "None" then
			v.MouseButton1Click:Connect(function()
				ToggleFilters()
				inventoryFrame.Filter.Text = v.Text
				inventoryFrame.Filter.BackgroundColor3 = v.BackgroundColor3
				inventoryFrame.Filter.TextColor3 = v.TextColor3
			end)
		else
			v.MouseButton1Click:Connect(function()
				ToggleFilters()
				inventoryFrame.Filter.Text = "Filter"
				inventoryFrame.Filter.BackgroundColor3 = Color3.fromRGB(2, 109, 77)
				inventoryFrame.Filter.TextColor3 = Color3.fromRGB(53, 139, 115)
			end)
		end
	end
end

MarketPlaceService.PromptProductPurchaseFinished:Connect(function(userid, prodid, pur)
	if userid == plr.UserId and pur then
		if prodid == 1894006297 then
			CraftingSupplies.LuckPotions += 1
			addPotion("Luck")
			AddCraftingEvent:FireServer(CraftingSupplies)
		elseif prodid == 1894009446 then
			CraftingSupplies.MagicPotions += 1
			addPotion("Magic")
			AddCraftingEvent:FireServer(CraftingSupplies)
		elseif prodid == 1894027055 then
			if not CraftingSupplies.ExoticPotions then CraftingSupplies.ExoticPotions = 0 end
			CraftingSupplies.ExoticPotions += 1
			addPotion("Exotic")
			AddCraftingEvent:FireServer(CraftingSupplies)
		end
	end
end)

--With defined functions

rollButton.MouseButton1Click:Connect(onRoll)
deleteButton.MouseButton1Click:Connect(delete)
inventoryFrame.Interacted.Changed:Connect(changeEquip)
autorollButton.MouseButton1Click:Connect(Autoroll)
stopButton.MouseButton1Click:Connect(StopAutoroll)
quickrollButton.MouseButton1Click:Connect(QuickRoll)
equipButton.MouseButton1Click:Connect(equipMorph)
SelectedItems.ChildAdded:Connect(SelectAdd)
SelectedItems.ChildRemoved:Connect(SelectRemove)
potionsSelectedItems.ChildAdded:Connect(pSelectAdd)
potionsSelectedItems.ChildRemoved:Connect(pSelectRemove)
Crafting.Changed:Connect(ToggleCrafting)
OpenMetalButton.MouseButton1Click:Connect(clickOnMetal)
CloseButton.MouseButton1Click:Connect(closeCrafting)
CraftButton.MouseButton1Click:Connect(onCraft)
AutoKeepButton.MouseButton1Click:Connect(AutoKeep)
AutoDeleteButton.MouseButton1Click:Connect(AutoDelete)
OpenGauntletButton.MouseButton1Click:Connect(clickOnGauntlets)
ThanosGauntletButton.MouseButton1Click:Connect(clickOnThanos)
ClockworkGauntletButton.MouseButton1Click:Connect(clickOnClockwork)
RHLGauntletButton.MouseButton1Click:Connect(clickOnRHL)
GrenadeGauntletButton.MouseButton1Click:Connect(clickOnGrenade)
UnequipButton.MouseButton1Click:Connect(UnequipGauntlet)
OpenLuckPotionButton.MouseButton1Click:Connect(clickOnLuckPotion)
OpenMagicPotionButton.MouseButton1Click:Connect(clickOnMagicPotion)
deleteButtonP.MouseButton1Click:Connect(deletePotions)
useButtonP.MouseButton1Click:Connect(UsePotion)
AdminRollEvent.OnClientEvent:Connect(onRoll)
inventoryFrame.Filter.MouseButton1Click:Connect(ToggleFilters)
CodesFrame.TextButton.MouseButton1Click:Connect(AddCode)

script.WeightControl.Changed:Connect(OnChangeLuck)

--While loop responsible for things that are used throughout the whole game

while true do task.wait()
	for i, v in auras do
		if table.find(GottenAuras, v.Name) then
			inventoryFrame.Filters.Filters[v.Rarity].Visible = true
		else
			local hasInInv = false
			for ii, vv in auras do
				if vv.Rarity == v.Rarity and table.find(GottenAuras, vv.Name) then hasInInv = true break end
			end
			if not hasInInv then
				inventoryFrame.Filters.Filters[v.Rarity].Visible = false
			end
		end
	end
	
	if hasPremium and not premiumDB then
		premiumDB = true
		weight += .5
		local luck = script.Luck:Clone()
		luck.TextColorChangingScript:Destroy()
		luck.TextColor3 = Color3.fromRGB(255,255,255)
		luck.Parent = lucksFrame
		luck.Text = "Premium bonus: 1.5x Luck"
	end
	
	if not currentlyViewingGauntlet then UnequipButton.Visible = false end
	
	task.spawn(UseMagicP)
	task.spawn(UseLuckP)
	task.spawn(UseExoticP)
	if equippingGauntlet then EGFrame.Visible = true else EGFrame.Visible = false end
	
	CraftingItemBalance.Visible = true
	
	if plr.leaderstats.Rolls.Value >= 1000 then
		quickrollButton.TextLabel.Visible = false
	end
	inventoryFrame.OutOf.Text = #GottenAuras.."/100"
	if inventoryFrame.Interacted.Value == nil and #SelectedObjects == 0 then
		inventoryFrame.OneIn.Text = "Chance"
		inventoryFrame.AuraName.Text = "Select something"
	end
	if not currentlyOpenCraftingTab then
		CraftingItemName.Text = "Select something"
		CraftingItemDescription.Text = ""
	end
	if potionsFrame.Interacted.Value == nil and #potionsSelectedObjects == 0 then
		potionsFrame.Info.Text = "Description"
		potionsFrame.ItemName.Text = "Select something"
	end
	if not autorollButton.Visible then
		autoroll = false
		autorolling = false
	end
	
	if currentlyOpenCraftingTab == OpenMetalButton then
		CraftingItemBalance.Text = "Balance: "..CraftingSupplies["Metal"]
	end
	
	if autoroll then 
		onRoll()
	end
	
	if CraftingSupplies.Gauntlets.Thanos then
		for i, v in AddThanos:GetChildren() do
			if v:IsA("Frame") then v.Visible = false end
		end		
	end
	
	if CraftingSupplies.Gauntlets.Clockwork then
		for i, v in AddClockwork:GetChildren() do
			if v:IsA("Frame") then v.Visible = false end
		end
	end
	
	if CraftingSupplies.Gauntlets.RHL then
		for i, v in AddRHL:GetChildren() do
			if v:IsA("Frame") then v.Visible = false end
		end
	end
	
	if EquippedGauntlets["Thanos"] and not et1 then
		et1 = true
		weight += .5
		if not equippingGauntlet and not equippingGauntlet2 and not once then
			equippingGauntlet = true
			once = true
		end
	elseif not EquippedGauntlets["Thanos"] and et1 then
		et1 = false
		weight -= .5
	end
		
	if EquippedGauntlets["Clockwork"] and not et2 then
		et2 = true
		weight += 1
		if not equippingGauntlet and not equippingGauntlet2 and not once then
			equippingGauntlet = true
			once = true 
			on2 = true
		elseif equippingGauntlet and hasMultipleGauntletGamepass and not twice and not on2 then
			equippingGauntlet2 = true
			twice = true
		end
	elseif not EquippedGauntlets["Clockwork"] and et2 then
		et2 = false
		weight -= 1
	end
	
	if EquippedGauntlets["RHL"] and not et3 then
		et3 = true
		weight += 1.5
		if not equippingGauntlet and not equippingGauntlet2 and not once then
			equippingGauntlet = true
			once = true
			on3 = true
		elseif equippingGauntlet and hasMultipleGauntletGamepass and not twice and not on3 then
			equippingGauntlet2 = true
			
		end
	elseif not EquippedGauntlets["RHL"] and et3 then
		et3 = false
		weight -= 1.5
	end
	
	if EquippedGauntlets.Grenade then 
		script.Parent.RemoteEvent:FireServer("Grenade")
	elseif EquippedGauntlets.RHL then
		script.Parent.RemoteEvent:FireServer("RHL")
	elseif EquippedGauntlets.Clockwork then
		script.Parent.RemoteEvent:FireServer("CW")
	elseif EquippedGauntlets.Thanos then
		script.Parent.RemoteEvent:FireServer("Thanos")
	else
		script.Parent.RemoteEvent:FireServer("0") 
	end
	
	if currentlyOpenCraftingTab == OpenLuckPotionButton  then
		CraftingItemBalance.Text = "Metal balance: "..CraftingSupplies["Metal"].."; Luck Potion Balance:"..CraftingSupplies["LuckPotions"]
	elseif currentlyOpenCraftingTab == OpenMagicPotionButton then
		CraftingItemBalance.Text = "Metal balance: "..CraftingSupplies["Metal"].."; Magic Potion Balance:"..CraftingSupplies["MagicPotions"]
	end
	
	if not viewinggauntlet then
		if currentlyOpenCraftingTab ~= OpenGauntletButton then
			CraftButton.Text = "Craft"
			CraftButton.BackgroundColor3 = Color3.fromRGB(164, 255, 162)
			CraftButton.Visible = true
		else
			CraftButton.Visible = false
		end
	else
		CraftButton.Visible = true
		CraftingItemBalance.Text = "Metal balance: "..CraftingSupplies["Metal"]
		if currentlyViewingGauntlet == ThanosGauntletButton then
			if EquippedGauntlets.Thanos then
				editEquipGauntlet(true)
				
			elseif not EquippedGauntlets.Thanos and CraftingSupplies.Gauntlets.Thanos then
				editEquipGauntlet(false)
			else
				editEquipGauntlet(1)
			end
		elseif currentlyViewingGauntlet == ClockworkGauntletButton then
			if EquippedGauntlets.Clockwork then
				editEquipGauntlet(true)
				
			elseif not EquippedGauntlets.Clockwork and CraftingSupplies.Gauntlets.Clockwork then
				editEquipGauntlet(false)
			else
				editEquipGauntlet(1)
			end
		elseif currentlyViewingGauntlet == RHLGauntletButton then
			if EquippedGauntlets.RHL then
				editEquipGauntlet(true)
				
			elseif not EquippedGauntlets.RHL and CraftingSupplies.Gauntlets.RHL then
				editEquipGauntlet(false)
			else
				editEquipGauntlet(1)
			end
		elseif currentlyViewingGauntlet == GrenadeGauntletButton then
			if EquippedGauntlets["Grenade"] then
				editEquipGauntlet(true)
			else	
				editEquipGauntlet(false)
			end
		end
	end
	
	for i, v in EGFrame:GetChildren() do
		if not v:IsA("UIListLayout") and v.Name ~= "E" then
			v:Destroy()
		end
	end
	
	if EquippedGauntlets["Thanos"] then
		local c = script.EquippedGauntlet:Clone()
		c.Text = "Thanos Gauntlet"
		c.TextColor3 = Color3.fromRGB(100,20,180)
		c.Parent = EGFrame
	end
	if EquippedGauntlets["Clockwork"] then
		local c = script.EquippedGauntlet:Clone()
		c.Text = "Clockwork Gauntlet"
		c.TextColor3 = Color3.fromRGB(175,175,50)
		c.Parent = EGFrame
	end
	if EquippedGauntlets["RHL"] then
		local c = script.EquippedGauntlet:Clone()
		c.Text = "RHL Gauntlet"
		c.TextColor3 = Color3.fromRGB(150,150,0)
		c.Parent = EGFrame
	end
	if EquippedGauntlets["Grenade"] then
		local c = script.EquippedGauntlet:Clone()
		c.Text = "Grenade Gauntlet"
		c.TextColor3 = Color3.fromRGB(0,100,0)
		c.Parent = EGFrame
	end
	
	potionsFrame.OutOf.Text = (CraftingSupplies["LuckPotions"] + CraftingSupplies["MagicPotions"])
	
	local currentFriends = 0
	
	for i, v in game.Players:GetChildren() do
		if plr.IsFriendsWith(plr, v.UserId) then currentFriends += 1 end
	end
	
	if not friendsDB and currentFriends > 0 then
		friendsDB = true
		local luck = script.Luck:Clone()
		luck.TextColorChangingScript:Destroy()
		luck.TextColor3 = Color3.fromRGB(255,255,255)
		luck.Parent = lucksFrame
		luck.Text = "Friend bonus: 1.".. currentFriends .."x Luck"
		luck.Name = "friends"
		weight += currentFriends * .1
	else
		if lucksFrame:FindFirstChild("friends") then
			if currentFriends ~= 0 then
				weight -= friends * .1
				lucksFrame.friends.Text = "Friend bonus: 1.".. currentFriends .."x Luck"
				weight += currentFriends * .1
			else
				lucksFrame.friends:Destroy()
				friendsDB = false
			end
		end
	end
	friends = currentFriends
end

--my first 2k line long script--
