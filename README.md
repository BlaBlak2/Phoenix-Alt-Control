if not game:IsLoaded() then
	game.Loaded:Wait()
end

local success,err = pcall(function()

Players = game:GetService('Players')
player = game.Players.LocalPlayer

local vu = game:GetService("VirtualUser")
game:GetService("Players").LocalPlayer.Idled:connect(function()
   vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
   wait(1)
   vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

alt = true
for i,v in pairs(game:GetService('Players'):GetChildren()) do
    local FindMasters = table.find(getgenv().Masters, v.UserId)
    if FindMasters then
	    if player.UserId == v.UserId then
		    alt = false
		    print('Currently a master')
        end
    end
end

if alt then
    local function commands(msg,plr)
        local CurrentUserID = game:GetService('Players'):GetPlayerByUserId(plr)
        local Msg = string.lower(msg)
        local SplitCMD = string.split(Msg,' ')
        local Lower = string.lower(player.Name)
        local Allowed = string.find(Lower, SplitCMD[2])
        local extracted = string.match(msg, ";(.*)")
        if Allowed then
            if string.find(SplitCMD[1], ':freeze') then
                player.Character.HumanoidRootPart.Anchored = true
            end
            if string.find(SplitCMD[1], ':unfreeze') then
                player.Character.HumanoidRootPart.Anchored = false
            end
            if string.find(SplitCMD[1], ':thaw') then
                player.Character.HumanoidRootPart.Anchored = false
            end
            if string.find(SplitCMD[1], ':kill') then
                game.Players.LocalPlayer.Character.Head:Remove()
            end
            if string.find(SplitCMD[1], ':kick') then
                    kickmsg = CurrentUserID.Name
                    player:Kick(getgenv().kickmsg)
                    wait(5)
                    while true do end
            end
            if string.find(SplitCMD[1], ':crash') then
                    while true do end
            end
            if string.find(SplitCMD[1], ':bring') then
                player.Character.HumanoidRootPart.CFrame = CFrame.new(game.Workspace:FindFirstChild(CurrentUserID.Name).HumanoidRootPart.Position)
            end
            if string.find(SplitCMD[1], ':ebring') then
                local plr1 = game.Players.LocalPlayer.Character
                local plr2 = game.Workspace:FindFirstChild(extracted)
                plr1.HumanoidRootPart.CFrame = plr2.HumanoidRootPart.CFrame * CFrame.new(0,2,0)
            end
            if string.find(SplitCMD[1], ':fling') then
                player.Character.HumanoidRootPart.Velocity = Vector3.new(500000,500000,500000)
            end
	        if string.find(SplitCMD[1], ':jump') then
		        game.Players.LocalPlayer.Character.Humanoid.Jump = true
            end
	        if string.find(SplitCMD[1], ':airlock') then
		        game.Players.LocalPlayer.Character.Humanoid.Jump = true
		        wait(.25)
		        player.Character.HumanoidRootPart.Anchored = true
            end
	        if string.find(SplitCMD[1], ':say') then
		        game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(extracted, "All")
            end
            if string.find(SplitCMD[1], ':orbit') then
		        local player2 = game.Workspace:FindFirstChild(CurrentUserID.Name).HumanoidRootPart
                local lpr = game.Players.LocalPlayer.Character.HumanoidRootPart
                local speed = 8
                local radius = 8 --- orbit size
                local eclipse = 1 --- width of orbit
                local rotation = CFrame.Angles(0,0,0) --only works for unanchored parts (not localplayer)
                local sin, cos = math.sin, math.cos
                local rotspeed = math.pi*2/speed
                eclipse = eclipse * radius
                local runservice = game:GetService('RunService')
                local rot = 0
                game:GetService('RunService').Stepped:connect(function(t, dt)
                    rot = rot + dt * rotspeed
                    lpr.CFrame = rotation * CFrame.new(sin(rot)*eclipse, 0, cos(rot)*radius) + player2.Position
                end)
            end
        end
    end

    Players.PlayerAdded:Connect(function(plr)
        local FindMasters = table.find(getgenv().Masters, plr.UserId)
        if FindMasters then
            print('found')
            plr.Chatted:Connect(function(msg)
                commands(msg, plr.UserId)
            end)
        end
    end)

    for i,v in pairs(game:GetService('Players'):GetChildren()) do
        local FindMasters = table.find(getgenv().Masters, v.UserId)
        if FindMasters then
            print('found')
            v.Chatted:Connect(function(msg)
                commands(msg, v.UserId)
            end)
        end
    end

end

end)
if err then print(err) end
