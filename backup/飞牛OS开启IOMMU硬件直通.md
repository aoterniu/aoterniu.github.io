# 飞牛OS开启IOMMU硬件直通完整指南

## 🔧 前置准备
### 开启SSH远程访问
1. **打开MobaXterm**  
   - 点击 `Session` → `SSH`
2. **配置连接参数**  
   ```bash
   # 连接参数示例
   Host     : 192.168.1.100  # 飞牛OS管理IP
   Port     : 22
   Username : root           # 默认管理员账户
   ```

---

## 🖥️ 核心配置流程
### 修改GRUB引导配置
```bash
sudo nano /etc/default/grub
```

#### 处理器类型配置
<table>
<tr>
<th>Intel平台</th>
<th>AMD平台</th>
</tr>
<tr>
<td>

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet i915.force_probe=7d55 intel_iommu=on iommu=pt"
```
</td>
<td>

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet i915.force_probe=7d55 amd_iommu=on iommu=pt"
```
</td>
</tr>
</table>

#### 操作说明
1. 使用 `Ctrl+W` 搜索原配置行
2. 按处理器类型修改参数
3. 保存退出：`Ctrl+S` → `Ctrl+X`

---

### 应用配置更新
```bash
# 更新GRUB配置
sudo update-grub

# 重建initramfs
sudo update-initramfs -u -k all

# 重启系统
sudo reboot
```

#### 预期成功输出
```bash
update-initramfs: Generating /boot/initrd.img-5.15.0-101-generic
Warning: No support for locale: zh_CN.utf8
```

---

## ✅ 验证IOMMU状态
### 检查内核日志
```bash
dmesg | grep -i iommu
```

#### 成功标志
```bash
[    0.000000] DMAR: IOMMU enabled
[    0.598712] iommu: Default domain type: Translated 
```

---

## ⚠️ 注意事项
<details>
<summary>点击查看风险提示</summary>

1. **硬件兼容性**  
   ```bash
   # 检查CPU虚拟化支持
   grep -E 'vmx|svm' /proc/cpuinfo
   ```
   - Intel需输出 `vmx`，AMD需输出 `svm`

2. **回滚方案**  
   ```bash
   # 恢复原始配置
   sudo nano /etc/default/grub → 删除iommu参数
   sudo update-grub
   sudo reboot
   ```

3. **日志监控**  
   ```bash
   journalctl -b -0 | grep -i dmar
   ```
</details>

---

## 🖼️ 效果预览
![配置流程图](https://via.placeholder.com/800x400.png/000/fff?text=IOMMU+Configuration+Flow)
