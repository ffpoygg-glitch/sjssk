-- สร้าง ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Hex0x_8000_Mode"
screenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

-- ตั้งค่าความยาวรวมที่ 8,000 ตัว
local maxTotalLength = 8000 

-- [1] ปุ่มเปิด/ปิด UI (Toggle)
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 60, 0, 30)
toggleBtn.Position = UDim2.new(0, 10, 0.45, 0)
toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 200)
toggleBtn.Text = "0x8K"
toggleBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.Parent = screenGui
Instance.new("UICorner", toggleBtn)

-- [2] หน้าต่างหลัก
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 350, 0, 300)
frame.Position = UDim2.new(0.5, -175, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(10, 15, 15)
frame.Visible = false
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui
Instance.new("UICorner", frame)

-- หัวข้อ
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 50)
title.Text = "HEX ENCODER (8,000 CHARS)"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.Parent = frame

-- ช่องใส่ ID
local input = Instance.new("TextBox")
input.Size = UDim2.new(0.85, 0, 0, 45)
input.Position = UDim2.new(0.075, 0, 0.22, 0)
input.PlaceholderText = "กรอก ID เพลง..."
input.BackgroundColor3 = Color3.fromRGB(30, 35, 35)
input.TextColor3 = Color3.fromRGB(255, 255, 255)
input.Font = Enum.Font.Code
input.Parent = frame
Instance.new("UICorner", input)

-- ปุ่ม Generate
local genBtn = Instance.new("TextButton")
genBtn.Size = UDim2.new(0.85, 0, 0, 45)
genBtn.Position = UDim2.new(0.075, 0, 0.42, 0)
genBtn.Text = "GENERATE 8,000 CHARS"
genBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 200)
genBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
genBtn.Font = Enum.Font.GothamBold
genBtn.Parent = frame
Instance.new("UICorner", genBtn)

-- ช่องแสดงตัวอย่าง (Preview)
local resultBox = Instance.new("TextBox")
resultBox.Size = UDim2.new(0.85, 0, 0, 60)
resultBox.Position = UDim2.new(0.075, 0, 0.62, 0)
resultBox.Text = "0x000..."
resultBox.TextEditable = false
resultBox.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
resultBox.TextColor3 = Color3.fromRGB(0, 255, 180)
resultBox.TextWrapped = true
resultBox.Font = Enum.Font.Code
resultBox.TextSize = 10
resultBox.Parent = frame

-- ปุ่มคัดลอก
local copyBtn = Instance.new("TextButton")
copyBtn.Size = UDim2.new(0.85, 0, 0, 40)
copyBtn.Position = UDim2.new(0.075, 0, 0.84, 0)
copyBtn.Text = "COPY ALL"
copyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
copyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
copyBtn.Font = Enum.Font.GothamBold
copyBtn.Parent = frame
Instance.new("UICorner", copyBtn)

--- LOGIC ---

local finalResult = ""

toggleBtn.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
end)

genBtn.MouseButton1Click:Connect(function()
    local idVal = tonumber(input.Text)
    if idVal then
        genBtn.Text = "ENCODING..."
        
        local idHex = string.format("%X", idVal)
        local prefix = "0x"
        local paddingCount = maxTotalLength - 2 - #idHex
        
        if paddingCount > 0 then
            finalResult = prefix .. string.rep("0", paddingCount) .. idHex
        else
            finalResult = prefix .. idHex
        end
        
        -- แสดงหัวท้ายให้ดู
        resultBox.Text = string.sub(finalResult, 1, 30) .. "..." .. string.sub(finalResult, -20)
        
        genBtn.Text = "READY (8,000)"
        wait(1)
        genBtn.Text = "GENERATE 8,000 CHARS"
    else
        input.Text = ""
        input.PlaceholderText = "กรุณาใส่ตัวเลข!"
    end
end)

copyBtn.MouseButton1Click:Connect(function()
    if finalResult ~= "" and setclipboard then
        setclipboard(finalResult)
        copyBtn.Text = "COPIED!"
        copyBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 100)
        wait(1.5)
        copyBtn.Text = "COPY ALL"
        copyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    end
end)

