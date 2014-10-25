#### Instance:GetFullName()
```lua
function Instance:GetFullName()
  local names = {}
  repeat
    table.insert(names, 1, self.Name)
    self = self.Parent
  until not self or self:IsA("ServiceProvider")
  return table.concat(names, ".")
end
```

#### Instance:FindFirstChild(string name, bool recursive = false)
```lua
function Instance:FindFirstChild(name, recursive)
  for _, child in pairs(self:GetChildren()) do
      if child.Name == name then
          return child
      end
      if recursive then
          local result = child:FindFirstChild(name, true)
          if result then
              return result
          end
      end
  end
  return nil
end
```

#### Instance:IsAncestorOf(Instance descendant)
```lua
function Instance:IsAncestorOf(descendant)
  return descendant:IsDescendantOf(self)
end
```

#### Instance:IsDescendantOf(Instance ancestor)
```lua
function Instance:IsDescendantOf(ancestor)
  if self.Parent == ancestor then
    return true
  end
  if self.Parent then
    return self.Parent:IsDescendantOf(ancestor)
  else
    return false
  end
end
```

#### Instance:WaitForChild(string childName)
```lua
function Instance:WaitForChild(childName)
  if self ~= game and not self:IsDescendantOf(game) then
    error('WaitForChild called on an Instance that is not in a DataModel.', 2)
  end
  local child = self:FindFirstChild(childName)
  while not child or child.Name ~= childName do
    child = self.ChildAdded:wait()
  end
  return child
end
```

#### Instance:Remove()
```lua
function Instance:Remove()
  self.Parent = nil
  for _, child in pairs(self:GetChildren()) do
    child:Remove()
  end
end
```

#### Instance:ClearAllChildren()
```lua
function Instance:ClearAllChildren()
  for _, child in pairs(self:GetChildren()) do
		child:Remove()
	end
end
```
####

#### Debris:AddItem(Instance item, double lifetime = 10)
```lua
function Debris:AddItem(item, lifetime)
  lifetime = lifetime or 10
  delay(lifetime, function()
    item:Remove()
  end)
end
```

#### Players:GetPlayers()
```lua
function Players:GetPlayer() do
	local players = {}
  for _, child in pairs(self:GetChildren()) do
		if child:IsA("Player") then
			table.insert(players, child)
		end
	end
	return players
end
```

#### Players:GetPlayerFromCharacter(Instance character)
```lua
function Players:GetPlayerFromCharacter(character)
	for _, player in pairs(self:GetPlayers()) do
		if player.Character == character then
			return player
		end
	end
	return nil
end
```