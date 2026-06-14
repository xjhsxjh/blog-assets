╭─── Claude Code v2.1.177 ─────────────────────────────────────────────────────╮
│                                         │ Tips for getting started           │
│              Welcome back!              │ Run /init to create a CLAUDE.md f… │
│                                         │ ────────────────────────────────── │
│                 ▐▛███▜▌                 │ What's new                         │
│                ▝▜█████▛▘                │ Session titles are now generated … │
│                  ▘▘ ▝▝                  │ Added `footerLinksRegexes` settin… │
│                                         │ Improved Bedrock credential cachi… │
│   deepseek-v4-pro · API Usage Billing   │ /release-notes for more            │
│           ~/drawio_workspace            │                                    │
╰──────────────────────────────────────────────────────────────────────────────╯


❯ claude mcp list                                                               

  Thought for 6s, ran 1 shell command (ctrl+o to expand)

● Here are your configured MCP servers:

  ┌────────┬────────────────────┬─────────────┐
  │ Server │      Command       │   Status    │
  ├────────┼────────────────────┼─────────────┤
  │ drawio │ npx -y @drawio/mcp │ ✔ Connected │
  └────────┴────────────────────┴─────────────┘

  You have one MCP server configured — the draw.io server, which provides the
  mcp__drawio__open_drawio_csv, mcp__drawio__open_drawio_mermaid, and
  mcp__drawio__open_drawio_xml tools for creating diagrams.

✻ Brewed for 9s

❯ arm64 linux平台, 画一下pcie epf的驱动框架图?                                  

  Thought for 9s, called drawio (ctrl+o to expand)

● 已为你生成 ARM64 Linux 平台 PCIe EPF
  驱动框架图并在浏览器中打开。框架分为以下层次：

  ┌───────────────┬─────────────────────────────────────────────────────────┐
  │     层次      │                          说明                           │
  ├───────────────┼─────────────────────────────────────────────────────────┤
  │ Userspace     │ 通过 configfs/sysfs 接口配置和管理 EPF                  │
  ├───────────────┼─────────────────────────────────────────────────────────┤
  │ EPF Core (pci │ 管理 Function 驱动的注册、bind/unbind                   │
  │ -epf-core)    │                                                         │
  ├───────────────┼─────────────────────────────────────────────────────────┤
  │ EPC Core (pci │ 管理 Controller 驱动的注册、BAR/IRQ/DMA 操作            │
  │ -epc-core)    │                                                         │
  ├───────────────┼─────────────────────────────────────────────────────────┤
  │ EPF Drivers   │ pci-epf-test（测试）、pci-epf-ntb、pci-epf-mhi、pci-epf │
  │               │ -vntb 及自定义 EPF                                      │
  ├───────────────┼─────────────────────────────────────────────────────────┤
  │ EPC Drivers   │ DesignWare DWC、Cadence、Rockchip、Qualcomm 等平台的    │
  │ (ARM64)       │ EPC 驱动                                                │
  ├───────────────┼─────────────────────────────────────────────────────────┤
  │ Hardware      │ 各厂商的 PCIe Endpoint 硬件 IP                          │
  ├───────────────┼─────────────────────────────────────────────────────────┤
  │ Remote RC     │ 远端 PCIe Root Complex（x86/ARM64 主机）                │
  └───────────────┴─────────────────────────────────────────────────────────┘

  核心流程：EPF 驱动和 EPC 驱动分别向各自的 Core 注册，EPF Core 与 EPC Core 通过
  epf_ops 交互完成 function 与 controller 的绑定，最终通过 PCIe Link 与远端 RC
  通信。

✻ Baked for 21s

! ls                                                                            
  ⎿  (Bash completed with no output)

❯ okay, 你输出的drawio图示呢?                                                   

  Thought for 4s (ctrl+o to expand)

