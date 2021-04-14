local NotificationBindable = game:GetService("CoreGui").Domain.Notification
NotificationBindable:Fire("FunkyFriday autoplay - Enabled","GothamSemibold",Color3.fromRGB(45,45,45))
local mode = 0 -- 0 = Legit, wont hit 100% accurately : 1 = Rage, hits everythings with 100% accuracy
local maxdelay = .09 -- Maximum delay b4 hitting a note while using legit mode, recomending max is .16, any higher and youll start missing a lot

-- Main Script Below

local framework;
local funcs = {}
local islclosure = islclosure or is_l_closure
local getinfo = getinfo or debug.getinfo
local getupvalues = getupvalues or debug.getupvalues
local getconstants = getconstants or debug.getconstants
local marked = {}
local map = { [0] = 'Left', [1] = 'Down', [2] = 'Up', [3] = 'Right', }
local keys = { Up = Enum.KeyCode.W; Down = Enum.KeyCode.S; Left = Enum.KeyCode.A; Right = Enum.KeyCode.D; }
local runService = game:GetService('RunService')
local fastWait, fastSpawn do
    function fastWait(t)
        local d = 0;
        while d < t do
            d += runService.RenderStepped:wait()
        end
    end
    function fastSpawn(f)
        coroutine.wrap(f)()
    end
end

for i, v in next, getgc(true) do
    if type(v) == 'table' and rawget(v, 'GameUI') then
        framework = v;
    end
    if type(v) == 'function' and islclosure(v) then
        local info = debug.getinfo(v);
        if info.name == '' then continue end
        if info.source:match('%.Arrows$') then
            local constants = debug.getconstants(v);
            if table.find(constants, 'Right') and table.find(constants, 'NewThread') then
                funcs.KeyDown = v;
            elseif table.find(constants, 'Multiplier') and table.find(constants, 'MuteVoices') then
                funcs.KeyUp = v;
            end
        end
    end
    if framework and funcs.KeyUp and funcs.KeyDown then break end
end
if type(framework) ~= 'table' or (not rawget(framework, 'UI')) then
    return game.Players.LocalPlayer:Kick('Failed to locate framework.')
elseif (not (funcs.KeyDown and funcs.KeyUp)) then
    return game.Players.LocalPlayer:Kick('Failed to locate key functions.')
end
while runService.RenderStepped:wait() do
    for _, arrow in next, framework.UI.ActiveSections do
        if _ == 1 then
        else
            if arrow.Side ~= framework.UI.CurrentSide then continue end -- ignore the opponent's arrows
            if marked[arrow] then continue end
            local index = arrow.Data.Position % 4
            local position = map[index]
            if (not position) then continue end
            local distance = (1 - math.abs(arrow.Data.Time - framework.SongPlayer.CurrentlyPlaying.TimePosition)) * 100 -- get the "distance" or whatever
            if distance >= 95 then
                marked[arrow] = true;
                fastSpawn(function()
                    if mode == 0 then wait(Random.new():NextNumber(0.02, maxdelay)) else end
                    funcs.KeyDown(position)
                    if arrow.Data.Length > 0 then
                        fastWait(arrow.Data.Length)
                    else
                        fastWait(0.05)
                    end
                    funcs.KeyUp(position)
                    marked[arrow] = nil
                end)
            end
        end
    end
end
