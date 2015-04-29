# 验证 Nova 的 flavor 部分

|内容编号|内容名称|
|--------|--------|
|04|Flavor|

|测试编号|测试目的|操作|预期结果|实际结果|备注|Rally/Tempest/None|
|--------|--------|----|--------|--------|----|------------------|
||创建 Flavor 并以此创建一台实例|<ul><li>UI:<ol><li>登录到 dashboard；</li><li>点击左侧导航栏的 【Admin】，展开选项卡；</li><li>点击 【System】 子选项卡，展开；</li><li>点击 【Flavors】 选项卡；</li><li>点击 【Create Flavor】，在弹出的 【Create Flavor】 窗口中，填写 Name 为 m1.normal，ID 设置为 01，RAM 设置为 1024(MB)，Root Disk 设置为 1(G)其他项设置为 0；</li><li>点击窗口下方的 【Create Flavor】 按钮，创建 Flavor；</li><li>切换到左侧导航栏的 【Project】 -> 【Compute】 -> 【Instances】 中，点击 【Launch Instance】 创建实例，在选择 Flavor 时，使用 m1.normal，其他配置填写完毕后，点击窗口下方的 【Launch Instance】 按钮。</li></ol></li><li>CLI:<ol><li>登录到 Controller 节点；</li><li>执行命令 <code>nova flavor-create \<name\> \<id\> \<ram\> \<disk\> \<vcpus\></code>；</li><li>创建后，使用 Rally 测试文件 boot.yaml，执行测试命令 <code>rally task start boot.yaml</code>。</li></ol></li></ul>|<ul><li>UI:<ul><li>Flavor 创建成功，在 【Flavors】 选项卡的右侧显示新建的 flavor 的信息</li><li>使用该 flavor 创建实例，实例的规格与 flavor 相同</li></ul></li><li>CLI:<ul><li>执行 nova flavor-create 命令成功，成功创建 flavor</li><li>执行 Rally 测试成功</li></ul></li></ul>|||None|
||创建一个空的 Flavor|<ul><li>UI:<ol><li>登录到 dashboard；</li><li>点击左侧导航栏的 【Admin】，展开选项卡；</li><li>点击 【System】 子选项卡，展开；</li><li>点击 【Flavors】 选项卡；</li><li>点击 【Create Flavor】，在弹出的 【Create Flavor】 窗口中，填写 Name 为 m1.empty，其他选项全部设置为 0，点击窗口下方的 【Create Flavor】 按钮。</li></ol></li><li>CLI:<ol><li>登录到 Controller 节点；</li><li>执行命令 <code>nova flavor-create m1.empty 0 0 0 0</code>。</li></ol></li></ul>|<ul><li>UI:<ul><li>点击窗口下方的 【Create Flavor】，无法创建，在 VCPUs 中提示："值必须大于或等于1"</li><li>将 VCPUs 修改为 1 后创建 flavor，无法创建，在 RAM 中提示："值必须大于或等于1"</li></ul></li><li>CLI:<ul><li>命令执行失败，提示："ERROR (BadRequest): Invalid input received: ram must be >= 1 (HTTP 400)"</li><li>将 ram 位置的数值修改为 1 后再次执行命令，执行失败，提示："ERROR (BadRequest): Invalid input received: vcpus must be >= 1 (HTTP 400) "</li></ul></li></ul>|||None|
||创建一个超出配额的 Flavor|||||None|
||删除 Flavor|<ul><li>UI:<ol><li>登录到 dashboard；</li><li>点击左侧导航栏的 【Admin】，展开选项卡；</li><li>点击 【System】 子选项卡，展开；</li><li>点击 【Flavors】 选项卡；</li><li>选择一个 flavor，点击 【Actions】 中的 【Delete Flavor】；</li><li>在弹出的 【Confirm Delete Flavor】 窗口下方点击 【Delete Flavor】，确认删除。</li></ol></li><li>CLI:<ol><li>登录到 Controller 节点；</li><li>执行命令 <code>nova flavor-delete \<flavor_id or flavor_name\></code>。</li></ol></li></ul>|<ul><li>UI:<ul><li>flavor 被成功删除，不再显示 flavor 信息</li><li>以该 flavor 所创建的实例不受影响</li></ul></li><li>CLI:<ul><li>命令执行成功</li></ul></li></ul>|||None|
||修改 Flavor|<ul><li>UI:<ol><li>登录到 dashboard；</li><li>点击左侧导航栏的 【Admin】，展开选项卡；</li><li>点击 【System】 子选项卡，展开；</li><li>点击 【Flavors】 选项卡；</li><li>选择名为 m1.normal 的 flavor，点击 【Actions】 中的 【Edit Flavor】；</li><li>在弹出的 【Edit Flavor】 窗口中，修改 Name 为 m1.normal_new，VCPUs 设置为 2，RAM 设置为 512(MB)，其他保持原来配置，点击窗口下方的 【Save】 按钮。</li></ol></li><li>CLI:<ol><li>?</li></ol></li></ul>|<ul><li>UI:<ul><li>修改成功，有 Success 的信息提示</li><li>ID 被自动更新</li></ul></li><li>CLI:<ul><li></li></ul></li></ul>|||None|
||修改 Flavor 的访问权限|<ul><li>UI:<ol><li>登录到 dashboard；</li><li>点击左侧导航栏的 【Admin】，展开选项卡；</li><li>点击 【System】 子选项卡，展开；</li><li>点击 【Flavors】 选项卡；</li><li>选择名为 m1.normal 的 flavor，点击 【Actions】 中的 【Modify Access】；</li><li>在弹出的 【Edit Flavor】 窗口的 Flavor Access 标签中，选择能够访问的 project，如 Admin，点击项目名称后的 【+】 按钮，点击 【Save】 保存修改。</li></ol></li><li>CLI:<ol><li>登录到 Controller 节点；</li><li>执行命令 <code>nova flavor-access-add \<flavor\> \<tenant_id\></code>。</li></ol></li></ul>|<ul><li>UI:<ul><li>访问权限修改成功，有 Success 的相关提示</li><li>使用其他 project(tenant) 中的用户登录到 dashboard，创建实例时无法访问名为 m1.normal 的 flavor</li></ul></li><li>CLI:<ul><li>命令执行成功</li><li>使用其他 tenant 的用户执行命令 <code>nova flavor-list</code>，看到列出的列表中没有名为 m1.normal 的 flavor</li></ul></li></ul>|||None|
||更新 Flavor 的 metadata|||||None|