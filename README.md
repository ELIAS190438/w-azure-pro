-- Biblioteca UI estilo W Azure
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/bloodball/-back-ups-for-libs/main/w-azure"))()
local Window = Library:CreateWindow("💥 Blox Fruits | Hub Completo")
local FarmTab = Window:AddTab("⚔️ Farm")
local MiscTab = Window:AddTab("📦 Extras")
local SettingsTab = Window:AddTab("⚙️ Config")

-- Variáveis
_G.AutoFarm = false
_G.AutoClick = false
_G.AutoHaki = true
_G.FruitGrab = false

-- Função: Auto Haki (buso)
function EnableHaki()
    pcall(function()
        if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
        end
    end)
end

-- Auto Click
function StartAutoClick()
    spawn(function()
        while _G.AutoClick do
            pcall(function()
                local tool = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool")
                if tool then tool:Activate() end
            end)
            wait(0.1)
        end
    end)
end

-- Auto Farm
function StartAutoFarm()
    spawn(function()
        while _G.AutoFarm do
            pcall(function()
                local player = game.Players.LocalPlayer
                local enemies = workspace.Enemies:GetChildren()

                for _, mob in pairs(enemies) do
                    if mob:FindFirstChild("Humanoid") and mob:FindFirstChild("HumanoidRootPart") and mob.Humanoid.Health > 0 then
                        repeat
                            if _G.AutoHaki then EnableHaki() end
                            player.Character.HumanoidRootPart.CFrame = mob.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
                            wait(0.1)
                        until mob.Humanoid.Health <= 0 or not _G.AutoFarm
                    end
                end
            end)
            wait(0.5)
        end
    end)
end

-- Pegar frutas no mapa
function GrabFruits()
    spawn(function()
        while _G.FruitGrab do
            pcall(function()
                for _, v in pairs(game.Workspace:GetChildren()) do
                    if string.find(v.Name, "Fruit") then
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
                        wait(1)
                    end
                end
            end)
            wait(15)
        end
    end)
end

-- Resgatar todos os códigos
function RedeemAllCodes()
    local codes = {
        "Sub2Fer999", "Enyu_is_Pro", "Magicbus", "JCWK", "Starcodeheo",
        "Bluxxy", "fudd10_v2", "FUDD10", "Bignews", "Sub2CaptainMaui",
        "Sub2NoobMaster123", "Sub2UncleKizaru", "Axiore", "TantaiGaming",
        "StrawHatMaine", "KittGaming", "THEGREATACE", "ADMIN_TROLL",
        "CHANDLER", "SECRET_ADMIN", "DRAGONABUSE", "EXP_5B"
    }

    for _, code in ipairs(codes) do
        pcall(function()
            game:GetService("ReplicatedStorage").Remotes.Redeem:InvokeServer(code)
        end)
        wait(0.5)
    end
end

-- UI: Botões e Toggles
FarmTab:AddSwitch("✅ Auto Farm NPC", function(bool)
    _G.AutoFarm = bool
    if bool then StartAutoFarm() end
end)

FarmTab:AddSwitch("⚔️ Auto Clicker", function(bool)
    _G.AutoClick = bool
    if bool then StartAutoClick() end
end)

FarmTab:AddSwitch("🍇 Pegar Frutas no Mapa", function(bool)
    _G.FruitGrab = bool
    if bool then GrabFruits() end
end)

MiscTab:AddButton("🎁 Resgatar Todos os Códigos", function()
    RedeemAllCodes()
end)

SettingsTab:AddSwitch("🎯 Ativar Haki Automaticamente", function(bool)
    _G.AutoHaki = bool
end)

SettingsTab:AddButton("❌ Fechar Painel", function()
    Library:Destroy()
end)
