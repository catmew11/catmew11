local rep = game:GetService("ReplicatedStorage")
local uis = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local ts = game:GetService("TweenService")

local plr = game:GetService("Players").LocalPlayer
local char = plr.Character
local hum = char:WaitForChild("Humanoid")
local hrp = char:WaitForChild("HumanoidRootPart")
local remote = rep:WaitForChild("Remotes"):WaitForChild("Standless")
local CdGui = plr.PlayerGui.CdGui

local Disa = false
local Holding = false
local cumbu = 1
local blocking = false


local CanM1 = true
local CanM2 = true
local CanBlock = true
local CanE = true
local CanR = true
local CanG = true
local CanH = true
local CanT = true
local CanY = true

local CdM1 = 0.25
local CdPunch = 2
local CdM2 = 4
local CdF = 1.5
local CdE = 8
local CdR = 12
local CdG = 16
local CdH = 120
local CdT = 19
local CdY = 24


local NameM1 = "Punch"
local NameM2 = "Strong Punch"
local NameF = "Block"
local NameE = "Human's Barrage"
local NameR = "Drop Kick"
local NameG = "Angry Throw"
local NameH = "Last Chance"
local NameT = "Pan Attack"
local NameY = "Skull Crusher"


local EHold = false


local function state()
	if (not char:FindFirstChild("Block") and not char:FindFirstChild("Gobal") and not char:FindFirstChild("Stun")) then
		return true
	end
end
local function CDGUI(Key,Name,CD)
	local gui = rep.Guis.CDGui:Clone()
	gui.Parent = CdGui
	gui.CdText.Text = Key.." ".."/".." "..Name
	gui.CDFrame.CurCD.Size = UDim2.fromScale(1,1)
	ts:Create(gui.CDFrame.CurCD,TweenInfo.new(CD-0.1,Enum.EasingStyle.Linear,Enum.EasingDirection.Out,0,false,0),{Size = UDim2.fromScale(0,1)}):Play()
	task.spawn(function()
		wait(CD-0.1)
		gui:Destroy()
	end)
end

uis.InputBegan:Connect(function(input,gpe)
	if gpe then return end
	if Holding == true then return end
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		if state() then
			if CanM1 == true then
				if cumbu < 5 then
					CanM1 = false
					local gobal = Instance.new("IntValue",char)
					gobal.Name = "Gobal"
					local Punching = Instance.new("IntValue",char)
					Punching.Name = "Punching"
					game.Debris:AddItem(Punching,0.9)
					print(cumbu)
					remote:FireServer("Punch",cumbu)
					cumbu = cumbu + 1
					hum.WalkSpeed = 4
					hum.JumpPower = 0
					task.spawn(function()
						wait(1)
						if not char:FindFirstChild("Punching") then
							cumbu = 1
							print("reseted")
						end
					end)
					task.spawn(function()
						wait(CdM1)
						gobal:Destroy()
						if not char:FindFirstChild("Stun") then
							hum.WalkSpeed = 12
							hum.JumpPower = 50
						end
						CanM1 = true
					end)
				elseif cumbu == 5 then
					CanM1 = false
					local gobal = Instance.new("IntValue",char)
					gobal.Name = "Gobal"
					remote:FireServer("Punch",cumbu)
					cumbu = 1
					hum.WalkSpeed = 4
					hum.JumpPower = 0
					task.spawn(function()
						wait(CdM1)
						gobal:Destroy()
						if not char:FindFirstChild("Stun") then
							hum.WalkSpeed = 12
							hum.JumpPower = 50
						end
						task.spawn(function()
							CDGUI("M1",NameM1,CdPunch)
						end)
						wait(CdPunch)
						CanM1 = true
					end)
				end
			end
		end
	end
	if input.UserInputType == Enum.UserInputType.MouseButton2 then
		if state() then
			if CanM2 == true then
				CanM2 = false
				local gobal = Instance.new("IntValue",char)
				gobal.Name = "Gobal"
				remote:FireServer("StrongPunch")
				hum.WalkSpeed = 4
				hum.JumpPower = 0
				wait(0.75)
				gobal:Destroy()
				if not char:FindFirstChild("Stun") then
					hum.WalkSpeed = 12
					hum.JumpPower = 50
				end
				task.spawn(function()
					CDGUI("M2",NameM2,CdM2)
				end)
				wait(CdM2)
				CanM2 = true
			end
		end
	end
	if input.KeyCode == Enum.KeyCode.F then
		if state() then
			if CanBlock == true then
				CanBlock = false
				blocking = true
				remote:FireServer("Block")
			end
		end
	end
	if input.KeyCode == Enum.KeyCode.E then
		if state() then
			if CanE == true then
				CanE = false
				EHold = true
				local gobal = Instance.new("IntValue",char)
				gobal.Name = "Gobal"
				hum.WalkSpeed = 4
				hum.JumpPower = 0
				remote:FireServer("Barrage")
			end
		end
	end
	if input.KeyCode == Enum.KeyCode.R then
		if state() then
			if CanR == true then
				CanR = false
				local gobal = Instance.new("IntValue",char)
				gobal.Name = "Gobal"
				hum.WalkSpeed = 4
				hum.JumpPower = 0
				remote:FireServer("DropKick")
				task.spawn(function()
					wait(1)
					gobal:Destroy()
					if not char:FindFirstChild("Stun") then
						hum.WalkSpeed = 12
						hum.JumpPower = 50
					end
					task.spawn(function()
						CDGUI("R",NameR,CdR)
					end)
					wait(CdR)
					CanR = true
				end)
				task.spawn(function()
					wait(.2)
					if not char:FindFirstChild("Stun") then
						hrp.Velocity = hrp.CFrame.LookVector * 100
						ts:Create(hrp,TweenInfo.new(0.65,Enum.EasingStyle.Linear,Enum.EasingDirection.Out,0,false,0),{Velocity = hrp.CFrame.LookVector * 0}):Play()
					end
				end)
			end
		end
	end
	if input.KeyCode == Enum.KeyCode.G then
		if state() then
			if CanG == true then
				CanG = false
				local gobal = Instance.new("IntValue",char)
				gobal.Name = "Gobal"
				hum.WalkSpeed = 0
				hum.JumpPower = 0
				remote:FireServer("AngryThrow")
			end
		end
	end
	if input.KeyCode == Enum.KeyCode.H then
		if state() then
			if CanH == true and hum.Health <= ((hum.MaxHealth*30)/100) then
				CanH = false
				local gobal = Instance.new("IntValue",char)
				gobal.Name = "Gobal"
				hum.WalkSpeed = 0
				hum.JumpPower = 0
				remote:FireServer("LastChance")
			end
		end
	end
	if input.KeyCode == Enum.KeyCode.T then
		if state() then
			if CanT == true then
				CanT = false
				local gobal = Instance.new("IntValue",char)
				gobal.Name = "Gobal"
				hum.WalkSpeed = 4
				hum.JumpPower = 0
				remote:FireServer("PanAttack")
			end
		end
	end
	if input.KeyCode == Enum.KeyCode.Y then
		if state() then
			if CanY == true then
				CanY = false
				local gobal = Instance.new("IntValue",char)
				gobal.Name = "Gobal"
				hum.WalkSpeed = 0
				hum.JumpPower = 0
				remote:FireServer("SkullCrusher")
			end
		end
	end
end)


