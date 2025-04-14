# Windows 网络高级配置指南

## 一、调整 IPv4/IPv6 优先级
### 修改注册表使 IPv4 优先

1. **打开注册表编辑器**：
   ```reg
   Win + R → 输入 regedit → Enter
   ```

2. **定位到路径**：
   ```reg
   HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters
   ```

3. **新建/修改 DWORD 值**：
   ```reg
   右键新建 → DWORD (32位) → 命名为 DisabledComponents
   ```

4. **设置值数据**：
   ```reg
   0x20：禁用 IPv6 部分功能（IPv4优先）
   0xFF：完全禁用 IPv6
   ```
   - 推荐值：`0x20`（保持 IPv6 部分功能）
   - 危险值：`0xFF`（完全禁用 IPv6）

5. **重启系统生效**
   ```powershell
   shutdown /r /t 0
   ```

---

## 格式说明
### 优化要点：
1. **代码块规范**：
   ```markdown
   ```reg  # 注册表操作专用代码块
   ```powershell  # PowerShell 命令块
   ```

2. **层级结构**：
   - 使用 `#` → `##` → `###` 三级标题
   - 操作步骤使用有序列表（数字编号）

3. **高亮强调**：
   - 关键路径用 **加粗**
   - 危险操作值用红色标注（GitHub 需CSS支持，此处用文字说明）

### GitHub 预览效果：
![示例](https://via.placeholder.com/800x400.png/000000/FFFFFF?text=Markdown+Preview)


## ⚠️ 注意事项
- 修改前备份注册表（`文件 → 导出`）
- 需要管理员权限
- Windows 11/10 不同版本路径可能微调

# 快速重启命令
shutdown /r /t 0

| 值数据 | 效果                  | 推荐指数 |
|--------|-----------------------|----------|
| 0x20   | IPv4优先（兼容模式）  | ★★★★★   |
| 0xFF   | 完全禁用IPv6          | ★★☆☆☆   |

