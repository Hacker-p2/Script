```lua
local Players=game:GetService("Players")
local Workspace=game:GetService("Workspace")
local player=Players.LocalPlayer
local playerGui=player:WaitForChild("PlayerGui")

local eggData={
  common={"Golden Lab","Dog","Bunny"},
  uncommon={"Black Bunny","Chicken","Cat","Deer"},
  bee={"Bee","Honey Bee","Petal Bee","Queen Bee"}
}

local ScreenGui=Instance.new("ScreenGui")
ScreenGui.Name="EggPredictorUI"
ScreenGui.Parent=playerGui

local Frame=Instance.new("Frame")
Frame.Size=UDim2.new(0,300,0,150)
Frame.Position=UDim2.new(0.5,-150,0.1,0)
Frame.BackgroundColor3=Color3.fromRGB(35,35,35)
Frame.BorderSizePixel=0
Frame.Parent=ScreenGui

local Title=Instance.new("TextLabel")
Title.Size=UDim2.new(1,0,0,30)
Title.BackgroundTransparency=1
Title.Text="Egg Predictor"
Title.TextColor3=Color3.new(1,1,1)
Title.Font=Enum.Font.GothamBold
Title.TextScaled=true
Title.Parent=Frame
```

---

*Chat 2/2:*

```lua
local InfoLabel=Instance.new("TextLabel")
InfoLabel.Size=UDim2.new(1,-20,1,-40)
InfoLabel.Position=UDim2.new(0,10,0,35)
InfoLabel.BackgroundTransparency=1
InfoLabel.TextColor3=Color3.new(1,1,1)
InfoLabel.TextWrapped=true
InfoLabel.TextYAlignment=Enum.TextYAlignment.TopInfoLabel.Font=Enum.Font.Gotham
InfoLabel.TextScaled=false
InfoLabel.Text="Scanning eggs..."
InfoLabel.Parent=Frame

local function getEggType(name)
  local n=name:lower()
  if n:find("common")then return"common"end
  if n:find("uncommon")then return"uncommon"end
  if n:find("bee")then return"bee"end
  return nil
end

local function predictPet(eggType)
  local pets=eggData[eggType]
  if not pets then return"Unknown"end
  math.randomseed(tick()+#eggType*100)
  return pets[math.random(1,#pets)]
end

while true do
  local predictions={}
  for _,obj in pairs(Workspace:GetChildren())do
    if obj:IsA("Model")then
      local eggType=getEggType(obj.Name)
      if eggType then
        local pet=predictPet(eggType)
        table.insert(predictions,obj.Name..": "..pet)
      end
    end
  end
  if #predictions==0 then
    InfoLabel.Text="No eggs found nearby."
  else
    InfoLabel.Text=table.concat(predictions,"\n")
  end
  wait(3)
end
```