● MCP drawio 工具已自动在浏览器中打开了图表（WSL 下可能没有 GUI
  浏览器所以你看不到）。我把 Mermaid 源码直接输出如下，你可以复制到任意在线
  Mermaid 编辑器查看，或者我也可以导出为本地文件：

  flowchart TB
      subgraph userspace["👤 Userspace"]
          APP["Application / Test Tools"]
          SYSFS["sysfs Interface"]
          CONFIGFS["configfs Interface"]
      end

      subgraph epf_framework["⚙️ PCIe Endpoint Framework
  (drivers/pci/endpoint/)"]
          EPF_CORE["pci-epf-core<br/>EPF Core<br/>───────────────<br/>•
  epf_device_register()<br/>• epf_driver_register()<br/>• epf_bind/unbind"]
          EPC_CORE["pci-epc-core<br/>EPC Core<br/>───────────────<br/>•
  epc_device_register()<br/>• epc_driver_register()<br/>• BAR/IRQ/DMA ops"]
      end

      subgraph epf_drivers["📦 EPF Drivers"]
          EPF_TEST["pci-epf-test<br/>───────────<br/>Loopback / DMA test"]
          EPF_NTB["pci-epf-ntb<br/>───────────<br/>NTB function"]
          EPF_MHI["pci-epf-mhi<br/>───────────<br/>MHI for modem"]
          EPF_VNTB["pci-epf-vntb<br/>───────────<br/>Virtual NTB"]
          CUSTOM_EPF["Custom EPF Driver<br/>───────────<br/>User-defined
  function"]
      end

      subgraph epc_drivers["🔧 EPC Drivers (ARM64)"]
          DWC_EP["pcie-dw-plat-ep<br/>──────────────<br/>DesignWare DWC EP"]
          CADENCE_EP["pcie-cadence-ep<br/>──────────────<br/>Cadence EP"]
          RK_EP["pcie-rockchip-ep<br/>──────────────<br/>Rockchip EP"]
          QCOM_EP["pcie-qcom-ep<br/>──────────────<br/>Qualcomm EP"]
      end

      subgraph hw["🖥️ ARM64 PCIe EP Hardware"]
          DWC_HW["Synopsys DWC<br/>PCIe Endpoint"]
          CDN_HW["Cadence<br/>PCIe Endpoint"]
          RK_HW["Rockchip<br/>PCIe Endpoint"]
          QCOM_HW["Qualcomm<br/>PCIe Endpoint"]
      end

      subgraph rc["🖥️ Remote PCIe Root Complex"]
          RC_HOST["PCIe RC Host<br/>(x86/ARM64)"]
      end

      APP --> CONFIGFS
      APP --> SYSFS

      CONFIGFS --> EPF_CORE
      SYSFS --> EPF_CORE
      SYSFS --> EPC_CORE

      EPF_CORE <-->|"epf_ops"| EPC_CORE

      EPF_TEST -->|"register"| EPF_CORE
      EPF_NTB -->|"register"| EPF_CORE
      EPF_MHI -->|"register"| EPF_CORE
      EPF_VNTB -->|"register"| EPF_CORE
      CUSTOM_EPF -->|"register"| EPF_CORE

      DWC_EP -->|"register"| EPC_CORE
      CADENCE_EP -->|"register"| EPC_CORE
      RK_EP -->|"register"| EPC_CORE
      QCOM_EP -->|"register"| EPC_CORE

      DWC_EP --> DWC_HW
      CADENCE_EP --> CDN_HW
      RK_EP --> RK_HW
      QCOM_EP --> QCOM_HW

      DWC_HW <==>|"PCIe Link"| RC_HOST
      CDN_HW <==>|"PCIe Link"| RC_HOST
      RK_HW <==>|"PCIe Link"| RC_HOST
      QCOM_HW <==>|"PCIe Link"| RC_HOST

  你可以在 Mermaid Live Editor (https://mermaid.live)
  粘贴查看渲染效果。如果你有本地浏览器可用，也可以直接打开这个 MCP 生成的
  draw.io 链接：

  https://app.diagrams.net/?grid=0&pv=0&border=10&edit=_blank#create=%7B%22type%
  22%3A%22mermaid%22%2C%22compressed%22%3Atrue%2C%22data%22%3A%22...

✻ Crunched for 13s

❯ ok, 我的意思, 你不能本地生成drawio文件吗? 是需要我安装什么软件吗              

  Thought for 3s (ctrl+o to expand)

● 不需要安装任何东西。我可以直接生成 .drawio 文件，它是纯 XML 格式，用 VS Code
  的 draw.io 插件或本地 draw.io 桌面版都能打开。

  让我为你生成：

● Write(pcie_epf_framework.drawio)
  ⎿  Wrote 238 lines to pcie_epf_framework.drawio
       1 <mxfile host="app.diagrams.net" modified="2026-06-14T00:00:00.00
       2   <diagram name="ARM64 PCIe EPF Framework" id="pcie-epf">
       3     <mxGraphModel dx="1400" dy="1000" grid="1" gridSize="10" gui
         geScale="1" pageWidth="1200" pageHeight="1000">
       4       <root>
       6         <mxCell id="1" parent="0" />
       7 
       9         <mxCell id="userspace-box" value="👤 Userspace" style="s
         wimlane;whiteSpace=wrap;html=1;fillColor=#e8f5e9;strokeColor=#2e
      10           <mxGeometry x="40" y="40" width="200" height="1
     … +409 lines (ctrl+o to expand)

  Thought for 1s (ctrl+o to expand)

● 已生成本地文件：

  /home/river/drawio_workspace/pcie_epf_framework.drawio

  你可以用以下方式打开：

  - VS Code: 安装 hediet.vscode-drawio 插件，直接打开文件即可编辑
  - draw.io 桌面版: flatpak install drawio 或官网下载 AppImage
  - 在线: 打开 https://app.diagrams.net → File → Open from → Device

  文件是标准 .drawio XML 格式，不需要额外安装任何依赖。图表包含 5
  层架构（Userspace → EPF/EPC Core → EPF Drivers → EPC Drivers → Hardware →
  Remote RC），以及注册调用关系和 PCIe Link 连接。

✻ Cooked for 55s

! ls                                                                            
  ⎿  pcie_epf_framework.drawio

❯ 我能说你前面输出的drawio文件, 我打开都是失败吗? Could not add object for      
  mxCell 我打开报这个错...                                                      

  Thought for 10s (ctrl+o to expand)

● 这是 draw.io XML
  格式问题，边标签（edgeLabel）的嵌套结构容易出错。我重新生成一个干净版本：

● Write(pcie_epf_framework.drawio)
  ⎿  Added 82 lines, removed 154 lines
        1  <mxfile host="app.diagrams.net" modified="2026-06-14T00:00:00.
           000Z" agent="claude" version="24.0.0">
        2    <diagram name="ARM64 PCIe EPF Framework" id="pcie-epf">
        3 -    <mxGraphModel dx="1400" dy="1000" grid="1" gridSize="10" g
          -uides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1
          -" pageScale="1" pageWidth="1200" pageHeight="1000">           
        3 +    <mxGraphModel dx="1400" dy="1000" grid="1" gridSize="10" g
          +uides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1
          +" pageScale="1" pageWidth="1200" pageHeight="950">            
        4        <root>
        5          <mxCell id="0" />
        6          <mxCell id="1" parent="0" />
        7  
        8 -        <!-- ===== Userspace ===== -->                        
        9 -        <mxCell id="userspace-box" value="👤 Userspace" style=
          -"swimlane;whiteSpace=wrap;html=1;fillColor=#e8f5e9;strokeColor
          -=#2e7d32;fontStyle=1;fontSize=13;" vertex="1" parent="1">     
       10 -          <mxGeometry x="40" y="40" width="200" height="160" a
          -s="geometry" />                                               
        8 +        <!-- ===== Userspace Group ===== -->                  
        9 +        <mxCell id="g-userspace" value="Userspace" style="swim
          +lane;whiteSpace=wrap;html=1;fillColor=#e8f5e9;strokeColor=#2e7
          +d32;fontStyle=1;fontSize=13;startSize=28;" vertex="1" parent="
          +1">                                                           
       10 +          <mxGeometry x="40" y="40" width="200" height="145" a
          +s="geometry" />                                               
       11          </mxCell>
       12 -        <mxCell id="app" value="Application&lt;br&gt;Test Tool
          -s" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;s
          -trokeColor=#2e7d32;" vertex="1" parent="userspace-box">       
       13 -          <mxGeometry x="15" y="35" width="170" height="35" as
          -="geometry" />                                                
       12 +        <mxCell id="app" value="Application / Test Tools" styl
          +e="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCo
          +lor=#2e7d32;" vertex="1" parent="g-userspace">                
       13 +          <mxGeometry x="12" y="32" width="176" height="34" as
          +="geometry" />                                                
       14          </mxCell>
       15 -        <mxCell id="configfs" value="configfs Interface" style
          -="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCol
          -or=#2e7d32;" vertex="1" parent="userspace-box">               
       16 -          <mxGeometry x="15" y="80" width="170" height="30" as
          -="geometry" />                                                
       15 +        <mxCell id="configfs" value="configfs Interface" style
          +="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCol
          +or=#2e7d32;" vertex="1" parent="g-userspace">                 
       16 +          <mxGeometry x="12" y="72" width="176" height="28" as
          +="geometry" />                                                
       17          </mxCell>
       18 -        <mxCell id="sysfs" value="sysfs Interface" style="roun
          -ded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColor=#2e
          -7d32;" vertex="1" parent="userspace-box">                     
       19 -          <mxGeometry x="15" y="118" width="170" height="30" a
          -s="geometry" />                                               
       18 +        <mxCell id="sysfs" value="sysfs Interface" style="roun
          +ded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColor=#2e
          +7d32;" vertex="1" parent="g-userspace">                       
       19 +          <mxGeometry x="12" y="106" width="176" height="28" a
          +s="geometry" />                                               
       20          </mxCell>
       21  
       22 -        <!-- ===== EPF Framework Core ===== -->               
       23 -        <mxCell id="framework-box" value="⚙️ PCIe Endpoint Fra
          -mework (drivers/pci/endpoint/)" style="swimlane;whiteSpace=wra
          -p;html=1;fillColor=#e3f2fd;strokeColor=#1565c0;fontStyle=1;fon
          -tSize=13;" vertex="1" parent="1">                             
       24 -          <mxGeometry x="330" y="40" width="460" height="160" 
          -as="geometry" />                                              
       22 +        <!-- ===== EPF Framework Group ===== -->              
       23 +        <mxCell id="g-framework" value="PCIe Endpoint Framewor
          +k (drivers/pci/endpoint/)" style="swimlane;whiteSpace=wrap;htm
          +l=1;fillColor=#e3f2fd;strokeColor=#1565c0;fontStyle=1;fontSize
          +=13;startSize=28;" vertex="1" parent="1">                     
       24 +          <mxGeometry x="310" y="40" width="460" height="145" 
          +as="geometry" />                                              
       25          </mxCell>
       26 -        <mxCell id="epf-core" value="pci-epf-core&lt;br&gt;&lt
          -;b&gt;EPF Core&lt;/b&gt;&lt;hr size=&quot;1&quot;&gt;epf_devic
          -e_register()&lt;br&gt;epf_driver_register()&lt;br&gt;epf_bind 
          -/ unbind" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#b
          -bdefb;strokeColor=#1565c0;align=left;spacingLeft=8;" vertex="1
          -" parent="framework-box">                                     
       27 -          <mxGeometry x="15" y="30" width="210" height="115" a
          -s="geometry" />                                               
       26 +        <mxCell id="epf-core" value="pci-epf-core&#xa;EPF Core
          +&#xa;─────────────────&#xa;epf_device_register()&#xa;epf_drive
          +r_register()&#xa;epf_bind / unbind" style="rounded=1;whiteSpac
          +e=wrap;html=1;fillColor=#bbdefb;strokeColor=#1565c0;align=left
          +;spacingLeft=8;" vertex="1" parent="g-framework">             
       27 +          <mxGeometry x="10" y="30" width="215" height="108" a
          +s="geometry" />                                               
       28          </mxCell>
       29 -        <mxCell id="epc-core" value="pci-epc-core&lt;br&gt;&lt
          -;b&gt;EPC Core&lt;/b&gt;&lt;hr size=&quot;1&quot;&gt;epc_devic
          -e_register()&lt;br&gt;epc_driver_register()&lt;br&gt;BAR / IRQ
          - / DMA ops" style="rounded=1;whiteSpace=wrap;html=1;fillColor=
          -#bbdefb;strokeColor=#1565c0;align=left;spacingLeft=8;" vertex=
          -"1" parent="framework-box">                                   
       30 -          <mxGeometry x="240" y="30" width="205" height="115" 
          -as="geometry" />                                              
       29 +        <mxCell id="epc-core" value="pci-epc-core&#xa;EPC Core
          +&#xa;─────────────────&#xa;epc_device_register()&#xa;epc_drive
          +r_register()&#xa;BAR / IRQ / DMA ops" style="rounded=1;whiteSp
          +ace=wrap;html=1;fillColor=#bbdefb;strokeColor=#1565c0;align=le
          +ft;spacingLeft=8;" vertex="1" parent="g-framework">           
       30 +          <mxGeometry x="238" y="30" width="212" height="108" 
          +as="geometry" />                                              
       31          </mxCell>
       32  
       33 -        <!-- ===== EPF Drivers ===== -->                      
       34 -        <mxCell id="epf-drivers-box" value="📦 EPF Drivers" st
          -yle="swimlane;whiteSpace=wrap;html=1;fillColor=#fff3e0;strokeC
          -olor=#ef6c00;fontStyle=1;fontSize=13;" vertex="1" parent="1"> 
       35 -          <mxGeometry x="860" y="40" width="300" height="280" 
          -as="geometry" />                                              
       33 +        <!-- ===== EPF Drivers Group ===== -->                
       34 +        <mxCell id="g-epf-drivers" value="EPF Drivers" style="
          +swimlane;whiteSpace=wrap;html=1;fillColor=#fff3e0;strokeColor=
          +#ef6c00;fontStyle=1;fontSize=13;startSize=28;" vertex="1" pare
          +nt="1">                                                       
       35 +          <mxGeometry x="850" y="40" width="290" height="280" 
          +as="geometry" />                                              
       36          </mxCell>
       37 -        <mxCell id="epf-test" value="pci-epf-test&lt;br&gt;───
          -────────&lt;br&gt;Loopback/DMA test" style="rounded=1;whiteSpa
          -ce=wrap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=lef
          -t;spacingLeft=6;" vertex="1" parent="epf-drivers-box">        
       38 -          <mxGeometry x="15" y="35" width="270" height="42" as
          -="geometry" />                                                
       37 +        <mxCell id="epf-test" value="pci-epf-test&#xa;────────
          +────&#xa;Loopback / DMA test" style="rounded=1;whiteSpace=wrap
          +;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spaci
          +ngLeft=8;" vertex="1" parent="g-epf-drivers">                 
       38 +          <mxGeometry x="12" y="32" width="266" height="42" as
          +="geometry" />                                                
       39          </mxCell>
       40 -        <mxCell id="epf-ntb" value="pci-epf-ntb&lt;br&gt;─────
          -──────&lt;br&gt;NTB function" style="rounded=1;whiteSpace=wrap
          -;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spaci
          -ngLeft=6;" vertex="1" parent="epf-drivers-box">               
       41 -          <mxGeometry x="15" y="85" width="270" height="42" as
          -="geometry" />                                                
       40 +        <mxCell id="epf-ntb" value="pci-epf-ntb&#xa;──────────
          +──&#xa;NTB function" style="rounded=1;whiteSpace=wrap;html=1;f
          +illColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8;
          +" vertex="1" parent="g-epf-drivers">                          
       41 +          <mxGeometry x="12" y="80" width="266" height="42" as
          +="geometry" />                                                
       42          </mxCell>
       43 -        <mxCell id="epf-mhi" value="pci-epf-mhi&lt;br&gt;─────
          -──────&lt;br&gt;MHI for modem" style="rounded=1;whiteSpace=wra
          -p;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spac
          -ingLeft=6;" vertex="1" parent="epf-drivers-box">              
       44 -          <mxGeometry x="15" y="135" width="270" height="42" a
          -s="geometry" />                                               
       43 +        <mxCell id="epf-mhi" value="pci-epf-mhi&#xa;──────────
          +──&#xa;MHI (modem)" style="rounded=1;whiteSpace=wrap;html=1;fi
          +llColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8;"
          + vertex="1" parent="g-epf-drivers">                           
       44 +          <mxGeometry x="12" y="128" width="266" height="42" a
          +s="geometry" />                                               
       45          </mxCell>
       46 -        <mxCell id="epf-vntb" value="pci-epf-vntb&lt;br&gt;───
          -────────&lt;br&gt;Virtual NTB" style="rounded=1;whiteSpace=wra
          -p;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spac
          -ingLeft=6;" vertex="1" parent="epf-drivers-box">              
       47 -          <mxGeometry x="15" y="185" width="270" height="42" a
          -s="geometry" />                                               
       46 +        <mxCell id="epf-vntb" value="pci-epf-vntb&#xa;────────
          +────&#xa;Virtual NTB" style="rounded=1;whiteSpace=wrap;html=1;
          +fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8
          +;" vertex="1" parent="g-epf-drivers">                         
       47 +          <mxGeometry x="12" y="176" width="266" height="42" a
          +s="geometry" />                                               
       48          </mxCell>
       49 -        <mxCell id="custom-epf" value="Custom EPF Driver&lt;br
          -&gt;───────────&lt;br&gt;User-defined function" style="rounded
          -=1;whiteSpace=wrap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c0
          -0;align=left;spacingLeft=6;dashed=1;dashPattern=4 4;" vertex="
          -1" parent="epf-drivers-box">                                  
       50 -          <mxGeometry x="15" y="235" width="270" height="38" a
          -s="geometry" />                                               
       49 +        <mxCell id="custom-epf" value="Custom EPF Driver&#xa;─
          +───────────&#xa;User-defined function" style="rounded=1;whiteS
          +pace=wrap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=l
          +eft;spacingLeft=8;dashed=1;" vertex="1" parent="g-epf-drivers"
          +>                                                             
       50 +          <mxGeometry x="12" y="224" width="266" height="42" a
          +s="geometry" />                                               
       51          </mxCell>
       52  
       53 -        <!-- ===== EPC Drivers ===== -->                      
       54 -        <mxCell id="epc-drivers-box" value="🔧 EPC Drivers (AR
          -M64)" style="swimlane;whiteSpace=wrap;html=1;fillColor=#f3e5f5
          -;strokeColor=#7b1fa2;fontStyle=1;fontSize=13;" vertex="1" pare
          -nt="1">                                                       
       55 -          <mxGeometry x="40" y="280" width="280" height="260" 
          -as="geometry" />                                              
       53 +        <!-- ===== EPC Drivers Group ===== -->                
       54 +        <mxCell id="g-epc-drivers" value="EPC Drivers (ARM64)"
          + style="swimlane;whiteSpace=wrap;html=1;fillColor=#f3e5f5;stro
          +keColor=#7b1fa2;fontStyle=1;fontSize=13;startSize=28;" vertex=
          +"1" parent="1">                                               
       55 +          <mxGeometry x="40" y="260" width="270" height="260" 
          +as="geometry" />                                              
       56          </mxCell>
       57 -        <mxCell id="dwc-ep" value="pcie-dw-plat-ep&lt;br&gt;──
          -────────────&lt;br&gt;DesignWare DWC EP" style="rounded=1;whit
          -eSpace=wrap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align
          -=left;spacingLeft=6;" vertex="1" parent="epc-drivers-box">    
       58 -          <mxGeometry x="15" y="35" width="250" height="45" as
          -="geometry" />                                                
       57 +        <mxCell id="dwc-ep" value="pcie-dw-plat-ep&#xa;───────
          +───────&#xa;DesignWare DWC EP" style="rounded=1;whiteSpace=wra
          +p;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spac
          +ingLeft=8;" vertex="1" parent="g-epc-drivers">                
       58 +          <mxGeometry x="12" y="32" width="246" height="48" as
          +="geometry" />                                                
       59          </mxCell>
       60 -        <mxCell id="cadence-ep" value="pcie-cadence-ep&lt;br&g
          -t;──────────────&lt;br&gt;Cadence EP" style="rounded=1;whiteSp
          -ace=wrap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=le
          -ft;spacingLeft=6;" vertex="1" parent="epc-drivers-box">       
       61 -          <mxGeometry x="15" y="90" width="250" height="45" as
          -="geometry" />                                                
       60 +        <mxCell id="cadence-ep" value="pcie-cadence-ep&#xa;───
          +───────────&#xa;Cadence EP" style="rounded=1;whiteSpace=wrap;h
          +tml=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacing
          +Left=8;" vertex="1" parent="g-epc-drivers">                   
       61 +          <mxGeometry x="12" y="86" width="246" height="48" as
          +="geometry" />                                                
       62          </mxCell>
       63 -        <mxCell id="rk-ep" value="pcie-rockchip-ep&lt;br&gt;──
          -────────────&lt;br&gt;Rockchip EP" style="rounded=1;whiteSpace
          -=wrap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;
          -spacingLeft=6;" vertex="1" parent="epc-drivers-box">          
       64 -          <mxGeometry x="15" y="145" width="250" height="45" a
          -s="geometry" />                                               
       63 +        <mxCell id="rk-ep" value="pcie-rockchip-ep&#xa;───────
          +───────&#xa;Rockchip EP" style="rounded=1;whiteSpace=wrap;html
          +=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacingLef
          +t=8;" vertex="1" parent="g-epc-drivers">                      
       64 +          <mxGeometry x="12" y="140" width="246" height="48" a
          +s="geometry" />                                               
       65          </mxCell>
       66 -        <mxCell id="qcom-ep" value="pcie-qcom-ep&lt;br&gt;────
          -──────────&lt;br&gt;Qualcomm EP" style="rounded=1;whiteSpace=w
          -rap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;sp
          -acingLeft=6;" vertex="1" parent="epc-drivers-box">            
       67 -          <mxGeometry x="15" y="200" width="250" height="45" a
          -s="geometry" />                                               
       66 +        <mxCell id="qcom-ep" value="pcie-qcom-ep&#xa;─────────
          +─────&#xa;Qualcomm EP" style="rounded=1;whiteSpace=wrap;html=1
          +;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacingLeft=
          +8;" vertex="1" parent="g-epc-drivers">                        
       67 +          <mxGeometry x="12" y="194" width="246" height="48" a
          +s="geometry" />                                               
       68          </mxCell>
       69  
       70 -        <!-- ===== Hardware ===== -->                         
       71 -        <mxCell id="hw-box" value="🖥️ ARM64 PCIe EP Hardware" 
          -style="swimlane;whiteSpace=wrap;html=1;fillColor=#fce4ec;strok
          -eColor=#c62828;fontStyle=1;fontSize=13;" vertex="1" parent="1"
          ->                                                             
       72 -          <mxGeometry x="380" y="280" width="280" height="260"
          - as="geometry" />                                             
       70 +        <!-- ===== Hardware Group ===== -->                   
       71 +        <mxCell id="g-hw" value="ARM64 PCIe EP Hardware" style
          +="swimlane;whiteSpace=wrap;html=1;fillColor=#fce4ec;strokeColo
          +r=#c62828;fontStyle=1;fontSize=13;startSize=28;" vertex="1" pa
          +rent="1">                                                     
       72 +          <mxGeometry x="370" y="260" width="270" height="260"
          + as="geometry" />                                             
       73          </mxCell>
       74 -        <mxCell id="dwc-hw" value="Synopsys DWC&lt;br&gt;PCIe 
          -Endpoint" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f
          -fcdd2;strokeColor=#c62828;" vertex="1" parent="hw-box">       
       75 -          <mxGeometry x="15" y="35" width="250" height="45" as
          -="geometry" />                                                
       74 +        <mxCell id="dwc-hw" value="Synopsys DWC&#xa;PCIe Endpo
          +int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          +;strokeColor=#c62828;" vertex="1" parent="g-hw">              
       75 +          <mxGeometry x="12" y="32" width="246" height="48" as
          +="geometry" />                                                
       76          </mxCell>
       77 -        <mxCell id="cdn-hw" value="Cadence&lt;br&gt;PCIe Endpo
          -int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          -;strokeColor=#c62828;" vertex="1" parent="hw-box">            
       78 -          <mxGeometry x="15" y="90" width="250" height="45" as
          -="geometry" />                                                
       77 +        <mxCell id="cdn-hw" value="Cadence&#xa;PCIe Endpoint" 
          +style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;stro
          +keColor=#c62828;" vertex="1" parent="g-hw">                   
       78 +          <mxGeometry x="12" y="86" width="246" height="48" as
          +="geometry" />                                                
       79          </mxCell>
       80 -        <mxCell id="rk-hw" value="Rockchip&lt;br&gt;PCIe Endpo
          -int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          -;strokeColor=#c62828;" vertex="1" parent="hw-box">            
       81 -          <mxGeometry x="15" y="145" width="250" height="45" a
          -s="geometry" />                                               
       80 +        <mxCell id="rk-hw" value="Rockchip&#xa;PCIe Endpoint" 
          +style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;stro
          +keColor=#c62828;" vertex="1" parent="g-hw">                   
       81 +          <mxGeometry x="12" y="140" width="246" height="48" a
          +s="geometry" />                                               
       82          </mxCell>
       83 -        <mxCell id="qcom-hw" value="Qualcomm&lt;br&gt;PCIe End
          -point" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcd
          -d2;strokeColor=#c62828;" vertex="1" parent="hw-box">          
       84 -          <mxGeometry x="15" y="200" width="250" height="45" a
          -s="geometry" />                                               
       83 +        <mxCell id="qcom-hw" value="Qualcomm&#xa;PCIe Endpoint
          +" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;st
          +rokeColor=#c62828;" vertex="1" parent="g-hw">                 
       84 +          <mxGeometry x="12" y="194" width="246" height="48" a
          +s="geometry" />                                               
       85          </mxCell>
       86  
       87          <!-- ===== Remote RC ===== -->
       88 -        <mxCell id="rc-box" value="🖥️ Remote PCIe Root Complex
          -" style="swimlane;whiteSpace=wrap;html=1;fillColor=#e0e0e0;str
          -okeColor=#424242;fontStyle=1;fontSize=13;" vertex="1" parent="
          -1">                                                           
       89 -          <mxGeometry x="750" y="360" width="250" height="80" 
          -as="geometry" />                                              
       88 +        <mxCell id="g-rc" value="Remote PCIe Root Complex" sty
          +le="swimlane;whiteSpace=wrap;html=1;fillColor=#e0e0e0;strokeCo
          +lor=#424242;fontStyle=1;fontSize=13;startSize=28;" vertex="1" 
          +parent="1">                                                   
       89 +          <mxGeometry x="730" y="345" width="240" height="75" 
          +as="geometry" />                                              
       90          </mxCell>
       91 -        <mxCell id="rc-host" value="PCIe RC Host&lt;br&gt;(x86
          - / ARM64)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#
          -eeeeee;strokeColor=#424242;" vertex="1" parent="rc-box">      
       92 -          <mxGeometry x="25" y="28" width="200" height="40" as
          -="geometry" />                                                
       91 +        <mxCell id="rc-host" value="PCIe RC Host (x86 / ARM64)
          +" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#eeeeee;st
          +rokeColor=#424242;fontStyle=1;" vertex="1" parent="g-rc">     
       92 +          <mxGeometry x="20" y="32" width="200" height="32" as
          +="geometry" />                                                
       93          </mxCell>
       94  
       95 -        <!-- ===== Edges ===== -->                            
       96 -        <!-- Userspace -> Framework -->                       
       97 -        <mxCell id="e-app-config" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          -X=0.5;exitY=1;entryX=0.5;entryY=0;" edge="1" parent="1" source
          -="app" target="configfs">                                     
       95 +        <!-- ===== EDGES ===== -->                            
       96 +                                                              
       97 +        <!-- Userspace internal -->                           
       98 +        <mxCell id="e-app-config" style="edgeStyle=orthogonalE
          +dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" ed
          +ge="1" parent="1" source="app" target="configfs">             
       99            <mxGeometry relative="1" as="geometry" />
      100          </mxCell>
      100 -        <mxCell id="e-app-sysfs" style="edgeStyle=orthogonalEd
          -geStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX
          -=0.5;exitY=1;entryX=0.5;entryY=0;" edge="1" parent="1" source=
          -"app" target="sysfs">                                         
      101 -          <mxGeometry relative="1" as="geometry" />           
      102 -        </mxCell>                                             
      101  
      104 -        <mxCell id="e-config-epf" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          -X=1;exitY=0.5;entryX=0;entryY=0.25;" edge="1" parent="1" sourc
          -e="configfs" target="epf-core">                               
      102 +        <!-- Userspace -> EPF Core -->                        
      103 +        <mxCell id="e-config-epf" style="edgeStyle=orthogonalE
          +dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          +X=1;exitY=0.5;entryX=0;entryY=0.3;entryDx=0;entryDy=0;" edge="
          +1" parent="1" source="configfs" target="epf-core">            
      104            <mxGeometry relative="1" as="geometry" />
      105          </mxCell>
      107 -        <mxCell id="e-sysfs-epf" style="edgeStyle=orthogonalEd
          -geStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX
          -=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source=
          -"sysfs" target="epf-core">                                    
      106 +        <mxCell id="e-sysfs-epf" style="edgeStyle=orthogonalEd
          +geStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX
          +=1;exitY=0.5;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1
          +" parent="1" source="sysfs" target="epf-core">                
      107            <mxGeometry relative="1" as="geometry" />
      108          </mxCell>
      110 -        <mxCell id="e-sysfs-epc" style="edgeStyle=orthogonalEd
          -geStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX
          -=1;exitY=0.5;entryX=0;entryY=0.75;" edge="1" parent="1" source
          -="sysfs" target="epc-core">                                   
      111 -          <mxGeometry relative="1" as="geometry">             
      112 -            <Array as="points">                               
      113 -              <mxPoint x="310" y="173" />                     
      114 -              <mxPoint x="310" y="178" />                     
      115 -            </Array>                                          
      116 -          </mxGeometry>                                       
      117 -        </mxCell>                                             
      109  
      119 -        <!-- EPF Core <-> EPC Core -->                        
      120 -        <mxCell id="e-epf-epc" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          -;exitY=0.5;entryX=0;entryY=0.5;fontColor=#1565c0;fontStyle=1;"
          - edge="1" parent="1" source="epf-core" target="epc-core">     
      121 -          <mxGeometry relative="1" as="geometry">             
      122 -            <mxCell style="edgeLabel;html=1;align=center;verti
          -calAlign=middle;resizable=0;points=[];" vertex="1" connectable
          -="0" parent="e-epf-epc">                                      
      123 -              <mxGeometry x="-0.1" relative="1" as="geometry">
      124 -                <mxPoint x="5" y="-8" as="offset" />          
      125 -              </mxGeometry>                                   
      126 -            </mxCell>                                         
      127 -          </mxGeometry>                                       
      110 +        <!-- EPF Core <-> EPC Core dual edge -->              
      111 +        <mxCell id="e-epf-epc" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          +;exitY=0.35;entryX=0;entryY=0.35;entryDx=0;entryDy=0;fontColor
          +=#1565c0;fontStyle=1;" edge="1" parent="1" source="epf-core" t
          +arget="epc-core">                                             
      112 +          <mxGeometry relative="1" as="geometry" />           
      113          </mxCell>
      129 -        <mxCell id="e-epc-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          -;exitY=0.75;entryX=0;entryY=0.75;fontColor=#1565c0;fontStyle=1
          -;" edge="1" parent="1" source="epf-core" target="epc-core">   
      114 +        <mxCell id="e-epc-epf" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          +;exitY=0.65;entryX=1;entryY=0.65;entryDx=0;entryDy=0;fontColor
          +=#1565c0;fontStyle=1;" edge="1" parent="1" source="epc-core" t
          +arget="epf-core">                                             
      115            <mxGeometry relative="1" as="geometry" />
      116          </mxCell>
      117  
      118          <!-- EPF Drivers -> EPF Core -->
      134 -        <mxCell id="e-test-epf" style="edgeStyle=orthogonalEdg
          -eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=
          -0;exitY=0.5;entryX=1;entryY=0.25;fontColor=#ef6c00;fontSize=10
          -;" edge="1" parent="1" source="epf-test" target="epf-core">   
      135 -          <mxGeometry relative="1" as="geometry">             
      136 -            <mxCell style="edgeLabel;html=1;align=center;verti
          -calAlign=middle;resizable=0;points=[];" vertex="1" connectable
          -="0" parent="e-test-epf">                                     
      137 -              <mxGeometry x="-0.1" relative="1" as="geometry">
      138 -                <mxPoint x="-4" y="-8" as="offset" />         
      139 -              </mxGeometry>                                   
      140 -            </mxCell>                                         
      141 -          </mxGeometry>                                       
      119 +        <mxCell id="e-test-epf" style="edgeStyle=orthogonalEdg
          +eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=
          +0;exitY=0.5;entryX=1;entryY=0.2;entryDx=0;entryDy=0;fontColor=
          +#ef6c00;fontSize=9;" edge="1" parent="1" source="epf-test" tar
          +get="epf-core">                                               
      120 +          <mxGeometry relative="1" as="geometry" />           
      121          </mxCell>
      143 -        <mxCell id="e-ntb-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          -;exitY=0.5;entryX=1;entryY=0.4;fontColor=#ef6c00;fontSize=10;"
          - edge="1" parent="1" source="epf-ntb" target="epf-core">      
      122 +        <mxCell id="e-ntb-epf" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          +;exitY=0.5;entryX=1;entryY=0.4;entryDx=0;entryDy=0;fontColor=#
          +ef6c00;fontSize=9;" edge="1" parent="1" source="epf-ntb" targe
          +t="epf-core">                                                 
      123            <mxGeometry relative="1" as="geometry" />
      124          </mxCell>
      146 -        <mxCell id="e-mhi-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          -;exitY=0.5;entryX=1;entryY=0.55;fontColor=#ef6c00;fontSize=10;
          -" edge="1" parent="1" source="epf-mhi" target="epf-core">     
      125 +        <mxCell id="e-mhi-epf" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          +;exitY=0.5;entryX=1;entryY=0.6;entryDx=0;entryDy=0;fontColor=#
          +ef6c00;fontSize=9;" edge="1" parent="1" source="epf-mhi" targe
          +t="epf-core">                                                 
      126            <mxGeometry relative="1" as="geometry" />
      127          </mxCell>
      149 -        <mxCell id="e-vntb-epf" style="edgeStyle=orthogonalEdg
          -eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=
          -0;exitY=0.5;entryX=1;entryY=0.7;fontColor=#ef6c00;fontSize=10;
          -" edge="1" parent="1" source="epf-vntb" target="epf-core">    
      128 +        <mxCell id="e-vntb-epf" style="edgeStyle=orthogonalEdg
          +eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=
          +0;exitY=0.5;entryX=1;entryY=0.8;entryDx=0;entryDy=0;fontColor=
          +#ef6c00;fontSize=9;" edge="1" parent="1" source="epf-vntb" tar
          +get="epf-core">                                               
      129            <mxGeometry relative="1" as="geometry" />
      130          </mxCell>
      152 -        <mxCell id="e-custom-epf" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          -X=0;exitY=0.5;entryX=1;entryY=0.85;fontColor=#ef6c00;fontSize=
          -10;dashed=1;dashPattern=4 4;" edge="1" parent="1" source="cust
          -om-epf" target="epf-core">                                    
      131 +        <mxCell id="e-custom-epf" style="edgeStyle=orthogonalE
          +dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          +X=0;exitY=0.5;entryX=1;entryY=0.9;entryDx=0;entryDy=0;fontColo
          +r=#ef6c00;fontSize=9;dashed=1;" edge="1" parent="1" source="cu
          +stom-epf" target="epf-core">                                  
      132            <mxGeometry relative="1" as="geometry" />
      133          </mxCell>
      134  
      156 -        <!-- EPC Drivers -> EPC Core -->                      
      157 -        <mxCell id="e-dwc-epc" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          -.5;exitY=0;entryX=0.25;entryY=1;fontColor=#7b1fa2;fontSize=10;
          -" edge="1" parent="1" source="dwc-ep" target="epc-core">      
      158 -          <mxGeometry relative="1" as="geometry">             
      159 -            <Array as="points">                               
      160 -              <mxPoint x="195" y="270" />                     
      161 -              <mxPoint x="555" y="270" />                     
      162 -            </Array>                                          
      163 -          </mxGeometry>                                       
      164 -        </mxCell>                                             
      165 -        <mxCell id="e-cadence-epc" style="edgeStyle=orthogonal
          -EdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exi
          -tX=0.5;exitY=0;entryX=0.4;entryY=1;fontColor=#7b1fa2;fontSize=
          -10;" edge="1" parent="1" source="cadence-ep" target="epc-core"
          ->                                                             
      166 -          <mxGeometry relative="1" as="geometry">             
      167 -            <Array as="points">                               
      168 -              <mxPoint x="195" y="270" />                     
      169 -              <mxPoint x="583" y="270" />                     
      170 -            </Array>                                          
      171 -          </mxGeometry>                                       
      172 -        </mxCell>                                             
      173 -        <mxCell id="e-rk-epc" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.
          -5;exitY=0;entryX=0.55;entryY=1;fontColor=#7b1fa2;fontSize=10;"
          - edge="1" parent="1" source="rk-ep" target="epc-core">        
      174 -          <mxGeometry relative="1" as="geometry">             
      175 -            <Array as="points">                               
      176 -              <mxPoint x="195" y="270" />                     
      177 -              <mxPoint x="614" y="270" />                     
      178 -            </Array>                                          
      179 -          </mxGeometry>                                       
      180 -        </mxCell>                                             
      181 -        <mxCell id="e-qcom-epc" style="edgeStyle=orthogonalEdg
          -eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=
          -0.5;exitY=0;entryX=0.7;entryY=1;fontColor=#7b1fa2;fontSize=10;
          -" edge="1" parent="1" source="qcom-ep" target="epc-core">     
      182 -          <mxGeometry relative="1" as="geometry">             
      183 -            <Array as="points">                               
      184 -              <mxPoint x="195" y="270" />                     
      185 -              <mxPoint x="645" y="270" />                     
      186 -            </Array>                                          
      187 -          </mxGeometry>                                       
      188 -        </mxCell>                                             
      189 -                                                              
      135          <!-- EPC Drivers -> Hardware -->
      191 -        <mxCell id="e-dwc-hw" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          -exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="dw
          -c-ep" target="dwc-hw">                                        
      136 +        <mxCell id="e-dwc-hw" style="edgeStyle=orthogonalEdgeS
          +tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          +exitY=0.5;entryX=0;entryY=0.15;entryDx=0;entryDy=0;" edge="1" 
          +parent="1" source="dwc-ep" target="dwc-hw">                   
      137            <mxGeometry relative="1" as="geometry" />
      138          </mxCell>
      194 -        <mxCell id="e-cadence-hw" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          -X=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source
          -="cadence-ep" target="cdn-hw">                                
      139 +        <mxCell id="e-cadence-hw" style="edgeStyle=orthogonalE
          +dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          +X=1;exitY=0.5;entryX=0;entryY=0.38;entryDx=0;entryDy=0;" edge=
          +"1" parent="1" source="cadence-ep" target="cdn-hw">           
      140            <mxGeometry relative="1" as="geometry" />
      141          </mxCell>
      197 -        <mxCell id="e-rk-hw" style="edgeStyle=orthogonalEdgeSt
          -yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;e
          -xitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="rk-
          -ep" target="rk-hw">                                           
      142 +        <mxCell id="e-rk-hw" style="edgeStyle=orthogonalEdgeSt
          +yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;e
          +xitY=0.5;entryX=0;entryY=0.62;entryDx=0;entryDy=0;" edge="1" p
          +arent="1" source="rk-ep" target="rk-hw">                      
      143            <mxGeometry relative="1" as="geometry" />
      144          </mxCell>
      200 -        <mxCell id="e-qcom-hw" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          -;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="q
          -com-ep" target="qcom-hw">                                     
      145 +        <mxCell id="e-qcom-hw" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          +;exitY=0.5;entryX=0;entryY=0.85;entryDx=0;entryDy=0;" edge="1"
          + parent="1" source="qcom-ep" target="qcom-hw">                
      146            <mxGeometry relative="1" as="geometry" />
      147          </mxCell>
      148  
      149          <!-- Hardware -> Remote RC -->
      205 -        <mxCell id="e-dwc-rc" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          -exitY=0.5;entryX=0;entryY=0.3;fontColor=#c62828;fontStyle=1;" 
          -edge="1" parent="1" source="dwc-hw" target="rc-host">         
      206 -          <mxGeometry relative="1" as="geometry">             
      207 -            <mxCell style="edgeLabel;html=1;align=center;verti
          -calAlign=middle;resizable=0;points=[];" vertex="1" connectable
          -="0" parent="e-dwc-rc">                                       
      208 -              <mxGeometry x="-0.1" relative="1" as="geometry">
      209 -                <mxPoint x="-4" y="-9" as="offset" />         
      210 -              </mxGeometry>                                   
      211 -            </mxCell>                                         
      212 -          </mxGeometry>                                       
      150 +        <mxCell id="e-dwc-rc" style="edgeStyle=orthogonalEdgeS
          +tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          +exitY=0.5;entryX=0;entryY=0.25;entryDx=0;entryDy=0;fontColor=#
          +c62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source=
          +"dwc-hw" target="rc-host">                                    
      151 +          <mxGeometry relative="1" as="geometry" />           
      152          </mxCell>
      214 -        <mxCell id="e-cdn-rc" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          -exitY=0.5;entryX=0;entryY=0.45;fontColor=#c62828;fontStyle=1;"
          - edge="1" parent="1" source="cdn-hw" target="rc-host">        
      153 +        <mxCell id="e-cdn-rc" style="edgeStyle=orthogonalEdgeS
          +tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          +exitY=0.5;entryX=0;entryY=0.45;entryDx=0;entryDy=0;fontColor=#
          +c62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source=
          +"cdn-hw" target="rc-host">                                    
      154            <mxGeometry relative="1" as="geometry" />
      155          </mxCell>
      217 -        <mxCell id="e-rk-rc" style="edgeStyle=orthogonalEdgeSt
          -yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;e
          -xitY=0.5;entryX=0;entryY=0.6;fontColor=#c62828;fontStyle=1;" e
          -dge="1" parent="1" source="rk-hw" target="rc-host">           
      156 +        <mxCell id="e-rk-rc" style="edgeStyle=orthogonalEdgeSt
          +yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;e
          +xitY=0.5;entryX=0;entryY=0.65;entryDx=0;entryDy=0;fontColor=#c
          +62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source="
          +rk-hw" target="rc-host">                                      
      157            <mxGeometry relative="1" as="geometry" />
      158          </mxCell>
      220 -        <mxCell id="e-qcom-rc" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          -;exitY=0.5;entryX=0;entryY=0.75;fontColor=#c62828;fontStyle=1;
          -" edge="1" parent="1" source="qcom-hw" target="rc-host">      
      159 +        <mxCell id="e-qcom-rc" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          +;exitY=0.5;entryX=0;entryY=0.85;entryDx=0;entryDy=0;fontColor=
          +#c62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source
          +="qcom-hw" target="rc-host">                                  
      160            <mxGeometry relative="1" as="geometry" />
      161          </mxCell>
      162  
      224 -        <!-- Legend -->                                       
      225 -        <mxCell id="legend-box" value="📋 Legend" style="swiml
          -ane;whiteSpace=wrap;html=1;fillColor=#fafafa;strokeColor=#9e9e
          -9e;fontStyle=1;fontSize=11;" vertex="1" parent="1">           
      226 -          <mxGeometry x="750" y="500" width="250" height="80" 
          -as="geometry" />                                              
      227 -        </mxCell>                                             
      228 -        <mxCell id="legend-reg" value="─── register / call" st
          -yle="text;html=1;align=left;verticalAlign=middle;fontSize=10;f
          -ontColor=#616161;" vertex="1" parent="legend-box">            
      229 -          <mxGeometry x="15" y="25" width="220" height="20" as
          -="geometry" />                                                
      230 -        </mxCell>                                             
      231 -        <mxCell id="legend-pcie" value="&lt;==&gt; PCIe Link" 
          -style="text;html=1;align=left;verticalAlign=middle;fontSize=10
          -;fontColor=#616161;" vertex="1" parent="legend-box">          
      232 -          <mxGeometry x="15" y="48" width="220" height="20" as
          -="geometry" />                                                
      233 -        </mxCell>                                             
      234 -                                                              
      163        </root>
      164      </mxGraphModel>
      165    </diagram>

  Thought for 24s (ctrl+o to expand)

● Write(pcie_epf_framework.drawio)
  ⎿  Added 114 lines, removed 81 lines
        5          <mxCell id="0" />
        6          <mxCell id="1" parent="0" />
        7  
        8 -        <!-- ===== Userspace Group ===== -->                  
        9 -        <mxCell id="g-userspace" value="Userspace" style="swim
          -lane;whiteSpace=wrap;html=1;fillColor=#e8f5e9;strokeColor=#2e7
          -d32;fontStyle=1;fontSize=13;startSize=28;" vertex="1" parent="
          -1">                                                           
       10 -          <mxGeometry x="40" y="40" width="200" height="145" a
          -s="geometry" />                                               
        8 +        <!-- ===== Group Backgrounds (visual containers) =====
          + -->                                                          
        9 +        <mxCell id="bg-userspace" value="" style="rounded=0;wh
          +iteSpace=wrap;html=1;fillColor=#e8f5e9;strokeColor=#2e7d32;str
          +okeWidth=2;opacity=30;" vertex="1" parent="1">                
       10 +          <mxGeometry x="30" y="60" width="210" height="170" a
          +s="geometry" />                                               
       11          </mxCell>
       12 -        <mxCell id="app" value="Application / Test Tools" styl
          -e="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCo
          -lor=#2e7d32;" vertex="1" parent="g-userspace">                
       13 -          <mxGeometry x="12" y="32" width="176" height="34" as
          -="geometry" />                                                
       12 +        <mxCell id="lbl-userspace" value="Userspace" style="te
          +xt;html=1;fontStyle=1;fontSize=14;fontColor=#2e7d32;" vertex="
          +1" parent="1">                                                
       13 +          <mxGeometry x="40" y="32" width="100" height="24" as
          +="geometry" />                                                
       14          </mxCell>
       15 -        <mxCell id="configfs" value="configfs Interface" style
          -="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCol
          -or=#2e7d32;" vertex="1" parent="g-userspace">                 
       16 -          <mxGeometry x="12" y="72" width="176" height="28" as
          -="geometry" />                                                
       15 +                                                              
       16 +        <mxCell id="bg-framework" value="" style="rounded=0;wh
          +iteSpace=wrap;html=1;fillColor=#e3f2fd;strokeColor=#1565c0;str
          +okeWidth=2;opacity=30;" vertex="1" parent="1">                
       17 +          <mxGeometry x="300" y="60" width="470" height="170" 
          +as="geometry" />                                              
       18          </mxCell>
       18 -        <mxCell id="sysfs" value="sysfs Interface" style="roun
          -ded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColor=#2e
          -7d32;" vertex="1" parent="g-userspace">                       
       19 -          <mxGeometry x="12" y="106" width="176" height="28" a
          -s="geometry" />                                               
       19 +        <mxCell id="lbl-framework" value="PCIe Endpoint Framew
          +ork (drivers/pci/endpoint/)" style="text;html=1;fontStyle=1;fo
          +ntSize=14;fontColor=#1565c0;" vertex="1" parent="1">          
       20 +          <mxGeometry x="310" y="32" width="400" height="24" a
          +s="geometry" />                                               
       21          </mxCell>
       22  
       22 -        <!-- ===== EPF Framework Group ===== -->              
       23 -        <mxCell id="g-framework" value="PCIe Endpoint Framewor
          -k (drivers/pci/endpoint/)" style="swimlane;whiteSpace=wrap;htm
          -l=1;fillColor=#e3f2fd;strokeColor=#1565c0;fontStyle=1;fontSize
          -=13;startSize=28;" vertex="1" parent="1">                     
       24 -          <mxGeometry x="310" y="40" width="460" height="145" 
          -as="geometry" />                                              
       23 +        <mxCell id="bg-epf-drivers" value="" style="rounded=0;
          +whiteSpace=wrap;html=1;fillColor=#fff3e0;strokeColor=#ef6c00;s
          +trokeWidth=2;opacity=30;" vertex="1" parent="1">              
       24 +          <mxGeometry x="850" y="60" width="300" height="300" 
          +as="geometry" />                                              
       25          </mxCell>
       26 -        <mxCell id="epf-core" value="pci-epf-core&#xa;EPF Core
          -&#xa;─────────────────&#xa;epf_device_register()&#xa;epf_drive
          -r_register()&#xa;epf_bind / unbind" style="rounded=1;whiteSpac
          -e=wrap;html=1;fillColor=#bbdefb;strokeColor=#1565c0;align=left
          -;spacingLeft=8;" vertex="1" parent="g-framework">             
       27 -          <mxGeometry x="10" y="30" width="215" height="108" a
          -s="geometry" />                                               
       26 +        <mxCell id="lbl-epf-drivers" value="EPF Drivers" style
          +="text;html=1;fontStyle=1;fontSize=14;fontColor=#ef6c00;" vert
          +ex="1" parent="1">                                            
       27 +          <mxGeometry x="860" y="32" width="150" height="24" a
          +s="geometry" />                                               
       28          </mxCell>
       29 -        <mxCell id="epc-core" value="pci-epc-core&#xa;EPC Core
          -&#xa;─────────────────&#xa;epc_device_register()&#xa;epc_drive
          -r_register()&#xa;BAR / IRQ / DMA ops" style="rounded=1;whiteSp
          -ace=wrap;html=1;fillColor=#bbdefb;strokeColor=#1565c0;align=le
          -ft;spacingLeft=8;" vertex="1" parent="g-framework">           
       30 -          <mxGeometry x="238" y="30" width="212" height="108" 
          -as="geometry" />                                              
       31 -        </mxCell>                                             
       29  
       33 -        <!-- ===== EPF Drivers Group ===== -->                
       34 -        <mxCell id="g-epf-drivers" value="EPF Drivers" style="
          -swimlane;whiteSpace=wrap;html=1;fillColor=#fff3e0;strokeColor=
          -#ef6c00;fontStyle=1;fontSize=13;startSize=28;" vertex="1" pare
          -nt="1">                                                       
       35 -          <mxGeometry x="850" y="40" width="290" height="280" 
          -as="geometry" />                                              
       30 +        <mxCell id="bg-epc-drivers" value="" style="rounded=0;
          +whiteSpace=wrap;html=1;fillColor=#f3e5f5;strokeColor=#7b1fa2;s
          +trokeWidth=2;opacity=30;" vertex="1" parent="1">              
       31 +          <mxGeometry x="30" y="290" width="280" height="280" 
          +as="geometry" />                                              
       32          </mxCell>
       37 -        <mxCell id="epf-test" value="pci-epf-test&#xa;────────
          -────&#xa;Loopback / DMA test" style="rounded=1;whiteSpace=wrap
          -;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spaci
          -ngLeft=8;" vertex="1" parent="g-epf-drivers">                 
       38 -          <mxGeometry x="12" y="32" width="266" height="42" as
          -="geometry" />                                                
       33 +        <mxCell id="lbl-epc-drivers" value="EPC Drivers (ARM64
          +)" style="text;html=1;fontStyle=1;fontSize=14;fontColor=#7b1fa
          +2;" vertex="1" parent="1">                                    
       34 +          <mxGeometry x="40" y="262" width="200" height="24" a
          +s="geometry" />                                               
       35          </mxCell>
       40 -        <mxCell id="epf-ntb" value="pci-epf-ntb&#xa;──────────
          -──&#xa;NTB function" style="rounded=1;whiteSpace=wrap;html=1;f
          -illColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8;
          -" vertex="1" parent="g-epf-drivers">                          
       41 -          <mxGeometry x="12" y="80" width="266" height="42" as
          -="geometry" />                                                
       36 +                                                              
       37 +        <mxCell id="bg-hw" value="" style="rounded=0;whiteSpac
          +e=wrap;html=1;fillColor=#fce4ec;strokeColor=#c62828;strokeWidt
          +h=2;opacity=30;" vertex="1" parent="1">                       
       38 +          <mxGeometry x="380" y="290" width="280" height="280"
          + as="geometry" />                                             
       39          </mxCell>
       43 -        <mxCell id="epf-mhi" value="pci-epf-mhi&#xa;──────────
          -──&#xa;MHI (modem)" style="rounded=1;whiteSpace=wrap;html=1;fi
          -llColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8;"
          - vertex="1" parent="g-epf-drivers">                           
       44 -          <mxGeometry x="12" y="128" width="266" height="42" a
          -s="geometry" />                                               
       40 +        <mxCell id="lbl-hw" value="ARM64 PCIe EP Hardware" sty
          +le="text;html=1;fontStyle=1;fontSize=14;fontColor=#c62828;" ve
          +rtex="1" parent="1">                                          
       41 +          <mxGeometry x="390" y="262" width="260" height="24" 
          +as="geometry" />                                              
       42          </mxCell>
       46 -        <mxCell id="epf-vntb" value="pci-epf-vntb&#xa;────────
          -────&#xa;Virtual NTB" style="rounded=1;whiteSpace=wrap;html=1;
          -fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8
          -;" vertex="1" parent="g-epf-drivers">                         
       47 -          <mxGeometry x="12" y="176" width="266" height="42" a
          -s="geometry" />                                               
       43 +                                                              
       44 +        <mxCell id="bg-rc" value="" style="rounded=0;whiteSpac
          +e=wrap;html=1;fillColor=#e0e0e0;strokeColor=#424242;strokeWidt
          +h=2;opacity=30;" vertex="1" parent="1">                       
       45 +          <mxGeometry x="750" y="375" width="250" height="90" 
          +as="geometry" />                                              
       46          </mxCell>
       49 -        <mxCell id="custom-epf" value="Custom EPF Driver&#xa;─
          -───────────&#xa;User-defined function" style="rounded=1;whiteS
          -pace=wrap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=l
          -eft;spacingLeft=8;dashed=1;" vertex="1" parent="g-epf-drivers"
          ->                                                             
       50 -          <mxGeometry x="12" y="224" width="266" height="42" a
          -s="geometry" />                                               
       47 +        <mxCell id="lbl-rc" value="Remote PCIe Root Complex" s
          +tyle="text;html=1;fontStyle=1;fontSize=14;fontColor=#424242;" 
          +vertex="1" parent="1">                                        
       48 +          <mxGeometry x="760" y="347" width="230" height="24" 
          +as="geometry" />                                              
       49          </mxCell>
       50  
       53 -        <!-- ===== EPC Drivers Group ===== -->                
       54 -        <mxCell id="g-epc-drivers" value="EPC Drivers (ARM64)"
          - style="swimlane;whiteSpace=wrap;html=1;fillColor=#f3e5f5;stro
          -keColor=#7b1fa2;fontStyle=1;fontSize=13;startSize=28;" vertex=
          -"1" parent="1">                                               
       55 -          <mxGeometry x="40" y="260" width="270" height="260" 
          -as="geometry" />                                              
       51 +        <!-- ===== Nodes ===== -->                            
       52 +                                                              
       53 +        <!-- Userspace -->                                    
       54 +        <mxCell id="app" value="Application / Test Tools" styl
          +e="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCo
          +lor=#2e7d32;fontStyle=1;" vertex="1" parent="1">              
       55 +          <mxGeometry x="50" y="72" width="170" height="32" as
          +="geometry" />                                                
       56          </mxCell>
       57 -        <mxCell id="dwc-ep" value="pcie-dw-plat-ep&#xa;───────
          -───────&#xa;DesignWare DWC EP" style="rounded=1;whiteSpace=wra
          -p;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spac
          -ingLeft=8;" vertex="1" parent="g-epc-drivers">                
       58 -          <mxGeometry x="12" y="32" width="246" height="48" as
          -="geometry" />                                                
       57 +        <mxCell id="configfs" value="configfs Interface" style
          +="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCol
          +or=#2e7d32;" vertex="1" parent="1">                           
       58 +          <mxGeometry x="50" y="118" width="170" height="30" a
          +s="geometry" />                                               
       59          </mxCell>
       60 -        <mxCell id="cadence-ep" value="pcie-cadence-ep&#xa;───
          -───────────&#xa;Cadence EP" style="rounded=1;whiteSpace=wrap;h
          -tml=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacing
          -Left=8;" vertex="1" parent="g-epc-drivers">                   
       61 -          <mxGeometry x="12" y="86" width="246" height="48" as
          -="geometry" />                                                
       60 +        <mxCell id="sysfs" value="sysfs Interface" style="roun
          +ded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColor=#2e
          +7d32;" vertex="1" parent="1">                                 
       61 +          <mxGeometry x="50" y="160" width="170" height="30" a
          +s="geometry" />                                               
       62          </mxCell>
       63 -        <mxCell id="rk-ep" value="pcie-rockchip-ep&#xa;───────
          -───────&#xa;Rockchip EP" style="rounded=1;whiteSpace=wrap;html
          -=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacingLef
          -t=8;" vertex="1" parent="g-epc-drivers">                      
       64 -          <mxGeometry x="12" y="140" width="246" height="48" a
          -s="geometry" />                                               
       63 +                                                              
       64 +        <!-- EPF Framework Core -->                           
       65 +        <mxCell id="epf-core" value="pci-epf-core&lt;br&gt;&lt
          +;b&gt;EPF Core&lt;/b&gt;&lt;br&gt;&lt;hr size=&quot;1&quot;&gt
          +;&lt;font style=&quot;font-size:10px&quot;&gt;epf_device_regis
          +ter()&lt;br&gt;epf_driver_register()&lt;br&gt;epf_bind / unbin
          +d&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillCo
          +lor=#bbdefb;strokeColor=#1565c0;align=left;spacingLeft=8;" ver
          +tex="1" parent="1">                                           
       66 +          <mxGeometry x="315" y="72" width="210" height="145" 
          +as="geometry" />                                              
       67          </mxCell>
       66 -        <mxCell id="qcom-ep" value="pcie-qcom-ep&#xa;─────────
          -─────&#xa;Qualcomm EP" style="rounded=1;whiteSpace=wrap;html=1
          -;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacingLeft=
          -8;" vertex="1" parent="g-epc-drivers">                        
       67 -          <mxGeometry x="12" y="194" width="246" height="48" a
          -s="geometry" />                                               
       68 +        <mxCell id="epc-core" value="pci-epc-core&lt;br&gt;&lt
          +;b&gt;EPC Core&lt;/b&gt;&lt;br&gt;&lt;hr size=&quot;1&quot;&gt
          +;&lt;font style=&quot;font-size:10px&quot;&gt;epc_device_regis
          +ter()&lt;br&gt;epc_driver_register()&lt;br&gt;BAR / IRQ / DMA 
          +ops&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fill
          +Color=#bbdefb;strokeColor=#1565c0;align=left;spacingLeft=8;" v
          +ertex="1" parent="1">                                         
       69 +          <mxGeometry x="545" y="72" width="210" height="145" 
          +as="geometry" />                                              
       70          </mxCell>
       71  
       70 -        <!-- ===== Hardware Group ===== -->                   
       71 -        <mxCell id="g-hw" value="ARM64 PCIe EP Hardware" style
          -="swimlane;whiteSpace=wrap;html=1;fillColor=#fce4ec;strokeColo
          -r=#c62828;fontStyle=1;fontSize=13;startSize=28;" vertex="1" pa
          -rent="1">                                                     
       72 -          <mxGeometry x="370" y="260" width="270" height="260"
          - as="geometry" />                                             
       72 +        <!-- EPF Drivers -->                                  
       73 +        <mxCell id="epf-test" value="pci-epf-test&lt;br&gt;───
          +───────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;L
          +oopback / DMA test&lt;/font&gt;" style="rounded=1;whiteSpace=w
          +rap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;sp
          +acingLeft=8;" vertex="1" parent="1">                          
       74 +          <mxGeometry x="865" y="72" width="270" height="42" a
          +s="geometry" />                                               
       75          </mxCell>
       74 -        <mxCell id="dwc-hw" value="Synopsys DWC&#xa;PCIe Endpo
          -int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          -;strokeColor=#c62828;" vertex="1" parent="g-hw">              
       75 -          <mxGeometry x="12" y="32" width="246" height="48" as
          -="geometry" />                                                
       76 +        <mxCell id="epf-ntb" value="pci-epf-ntb&lt;br&gt;─────
          +─────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;NTB
          + function&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=
          +1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft
          +=8;" vertex="1" parent="1">                                   
       77 +          <mxGeometry x="865" y="122" width="270" height="42" 
          +as="geometry" />                                              
       78          </mxCell>
       77 -        <mxCell id="cdn-hw" value="Cadence&#xa;PCIe Endpoint" 
          -style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;stro
          -keColor=#c62828;" vertex="1" parent="g-hw">                   
       78 -          <mxGeometry x="12" y="86" width="246" height="48" as
          -="geometry" />                                                
       79 +        <mxCell id="epf-mhi" value="pci-epf-mhi&lt;br&gt;─────
          +─────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;MHI
          + (modem)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1
          +;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=
          +8;" vertex="1" parent="1">                                    
       80 +          <mxGeometry x="865" y="172" width="270" height="42" 
          +as="geometry" />                                              
       81          </mxCell>
       80 -        <mxCell id="rk-hw" value="Rockchip&#xa;PCIe Endpoint" 
          -style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;stro
          -keColor=#c62828;" vertex="1" parent="g-hw">                   
       81 -          <mxGeometry x="12" y="140" width="246" height="48" a
          -s="geometry" />                                               
       82 +        <mxCell id="epf-vntb" value="pci-epf-vntb&lt;br&gt;───
          +───────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;V
          +irtual NTB&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html
          +=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLef
          +t=8;" vertex="1" parent="1">                                  
       83 +          <mxGeometry x="865" y="222" width="270" height="42" 
          +as="geometry" />                                              
       84          </mxCell>
       83 -        <mxCell id="qcom-hw" value="Qualcomm&#xa;PCIe Endpoint
          -" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;st
          -rokeColor=#c62828;" vertex="1" parent="g-hw">                 
       84 -          <mxGeometry x="12" y="194" width="246" height="48" a
          -s="geometry" />                                               
       85 +        <mxCell id="custom-epf" value="Custom EPF Driver&lt;br
          +&gt;──────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quo
          +t;&gt;User-defined function&lt;/font&gt;" style="rounded=1;whi
          +teSpace=wrap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;alig
          +n=left;spacingLeft=8;dashed=1;" vertex="1" parent="1">        
       86 +          <mxGeometry x="865" y="272" width="270" height="42" 
          +as="geometry" />                                              
       87          </mxCell>
       88  
       87 -        <!-- ===== Remote RC ===== -->                        
       88 -        <mxCell id="g-rc" value="Remote PCIe Root Complex" sty
          -le="swimlane;whiteSpace=wrap;html=1;fillColor=#e0e0e0;strokeCo
          -lor=#424242;fontStyle=1;fontSize=13;startSize=28;" vertex="1" 
          -parent="1">                                                   
       89 -          <mxGeometry x="730" y="345" width="240" height="75" 
          -as="geometry" />                                              
       89 +        <!-- EPC Drivers -->                                  
       90 +        <mxCell id="dwc-ep" value="pcie-dw-plat-ep&lt;br&gt;──
          +────────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;
          +&gt;DesignWare DWC EP&lt;/font&gt;" style="rounded=1;whiteSpac
          +e=wrap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left
          +;spacingLeft=8;" vertex="1" parent="1">                       
       91 +          <mxGeometry x="45" y="302" width="250" height="52" a
          +s="geometry" />                                               
       92          </mxCell>
       91 -        <mxCell id="rc-host" value="PCIe RC Host (x86 / ARM64)
          -" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#eeeeee;st
          -rokeColor=#424242;fontStyle=1;" vertex="1" parent="g-rc">     
       92 -          <mxGeometry x="20" y="32" width="200" height="32" as
          -="geometry" />                                                
       93 +        <mxCell id="cadence-ep" value="pcie-cadence-ep&lt;br&g
          +t;──────────────&lt;br&gt;&lt;font style=&quot;font-size:9px&q
          +uot;&gt;Cadence EP&lt;/font&gt;" style="rounded=1;whiteSpace=w
          +rap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;sp
          +acingLeft=8;" vertex="1" parent="1">                          
       94 +          <mxGeometry x="45" y="362" width="250" height="52" a
          +s="geometry" />                                               
       95          </mxCell>
       96 +        <mxCell id="rk-ep" value="pcie-rockchip-ep&lt;br&gt;──
          +────────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;
          +&gt;Rockchip EP&lt;/font&gt;" style="rounded=1;whiteSpace=wrap
          +;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spaci
          +ngLeft=8;" vertex="1" parent="1">                             
       97 +          <mxGeometry x="45" y="422" width="250" height="52" a
          +s="geometry" />                                               
       98 +        </mxCell>                                             
       99 +        <mxCell id="qcom-ep" value="pcie-qcom-ep&lt;br&gt;────
          +──────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&g
          +t;Qualcomm EP&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;h
          +tml=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacing
          +Left=8;" vertex="1" parent="1">                               
      100 +          <mxGeometry x="45" y="482" width="250" height="52" a
          +s="geometry" />                                               
      101 +        </mxCell>                                             
      102  
       95 -        <!-- ===== EDGES ===== -->                            
      103 +        <!-- Hardware -->                                     
      104 +        <mxCell id="dwc-hw" value="Synopsys DWC&lt;br&gt;PCIe 
          +Endpoint" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f
          +fcdd2;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">
      105 +          <mxGeometry x="395" y="302" width="250" height="52" 
          +as="geometry" />                                              
      106 +        </mxCell>                                             
      107 +        <mxCell id="cdn-hw" value="Cadence&lt;br&gt;PCIe Endpo
          +int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          +;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">     
      108 +          <mxGeometry x="395" y="362" width="250" height="52" 
          +as="geometry" />                                              
      109 +        </mxCell>                                             
      110 +        <mxCell id="rk-hw" value="Rockchip&lt;br&gt;PCIe Endpo
          +int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          +;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">     
      111 +          <mxGeometry x="395" y="422" width="250" height="52" 
          +as="geometry" />                                              
      112 +        </mxCell>                                             
      113 +        <mxCell id="qcom-hw" value="Qualcomm&lt;br&gt;PCIe End
          +point" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcd
          +d2;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">   
      114 +          <mxGeometry x="395" y="482" width="250" height="52" 
          +as="geometry" />                                              
      115 +        </mxCell>                                             
      116  
       97 -        <!-- Userspace internal -->                           
      117 +        <!-- Remote RC -->                                    
      118 +        <mxCell id="rc-host" value="PCIe RC Host (x86 / ARM64)
          +" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#eeeeee;st
          +rokeColor=#424242;fontStyle=1;" vertex="1" parent="1">        
      119 +          <mxGeometry x="770" y="400" width="210" height="40" 
          +as="geometry" />                                              
      120 +        </mxCell>                                             
      121 +                                                              
      122 +        <!-- ===== EDGES (all parent="1") ===== -->           
      123 +                                                              
      124          <mxCell id="e-app-config" style="edgeStyle=orthogonalE
           dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" ed
           ge="1" parent="1" source="app" target="configfs">
      125            <mxGeometry relative="1" as="geometry" />
      126          </mxCell>
      127  
      102 -        <!-- Userspace -> EPF Core -->                        
      103 -        <mxCell id="e-config-epf" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          -X=1;exitY=0.5;entryX=0;entryY=0.3;entryDx=0;entryDy=0;" edge="
          -1" parent="1" source="configfs" target="epf-core">            
      128 +        <mxCell id="e-config-epf" style="edgeStyle=orthogonalE
          +dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" ed
          +ge="1" parent="1" source="configfs" target="epf-core">        
      129            <mxGeometry relative="1" as="geometry" />
      130          </mxCell>
      106 -        <mxCell id="e-sysfs-epf" style="edgeStyle=orthogonalEd
          -geStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX
          -=1;exitY=0.5;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1
          -" parent="1" source="sysfs" target="epf-core">                
      131 +                                                              
      132 +        <mxCell id="e-sysfs-epf" style="edgeStyle=orthogonalEd
          +geStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edg
          +e="1" parent="1" source="sysfs" target="epf-core">            
      133            <mxGeometry relative="1" as="geometry" />
      134          </mxCell>
      135  
      110 -        <!-- EPF Core <-> EPC Core dual edge -->              
      111 -        <mxCell id="e-epf-epc" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          -;exitY=0.35;entryX=0;entryY=0.35;entryDx=0;entryDy=0;fontColor
          -=#1565c0;fontStyle=1;" edge="1" parent="1" source="epf-core" t
          -arget="epc-core">                                             
      136 +        <mxCell id="e-epf-epc" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontCol
          +or=#1565c0;fontStyle=1;" edge="1" parent="1" source="epf-core"
          + target="epc-core">                                           
      137            <mxGeometry relative="1" as="geometry" />
      138          </mxCell>
      114 -        <mxCell id="e-epc-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          -;exitY=0.65;entryX=1;entryY=0.65;entryDx=0;entryDy=0;fontColor
          -=#1565c0;fontStyle=1;" edge="1" parent="1" source="epc-core" t
          -arget="epf-core">                                             
      139 +                                                              
      140 +        <mxCell id="e-epc-epf" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontCol
          +or=#1565c0;fontStyle=1;" edge="1" parent="1" source="epc-core"
          + target="epf-core">                                           
      141            <mxGeometry relative="1" as="geometry" />
      142          </mxCell>
      143  
      118 -        <!-- EPF Drivers -> EPF Core -->                      
      119 -        <mxCell id="e-test-epf" style="edgeStyle=orthogonalEdg
          -eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=
          -0;exitY=0.5;entryX=1;entryY=0.2;entryDx=0;entryDy=0;fontColor=
          -#ef6c00;fontSize=9;" edge="1" parent="1" source="epf-test" tar
          -get="epf-core">                                               
      144 +        <mxCell id="e-test-epf" style="edgeStyle=orthogonalEdg
          +eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge
          +="1" parent="1" source="epf-test" target="epf-core">          
      145            <mxGeometry relative="1" as="geometry" />
      146          </mxCell>
      122 -        <mxCell id="e-ntb-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          -;exitY=0.5;entryX=1;entryY=0.4;entryDx=0;entryDy=0;fontColor=#
          -ef6c00;fontSize=9;" edge="1" parent="1" source="epf-ntb" targe
          -t="epf-core">                                                 
      147 +                                                              
      148 +        <mxCell id="e-ntb-epf" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge=
          +"1" parent="1" source="epf-ntb" target="epf-core">            
      149            <mxGeometry relative="1" as="geometry" />
      150          </mxCell>
      125 -        <mxCell id="e-mhi-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0
          -;exitY=0.5;entryX=1;entryY=0.6;entryDx=0;entryDy=0;fontColor=#
          -ef6c00;fontSize=9;" edge="1" parent="1" source="epf-mhi" targe
          -t="epf-core">                                                 
      151 +                                                              
      152 +        <mxCell id="e-mhi-epf" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge=
          +"1" parent="1" source="epf-mhi" target="epf-core">            
      153            <mxGeometry relative="1" as="geometry" />
      154          </mxCell>
      128 -        <mxCell id="e-vntb-epf" style="edgeStyle=orthogonalEdg
          -eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=
          -0;exitY=0.5;entryX=1;entryY=0.8;entryDx=0;entryDy=0;fontColor=
          -#ef6c00;fontSize=9;" edge="1" parent="1" source="epf-vntb" tar
          -get="epf-core">                                               
      155 +                                                              
      156 +        <mxCell id="e-vntb-epf" style="edgeStyle=orthogonalEdg
          +eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge
          +="1" parent="1" source="epf-vntb" target="epf-core">          
      157            <mxGeometry relative="1" as="geometry" />
      158          </mxCell>
      131 -        <mxCell id="e-custom-epf" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          -X=0;exitY=0.5;entryX=1;entryY=0.9;entryDx=0;entryDy=0;fontColo
          -r=#ef6c00;fontSize=9;dashed=1;" edge="1" parent="1" source="cu
          -stom-epf" target="epf-core">                                  
      159 +                                                              
      160 +        <mxCell id="e-custom-epf" style="edgeStyle=orthogonalE
          +dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;dash
          +ed=1;" edge="1" parent="1" source="custom-epf" target="epf-cor
          +e">                                                           
      161            <mxGeometry relative="1" as="geometry" />
      162          </mxCell>
      163  
      135 -        <!-- EPC Drivers -> Hardware -->                      
      136 -        <mxCell id="e-dwc-hw" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          -exitY=0.5;entryX=0;entryY=0.15;entryDx=0;entryDy=0;" edge="1" 
          -parent="1" source="dwc-ep" target="dwc-hw">                   
      164 +        <mxCell id="e-dwc-hw" style="edgeStyle=orthogonalEdgeS
          +tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="
          +1" parent="1" source="dwc-ep" target="dwc-hw">                
      165            <mxGeometry relative="1" as="geometry" />
      166          </mxCell>
      139 -        <mxCell id="e-cadence-hw" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exit
          -X=1;exitY=0.5;entryX=0;entryY=0.38;entryDx=0;entryDy=0;" edge=
          -"1" parent="1" source="cadence-ep" target="cdn-hw">           
      167 +                                                              
      168 +        <mxCell id="e-cadence-hw" style="edgeStyle=orthogonalE
          +dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" ed
          +ge="1" parent="1" source="cadence-ep" target="cdn-hw">        
      169            <mxGeometry relative="1" as="geometry" />
      170          </mxCell>
      142 -        <mxCell id="e-rk-hw" style="edgeStyle=orthogonalEdgeSt
          -yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;e
          -xitY=0.5;entryX=0;entryY=0.62;entryDx=0;entryDy=0;" edge="1" p
          -arent="1" source="rk-ep" target="rk-hw">                      
      171 +                                                              
      172 +        <mxCell id="e-rk-hw" style="edgeStyle=orthogonalEdgeSt
          +yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1
          +" parent="1" source="rk-ep" target="rk-hw">                   
      173            <mxGeometry relative="1" as="geometry" />
      174          </mxCell>
      145 -        <mxCell id="e-qcom-hw" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          -;exitY=0.5;entryX=0;entryY=0.85;entryDx=0;entryDy=0;" edge="1"
          - parent="1" source="qcom-ep" target="qcom-hw">                
      175 +                                                              
      176 +        <mxCell id="e-qcom-hw" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge=
          +"1" parent="1" source="qcom-ep" target="qcom-hw">             
      177            <mxGeometry relative="1" as="geometry" />
      178          </mxCell>
      179  
      149 -        <!-- Hardware -> Remote RC -->                        
      150 -        <mxCell id="e-dwc-rc" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          -exitY=0.5;entryX=0;entryY=0.25;entryDx=0;entryDy=0;fontColor=#
          -c62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source=
          -"dwc-hw" target="rc-host">                                    
      180 +        <mxCell id="e-dwc-rc" style="edgeStyle=orthogonalEdgeS
          +tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColo
          +r=#c62828;strokeWidth=2;" edge="1" parent="1" source="dwc-hw" 
          +target="rc-host">                                             
      181            <mxGeometry relative="1" as="geometry" />
      182          </mxCell>
      153 -        <mxCell id="e-cdn-rc" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;
          -exitY=0.5;entryX=0;entryY=0.45;entryDx=0;entryDy=0;fontColor=#
          -c62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source=
          -"cdn-hw" target="rc-host">                                    
      183 +                                                              
      184 +        <mxCell id="e-cdn-rc" style="edgeStyle=orthogonalEdgeS
          +tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColo
          +r=#c62828;strokeWidth=2;" edge="1" parent="1" source="cdn-hw" 
          +target="rc-host">                                             
      185            <mxGeometry relative="1" as="geometry" />
      186          </mxCell>
      156 -        <mxCell id="e-rk-rc" style="edgeStyle=orthogonalEdgeSt
          -yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;e
          -xitY=0.5;entryX=0;entryY=0.65;entryDx=0;entryDy=0;fontColor=#c
          -62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source="
          -rk-hw" target="rc-host">                                      
      187 +                                                              
      188 +        <mxCell id="e-rk-rc" style="edgeStyle=orthogonalEdgeSt
          +yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor
          +=#c62828;strokeWidth=2;" edge="1" parent="1" source="rk-hw" ta
          +rget="rc-host">                                               
      189            <mxGeometry relative="1" as="geometry" />
      190          </mxCell>
      159 -        <mxCell id="e-qcom-rc" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1
          -;exitY=0.5;entryX=0;entryY=0.85;entryDx=0;entryDy=0;fontColor=
          -#c62828;fontStyle=1;strokeWidth=2;" edge="1" parent="1" source
          -="qcom-hw" target="rc-host">                                  
      191 +                                                              
      192 +        <mxCell id="e-qcom-rc" style="edgeStyle=orthogonalEdge
          +Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontCol
          +or=#c62828;strokeWidth=2;" edge="1" parent="1" source="qcom-hw
          +" target="rc-host">                                           
      193            <mxGeometry relative="1" as="geometry" />
      194          </mxCell>
      195  

  Thought for 8s (ctrl+o to expand)

● 这次我用了完全扁平的结构——所有节点和边都是
  parent="1"（无嵌套），用透明矩形做视觉分组。请再试试打开。

  如果还是失败，我换个思路——用 MCP 工具生成 XML，那个一定符合 draw.io 格式规范：

  Thought for 10s, ran 1 shell command (ctrl+o to expand)

● XML 结构验证通过（所有引用的 source/target 都存在）。让我用 MCP 工具生成一份
  draw.io 原生验证过的 XML，同时保存到本地：

  Thought for 5s, called drawio (ctrl+o to expand)

● MCP 工具接受了这份 XML 并成功渲染。我把同一份 XML 存到本地文件：

● Write(pcie_epf_framework.drawio)  ⎿  Added 89 lines, removed 142 lines
        2    <diagram name="ARM64 PCIe EPF Framework" id="pcie-epf">
        3      <mxGraphModel dx="1400" dy="1000" grid="1" gridSize="10" g
           uides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1
           " pageScale="1" pageWidth="1200" pageHeight="950">
        4        <root>
        5 -        <mxCell id="0" />                                     
        6 -        <mxCell id="1" parent="0" />                          
        5 +        <mxCell id="0"/>                                      
        6 +        <mxCell id="1" parent="0"/>                           
        7  
        8 -        <!-- ===== Group Backgrounds (visual containers) =====
          - -->                                                          
        9 -        <mxCell id="bg-userspace" value="" style="rounded=0;wh
          -iteSpace=wrap;html=1;fillColor=#e8f5e9;strokeColor=#2e7d32;str
          -okeWidth=2;opacity=30;" vertex="1" parent="1">                
       10 -          <mxGeometry x="30" y="60" width="210" height="170" a
          -s="geometry" />                                               
        8 +        <mxCell id="2" value="Userspace" style="swimlane;white
          +Space=wrap;html=1;fillColor=#e8f5e9;strokeColor=#2e7d32;fontSt
          +yle=1;fontSize=13;startSize=28;" vertex="1" parent="1">       
        9 +          <mxGeometry x="40" y="40" width="200" height="145" a
          +s="geometry"/>                                                
       10          </mxCell>
       12 -        <mxCell id="lbl-userspace" value="Userspace" style="te
          -xt;html=1;fontStyle=1;fontSize=14;fontColor=#2e7d32;" vertex="
          -1" parent="1">                                                
       13 -          <mxGeometry x="40" y="32" width="100" height="24" as
          -="geometry" />                                                
       11 +        <mxCell id="3" value="Application / Test Tools" style=
          +"rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColo
          +r=#2e7d32;fontStyle=1;" vertex="1" parent="2">                
       12 +          <mxGeometry x="12" y="32" width="176" height="34" as
          +="geometry"/>                                                 
       13          </mxCell>
       15 -                                                              
       16 -        <mxCell id="bg-framework" value="" style="rounded=0;wh
          -iteSpace=wrap;html=1;fillColor=#e3f2fd;strokeColor=#1565c0;str
          -okeWidth=2;opacity=30;" vertex="1" parent="1">                
       17 -          <mxGeometry x="300" y="60" width="470" height="170" 
          -as="geometry" />                                              
       14 +        <mxCell id="4" value="configfs Interface" style="round
          +ed=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColor=#2e7
          +d32;" vertex="1" parent="2">                                  
       15 +          <mxGeometry x="12" y="72" width="176" height="28" as
          +="geometry"/>                                                 
       16          </mxCell>
       19 -        <mxCell id="lbl-framework" value="PCIe Endpoint Framew
          -ork (drivers/pci/endpoint/)" style="text;html=1;fontStyle=1;fo
          -ntSize=14;fontColor=#1565c0;" vertex="1" parent="1">          
       20 -          <mxGeometry x="310" y="32" width="400" height="24" a
          -s="geometry" />                                               
       17 +        <mxCell id="5" value="sysfs Interface" style="rounded=
          +1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColor=#2e7d32
          +;" vertex="1" parent="2">                                     
       18 +          <mxGeometry x="12" y="106" width="176" height="28" a
          +s="geometry"/>                                                
       19          </mxCell>
       20  
       23 -        <mxCell id="bg-epf-drivers" value="" style="rounded=0;
          -whiteSpace=wrap;html=1;fillColor=#fff3e0;strokeColor=#ef6c00;s
          -trokeWidth=2;opacity=30;" vertex="1" parent="1">              
       24 -          <mxGeometry x="850" y="60" width="300" height="300" 
          -as="geometry" />                                              
       21 +        <mxCell id="6" value="PCIe Endpoint Framework (drivers
          +/pci/endpoint/)" style="swimlane;whiteSpace=wrap;html=1;fillCo
          +lor=#e3f2fd;strokeColor=#1565c0;fontStyle=1;fontSize=13;startS
          +ize=28;" vertex="1" parent="1">                               
       22 +          <mxGeometry x="310" y="40" width="460" height="145" 
          +as="geometry"/>                                               
       23          </mxCell>
       26 -        <mxCell id="lbl-epf-drivers" value="EPF Drivers" style
          -="text;html=1;fontStyle=1;fontSize=14;fontColor=#ef6c00;" vert
          -ex="1" parent="1">                                            
       27 -          <mxGeometry x="860" y="32" width="150" height="24" a
          -s="geometry" />                                               
       24 +        <mxCell id="7" value="pci-epf-core&#xa;EPF Core&#xa;──
          +───────────────&#xa;epf_device_register()&#xa;epf_driver_regis
          +ter()&#xa;epf_bind / unbind" style="rounded=1;whiteSpace=wrap;
          +html=1;fillColor=#bbdefb;strokeColor=#1565c0;align=left;spacin
          +gLeft=8;" vertex="1" parent="6">                              
       25 +          <mxGeometry x="10" y="30" width="215" height="108" a
          +s="geometry"/>                                                
       26          </mxCell>
       29 -                                                              
       30 -        <mxCell id="bg-epc-drivers" value="" style="rounded=0;
          -whiteSpace=wrap;html=1;fillColor=#f3e5f5;strokeColor=#7b1fa2;s
          -trokeWidth=2;opacity=30;" vertex="1" parent="1">              
       31 -          <mxGeometry x="30" y="290" width="280" height="280" 
          -as="geometry" />                                              
       27 +        <mxCell id="8" value="pci-epc-core&#xa;EPC Core&#xa;──
          +───────────────&#xa;epc_device_register()&#xa;epc_driver_regis
          +ter()&#xa;BAR / IRQ / DMA ops" style="rounded=1;whiteSpace=wra
          +p;html=1;fillColor=#bbdefb;strokeColor=#1565c0;align=left;spac
          +ingLeft=8;" vertex="1" parent="6">                            
       28 +          <mxGeometry x="238" y="30" width="212" height="108" 
          +as="geometry"/>                                               
       29          </mxCell>
       33 -        <mxCell id="lbl-epc-drivers" value="EPC Drivers (ARM64
          -)" style="text;html=1;fontStyle=1;fontSize=14;fontColor=#7b1fa
          -2;" vertex="1" parent="1">                                    
       34 -          <mxGeometry x="40" y="262" width="200" height="24" a
          -s="geometry" />                                               
       35 -        </mxCell>                                             
       30  
       37 -        <mxCell id="bg-hw" value="" style="rounded=0;whiteSpac
          -e=wrap;html=1;fillColor=#fce4ec;strokeColor=#c62828;strokeWidt
          -h=2;opacity=30;" vertex="1" parent="1">                       
       38 -          <mxGeometry x="380" y="290" width="280" height="280"
          - as="geometry" />                                             
       31 +        <mxCell id="9" value="EPF Drivers" style="swimlane;whi
          +teSpace=wrap;html=1;fillColor=#fff3e0;strokeColor=#ef6c00;font
          +Style=1;fontSize=13;startSize=28;" vertex="1" parent="1">     
       32 +          <mxGeometry x="850" y="40" width="290" height="280" 
          +as="geometry"/>                                               
       33          </mxCell>
       40 -        <mxCell id="lbl-hw" value="ARM64 PCIe EP Hardware" sty
          -le="text;html=1;fontStyle=1;fontSize=14;fontColor=#c62828;" ve
          -rtex="1" parent="1">                                          
       41 -          <mxGeometry x="390" y="262" width="260" height="24" 
          -as="geometry" />                                              
       34 +        <mxCell id="10" value="pci-epf-test&#xa;────────────&#
          +xa;Loopback / DMA test" style="rounded=1;whiteSpace=wrap;html=
          +1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft
          +=8;" vertex="1" parent="9">                                   
       35 +          <mxGeometry x="12" y="32" width="266" height="42" as
          +="geometry"/>                                                 
       36          </mxCell>
       43 -                                                              
       44 -        <mxCell id="bg-rc" value="" style="rounded=0;whiteSpac
          -e=wrap;html=1;fillColor=#e0e0e0;strokeColor=#424242;strokeWidt
          -h=2;opacity=30;" vertex="1" parent="1">                       
       45 -          <mxGeometry x="750" y="375" width="250" height="90" 
          -as="geometry" />                                              
       37 +        <mxCell id="11" value="pci-epf-ntb&#xa;────────────&#x
          +a;NTB function" style="rounded=1;whiteSpace=wrap;html=1;fillCo
          +lor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8;" ver
          +tex="1" parent="9">                                           
       38 +          <mxGeometry x="12" y="80" width="266" height="42" as
          +="geometry"/>                                                 
       39          </mxCell>
       47 -        <mxCell id="lbl-rc" value="Remote PCIe Root Complex" s
          -tyle="text;html=1;fontStyle=1;fontSize=14;fontColor=#424242;" 
          -vertex="1" parent="1">                                        
       48 -          <mxGeometry x="760" y="347" width="230" height="24" 
          -as="geometry" />                                              
       40 +        <mxCell id="12" value="pci-epf-mhi&#xa;────────────&#x
          +a;MHI (modem)" style="rounded=1;whiteSpace=wrap;html=1;fillCol
          +or=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8;" vert
          +ex="1" parent="9">                                            
       41 +          <mxGeometry x="12" y="128" width="266" height="42" a
          +s="geometry"/>                                                
       42          </mxCell>
       50 -                                                              
       51 -        <!-- ===== Nodes ===== -->                            
       52 -                                                              
       53 -        <!-- Userspace -->                                    
       54 -        <mxCell id="app" value="Application / Test Tools" styl
          -e="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCo
          -lor=#2e7d32;fontStyle=1;" vertex="1" parent="1">              
       55 -          <mxGeometry x="50" y="72" width="170" height="32" as
          -="geometry" />                                                
       43 +        <mxCell id="13" value="pci-epf-vntb&#xa;────────────&#
          +xa;Virtual NTB" style="rounded=1;whiteSpace=wrap;html=1;fillCo
          +lor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=8;" ver
          +tex="1" parent="9">                                           
       44 +          <mxGeometry x="12" y="176" width="266" height="42" a
          +s="geometry"/>                                                
       45          </mxCell>
       57 -        <mxCell id="configfs" value="configfs Interface" style
          -="rounded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeCol
          -or=#2e7d32;" vertex="1" parent="1">                           
       58 -          <mxGeometry x="50" y="118" width="170" height="30" a
          -s="geometry" />                                               
       46 +        <mxCell id="14" value="Custom EPF Driver&#xa;─────────
          +───&#xa;User-defined function" style="rounded=1;whiteSpace=wra
          +p;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spac
          +ingLeft=8;dashed=1;" vertex="1" parent="9">                   
       47 +          <mxGeometry x="12" y="224" width="266" height="42" a
          +s="geometry"/>                                                
       48          </mxCell>
       60 -        <mxCell id="sysfs" value="sysfs Interface" style="roun
          -ded=1;whiteSpace=wrap;html=1;fillColor=#c8e6c9;strokeColor=#2e
          -7d32;" vertex="1" parent="1">                                 
       61 -          <mxGeometry x="50" y="160" width="170" height="30" a
          -s="geometry" />                                               
       62 -        </mxCell>                                             
       49  
       64 -        <!-- EPF Framework Core -->                           
       65 -        <mxCell id="epf-core" value="pci-epf-core&lt;br&gt;&lt
          -;b&gt;EPF Core&lt;/b&gt;&lt;br&gt;&lt;hr size=&quot;1&quot;&gt
          -;&lt;font style=&quot;font-size:10px&quot;&gt;epf_device_regis
          -ter()&lt;br&gt;epf_driver_register()&lt;br&gt;epf_bind / unbin
          -d&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillCo
          -lor=#bbdefb;strokeColor=#1565c0;align=left;spacingLeft=8;" ver
          -tex="1" parent="1">                                           
       66 -          <mxGeometry x="315" y="72" width="210" height="145" 
          -as="geometry" />                                              
       50 +        <mxCell id="15" value="EPC Drivers (ARM64)" style="swi
          +mlane;whiteSpace=wrap;html=1;fillColor=#f3e5f5;strokeColor=#7b
          +1fa2;fontStyle=1;fontSize=13;startSize=28;" vertex="1" parent=
          +"1">                                                          
       51 +          <mxGeometry x="40" y="260" width="270" height="260" 
          +as="geometry"/>                                               
       52          </mxCell>
       68 -        <mxCell id="epc-core" value="pci-epc-core&lt;br&gt;&lt
          -;b&gt;EPC Core&lt;/b&gt;&lt;br&gt;&lt;hr size=&quot;1&quot;&gt
          -;&lt;font style=&quot;font-size:10px&quot;&gt;epc_device_regis
          -ter()&lt;br&gt;epc_driver_register()&lt;br&gt;BAR / IRQ / DMA 
          -ops&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fill
          -Color=#bbdefb;strokeColor=#1565c0;align=left;spacingLeft=8;" v
          -ertex="1" parent="1">                                         
       69 -          <mxGeometry x="545" y="72" width="210" height="145" 
          -as="geometry" />                                              
       53 +        <mxCell id="16" value="pcie-dw-plat-ep&#xa;───────────
          +───&#xa;DesignWare DWC EP" style="rounded=1;whiteSpace=wrap;ht
          +ml=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacingL
          +eft=8;" vertex="1" parent="15">                               
       54 +          <mxGeometry x="12" y="32" width="246" height="48" as
          +="geometry"/>                                                 
       55          </mxCell>
       71 -                                                              
       72 -        <!-- EPF Drivers -->                                  
       73 -        <mxCell id="epf-test" value="pci-epf-test&lt;br&gt;───
          -───────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;L
          -oopback / DMA test&lt;/font&gt;" style="rounded=1;whiteSpace=w
          -rap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;sp
          -acingLeft=8;" vertex="1" parent="1">                          
       74 -          <mxGeometry x="865" y="72" width="270" height="42" a
          -s="geometry" />                                               
       56 +        <mxCell id="17" value="pcie-cadence-ep&#xa;───────────
          +───&#xa;Cadence EP" style="rounded=1;whiteSpace=wrap;html=1;fi
          +llColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacingLeft=8;"
          + vertex="1" parent="15">                                      
       57 +          <mxGeometry x="12" y="86" width="246" height="48" as
          +="geometry"/>                                                 
       58          </mxCell>
       76 -        <mxCell id="epf-ntb" value="pci-epf-ntb&lt;br&gt;─────
          -─────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;NTB
          - function&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=
          -1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft
          -=8;" vertex="1" parent="1">                                   
       77 -          <mxGeometry x="865" y="122" width="270" height="42" 
          -as="geometry" />                                              
       59 +        <mxCell id="18" value="pcie-rockchip-ep&#xa;──────────
          +────&#xa;Rockchip EP" style="rounded=1;whiteSpace=wrap;html=1;
          +fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacingLeft=8
          +;" vertex="1" parent="15">                                    
       60 +          <mxGeometry x="12" y="140" width="246" height="48" a
          +s="geometry"/>                                                
       61          </mxCell>
       79 -        <mxCell id="epf-mhi" value="pci-epf-mhi&lt;br&gt;─────
          -─────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;MHI
          - (modem)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1
          -;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLeft=
          -8;" vertex="1" parent="1">                                    
       80 -          <mxGeometry x="865" y="172" width="270" height="42" 
          -as="geometry" />                                              
       62 +        <mxCell id="19" value="pcie-qcom-ep&#xa;──────────────
          +&#xa;Qualcomm EP" style="rounded=1;whiteSpace=wrap;html=1;fill
          +Color=#e1bee7;strokeColor=#7b1fa2;align=left;spacingLeft=8;" v
          +ertex="1" parent="15">                                        
       63 +          <mxGeometry x="12" y="194" width="246" height="48" a
          +s="geometry"/>                                                
       64          </mxCell>
       82 -        <mxCell id="epf-vntb" value="pci-epf-vntb&lt;br&gt;───
          -───────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;V
          -irtual NTB&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html
          -=1;fillColor=#ffe0b2;strokeColor=#ef6c00;align=left;spacingLef
          -t=8;" vertex="1" parent="1">                                  
       83 -          <mxGeometry x="865" y="222" width="270" height="42" 
          -as="geometry" />                                              
       84 -        </mxCell>                                             
       85 -        <mxCell id="custom-epf" value="Custom EPF Driver&lt;br
          -&gt;──────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quo
          -t;&gt;User-defined function&lt;/font&gt;" style="rounded=1;whi
          -teSpace=wrap;html=1;fillColor=#ffe0b2;strokeColor=#ef6c00;alig
          -n=left;spacingLeft=8;dashed=1;" vertex="1" parent="1">        
       86 -          <mxGeometry x="865" y="272" width="270" height="42" 
          -as="geometry" />                                              
       87 -        </mxCell>                                             
       65  
       89 -        <!-- EPC Drivers -->                                  
       90 -        <mxCell id="dwc-ep" value="pcie-dw-plat-ep&lt;br&gt;──
          -────────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;
          -&gt;DesignWare DWC EP&lt;/font&gt;" style="rounded=1;whiteSpac
          -e=wrap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left
          -;spacingLeft=8;" vertex="1" parent="1">                       
       91 -          <mxGeometry x="45" y="302" width="250" height="52" a
          -s="geometry" />                                               
       66 +        <mxCell id="20" value="ARM64 PCIe EP Hardware" style="
          +swimlane;whiteSpace=wrap;html=1;fillColor=#fce4ec;strokeColor=
          +#c62828;fontStyle=1;fontSize=13;startSize=28;" vertex="1" pare
          +nt="1">                                                       
       67 +          <mxGeometry x="370" y="260" width="270" height="260"
          + as="geometry"/>                                              
       68          </mxCell>
       93 -        <mxCell id="cadence-ep" value="pcie-cadence-ep&lt;br&g
          -t;──────────────&lt;br&gt;&lt;font style=&quot;font-size:9px&q
          -uot;&gt;Cadence EP&lt;/font&gt;" style="rounded=1;whiteSpace=w
          -rap;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;sp
          -acingLeft=8;" vertex="1" parent="1">                          
       94 -          <mxGeometry x="45" y="362" width="250" height="52" a
          -s="geometry" />                                               
       69 +        <mxCell id="21" value="Synopsys DWC&#xa;PCIe Endpoint"
          + style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;str
          +okeColor=#c62828;fontStyle=1;" vertex="1" parent="20">        
       70 +          <mxGeometry x="12" y="32" width="246" height="48" as
          +="geometry"/>                                                 
       71          </mxCell>
       96 -        <mxCell id="rk-ep" value="pcie-rockchip-ep&lt;br&gt;──
          -────────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;
          -&gt;Rockchip EP&lt;/font&gt;" style="rounded=1;whiteSpace=wrap
          -;html=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spaci
          -ngLeft=8;" vertex="1" parent="1">                             
       97 -          <mxGeometry x="45" y="422" width="250" height="52" a
          -s="geometry" />                                               
       72 +        <mxCell id="22" value="Cadence&#xa;PCIe Endpoint" styl
          +e="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;strokeCo
          +lor=#c62828;fontStyle=1;" vertex="1" parent="20">             
       73 +          <mxGeometry x="12" y="86" width="246" height="48" as
          +="geometry"/>                                                 
       74          </mxCell>
       99 -        <mxCell id="qcom-ep" value="pcie-qcom-ep&lt;br&gt;────
          -──────────&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&g
          -t;Qualcomm EP&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;h
          -tml=1;fillColor=#e1bee7;strokeColor=#7b1fa2;align=left;spacing
          -Left=8;" vertex="1" parent="1">                               
      100 -          <mxGeometry x="45" y="482" width="250" height="52" a
          -s="geometry" />                                               
       75 +        <mxCell id="23" value="Rockchip&#xa;PCIe Endpoint" sty
          +le="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;strokeC
          +olor=#c62828;fontStyle=1;" vertex="1" parent="20">            
       76 +          <mxGeometry x="12" y="140" width="246" height="48" a
          +s="geometry"/>                                                
       77          </mxCell>
      102 -                                                              
      103 -        <!-- Hardware -->                                     
      104 -        <mxCell id="dwc-hw" value="Synopsys DWC&lt;br&gt;PCIe 
          -Endpoint" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f
          -fcdd2;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">
      105 -          <mxGeometry x="395" y="302" width="250" height="52" 
          -as="geometry" />                                              
       78 +        <mxCell id="24" value="Qualcomm&#xa;PCIe Endpoint" sty
          +le="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2;strokeC
          +olor=#c62828;fontStyle=1;" vertex="1" parent="20">            
       79 +          <mxGeometry x="12" y="194" width="246" height="48" a
          +s="geometry"/>                                                
       80          </mxCell>
      107 -        <mxCell id="cdn-hw" value="Cadence&lt;br&gt;PCIe Endpo
          -int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          -;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">     
      108 -          <mxGeometry x="395" y="362" width="250" height="52" 
          -as="geometry" />                                              
      109 -        </mxCell>                                             
      110 -        <mxCell id="rk-hw" value="Rockchip&lt;br&gt;PCIe Endpo
          -int" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcdd2
          -;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">     
      111 -          <mxGeometry x="395" y="422" width="250" height="52" 
          -as="geometry" />                                              
      112 -        </mxCell>                                             
      113 -        <mxCell id="qcom-hw" value="Qualcomm&lt;br&gt;PCIe End
          -point" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffcd
          -d2;strokeColor=#c62828;fontStyle=1;" vertex="1" parent="1">   
      114 -          <mxGeometry x="395" y="482" width="250" height="52" 
          -as="geometry" />                                              
      115 -        </mxCell>                                             
       81  
      117 -        <!-- Remote RC -->                                    
      118 -        <mxCell id="rc-host" value="PCIe RC Host (x86 / ARM64)
          -" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#eeeeee;st
          -rokeColor=#424242;fontStyle=1;" vertex="1" parent="1">        
      119 -          <mxGeometry x="770" y="400" width="210" height="40" 
          -as="geometry" />                                              
       82 +        <mxCell id="25" value="Remote PCIe Root Complex" style
          +="swimlane;whiteSpace=wrap;html=1;fillColor=#e0e0e0;strokeColo
          +r=#424242;fontStyle=1;fontSize=13;startSize=28;" vertex="1" pa
          +rent="1">                                                     
       83 +          <mxGeometry x="730" y="345" width="240" height="75" 
          +as="geometry"/>                                               
       84          </mxCell>
      121 -                                                              
      122 -        <!-- ===== EDGES (all parent="1") ===== -->           
      123 -                                                              
      124 -        <mxCell id="e-app-config" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" ed
          -ge="1" parent="1" source="app" target="configfs">             
      125 -          <mxGeometry relative="1" as="geometry" />           
       85 +        <mxCell id="26" value="PCIe RC Host (x86 / ARM64)" sty
          +le="rounded=1;whiteSpace=wrap;html=1;fillColor=#eeeeee;strokeC
          +olor=#424242;fontStyle=1;" vertex="1" parent="25">            
       86 +          <mxGeometry x="20" y="32" width="200" height="32" as
          +="geometry"/>                                                 
       87          </mxCell>
       88  
      128 -        <mxCell id="e-config-epf" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" ed
          -ge="1" parent="1" source="configfs" target="epf-core">        
      129 -          <mxGeometry relative="1" as="geometry" />           
       89 +        <mxCell id="27" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="3" target="4">                                
       90 +          <mxGeometry relative="1" as="geometry"/>            
       91          </mxCell>
      131 -                                                              
      132 -        <mxCell id="e-sysfs-epf" style="edgeStyle=orthogonalEd
          -geStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edg
          -e="1" parent="1" source="sysfs" target="epf-core">            
      133 -          <mxGeometry relative="1" as="geometry" />           
       92 +        <mxCell id="28" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="4" target="7">                                
       93 +          <mxGeometry relative="1" as="geometry"/>            
       94          </mxCell>
      135 -                                                              
      136 -        <mxCell id="e-epf-epc" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontCol
          -or=#1565c0;fontStyle=1;" edge="1" parent="1" source="epf-core"
          - target="epc-core">                                           
      137 -          <mxGeometry relative="1" as="geometry" />           
       95 +        <mxCell id="29" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="5" target="7">                                
       96 +          <mxGeometry relative="1" as="geometry"/>            
       97          </mxCell>
      139 -                                                              
      140 -        <mxCell id="e-epc-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontCol
          -or=#1565c0;fontStyle=1;" edge="1" parent="1" source="epc-core"
          - target="epf-core">                                           
      141 -          <mxGeometry relative="1" as="geometry" />           
       98 +        <mxCell id="30" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor=#156
          +5c0;fontStyle=1;" edge="1" parent="1" source="7" target="8">  
       99 +          <mxGeometry relative="1" as="geometry"/>            
      100          </mxCell>
      143 -                                                              
      144 -        <mxCell id="e-test-epf" style="edgeStyle=orthogonalEdg
          -eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge
          -="1" parent="1" source="epf-test" target="epf-core">          
      145 -          <mxGeometry relative="1" as="geometry" />           
      101 +        <mxCell id="31" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor=#156
          +5c0;fontStyle=1;" edge="1" parent="1" source="8" target="7">  
      102 +          <mxGeometry relative="1" as="geometry"/>            
      103          </mxCell>
      147 -                                                              
      148 -        <mxCell id="e-ntb-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge=
          -"1" parent="1" source="epf-ntb" target="epf-core">            
      149 -          <mxGeometry relative="1" as="geometry" />           
      104 +        <mxCell id="32" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="10" target="7">                               
      105 +          <mxGeometry relative="1" as="geometry"/>            
      106          </mxCell>
      151 -                                                              
      152 -        <mxCell id="e-mhi-epf" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge=
          -"1" parent="1" source="epf-mhi" target="epf-core">            
      153 -          <mxGeometry relative="1" as="geometry" />           
      107 +        <mxCell id="33" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="11" target="7">                               
      108 +          <mxGeometry relative="1" as="geometry"/>            
      109          </mxCell>
      155 -                                                              
      156 -        <mxCell id="e-vntb-epf" style="edgeStyle=orthogonalEdg
          -eStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge
          -="1" parent="1" source="epf-vntb" target="epf-core">          
      157 -          <mxGeometry relative="1" as="geometry" />           
      110 +        <mxCell id="34" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="12" target="7">                               
      111 +          <mxGeometry relative="1" as="geometry"/>            
      112          </mxCell>
      159 -                                                              
      160 -        <mxCell id="e-custom-epf" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;dash
          -ed=1;" edge="1" parent="1" source="custom-epf" target="epf-cor
          -e">                                                           
      161 -          <mxGeometry relative="1" as="geometry" />           
      113 +        <mxCell id="35" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="13" target="7">                               
      114 +          <mxGeometry relative="1" as="geometry"/>            
      115          </mxCell>
      163 -                                                              
      164 -        <mxCell id="e-dwc-hw" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="
          -1" parent="1" source="dwc-ep" target="dwc-hw">                
      165 -          <mxGeometry relative="1" as="geometry" />           
      116 +        <mxCell id="36" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;dashed=1;" edg
          +e="1" parent="1" source="14" target="7">                      
      117 +          <mxGeometry relative="1" as="geometry"/>            
      118          </mxCell>
      167 -                                                              
      168 -        <mxCell id="e-cadence-hw" style="edgeStyle=orthogonalE
          -dgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" ed
          -ge="1" parent="1" source="cadence-ep" target="cdn-hw">        
      169 -          <mxGeometry relative="1" as="geometry" />           
      119 +        <mxCell id="37" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="16" target="21">                              
      120 +          <mxGeometry relative="1" as="geometry"/>            
      121          </mxCell>
      171 -                                                              
      172 -        <mxCell id="e-rk-hw" style="edgeStyle=orthogonalEdgeSt
          -yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1
          -" parent="1" source="rk-ep" target="rk-hw">                   
      173 -          <mxGeometry relative="1" as="geometry" />           
      122 +        <mxCell id="38" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="17" target="22">                              
      123 +          <mxGeometry relative="1" as="geometry"/>            
      124          </mxCell>
      175 -                                                              
      176 -        <mxCell id="e-qcom-hw" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge=
          -"1" parent="1" source="qcom-ep" target="qcom-hw">             
      177 -          <mxGeometry relative="1" as="geometry" />           
      125 +        <mxCell id="39" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="18" target="23">                              
      126 +          <mxGeometry relative="1" as="geometry"/>            
      127          </mxCell>
      179 -                                                              
      180 -        <mxCell id="e-dwc-rc" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColo
          -r=#c62828;strokeWidth=2;" edge="1" parent="1" source="dwc-hw" 
          -target="rc-host">                                             
      181 -          <mxGeometry relative="1" as="geometry" />           
      128 +        <mxCell id="40" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;" edge="1" par
          +ent="1" source="19" target="24">                              
      129 +          <mxGeometry relative="1" as="geometry"/>            
      130          </mxCell>
      183 -                                                              
      184 -        <mxCell id="e-cdn-rc" style="edgeStyle=orthogonalEdgeS
          -tyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColo
          -r=#c62828;strokeWidth=2;" edge="1" parent="1" source="cdn-hw" 
          -target="rc-host">                                             
      185 -          <mxGeometry relative="1" as="geometry" />           
      131 +        <mxCell id="41" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor=#c62
          +828;strokeWidth=2;" edge="1" parent="1" source="21" target="26
          +">                                                            
      132 +          <mxGeometry relative="1" as="geometry"/>            
      133          </mxCell>
      187 -                                                              
      188 -        <mxCell id="e-rk-rc" style="edgeStyle=orthogonalEdgeSt
          -yle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor
          -=#c62828;strokeWidth=2;" edge="1" parent="1" source="rk-hw" ta
          -rget="rc-host">                                               
      189 -          <mxGeometry relative="1" as="geometry" />           
      134 +        <mxCell id="42" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor=#c62
          +828;strokeWidth=2;" edge="1" parent="1" source="22" target="26
          +">                                                            
      135 +          <mxGeometry relative="1" as="geometry"/>            
      136          </mxCell>
      191 -                                                              
      192 -        <mxCell id="e-qcom-rc" style="edgeStyle=orthogonalEdge
          -Style;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontCol
          -or=#c62828;strokeWidth=2;" edge="1" parent="1" source="qcom-hw
          -" target="rc-host">                                           
      193 -          <mxGeometry relative="1" as="geometry" />           
      137 +        <mxCell id="43" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor=#c62
          +828;strokeWidth=2;" edge="1" parent="1" source="23" target="26
          +">                                                            
      138 +          <mxGeometry relative="1" as="geometry"/>            
      139          </mxCell>
      195 -                                                              
      140 +        <mxCell id="44" style="edgeStyle=orthogonalEdgeStyle;r
          +ounded=0;orthogonalLoop=1;jettySize=auto;html=1;fontColor=#c62
          +828;strokeWidth=2;" edge="1" parent="1" source="24" target="26
          +">                                                            
      141 +          <mxGeometry relative="1" as="geometry"/>            
      142 +        </mxCell>                                             
      143        </root>
      144      </mxGraphModel>
      145    </diagram>

  Thought for 2s (ctrl+o to expand)

● 这份 XML 刚刚被 draw.io MCP 工具成功接受并渲染。文件已更新：

  /home/river/drawio_workspace/pcie_epf_framework.drawio

  请再用 VS Code（draw.io 插件）或 draw.io
  本地应用打开试试看。如果还报错，请告诉我完整的错误信息和你用的打开方式（VS
  Code 插件版本 / draw.io 桌面版版本 / 在线版），我来排查。

✻ Cogitated for 3m 19s

● Unknown command: /messages