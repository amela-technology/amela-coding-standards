## Amela Git flow
Git flow này trong tương lai sẽ có thể cập nhật, thay đổi các phần không hợp lý.
### Changelog
17-10-2019: Fist release

11-05-2020:
* Thêm một số usecase thực tế và cách áp dụng
* Thêm nhánh release nhằm hỗ trợ tốt hơn việc fixbug, hotfix cho phiên bản production
### Giả định
* Đã tạo Repository trên Github（hoặc Bitbucket, gitlab ....）.
* Branch mặc định của Repository là master.
* Developer đã có quyền truy cập, tạo nhánh mới, tạo pull request đối với Repository.
* Đã quyết định người có quyền review và merge.
* Sử dụng Shared Repository Model
* Branch master, develop, release phải được bảo vệ bằng chức năng protected branchs (không push trực tiếp từ local lên branch này)
### Các nhánh chính
Repo trung tâm sẽ có hai nhánh chính với tuổi thọ vô hạn:
* `master`
* `develop`

Các nhánh phụ lưu giữ source code cho từng version ứng dụng
* `release/xxx` (vd: release/0.1.1)

`origin/master` là nhánh chính trong đó source code phải luôn luôn trong trạng thái có thể release ngay lập tức.

`origin/develop` là nhánh chứa source code đang phát triển cho lần release tiếp theo.

`origin/release_xxxx` là nhánh chứa source code của phiên bản đã được release được checkout ra từ nhánh master (đặt tên theo số hiệu phiên bản).

Khi source code trong nhánh develop đã ổn định và sẵn sàng để được release, tất cả các thay đổi sẽ được hợp nhất(merged) trở lại vào `master` và sẽ được gắn liền với một số hiệu phiên bản
### Các nhánh hỗ trợ (hoặc nhánh phụ)
Mỗi nhánh này tương ứng với 1 task trên trình quản lý dự án, sử dụng các nhánh này để dễ dàng phát triển song song giữa các thành viên trong nhóm, dễ dàng theo dõi các task, feature, bug, hotfix, pre-release  . Không giống như các nhánh chính, các nhánh này luôn có thời gian sống hạn chế, vì cuối cùng chúng sẽ bị loại bỏ.
### Quy trình làm việc đối với dự án 1 pull request có thể có nhiều commit

1. Developer hãy checkout và đồng bộ hóa branch develop tại local với remote.
    ```sh
    $ git checkout develop # <--- Không cần thiết nếu đang ở trên branch develop
    ```
    ```sh
    $ git pull origin develop
    ```
    
2. Khi tiến hành xử lý các task tiếp theo, hãy tạo branch mới từ nhánh develop tương ứng với task đã nhận:
    ```sh
    $ git checkout -b feature/1234
    ```
3. Tiến hành làm task.(số lượng commit trong 1 pull request tùy từng dự án mà thay đổi cho phù hợp)
   
4. Push code lên origin.
    ```sh
    $ git push origin feature/1234
    ```
5. Tại origin trên Github（Bitbucket）, từ branch feature/1234 đã được push lên hãy gửi pull-request đối với branch develop của origin. (lưu ý nội dung của pull request cần tuân thủ các quy tắc theo templet pull request (nếu có) hoặc theo bên dưới)
6. Gửi link URL của  pull-request cho reviewer trên Cliq hoặc Skype để tiến hành review code.

    6.1. Trong trường hợp reviewer có yêu cầu chỉnh sửa, hãy thực hiện các bước 3. 〜 5.. 6.2. Tiếp tục gửi lại URL cho reviewer trên skype/cliq để tiến hành việc review code.
   
7. Nếu trên x người(tuỳ dự án x có thể 1 hoặc 2) reviewer đồng ý với pull-request, người reviewer cuối cùng sẽ thực hiện việc merge pull-request. Revewer xác nhận sự đồng ý bằng comment LGTM (looks good to me, anh tin chú....).
8. Trở lại 1.
### Quy trình làm việc đối với dự án 1 pull request chỉ được phép có 1 commit
1. Developer hãy checkout và đồng bộ hóa branch develop tại local với remote.
    ```sh
    $ git checkout develop # <--- Không cần thiết nếu đang ở trên branch develop
    ```
    ```sh
    $ git pull origin develop
    ```
2. Khi tiến hành xử lý các task tiếp theo, hãy tạo branch mới từ nhánh develop tương ứng với task đã nhận:
    ```sh
    $ git checkout -b feature/1234
    ```
3. Tiến hành làm task. (chỉ được 1 commit duy nhất)
  3.1 Trường hợp đã lỡ tay tạo nhiều commit, hãy dùng git rebase -i để gộp commit thành 1 commit duy nhất trước khi push code :
    ```sh
    $ git rebase -i [Hash của commit đầu tiên khi làm task]
    ```
4. Push code lên origin.
    ```sh
    $ git push origin feature/1234
    ```
5. Tại origin trên Github（Bitbucket）、từ branch feature/1234 đã được push lên hãy gửi pull-request đối với branch develop của origin. (lưu ý nội dung của pull request cần tuân thủ các quy tắc theo templet pull request (nếu có) hoặc theo bên dưới)
     
