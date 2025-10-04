-- AdminModule (ModuleScript)
local AdminModule = {}

-- Lista inicial de admins: coloque os UserIds dos administradores aqui.
-- Ex: 12345678
AdminModule.Admins = {
    -- Coloque seu UserId aqui para ser admin
    [12345678] = true,
}

-- Ajustes
AdminModule.CommandPrefix = ":" -- prefixo para comandos no chat
AdminModule.AdminRankName = "Admin" -- (usado caso queira expandir)

-- Função para checar admin
function AdminModule:IsAdmin(userId)
    return type(userId) == "number" and self.Admins[userId] == true
end

-- Add / remove admin (server-only)
function AdminModule:AddAdmin(userId)
    if type(userId) ~= "number" then return false end
    self.Admins[userId] = true
    return true
end

function AdminModule:RemoveAdmin(userId)
    if type(userId) ~= "number" then return false end
    self.Admins[userId] = nil
    return true
end

-- Utils: encontra jogador por nome/parte do nome (case-insensitive)
function AdminModule:FindPlayerByName(search)
    if not search or search == "" then return nil end
    local Players = game:GetService("Players")
    search = string.lower(search)
    -- tenta match exato
    for _,p in pairs(Players:GetPlayers()) do
        if string.lower(p.Name) == search then
            return p
        end
    end
    -- tenta partial match
    for _,p in pairs(Players:GetPlayers()) do
        if string.find(string.lower(p.Name), search, 1, true) then
            return p
        end
    end
    return nil
end

return AdminModule
