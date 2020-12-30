# Hackintosh guilde

> Getting started

- Guilde này được tổng hợp từ các bài viết trên blog [vnohackintosh.com](http://vnohackintosh.com/) của Nguyễn Văn Hùng – rất cám ơn tác giả đã viết nên series bài viết chất lượng này.
- Thời điểm guilde này được viết là 9-2020, bản lastest OS là Catalina 10.15.6, tuy nhiên, mình sẽ hướng dẫn mọi người cài bản 10.15.1 – để tiện làm guilde update OS lên vesion mới hơn. Và bản 10.13.6 – version cao nhất đối với các máy chạy card nvidia không phải dòng Nvidia Kepler (cụ thể mình sẽ nói ở dưới đây).
- Nếu bạn muốn dual boot MacOS với Windows, hãy kéo xuống dưới cùng và đọc bước 8. Còn không, hãy bắt đàu từ đây.
> Let's go

1. **Lựa chọn cấu hình.**
- Hiện tại, cả chip CPU Intel và AMD đều có thể chạy được hackintosh, và có 2 "trường phái" để cài được hackintosh, đó là Clover và OpenCore. OpenCore ra sau, nhưng đang phát triển rất mạnh và được cộng đồng support và fix bug hùng hậu và thiên về hỗ trợ chip AMD. Còn Clover ra đời trước, nên sở hữu sự ổn định và tính support native chip Intel.
- Case hay laptop cài hackintosh dễ hơn. Với case, chúng ta dễ dàng lựa chọn cấu hình các phần cứng, sao cho gần giống cấu hình của một chiếc Imac thật nhất, do đó, mọi thứ dường như dễ dàng hơn, hoạt động trơn tru hơn. Còn về laptop, Thật khó mà tìm được một chiếc máy hỗ trợ native MacOS, sau khi cài đặt Hacktintosh thành công, còn rất nhiều việc cần config để chiếc laptop của bạn hoạt động "ngon nghẻ", ví dụ như trackpad, sleep, chỉnh độ sáng màn hình, và card wifi...
- Xây dựng cấu hình.

    Tuỳ vào tài chính, chúng ta sẽ lựa chọn cấu hình khác nhau cho ra hiệu năng khác nhau. Ở đây, chúng ta cùng build một case chạy chip Intel và chạy Clover.

    - Chip: Intel đời thứ 6 (6xxx) trở lên.
    - Main: Đời main phù hợp với đời chip, nên chọn main của các brand như Gigabyte, Asrock, MSI để tránh vài lỗi lặt vặt có thể xảy ra.
    - Ram: nên chọn các brand Corsair, G.Skill, Kingston... Không nên chọn Samsung vì đôi khi có thể gây ra lỗi vặt, dung lượng tuỳ vào từng nhu cầu, khuyến nghị 8gb trở lên. Nếu là Developer thì 16gb trở lên.
    - Nguồn: **Không nên** tiếc tiền khi đầu tư vào nguồn cho bộ Hackintosh nói riêng và Pc thường nói chung, nó là thứ thiết yếu, giúp cho máy cháy ổn định. khi mua nguồn lởm, có thể gây ra sụt áp, hoặc tụt áp đột ngột, không đủ cung cấp điện cho dàn máy, đen thì tèo nguồn, đen hơn thì tèo thêm các bộ phận khác, đen hơn nữa thì bốc khói cả dàn máy. Các brand nguồn phần khúc tầm trung trở lên ở top đầu như: Corsair, Seasonic, Cooler Master, Andyson, NZXT, FSP, ...
    - SSD: Tuỳ vào nhu cầu, Nếu như tài chính không phải vấn đề thì quất con ssd m2 nvme samsung 980 evo plus / 980 pro nếu như main hỗ trợ khe ssd m2 4.0. Còn không thì có thể chọn ssd sata3 bình thường. Không nên chọn samsung m2 nvme 981/981a vì firmwave bọn này không hỗ trợ hackintosh.
    - Tản nhiệt, vỏ case, ... tuỳ vào nhu cầu.
    - Card màn hình: Có thể chọn có card rời hoặc không có card rời.
        - Nếu chọn không có card rời, thì máy sẽ sử dụng card onboard → tránh sử dụng những chip không có card onboard (có hậu tố F đằng sau, ví dụ i3 9100F, i5 9400F ...).
        - Nếu chọn có card rời, thì cần lưu ý vài vấn đề như sau:
            - Hầu hết các card của AMD như seri RX 4xx, RX 5xx, series 5500, series 5600, series 5700 đều hỗ trợ native Hackintosh, điều này có nghĩa là máy sẽ tự nhận card màn hình sau khi cài xong hackintosh mà không cần config gì thêm.
            - Danh sách các card của Nvidia hỗ trợ native hackintosh:

                **Nvidia Kepler** Cards that are Natively Supported:

                - GTX Titan (GK110)
                - GTX Titan Black (GK110)
                - GTX Titan Z
                - GTX 780/Ti
                - GTX 770
                - GTX 760/Ti
                - GT 740
                - GT 730(DDR5 versions)
                - GT 720
                - GT 710 (DDR3 versions)
                - GTX 690
                - GTX 680
                - GTX 670
                - GTX 660/Ti
                - GTX 650/Ti
                - GTX 645 (GT 645 is Fermi)
                - GT 640 (Kepler edition, GK107/208 core)
                - GT 630 (Kepler edition, GK208 core)
                - Quadro 410
                - Quadro K420
                - Quadro K600
                - Quadro K2000/D
                - Quadro K4000/D
                - Quadro K4200
                - Quadro K5000
                - Quadro K5200
                - Quadro K6000
                - Quadro NVS510

            - Danh sách card hỗ trợ hackintosh tối đa đến bản HighSerra 10.13.6 → Tối đa Xcode 10.2. Không hỗ trợ Mojave (10.14) or Catalina 10.15:
                - GTX 1080/Ti
                - GTX 1070/Ti
                - GTX 1060
                - GTX 1050/Ti
                - GT 1030
                - GTX Titan X (GM200 Maxwell core)
                - GTX 980/Ti
                - GTX 970
                - GTX 960
                - GTX 950
                - GTX 750/Ti
                - GTX 745
                - Quadro K620
                - Quadro K1200
                - Quadro K220
                - Quadro M (all models)
                - Quadro P (all models)
            - Danh sách các card không hỗ trợ hackintosh:
                - Titan RTX
                - RTX 2080 Ti
                - RTX 2080 Super
                - RTX 2080
                - RTX 2070 Super
                - RTX 2070
                - RTX 2060 Super
                - RTX 2060
                - GTX 1660 Ti
                - GTX 1660 Super
                - GTX 1650

**Với những card hỗ trợ hackintosh tối đa đến bản HighSerra 10.13.6, sau khi cài xong hackintosh, thì máy chỉ nhận 7mb bộ nhớ card. Bạn phải  config bằng tay để nó nhận đúng card. Bước này sẽ nói ở sau.**

→ Đến đây, cơ bản đã hoàn thành xong một case có thể chạy được hackintosh. Chuyển sang bước 2.

**2. Làm bộ cài Hackintosh bằng hệ điều hành MacOS hoặc làm trên Windows**

**2.1. Giới thiệu**

Nói sơ qua về bộ cài cách cài thì mình dựa theo cách làm của Olarila để làm ra file disk image của một cái usb cài vanilla hackintosh. Bộ cài đã có sẵn clover bao gồm config, driver và kext cơ bản để có thể cài đặt do mình tự chỉnh sửa và được đảm bảo sẽ tương thích với phiên bản macOS đi kèm. Bộ cài này có thể được dùng trực tiếp trên cả macOS lẫn Windows.

**2.2. Chuẩn bị**

- 1 usb 16GB, tốt nhất lấy usb 3.0 cho nhanh
- File bộ cài, tải về và giải nén ra sẵn (nếu dùng windows, hãy dùng winrar hoặc 7zip để giải nén):
    - [High Sierra 10.13.6 17G66](https://drive.google.com/file/d/1OsP32xYJhJ4TpyXrNcM3h0D317O5tIDr/view?usp=sharing)
    - [Mojave 10.14.6 18G103](https://drive.google.com/file/d/1oNHy8ykQc5u_bhHQF-Oo4aDIbjyAJAXN/view?usp=sharing)
    - [Catalina 10.15.1 19B88](https://drive.google.com/file/d/1xQ1pCDs0hbMKbN5U051ohX4kxj42wz2m/view?usp=sharing)
    - [Catalina 10.15.6 (lastest)](https://drive.google.com/file/d/1HAUgL1nPfOh1T_IJVss3qwx2pWywbA_T/view)
- Đối với **macOS**:
    - [Etcher](https://www.balena.io/etcher/)
    - [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/)
- Đối với **Windows**:
    - [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)
    - [Etcher](https://www.balena.io/etcher/)
    - [MiniTool Partition Wizard](https://www.partitionwizard.com/free-partition-manager.html)
    - [Explorer++](https://explorerplusplus.com/download)

**2.3. Thực hiện ghi bộ cài lên USB**

- **Trên macOS**
    - Mở Etcher lên -> chọn file bộ cài (đuôi .raw) -> chọn usb -> Flash!

        ![https://vnohackintosh.com/img/hackintosh/etcher/etcher.png](https://vnohackintosh.com/img/hackintosh/etcher/etcher.png)

    - Cách này áp dụng bên windows cũng tương tự luôn, Etcher cũng có phiên bản cho windows

- **Trên Windows**
    1. Format USB:

    ![https://vnohackintosh.com/img/hackintosh/windows/format-usb.png](https://vnohackintosh.com/img/hackintosh/windows/format-usb.png)

    1. Dùng win32diskimager ghi file raw ra USB, hoặc dùng Etcher như phí trên.

    ![https://vnohackintosh.com/img/hackintosh/windows/win32diskimager.png](https://vnohackintosh.com/img/hackintosh/windows/win32diskimager.png)

    ![https://vnohackintosh.com/img/hackintosh/windows/show-all.png](https://vnohackintosh.com/img/hackintosh/windows/show-all.png)

    ![https://vnohackintosh.com/img/hackintosh/windows/choose-image.png](https://vnohackintosh.com/img/hackintosh/windows/choose-image.png)

    ![https://vnohackintosh.com/img/hackintosh/windows/write-image.png](https://vnohackintosh.com/img/hackintosh/windows/write-image.png)

**2.3. Mount EFI**

- Trên macOS
    - Dùng Clover Configurator mount efi của usb ra

        ![https://vnohackintosh.com/img/hackintosh/cc/cc-usb.png](https://vnohackintosh.com/img/hackintosh/cc/cc-usb.png)

- **Trên Windows**
    - Dùng Minitool Partition Wizzard:

    ![https://vnohackintosh.com/img/hackintosh/windows/change-letter.png](https://vnohackintosh.com/img/hackintosh/windows/change-letter.png)

    ![https://vnohackintosh.com/img/hackintosh/windows/choose-letter.png](https://vnohackintosh.com/img/hackintosh/windows/choose-letter.png)

    ![https://vnohackintosh.com/img/hackintosh/windows/apply.png](https://vnohackintosh.com/img/hackintosh/windows/apply.png)

**2.4. Chỉnh sửa clover**

- Tất cả các file config, kext, driver uefi đều có sẵn trong clover usb rồi.
- **Việc cần làm là chọn thay config phù hợp, nếu bạn cần dùng thêm thì bạn có thể copy từ bên ngoài vào.**
- Windows 8/10 sẽ không cho phép bạn dùng file explorer để truy cập vào phân vùng EFI, dùng **Explorer++ chạy quyền admin để truy cập EFI.**

![https://vnohackintosh.com/img/hackintosh/windows/explorer++.png](https://vnohackintosh.com/img/hackintosh/windows/explorer++.png)

**2.5. Config**

- Tất cả config cho pc và laptop đều có sẵn ở thư mục **EFI/CLOVER/backup/config/**
- Chọn config cần dùng copy đến thư mục **EFI/CLOVER/** và đổi tên thành **config.plist**
- **Nếu bạn không biết dùng config nào thì hãy đọc lại các bài viết trước để xác định cấu hình máy của bạn.**

![https://vnohackintosh.com/img/hackintosh/usb-efi/config-laptop.png](https://vnohackintosh.com/img/hackintosh/usb-efi/config-laptop.png)

![https://vnohackintosh.com/img/hackintosh/usb-efi/config-pc-dgpu.png](https://vnohackintosh.com/img/hackintosh/usb-efi/config-pc-dgpu.png)

![https://vnohackintosh.com/img/hackintosh/usb-efi/config-pc-igpu.png](https://vnohackintosh.com/img/hackintosh/usb-efi/config-pc-igpu.png)

**2.6. Kexts**

- Trong thư mục **EFI/CLOVER/Kexts/Other** đã có đủ kext để dùng cho việc cài đặt, mình cũng để sẵn thêm một số kexts khác đôi lúc cần trong thư mục **EFI/CLOVER/Kexts/Other/backup**.

![https://vnohackintosh.com/img/hackintosh/usb-efi/kexts.png](https://vnohackintosh.com/img/hackintosh/usb-efi/kexts.png)

**2.7. Drivers UEFI**

- Đây là nơi chứa driver của clover bootloader chứ không phải driver của mac, đừng hiểu nhầm, driver của mac được gọi là kext.
- Mình để sẵn các driver cần thiết trong Driver/UEFI, chỉ cần từng đó là boot được.

![https://vnohackintosh.com/img/hackintosh/usb-efi/driver-uefi.png](https://vnohackintosh.com/img/hackintosh/usb-efi/driver-uefi.png)

- Nếu bạn boot gặp lỗi như dưới hãy thay **AptioMemoryFix.efi** bằng một trong các file **OsxAptioFixDrv.efi** hoặc **OsxAptioFix3Drv.efi** trong thư mục **EFI/CLOVER/drivers/UEFI/Off/**

![https://vnohackintosh.com/img/hackintosh/boot/error-boot.png](https://vnohackintosh.com/img/hackintosh/boot/error-boot.png)

→ Summary: 

- Bài viết hướng dẫn cách làm bộ cài hackintosh một cách dễ nhất, bạn không cần macOS, không cần tạo máy ảo để làm usb, rút ngắn một công đoạn dài nếu bạn không có real mac.
- **Nhưng mà cách này cũng sẽ có nhiều rủi ro sẽ gây lỗi nếu file không được bảo toàn do đó vấn khuyễn khích các bạn làm bộ cài trên macOS**

**3.  Setup mainboard**

**3.1.**  **Hướng dẫn setup cho một số loại mainboard PC**

Cấu trúc bios utility của mỗi hãng sẽ khác nhau nên mình sẽ chia theo hãng, đối với các main cùng hãng thì đời cũ và đời mới nó cũng không giống nhau hoàn toàn. Trong bài viết này mình chỉ viết hướng dẫn cho các main mới hơn cụ thể là main đi với CPU Intel đầu 6 trở đi.

- **Main Gigabyte**

    **Setup chung**

    - Save & Exit → Load Optimized Defaults
    - M.I.T. → Advanced Memory Settings Extreme Memory Profile(X.M.P.) : Profile1
    - BIOS → Fast Boot : Disabled
    - BIOS → LAN PXE Boot Option ROM : Disabled
    - BIOS → Storage Boot Option Control : UEFI
    - Peripherals → Trusted Computing → Security Device Support : Disable
    - Peripherals → Network Stack Configuration → Network Stack : Disabled
    - Peripherals → USB Configuration → Legacy USB Support : Auto
    - Peripherals → USB Configuration → XHCI Hand-off : Enabled
    - Chipset → Vt-d : Disabled

    **Dành cho card rời**

    - Peripherals → Initial Display Output : PCIe 1 Slot
    - Chipset → Integrated Graphics : Disabled

    **Dành cho card onboard**

    - Peripherals → Initial Display Output : IGFX
    - Chipset → Integrated Graphics : Enabled
    - Chipset → DVMT Pre-Allocated : 128MB

- **Main ASRock**

    **Setup chung**

    - OC Tweaker \ DRAM Configuration → Load XMP Setting : XMP 2.0 Profile 1
    - Advanced \ CPU Configuration → Intel Virtualization Technology : Enabled
    - Advanced \ Chipset Configuration → Vt-d : Disabled
    - Advanced \ Storage Configuration → Sata Mode Selection: AHCI
    - Advanced \ Super IO Configuration → Serial Port: Disabled
    - Advanced \ USB Configuration → Legacy USB Support : Enabled
    - Advanced \ USB Configuration → PS/2 Simulator : Disabled
    - Advanced \ USB Configuration → XHCI Hand-off : Enabled
    - Security \ Secure Boot → Secure Boot: Disabled
    - Boot → Fast Boot: Disabled
    - Boot → Boot From Onboard LAN: Disabled

    **Dành cho card rời**

    - Advanced \ Chipset Configuration → Primary Graphics Adapter : PCI Express
    - Advanced \ Chipset Configuration → IGPU Multi-Monitor : Disabled

    **Dành cho card onboard**

    - Advanced \ Chipset Configuration → Primary Graphics Adapter : Onboard
    - Advanced \ Chipset Configuration → Share Memory : 128MB
    - Advanced \ Chipset Configuration → IGPU Multi-Monitor : Enabled

- **Main ASUS**

    **Setup Chung**

    - Exit → Load Optimized Defaults : Yes
    - Advanced \ CPU Configuration → Intel Virtualizaiton Technology: Enabled
    - Advanced \ System Agent (SA) Configuration → Vt-d: Disabled
    - Advanced \ PCH Configuration → IOAPIC 24-119 Entries: Enabled
    - Advanced \ AMP Configuration → Power On By PCI-E/PCI: Enabled
    - Advanced \ Network Stack Configuration → Network Stack: Disabled
    - Advanced \ USB Configuration -> Legacy USB Support: Auto
    - Boot → Fast Boot : Disabled
    - Boot → Secure Boot → OS Type : Other OS

    **Dành cho card rời**

    - Advanced \ System Agent (SA) Configuration \ Graphics Configuration → Primary Display: PEG

    **Dành cho card onboard**

    - Advanced \ System Agent (SA) Configuration \ Graphics Configuration → Primary Display: IGFX
    - Advanced \ System Agent (SA) Configuration \ Graphics Configuration → DVMT Pre-Allocated: 128MB

- **Main MSI**

    **Setup chung**

    - Save & Exit → Restore Defaults : Yes
    - Settings \ Advanced \ Integrated Peripherals → Network Stack : [Disabled]
    - Settings \ Advanced \Integrated Peripherals → Intel Serial IO : [Disabled]
    - Settings \ Advanced \ Integrated Graphics Configuration → DVMT Pre-Allocated : 128MB hoặc 64MB
    - Settings \ Advanced \ USB Configuration → XHCI Hand-off : [Enabled]
    - Settings \ Advanced \ USB Configuration → Legacy USB Support : [Auto]
    - Settings \ Advanced \ Windows OS Configuration → MSI Fast Boot : [Disabled]
    - Settings \ Advanced \ Windows OS Configuration → Fast Boot : [Disabled]
    - Overclocking → Extreme Memory Profile(X.M.P) : [Enabled]
    - Overclocking \ CPU Features → Intel Virtualization Tech : [Enabled]
    - Overclocking \ CPU Features → Intel VT-D Tech : [Disabled]
    - Settings \ Boot → Boot mode select : [LEGACY+UEFI]

    **Dành cho card rời**

    - Settings \ Advanced \ Integrated Graphics Configuration → Initiate Graphic Adapter : PEG

    **Dành cho card onboard**

    - Advanced \ Integrated Graphics Configuration → Initiate Graphic Adapter : IGD

3**.2. Nếu main của bạn không có trong danh sách trên, bạn có thể setup như sau**

- **Đối với PC**
    - XHCI Handoff: Enabled (nếu có, một số main sẽ không có lựa chọn này)
    - OS Type: Other (nếu có)
    - Secure Boot: Disabled (nếu không thấy đâu thì sẽ thấy cái OS Type bên trên chỉnh nó về other là được)
    - Legacy/CSM support: Enable [Nhiều main tắt cũng được nhưng nhiều main tắt thì boot màn hình sẽ bị lỗi, bật cho chắc]
    - Fast Boot: Disable
    - SATA Mode: AHCI
    - VT-d: Disable
    - Nếu bạn dùng Nvidia/AMD GPU:
        - **KHÔNG** kết nối màn hình với các cổng DP/HDMI/VGA/DVI trên main.
        - Phần Graphics Settings, Main Display, Initial Display/Graphic: PEG hoặc PCIE (Chọn đến khe cắm card rời)
        - Disable những lựa chọn liên quan đến card onboard như: Hybrid Graphics, Dual Graphics, DVMT size …
    - Nếu bạn dùng Intel HD Graphics GPU:
        - Phần Graphics Settings, Main Display, Initial Display/Graphic: IGD hoặc IGFX (chọn card onboard)
        - DVMT pre-alloc, Graphic Memory: 64MB (hoặc cao hơn, phần lớn thì 64MB là đủ)
        - DMVT total/size/apertures/whatever: MAX

- **Đối với laptop**
    - Disabled Secure Boot
    - Disable Fast Boot
    - SATA Mode: AHCI
    - Disable VT-d, VT-x nên enable không thì không chạy máy ảo được.
    - Security Chips/Security modules: Disabled (nếu có thể, chúng có thể gây lỗi khi boot)
    - DVMT-prealloc, Graphics Memory: 64MB (nếu có thể, một số bios laptop không có lựa chọn này mặc định ở 32MB thì bạn cần phải patch framebuffer)
    - Nếu màn hình của bạn là 2K, 4K thì cần chỉnh Graphics Memory lên 128MB hoặc cao hơn.
    - Legacy/CSM support: Enable (nếu tắt hay bị vỡ hình khi boot, nếu boot bình thường thì cứ tắt)
    - Disable LAN/WLAN/WWAN boot/wake
    - Disable wake on usb

→ **Boot vào cài đặt**

Setup bios xong lưu lại rồi **boot usb** vào cài đặt thôi.

**4. Cài đặt hackintosh vào máy**

**4.1.  Chuẩn bị phân vùng ổ cứng cài đặt macOS**

**Hãy backup dữ liệu**, vì sẽ phải format ổ cứng hoăc phân chia phân vùng ổ cứng. Cũng có thể sẽ phải cài lại win. Bạn phải tự trách nhiệm với dữ liệu của bạn.

**Lưu ý:** macOS không có khái niệm format mà chỉ có erase, mình sẽ nói là xóa ổ cứng, xóa phân vùng.

Mình sẽ chỉ hướng dẫn cài UEFI thôi nên yêu cầu ổ cứng của bạn sẽ ở dạng GPT. **macOS yêu cần có phân vùng EFI >= 200MB**, nếu không thì bạn sẽ không thể xóa phân vùng cài sang định dạng HFS hoặc APFS để cài. Do đó tùy vào trình trạng ổ cứng của bạn, sẽ các phương án sau: - Cài lên nguyên cả ổ cứng: Bạn không cần chuẩn bị gì, tí nữa bạn sẽ xóa cả ổ cứng bằng disk utility như lúc làm usb vì nó chỉ chứa macOS. Nếu có sẵn hoặc cài lại windows lên ổ cứng khác yêu cầu dùng windows UEFI, clover sẽ không boot windows legacy được. - Cài macOS lên một phân vùng ổ cứng: + Nếu bạn đang dùng Windows Legacy, ổ cứng dạng MBR thì tốt nhất bạn nên xóa cả ổ cứng để cài macOS trước rồi cài lại Windows UEFI sau. Bạn vẫn có thể tìm những cách khác để chuyển từ legacy sang uefi và hãy đảm bảo bạn tạo phân vùng EFI >= 200MB. + Nếu bạn đang có Windows UEFI và có phân vùng recovery 500MB và efi 100MB ở trước phân vùng win. Cần EFI 200MB nên bạn sẽ xóa phân vùng recovery và thêm vào EFI. Sau đó chia ra một phân vùng dành cho macOS, format NTFS và đặt tên cho nó (không đặt tên tí nữa nó không hiện trong disk utility). Hãy dùng [MiniTool Partition Wizzard](https://www.partitionwizard.com/free-partition-manager.html) để thực hiện bước này.

**4.2. Boot USB và cài đặt**

Hãy thực hiện các bước sau:

- Cắm usb vào, tốt nhất không cắm dây lan vào máy
- Bật nguồn và vào boot option, tùy main và hãng máy mà phím tắt khác nhau, nó có hiển thị trên màn hình lúc mới bật hoặc bạn có thể hỏi google
- Chọn USB của bạn trong danh sách (hãy chọn cái boot mà có chữ UEFI đứng trước cái tên USB)
- Clover sẽ khởi động, bạn hãy di chuyển trái phải và chọn **Boot macOS Install from install_osx** (mình đặt lại tên là vì chọn cái này cho dễ hướng dẫn, vì có nhiều phiên bản macOS)
- Chờ đợi cho một màn hình đen toàn chữ chạy, và cảm nhận mình như một **hacker**
    - Nếu bạn bị dừng ở chỗ nào đó và kèm theo một màn hình đen với một vòng tròn gạch chéo thì xin chia buồn bạn đã bị panic. Cần chỉnh sửa lại clover hoặc bios.
    - Nếu bị panic bạn hãy cố chụp lại lỗi và đăng lên group hỏi. Bạn nên thử lại bằng cổng usb khác hoặc thay usb tạo lại bộ cài hoặc xem xét lại bios kĩ trước khi hỏi.
- Sau khi màn hình chữ chạy xong tiếp đó là logo Apple và thanh loading thì chúc mừng bạn đã boot vào được bộ cài.

    ![https://vnohackintosh.com/img/hackintosh/install/lang.png](https://vnohackintosh.com/img/hackintosh/install/lang.png)

- Chọn **Disk Utility**

    ![https://vnohackintosh.com/img/hackintosh/install/usb-disk-utility.png](https://vnohackintosh.com/img/hackintosh/install/usb-disk-utility.png)

- Giờ đến lúc xóa ổ cứng hoặc phân vùn để cài macOS, đặt tên thế nào là tùy bạn (ở đây mình đặt nó là “macOS”)
    - Nếu bạn sạch cả ổ cứng thì hãy chọn View -> Show All Devices -> chọn tên ổ cứng của bạn ở cột bên trái -> Erase -> lựa chọn như hình sau rồi chọn Erase

        ![https://vnohackintosh.com/img/hackintosh/install/erase-disk.png](https://vnohackintosh.com/img/hackintosh/install/erase-disk.png)

    - Nếu bạn chỉ xóa phân vùng ổ cứng đã chuẩn bị (cần efi >= 200MB mới xóa theo cách này được) thì chọn tên phân vùng bạn đã chọn ở cột bên trái -> Erase -> lựa chọn như hình sau rồi chọn Erase

        ![https://vnohackintosh.com/img/hackintosh/install/erase-partition.png](https://vnohackintosh.com/img/hackintosh/install/erase-partition.png)

- Xóa xong rồi thì đóng disk-utility lại (bấm nút đỏ trên góc)
- Quay trở lại màn hình cũ, giờ bạn chọn **Install macOS**
- Chọn Continue cho đến khi nào ra cái màn hình chọn phân vùng cài, lúc này bạn chọn phân vùng tên là macOS (nếu đặt tên khác thì chọn tên đó), chọn Continue và quá trình cài sẽ bắt đầu.
- Chờ đợi khoảng 15-30 phút, tùy vào tốc độ ổ cứng của bạn.

**4.3. Boot macOS trên ổ cứng lần đầu**

- Sau khi cài đặt xong máy sẽ tự khởi động lại, bạn chú ý sẽ phải vào boot vào usb trước rồi mới boot được macOS từ ổ cứng.
- Di chuyển trái phải chọn ổ có tên là **Boot macOS from macOS**, macOS là tên mình đặt, bạn đặt tên khác thì chọn Boot **macOS from <tên bạn đặt>**
- Chờ màn hình toàn chữ chạy đến cái màn hình loading
- Làm theo hình cho dễ

![https://vnohackintosh.com/img/hackintosh/install/1.png](https://vnohackintosh.com/img/hackintosh/install/1.png)

![https://vnohackintosh.com/img/hackintosh/install/2.png](https://vnohackintosh.com/img/hackintosh/install/2.png)

![https://vnohackintosh.com/img/hackintosh/install/3.png](https://vnohackintosh.com/img/hackintosh/install/3.png)

![https://vnohackintosh.com/img/hackintosh/install/4.png](https://vnohackintosh.com/img/hackintosh/install/4.png)

![https://vnohackintosh.com/img/hackintosh/install/5.png](https://vnohackintosh.com/img/hackintosh/install/5.png)

![https://vnohackintosh.com/img/hackintosh/install/6.png](https://vnohackintosh.com/img/hackintosh/install/6.png)

![https://vnohackintosh.com/img/hackintosh/install/7.png](https://vnohackintosh.com/img/hackintosh/install/7.png)

![https://vnohackintosh.com/img/hackintosh/install/8.png](https://vnohackintosh.com/img/hackintosh/install/8.png)

![https://vnohackintosh.com/img/hackintosh/install/9.png](https://vnohackintosh.com/img/hackintosh/install/9.png)

![https://vnohackintosh.com/img/hackintosh/install/10.png](https://vnohackintosh.com/img/hackintosh/install/10.png)

![https://vnohackintosh.com/img/hackintosh/install/11.png](https://vnohackintosh.com/img/hackintosh/install/11.png)

**4.4. Những việc cần làm sau cài đặt lên ổ cứng**

Như vậy là đã xong quá trình cài đặt vào ổ cứng. Tiếp theo cần phải làm một số việc để mac có thể chạy ổn định. Mình sẽ viết bài sau. + Cài đặt clover lên ổ cứng và chỉnh sửa + Đối với card NVIDIA dòng Maxwell và bạn cần cài đặt web driver nhận card rời. + Thêm các kext cần thiết và chỉnh sửa config + Patch DSDT/SSDT, bắt buộc đối với laptop, PC có thể không cần patch toàn bộ nhưng ít nhất bạn cần patch cổng usb.

**5. Cài đặt clover vào ổ cứng**

Phần này mình sẽ hướng dẫn cài và tinh chỉnh clover lên ổ cứng để bạn không cần usb mồi nữa. Đối với PC thì làm đến này là đã có thể dùng ổn định. Còn laptop thì vẫn còn một chặng đường dài nữa.

**5.1. Vô hiệu hóa Gatekeeper**

Mở Terminal lên và chạy lệnh sau và nhập password:

```
sudo spctl --master-disable
```

Chạy lệnh trên để mở khóa cho phép cài đặt phần mềm bên thứ ba vào macOS. Nếu không dùng lệnh này khi muốn mở phần mềm gì đó bạn cần chuột phải và chọn **Open** sau đó sẽ không cần làm lại.

**5.2. Cài Clover UEFI lên ổ cứng**

Để bạn có thể boot và macOS mà không cần usb mồi thì cần cài clover vào phân vùng efi của ổ cứng.

- **Sao chép clover usb vào efi ổ cứng**

    Clover usb đã boot được và có sẵn config, drivers64UEFI và một số kext cơ bản chép vào ổ cứng luôn sau chỉnh sửa cho đỡ mất công.

    Download Clover Configurator mới nhất theo link sau [https://mackie100projects.altervista.org/download-clover-configurator/](https://mackie100projects.altervista.org/download-clover-configurator/)

    Mở Clover Configurator lên -> Mount EFI -> Mount Partition

    ![https://vnohackintosh.com/img/hackintosh/post-install/cc.png](https://vnohackintosh.com/img/hackintosh/post-install/cc.png)

    Mount cả efi usb và efi ổ cứng ra sau đó copy thư mục /EFI/CLOVER trong USB qua vị trí tương ứng trên phân vùng EFI ổ cứng

- **Chạy clover installer**

    Download phiên bản **clover efi** mới nhất [https://sourceforge.net/projects/cloverefiboot/](https://sourceforge.net/projects/cloverefiboot)

    Thực hiện các bước sau

    ![https://vnohackintosh.com/img/hackintosh/post-install/1.png](https://vnohackintosh.com/img/hackintosh/post-install/1.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/2.png](https://vnohackintosh.com/img/hackintosh/post-install/2.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/3.png](https://vnohackintosh.com/img/hackintosh/post-install/3.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/4.png](https://vnohackintosh.com/img/hackintosh/post-install/4.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/5.png](https://vnohackintosh.com/img/hackintosh/post-install/5.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/6.png](https://vnohackintosh.com/img/hackintosh/post-install/6.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/7.png](https://vnohackintosh.com/img/hackintosh/post-install/7.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/8.png](https://vnohackintosh.com/img/hackintosh/post-install/8.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/9.png](https://vnohackintosh.com/img/hackintosh/post-install/9.png)

    - Đa số pc hay laptop mình chỉ cần dùng AptioMemoryFix-64 là có thể load nvram
    - Nếu cài như trên mà bị lỗi shutdown hay restart không được thì hãy sử dụng OsxAptioFixDrv-64 + EmuVariableUefi-64 thay cho AptioMemoryFix-64

    ![https://vnohackintosh.com/img/hackintosh/post-install/10.png](https://vnohackintosh.com/img/hackintosh/post-install/10.png)

    ![https://vnohackintosh.com/img/hackintosh/post-install/11.png](https://vnohackintosh.com/img/hackintosh/post-install/11.png)

    - Nếu bạn dùng main MSI thì hãy dùng OsxAptioFix2Drv-free2000.efi + EmuVariableUefi-64. Download [OsxAptioFix2Drv-free2000.efi](http://hackintosher.com/wp-content/uploads/2017/07/OsxAptioFix2Drv-free2000.efi_.zip) và thêm vào **EFI/Clover/drivers64UEFI** và xóa AptioMemoryFix-64.efi đi. EmuVariableUefi-64 bạn có lấy từ bộ cài Clover để cài hoặc dùng Clover Configurator.

        ![https://vnohackintosh.com/img/hackintosh/post-install/cc-uefi.png](https://vnohackintosh.com/img/hackintosh/post-install/cc-uefi.png)

    - **Kexts**

        Hiện tại trong clover đã có kext cơ bản, đối với PC thì từng đó kext là đủ nhưng đối với laptop thì sẽ cần một số kext khác nữa.

        - Kext chung cho PC và laptop:
            - **FakePCIID + FakePCIID_XHCIMux**: điều hướng USB2 sang EHCI dành Broadwell trở về trước, Skylake trở về sau không cần. [Download](https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads/)
            - **AirportBrcmFixup**: đi kèm với Lilu hỗ trợ wifi cho một số card wifi broadcom có thể chạy trên macOS [Download](https://github.com/acidanthera/AirportBrcmFixup)
            - **BT4LEContiunityFixup**: Đi kèm với Lilu giúp bật tính năng Handoff, Hostspot đối với một card wifi broadcom hỗ trợ macOS [Download](https://github.com/acidanthera/BT4LEContiunityFixup)
            - **AppleALC**: kext hỗ trợ audio cho macOS đi kèm với **Lilu** cần inject layout-id phù hợp trong config.plist (sẽ có bài hướng dẫn sau) để có âm thanh [Download](https://github.com/acidanthera/AppleALC)
            - **VirtualSMC**: kext mới để thay thế cho **FakeSMC** cần đi kèm với **Lilu**. Chỉ dùng một trong hai kext FakeSMC hoặc VirtualSMC, hiện tại cá nhân mình dùng FakeSMC vẫn ổn. Nếu dùng VirtualSMC hãy xóa SMCHelper-64.efi trong Clover/driver64Uefi và thay vào đó là VirtualSmc-64.efi [Download](https://github.com/acidanthera/VirtualSMC)
        - Dành cho laptop:
            - **ACPIBatteryManager**: kext quản lí pin dành cho laptop [Download](https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads/)
            - **SmartTouchpad**: dành cho một số touchpad ELAN đời cũ, hãy thử kext này nếu dùng VoodooPS2Controller không có touchpad. Tải bản beta mới nhất ở đây [Download](https://osxlatitude.com/forums/topic/1948-elan-focaltech-and-synaptics-smart-touchpad-driver-mac-os-x/)
            - **VoodooI2C + Plugin**: dành cho laptop có touchpad I2C, yêu cầu phải patch DSDT/SSDT mới dùng được. Nếu patch được thì có thể dùng được gesture như real mac [Download](https://github.com/alexandred/VoodooI2C/releases)

        Chọn được kext phù hợp rồi thì thêm hết vào **EFI/Clover/kexts/Other**

**6. Boot mặc định vào clover**

Tùy vào main mà việc boot vào clover sẽ có độ khó khăn khác nhau.

Sẽ có các cách sau đây theo thứ tự từ dễ tới phức tạp.

- **Boot từ BOOTX64.efi**

    Hệ thống UEFI sẽ tự tìm các file đuôi .efi và chứa boot loader để chạy. Mặc định nếu bạn chọn boot option là ổ cứng nào đó thì hệ thống sẽ boot từ file EFI/BOOT/BOOTX64.efi .

    ![https://vnohackintosh.com/img/hackintosh/boot/bootx64.png](https://vnohackintosh.com/img/hackintosh/boot/bootx64.png)

    Nếu ổ cứng chỉ có chứa duy nhất macOS thì chỉ cần chỉnh boot của cái ổ cứng đó lên đầu là được vì sau khi chạy clover installer thì nó đã tự tạo EFI/BOOT/BOOTX64.efi tương ứng với EFI/CLOVER/CLOVERX64.efi.

    Nếu ổ cứng của bạn đã có chứa windows trước, thì BOOTX64.efi lúc này lại là boot của windows. Bạn hãy thử copy CLOVER/CLOVERX64.efi qua thư mục BOOT và đổi tên nó thành BOOTX64.efi sau đó chỉnh boot của ổ cứng lên đầu. Cách này thành công thì ít mà thất bại thì nhiều.

- **Thêm boot option**

    Đa số BIOS đều cho phép thêm boot option. Nếu không thể boot mặc định từ BOOTX64.efi thì hãy tìm trong BIOS dòng tùy chọn này **Add New Boot Option**, nếu không thấy vui lòng chuyển qua cách cuối cùng.

    ![https://2.bp.blogspot.com/-deg_-y4Aunk/VEYuB8HOqmI/AAAAAAAAAz0/-34xaLPTHjE/s1600/IMG_0225_zpsc090d8b7.jpg](https://2.bp.blogspot.com/-deg_-y4Aunk/VEYuB8HOqmI/AAAAAAAAAz0/-34xaLPTHjE/s1600/IMG_0225_zpsc090d8b7.jpg)

    Sau đó thêm một boot option và chỉnh đường dẫn như sau:

    Chọn ổ cứng có EFI chứa clover -> EFI -> CLOVER -> CLOVERX64.efi

    ![https://3.bp.blogspot.com/-7_lMzbDn9os/VEYuPKsqV2I/AAAAAAAAAz8/FTZ4dGr8AaI/s1600/IMG_0226_zps2ef2c14c.jpg](https://3.bp.blogspot.com/-7_lMzbDn9os/VEYuPKsqV2I/AAAAAAAAAz8/FTZ4dGr8AaI/s1600/IMG_0226_zps2ef2c14c.jpg)

    Cuối cùng thì chỉnh boot option mới lên đầu.

- **Boot từ bootmgfw.efi**

    Nếu bạn đã cài windows lên ổ cứng và trong BIOS không thể thêm boot option (mấy cái máy ACER hay thế này), không thấy boot ổ cứng đâu chỉ thấy Windows Boot Manager ở đầu tiên thì chỉ còn cách này. Nhưng cách này thì đảm bảo chắc chắn bạn có thể boot vào clover.

    ![https://vnohackintosh.com/img/hackintosh/boot/cloverx64.png](https://vnohackintosh.com/img/hackintosh/boot/cloverx64.png)

    Cách làm như sau:

    1. Vào /EFI/Microsoft/Boot/ đổi tên file **bootmgfw.efi** thành **bootmgfw-orig.efi**
    2. Vào /EFI/CLOVER, copy **CLOVERX64.efi** sang /EFI/Microsoft/Boot/ và đổi tên nó thành **bootmgfw.efi**
    3. Giữ nguyên Windows Boot Manager ở đầu trong list boot option

    ![https://vnohackintosh.com/img/hackintosh/boot/bootmgfw.png](https://vnohackintosh.com/img/hackintosh/boot/bootmgfw.png)

    Nguyên lí rất đơn giản là mặc định nó boot vào windows thì thay boot windows thành boot của clover là được. Bước một khá quan trọng nếu bạn không làm theo thì sẽ mất boot windows.

**7. Cài web driver cho card rời nvidia**

- **Giới thiệu**
    - Đối với những máy dùng card rời NVIDIA dòng Maxwell và Pascal thì lúc mới cài máy sẽ chưa nhận card và sẽ rất lag, cần phải cài web driver vào thì máy mới nhận card rời.
    - Cụ thể thì là những card sau
        - NVIDIA Pascal: GeForce GTX 1030, 1050, 1050 Ti, 1060, 1070, 1070 Ti, 1080, 1080 Ti, TITAN Pascal và TITAN Xp
        - NVIDIA Maxwell: GeForce GTX 750, 750 Ti, 950, 960, 970, 980, 980 Ti, and TITAN X
    - Dòng card NVIDIA RTX mới ra hiện chưa có web driver nên chưa thể dùng cho hackintosh.
    - Chưa có Web Driver cho Mojave nên chỉ chỉ dừng lại ở High Sierra 10.13.6 không được update lên.
    - Dòng card NVIDIA Kelper sẽ chạy native với kext của Apple nên không cần cài web driver và có thể cài Mojave trở lên.

- **Yêu cầu**
    - Cần có **Lilu + Whatevergreen** trong Clover/kexts/Other
    - Nếu làm theo hướng dẫn của mình thì đã có sẵn

**Cài NVIDIA Web Driver**

**Mình sẽ giới thiệu hai cách cài cho các bạn tham khảo**

- **Dùng bộ cài từ NVIDIA**
    - Kiểm tra build version của macOS đang dùng. Mở Terminal lên và chạy lệnh sau:

        ```
        sw_vers

        ```

    - Download bộ cài Web Driver đặt phù hợp
    - Danh sách các phiên bản web driver đã được tổng hợp. Bạn có thể download từ các nguồn sau:
        - **tonymacx86**: [https://www.tonymacx86.com/nvidia-drivers/](https://www.tonymacx86.com/nvidia-drivers/)
        - **InsanelyMac**: [https://www.insanelymac.com/forum/topic/324195-nvidia-web-driver-updates-for-macos-high-sierra-update-apr-02-2019/](https://www.insanelymac.com/forum/topic/324195-nvidia-web-driver-updates-for-macos-high-sierra-update-apr-02-2019/)
    - Download phiên bản web driver phù hợp với build version macOS của bạn
    - Chạy bộ cài web drvier (ảnh chỉ mang tính chất minh họa)
    - Web Driver sẽ báo restart thì bạn đừng restart vội, còn cần chỉnh config.plist nữa.
    - Nếu không thấy bản web driver nào giống build version macOS thì hãy update macOS lên phiên bản phù hợp nhưng không được update lên Mojave hoặc thử cách phía dưới.

- **Dùng nvidia-update script của Benjamin-Dobell**
    - Không cần làm gì nhiều chỉ mở Terminal lên và chạy lệnh sau

        ```
        bash <(curl -s https://raw.githubusercontent.com/Benjamin-Dobell/nvidia-update/master/nvidia-update.sh)

        ```

    - Link github của repository để ủng hộ tác giả: [https://github.com/Benjamin-Dobell/nvidia-update](https://github.com/Benjamin-Dobell/nvidia-update)
    - Cách này cực dễ làm nhưng đôi lúc có thể lỗi, vẫn khuyến khích dùng cách bên trên. Nếu bạn dùng bộ cài từ olarila thì hãy dùng cách này.

**Chỉnh sửa config.plist**

- Đừng vội restart máy, hãy mount EFI ra và mở config.plist lên bằng Clover Configurator
- Chuyển qua tab **Boot** và thêm boot argument sau: **nvda-drv=1**
- Chuyển qua tab **System Parameters** và tích **NvidiaWeb** lên
- Lưu config.plist lại và restart

→ **Summary**

Mình đã hướng dẫn cơ bản cách cài web driver. Đến đây gần như là PC đã có thể hoạt động ổn định. Để chơi hackintosh lâu dài thì bạn vẫn nên qua với team đỏ AMD.

Cảm ơn NVIDIA, team Acidanthera, Benjamin Dobell và tất cả các lập trình viên đã viết kext và tool cho Hackintosh.

**7. Update MacOS version lên phiên bản cao hơn cho hackintosh**

7.1. Update EFI lên phiên bản mới nhất. 

Quay lại bước 5.2 và làm theo phần **Chạy clover installer**

7.2. Download bản cập nhật.

**Tắt hết các ứng dụng hiện tại.**

Vào System Preferences trên Mac hiện tại → Software Update → Update now để tải bản cập nhật

![Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/Screen_Shot_2020-09-23_at_9.51.21_AM.png](Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/Screen_Shot_2020-09-23_at_9.51.21_AM.png)

Quá trình tải hoàn tất, ấn restart để khởi động lại. Khi này Mac sẽ tạo ra một bản sao hiện tại vào ổ tên là Your-disk Recovery. Và một ổ preboot để boot bản cập nhật tên Data via Preboot

**Note: Your-disk là tên ổ cứng mà bạn đặt, ở đây của mình tên là Amela**

![Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/4A2DC59A-4105-453D-B198-C0AB8F0200D4.jpg](Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/4A2DC59A-4105-453D-B198-C0AB8F0200D4.jpg)

Chọn boot bằng ổ Data via Preboot. Mac sẽ update lên phiên bản mà bạn mới download. Qua trình này hoàn tất, Máy sẽ khởi động lại, Nếu ổ Data via Preboot đã biến mất như dưới đây.

![Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/66DB6201-693E-4E4A-9F66-73F71FE922AA.jpg](Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/66DB6201-693E-4E4A-9F66-73F71FE922AA.jpg)

thì quá trình cập nhật đã thành công, Hãy boot vào ổ Your-disk  như thường

Trong một vài trường hợp, boot vào Data via Preboot, trong khi cài đặt sẽ bị treo máy ở màn hình trông như thế này. Hãy chờ đợi thêm 10-15p và reset máy. Sau đó boot vào Your-disk  và xem đã update thành công chưa. 

![Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/Untitled.png](Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/Untitled.png)

Một vài trường hợp khác, việc update suôn sẻ nhưng khi boot lại vào Your-disk, thì Mấy vẫn báo chưa được update. Hãy tắt hết ứng dụng và update lại từ đầu.

![Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/Screen_Shot_2020-09-23_at_1.44.52_PM.png](Hackintosh%20guilde%2082a803e91b8542a4b572e25da3aab999/Screen_Shot_2020-09-23_at_1.44.52_PM.png)

Update thành công.

**8. Cài đặt dual boot MacOS với Windows. (source luulam.dev)**

**8.1. Cài dual boot windows và hackintosh trên cùng 1 ổ cứng**

**Bước 1: Tạo phân vùng để cài thêm Windows**

1. Mở **Disk Utility**
2. Chọn **Show All Devices** để hiện thỉ toàn biij các device đang được cắm vào thiết bị
3. Click **Partition** nút ở trên toolbar
4. Thêm một phân vùng mới bằng cách nhấp vào nút [**+**] bên dưới hình tròn
5. Điền một tên và kích thước mong muốn phân vùng đề cài Window 10 là 50GB cho thỏa mái
6. Chọn **Format: ExFat**
7. Chọn **Apply**
8. Xong đợi done thôi

**Bước 2: Bắt đầu cài Đặt Window 10**

![https://hackintosher.com/wp-content/uploads/Windows-10-Hackintosh-Dual-Boot.jpg](https://hackintosher.com/wp-content/uploads/Windows-10-Hackintosh-Dual-Boot.jpg)

1. Download **[Windows Installer Manager/ISO](https://www.microsoft.com/en-us/software-download/windows10)** cho nó chuẩn
2. Dùng bản Iso để tạo usb boot
3. Khởi động lại máy rồi bắt đầu Boot vào usb để bắt đầu cài window
4. Tại bước chọn phân vùng cài đặt này chúng ta sẽ chọn phân vùng đã tạo ở trên

    ![https://hackintosher.com/wp-content/uploads/Windows-10-formatting-GUID-1024x769.jpg](https://hackintosher.com/wp-content/uploads/Windows-10-formatting-GUID-1024x769.jpg)

5. Click **Format**
6. Click **OK**
7. Hoàn tất cài đặt… Lưu ý: Hệ thống sẽ khởi động lại nhiều lần trong khi cài đặt

**Bước 3: tạo boot Drive**

Khi cài đặt xong reset máy chúng ta sẽ vào trong Windows không vào macOs bởi vì Windows Installer đã sửa đổi thư mục EFI của chúng tôi thay thế bộ nạp khởi động Clover thành Windows Boot Manager

Để sửa chữa Dual-khởi động hackintosh chúng ta cần phải sửa tập tin được gọi là bootmgfw. EFI đó là ngăn chặn truy cập vào Clover tại hệ thống boot. Tất cả bạn cần làm là đổi tên bootmgfw. EFI để bootmgfw-orig. EFI gây Clover để trở thành không bị chặn. Chúng tôi thêm-orig vào tên để nó vẫn dễ nhận biết tập tin và sẽ hiển thị phân vùng Windows EFI để khởi động trong bộ nạp Clover Boot menu.

**đổi tên bootmgfw.efi:**

1. Sử dụng Usb để cài Macos để có thể boot vào macOS đã cài đặt
2. Mở **Clover Configurator** lên nếu chưa có download tại dây
3. **Click Mount EFI** cột bên trái
4. Click **Mount Partition**
5. Click **Open Partition**
6. Mở **Finder** rồi đến đường dẫn **EFI/Microsoft/Boot/**
7. Đổi tên **bootmgfw.efi** to **bootmgfw-orig.efi**

**(Tùy chọn) ghi đè Windows Boot Manager**

để ghi đè lên Windows Boot Manager băng Clover làm theo các bước sau

1. **Mount** phân vùng EFI bằng **Clover Configurator** lại giống như các trên
2. Mở **Finder** rồi đến đường dẫn **EFI/BOOT/**
3. Copy **BOOTX64.efi**
4. Rồi đến đường dẫn **EFI/Windows/Boot**
5. Paste **BOOTX64.efi** tại đây
6. Đổi tên **BOOTX64.efi** thành **bootmgfw.efi** sau khi đã paste vào đây
7. Giờ có thể khởi động lại và chọn **Boot Windows EFI from EFI**

![https://hackintosher.com/wp-content/uploads/Boot-Microsoft-EFI-from-EFI-Clover.jpg](https://hackintosher.com/wp-content/uploads/Boot-Microsoft-EFI-from-EFI-Clover.jpg)

**8.2. Cài dual boot windows vs hackintosh trên 2 ổ cứng khác nhau**

**Trường hợp 1.** Bạn có 1 ổ cứng cài sẵn MacOS, một ổ cứng mới, dự định cài Windows lên ổ cứng mới đó. 

- Cắm ổ cứng mới và USB boot windows  vào máy.
- Boot vào Clover Bootloader, lúc này sẽ xuất hiện thêm ổ để boot vào usb, Chọn ổ đó, và cài đặt windows lên ổ cứng mới. Sau khi cài xong thì trong clover sẽ xuất hiện thêm một ổ boot windows từ ổ cứng mới, chọn vào đó là vào được windows.

**Trường hợp 2.** Bạn đã có một ổ cài sẵn windows, một ổ mới dự định cài hackintosh lên ổ cứng mới. 

- Cắm USB boot MacOS vào máy, boot vào đó và cài đặt hackintosh lên ổ cứng mới. Sau khi cài đặt xong, trong clover sẽ xuất hiện cả ổ để boot vào Mac và Windows

**Trường hợp 3.** Bạn có 1 ổ cứng cài sẵn MacOS, và bạn kiếm đâu đó về một cái ổ cứng đã cài Windows, và bạn không muốn cài lại 1 trong 2 hệ điều hành một lần nữa.

- Hãy thứ cắm ổ mới vào máy, boot vào clover xem có xuất hiện thêm ô để boot vào Window hay không. Nếu Clover không nhận diện được ổ cứng mới cắm. Hãy boot vào MacOS, và làm theo các bước sau:
    1. Mở **Clover configurator** => **Mount EFI** lên phân vùng **EFI của Windows** ( không mount lên EFI của macOS (200MB))
    2. Mở phân vùng EFI đó lên sau khi mới mount xong
    3. Vào **EFI/Microsoft/boot** tìm và đổi tên “**bootmgfw.efi**” thành “**bootmgfw-orig.efi**“
    4. Copy **EFI/CLover/CLOVERX64.efi** đến **EFI/Microsoft/boot** đổi tên thành “**bootmgfw.efi**“
    5. Khởi động lại máy sẽ hiện Menu boot với cả 2 hệ điều hành