6. Gửi link URL của  pull-request cho reviewer trên Cliq hoặc Skype để tiến hành review code.

    6.1. Trong trường hợp reviewer có yêu cầu chỉnh sửa, hãy thực hiện các bước 3. 〜 5.. 6.2. Tiếp tục gửi lại URL cho reviewer trên skype/cliq để tiến hành việc review code.
    6.2. Nếu 6.1 xảy ra, khi sửa code xong có thể sử dụng git commit --amend để thực hiện gộp các chỉnh sửa vào commit trước đó:
      ```sh
        $ git commit --amend --no-edit
      ```
7. Nếu trên x người(tuỳ dự án x có thể 1 hoặc 2) reviewer đồng ý với pull-request, người reviewer cuối cùng sẽ thực hiện việc merge pull-request. Revewer xác nhận sự đồng ý bằng comment LGTM (looks good to me, anh tin chú....).
8. Trở lại 1.
### Nguyên tắc
* Mỗi pull-request tương ứng với một task.
* Mỗi một pull-request sẽ không hạn chế số lượng commit
* Đối với branch, hãy đặt branch name theo định dạng `[Task type]/[Task Id]` (Ví dụ: `feature/1234`, `bug/4567`,...)
* Đối với commit message, trong trường hợp pull-request đó chỉ có 1 commit thì có thể đặt commit message tương tự như trên là `[Task type]/[Task Id] [Task description]`
* Pull-request title phải đặt sao cho tương ứng với title của task với format `[Task type]/[Task Id] [Task description]` （Ví dụ: `bug/1234 Fix request login timeout` , `feature/1234 Login UI`）.
  Trường hợp pull-request có chứa nhiêù commit thì cần phải ghi rõ trong nội dung commit message là trong commit đó xử lý đối ứng vấn đề gì trong task đó.
    * Ví dụ:
        1. Pull-request title: `bug/1234 Fix request login timeout`
        2. Trong trường hợp pull-request có 2 commit thì nội dung commit message của 2 commit sẽ tương ứng như sau
            * `bug/1234 Added header time out`
            * `bug/1234#1 Fix dialog loading too long`
* Tại môi trường local repository, tuyệt đối không được thay đổi code khi ở branch master và develop. Nhất định phải thao tác trên branch khởi tạo để làm task.
### Sửa conflict không thể tạo pull request:
* Lưu ý nếu bị confict không thể tạo pull request hãy tiến hành cập nhật nhánh develop và rebase với nhánh đang làm việc:
    ```sh
     $ git checkout develop
     $ git pull origin develop
     $ git checkout feature/1234
     $ git rebase develop
    ```
    
1. Hãy thực hiện fix lỗi conflict thủ công（những phần được bao bởi <<< và >>> ）.
Trong trường hợp muốn dừng việc rebase lại, hãy dùng lệnh `git rebase --abort`.

2. Khi đã giải quyết được tất cả các conflict, tiếp tục thực hiện việc rebase bằng:

    ```sh
    $ git add .
    $ git rebase --continue
    ```
### Đang làm dở task A nhánh develop có code mới, làm thế nào để rebase luôn ?
Trong trường hợp bạn đang làm việc trên 1 brand mà dự án yêu cầu sửa gấp hoặc ưu tiên cho task khác thì hãy sử dụng git stash
1. Lưu tất cả code đang làm dở vào git stash

    ```$ git stash save ```
2. Checkout về nhánh develop và pull code mới về

    ```sh
       $ git checkout develop
       $ git pull origin develop
   ```
3. Checkout về nhánh đang làm dở và rebase develop
      ```sh
       $ git checkout -
       $ git rebase develop
      ```
ProTip: Sử dụng ' - ' như là một cách gọi nhánh trước đó (previous ) nếu không nhớ chính xác nhánh vừa rồi đã checkout là gì
###Quy trình release
1. Trên nhánh develop chuẩn bị sẵn code sạch cho việc release (pass review tester và các bên liên quan)
2. Tạo pull request và merge code vào master với pull request title (release/xxx)
3. Từ nhánh master, checkout code sang một nhánh mới có tên release/xxx (vd: release_0.0.1) và tiến hành deploy/build code trên nhánh này
4. Khi có bug xảy ra hãy tạo brand hotfix/xxx từ nhánh release_xxx
5. Sau khi làm xong thì gửi pull request vào release, merge và deploy lên.
6. Gửi pull request vào master và merge.
### Một số gợi ý giúp triển khai tốt hơn 
1. Nêu có một chuẩn chung cho nội dung của pull request, tham khảo file PULL_REQUEST_TEMPLATE.MD trong thư mục templet
2. Nêu tạo các label cho pull request để dễ dàng trong việc tracking:
* Label theo loại task:
   * feature
   * bug
   * hotfix
   * release
* Label theo trạng thái pull request:
   * Waitting for review
   * Closed
   * Rejected
   * Ignore
3. Hạn chế conflict bằng cách phân chia thư mục dự án hợp lý (có thể tham khảo các phân chia thư mục package-by-feature)