uis.InputEnded:Connect(function(input,gpe)
	if gpe then return end
	if input.KeyCode == Enum.KeyCode.F then
		if blocking == true then
			blocking = false
			remote:FireServer("Unblock")
			task.spawn(function()
				CDGUI("F",NameF,CdF)
			end)
			wait(CdF)
			CanBlock = true
		end
	end
	if input.KeyCode == Enum.KeyCode.E then
		if EHold == true then
			EHold = false
			remote:FireServer("BarrageEnd")
		end
	end
end)

remote.OnClientEvent:Connect(function(wut)
	if wut == "DoneE" then
		if char:FindFirstChild("Gobal") then
			char:FindFirstChild("Gobal"):Destroy()
			if EHold == true then
				EHold = false
			end
			if not char:FindFirstChild("Stun") then
				hum.WalkSpeed = 12
				hum.JumpPower = 50
			end
			task.spawn(function()
				CDGUI("E",NameE,CdE)
			end)
			wait(CdE)
			CanE = true
		end
	end
	if wut == "DoneG" then
		if char:FindFirstChild("Gobal") then
			char:FindFirstChild("Gobal"):Destroy()
			if not char:FindFirstChild("Stun") then
				hum.WalkSpeed = 12
				hum.JumpPower = 50
			end
			task.spawn(function()
				CDGUI("G",NameG,CdG)
			end)
			wait(CdG)
			CanG = true
		end
	end
	if wut == "DoneH" then
		if char:FindFirstChild("Gobal") then
			char:FindFirstChild("Gobal"):Destroy()
			if not char:FindFirstChild("Stun") then
				hum.WalkSpeed = 12
				hum.JumpPower = 50
			end
			task.spawn(function()
				CDGUI("H",NameH,CdH)
			end)
			wait(CdH)
			CanH = true
		end
	end
	if wut == "DoneT" then
		if char:FindFirstChild("Gobal") then
			char:FindFirstChild("Gobal"):Destroy()
			if not char:FindFirstChild("Stun") then
				hum.WalkSpeed = 12
				hum.JumpPower = 50
			end
			task.spawn(function()
				CDGUI("T",NameT,CdT)
			end)
			wait(CdT)
			CanT = true
		end
	end
	if wut == "DoneY" then
		if char:FindFirstChild("Gobal") then
			char:FindFirstChild("Gobal"):Destroy()
			if not char:FindFirstChild("Stun") then
				hum.WalkSpeed = 12
				hum.JumpPower = 50
			end
			task.spawn(function()
				CDGUI("Y",NameY,CdY)
			end)
			wait(CdY)
			CanY = true
		end
	end
end)

char.ChildAdded:Connect(function(add)
	if add:IsA("Tool") then
		if Holding == false then
			Holding = true
		end
	end
	if char:FindFirstChild("Gobal") then
		game.StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.Backpack,false)
		repeat
			wait()
		until not char:FindFirstChild("Gobal")
		game.StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.Backpack,true)
	end
end)

RS.Heartbeat:Connect(function()
	if char:FindFirstChild("Gobal") then
		if Disa == false then
			Disa = true
			game.StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.Backpack,false)
			repeat
				wait()
			until not char:FindFirstChild("Gobal")
			game.StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.Backpack,true)
			Disa = false
		end
	end
end)
char.ChildRemoved:Connect(function(add)
	if add.Name == "Block" and blocking == true then
		blocking = false
		task.spawn(function()
			CDGUI("F",NameF,CdF)
		end)
		wait(CdF)
		CanBlock = true
	end
	if add:IsA("Tool") then
		if Holding == true then
			Holding = false
		end
	end
end)

hum.Died:Connect(function()
	script.Enabled = false
end)

uis.JumpRequest:Connect(function()
	if char:FindFirstChild("Punching") then
		hum:SetStateEnabled(Enum.HumanoidStateType.Jumping,false)
	else
		hum:SetStateEnabled(Enum.HumanoidStateType.Jumping,true)
	end
end)
