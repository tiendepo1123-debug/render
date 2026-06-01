-- 1. Khởi chạy Thư viện Rayfield UI
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "TIENHZIO HUB",
   LoadingTitle = "Đang Tải GPU Saver...",
   LoadingSubtitle = "by Tiến",
   ConfigurationSaving = {
      Enabled = false
   },
   Discord = {
      Enabled = false,
      Invite = "",
      RememberJoins = true
   },
   KeySystem = false
})

-- 2. Tạo Tab Tối Ưu Hệ Thống
local MainTab = Window:CreateTab("Tối Ưu Máy", 4483362458)

-- Variables phục vụ tính năng tắt render
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui") or game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
local screenName = "Rayfield_GPU_Saver_White"

-- 3. Tạo Nút Bật/Tắt (Toggle) gộp hoàn chỉnh trong Tab
MainTab:CreateToggle({
   Name = "Bật Màn Hình Trắng (Tắt GPU)",
   CurrentValue = false,
   Flag = "GpuSaverToggle",
   Callback = function(Value)
      if Value then
         -- [ CHẾ ĐỘ BẬT ]: Khóa render đồ họa & Phủ trắng màn hình game
         pcall(function()
            RunService:Set3DRenderingEnabled(false) -- Ép tắt render 3D ngầm để GPU nghỉ 100%
         end)
         
         if setfpscap then 
            pcall(function() setfpscap(1) end) -- Ép FPS về 1 để máy siêu mát
         end
         
         -- Xóa màn hình trắng cũ nếu còn sót
         if CoreGui:FindFirstChild(screenName) then 
            CoreGui[screenName]:Destroy() 
         end
         
         -- Tạo khung nền trắng đè lên game (Nhưng nằm DƯỚI Menu Rayfield để bạn vẫn bấm tắt được)
         local sg = Instance.new("ScreenGui")
         sg.Name = screenName
         sg.IgnoreGuiInset = true
         sg.DisplayOrder = 0 -- Đặt bằng 0 để nó nằm dưới cùng, không che mất menu Rayfield
         
         local fr = Instance.new("Frame")
         fr.Size = UDim2.new(15, 0, 15, 0) -- Kích thước siêu lớn để tràn hết toàn bộ màn hình
         fr.Position = UDim2.new(-2, 0, -2, 0)
         fr.BackgroundColor3 = Color3.fromRGB(255, 255, 255) -- Màu trắng tinh
         fr.BorderSizePixel = 0
         fr.Active = true
         fr.Visible = true
         fr.Parent = sg
         
         sg.Parent = CoreGui
         
         Rayfield:Notify({Title = "TIENHZIO HUB", Content = "Đã BẬT màn hình trắng & Ngừng hoạt động GPU!", Duration = 3})
      else
         -- [ CHẾ ĐỘ TẮT ]: Khôi phục lại đồ họa game bình thường
         pcall(function()
            RunService:Set3DRenderingEnabled(true) -- Bật lại render 3D
         end)
         
         if setfpscap then 
            pcall(function() setfpscap(60) end) -- Trả lại 60 FPS mượt mà
         end
         
         -- Hủy bỏ màn hình trắng xóa
         if CoreGui:FindFirstChild(screenName) then
            CoreGui[screenName]:Destroy()
         end
         
         Rayfield:Notify({Title = "TIENHZIO HUB", Content = "Đã KHÔI PHỤC đồ họa game bình thường!", Duration = 3})
      end
   end,
})

-- Thông báo cho người dùng khi menu tải xong
Rayfield:Notify({Title = "TIENHZIO HUB", Content = "Menu đã sẵn sàng hoạt động!", Duration = 3})
