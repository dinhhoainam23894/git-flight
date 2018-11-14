# git-flight


#### "flight rules" là gì?

Một [hướng dẫn cho các phi hành gia](https://www.jsc.nasa.gov/news/columbia/fr_generic.pdf) (các lập trình viên sử dụng Git) về những việc cần làm khi một thứ gì đó xảy ra bị lỗi.

>  *Flight Rules* là kiến thức khó thấy được trong các hướng dẫn sử dụng, từng bước, phải làm gì đếu điều X xảy ra và tại sao. Về cơ bản, chúng là các quy trình thực hiện theo tiêu chuẩn cụ thể cho từng kịch bản cụ thể. [...]

> NASA đã nắm bắt được những sai lầm, thảm hoạ và giải pháp của chúng tôi kể từ đầu những năm 1960s, khi các nhóm Mercury-era bắt đầu thu thập "lessons learned" vào một bản tóm tắt mà bây giờ liệt kê hàng ngàn tình huống có vấn đề, từ lỗi động cơ đến busted hatch xử lý để ổn định máy tính, và các giải pháp của họ.

&mdash; Chris Hadfield, *Hướng dẫn về cuộc sống của một du hành vũ trụ.*.

#### Các quy ước cho tài liệu này

Vì để rõ ràng nên tất cả các ví dụ trong tài liệu này sử dụng thêm  dấu nhắc bash được tuỳ chỉnh để chỉ ra nhánh hiện tại và có hay không có các thay đổi theo giai đoạn. Nhánh được đặt trong dấu ngoặc đơn và dấu * bên cạnh tên nhánh cho biết các thay đổi được tổ chức.

Tất cả các command nên làm việc với phiên bản nhỏ nhất là 2.13.0. Xem [git website](https://www.git-scm.com/) để cập nhật phiên bản git trên local của bạn.

**Danh mục nội dung**  *được tạo bằng [DocToc](https://github.com/thlorenz/doctoc)*

  - [Repositories](#repositories)
    - [Tôi muốn bắt đầu một repository cục bộ](#t%C3%B4i-mu%E1%BB%91n-b%E1%BA%AFt-%C4%91%E1%BA%A7u-m%E1%BB%99t-repository-tr%C3%AAn-local)
    - [Tôi muốn clone một remote repository](#t%C3%B4i-mu%E1%BB%91n-clone-m%E1%BB%99t-remote-repository)
  - [Chỉnh sửa Commit](#ch%E1%BB%89nh-s%E1%BB%ADa-commit)
    - [Bạn vừa commit điều gì ?](#b%E1%BA%A1n-v%E1%BB%ABa-commit-%C4%91i%E1%BB%81u-g%C3%AC-)
    - [Tôi đã viết sai điều gì đó trong một commit message ](#t%C3%B4i-%C4%91%C3%A3-vi%E1%BA%BFt-sai-v%C3%A0i-th%E1%BB%A9-trong-message-c%E1%BB%A7a-commit)
    - [Tôi đã commit với tên và email cấu hình sai](#t%C3%B4i-%C4%91%C3%A3-commit-v%E1%BB%9Bi-t%C3%AAn-v%C3%A0-email-c%E1%BA%A5u-h%C3%ACnh-sai)
    - [Tôi muốn xoá một file từ commit trước](#t%C3%B4i-mu%E1%BB%91n-xo%C3%A1-m%E1%BB%99t-file-t%E1%BB%AB-commit-tr%C6%B0%E1%BB%9Bc)
    - [Tôi muốn xóa hoặc xóa cam kết cuối cùng của tôi](#t%C3%B4i-mu%E1%BB%91n-xo%C3%A1-ho%E1%BA%B7c-lo%E1%BA%A1i-b%E1%BB%8F-commit-cu%E1%BB%91i-c%C3%B9ng-nh%E1%BA%A5t-c%E1%BB%A7a-t%C3%B4i)
    - [Xoá/loại bỏ commit tuỳ ý](#xo%C3%A1lo%E1%BA%A1i-b%E1%BB%8F-commit-tu%E1%BB%B3-%C3%BD)
    - [Tôi đã cố gắng push commit đã sửa đổi lên remote, nhưng tôi gặp một thông báo lỗi](#t%C3%B4i-%C4%91%C3%A3-c%E1%BB%91-g%E1%BA%AFng-push-commit-%C4%91%C3%A3-s%E1%BB%ADa-%C4%91%E1%BB%95i-l%C3%AAn-remote-nh%C6%B0ng-t%C3%B4i-g%E1%BA%B7p-m%E1%BB%99t-th%C3%B4ng-b%C3%A1o-l%E1%BB%97i)
    - [Tôi đã vô tình thực hiện hard reset, và tôi muốn các thay đổi của tôi trở lại trước đó.](#t%C3%B4i-%C4%91%C3%A3-v%C3%B4-t%C3%ACnh-th%E1%BB%B1c-hi%E1%BB%87n-hard-reset-v%C3%A0-t%C3%B4ii-mu%E1%BB%91n-c%C3%A1c-thay-%C4%91%E1%BB%95i-c%E1%BB%A7a-t%C3%B4i-tr%E1%BB%9F-l%E1%BA%A1i-tr%C6%B0%E1%BB%9Bc-%C4%91%C3%B3)
    - [Tôi vô tình commit và đẩy lên một merge](#t%C3%B4i-v%C3%B4-t%C3%ACnh-commit-v%C3%A0-%C4%91%E1%BA%A9y-l%C3%AAn-m%E1%BB%99t-merge)
    - [Tôi vô tình commit và đẩy các tệp chứa dữ liệu nhạy cảm](#t%C3%B4i-v%C3%B4-t%C3%ACnh-commit-v%C3%A0-%C4%91%E1%BA%A9y-c%C3%A1c-file-ch%E1%BB%A9a-c%C3%A1c-d%E1%BB%AF-li%E1%BB%87u-nh%E1%BA%A3y-c%E1%BA%A3m)
  - [Staging](#staging)
    - [Tôi cần thêm các thay đổi đã stage cho commit trước đó](#t%C3%B4i-c%E1%BA%A7n-th%C3%AAm-c%C3%A1c-thay-%C4%91%E1%BB%95i-%C4%91%C3%A3-stage-cho-commit-tr%C6%B0%E1%BB%9Bc-%C4%91%C3%B3)
    - [Tôi muốn stage một phần của một file mới, nhưng không phải toàn bộ file](#t%C3%B4i-mu%E1%BB%91n-stage-m%E1%BB%99t-ph%E1%BA%A7n-c%E1%BB%A7a-m%E1%BB%99t-file-m%E1%BB%9Bi-nh%C6%B0ng-kh%C3%B4ng-ph%E1%BA%A3i-to%C3%A0n-b%E1%BB%99-file)
    - [Tôi muốn thêm các thay đổi trong một file vào 2 commit khác nhau](#t%C3%B4i-mu%E1%BB%91n-th%C3%AAm-c%C3%A1c-thay-%C4%91%E1%BB%95i-trong-m%E1%BB%99t-file-v%C3%A0o-2-commit-kh%C3%A1c-nhau)
    - [Tôi muốn stage các chỉnh sửa chưa được stage, và unstage các chỉnh sửa đã stage](#t%C3%B4i-mu%E1%BB%91n-stage-c%C3%A1c-ch%E1%BB%89nh-s%E1%BB%ADa-ch%C6%B0a-%C4%91%C6%B0%E1%BB%A3c-stage-v%C3%A0-unstage-c%C3%A1c-ch%E1%BB%89nh-s%E1%BB%ADa-%C4%91%C3%A3-stage)
  - [Unstaged Edits](#unstaged-edits)
    - [Tôi muốn di chuyển các chỉnh sửa chưa được staged đến một nhánh mới](#t%C3%B4i-mu%E1%BB%91n-di-chuy%E1%BB%83n-c%C3%A1c-ch%E1%BB%89nh-s%E1%BB%ADa-ch%C6%B0a-%C4%91%C6%B0%E1%BB%A3c-staged-%C4%91%E1%BA%BFn-m%E1%BB%99t-nh%C3%A1nh-m%E1%BB%9Bi)
    - [Tôi muốn di chuyển các chỉnh sửa chưa stage của tôi đến một nhánh khác đã tồn tại](#t%C3%B4i-mu%E1%BB%91n-di-chuy%E1%BB%83n-c%C3%A1c-ch%E1%BB%89nh-s%E1%BB%ADa-ch%C6%B0a-stage-c%E1%BB%A7a-t%C3%B4i-%C4%91%E1%BA%BFn-m%E1%BB%99t-nh%C3%A1nh-kh%C3%A1c-%C4%91%C3%A3-t%E1%BB%93n-t%E1%BA%A1i)
    - [Tôi muốn bỏ các thay đổi chưa commit trên local (đã stage và chưa stage)](#t%C3%B4i-mu%E1%BB%91n-b%E1%BB%8F-c%C3%A1c-thay-%C4%91%C3%B4i-ch%C6%B0a-commit-tr%C3%AAn-local-%C4%91%C3%A3-stage-v%C3%A0-ch%C6%B0a-stage)
    - [Tôi muốn loại bỏ các thay đổi chưa stage cụ thể](#t%C3%B4i-mu%E1%BB%91n-lo%E1%BA%A1i-b%E1%BB%8F-c%C3%A1c-thay-%C4%91%E1%BB%95i-ch%C6%B0a-stage-c%E1%BB%A5-th%E1%BB%83)
    - [Tôi muốn loại bỏ các file chưa stage cụ thể](#t%C3%B4i-mu%E1%BB%91n-lo%E1%BA%A1i-b%E1%BB%8F-c%C3%A1c-file-ch%C6%B0a-stage-c%E1%BB%A5-th%E1%BB%83)
    - [Tôi chỉ loại bỏ các thay đổi chưa stage trên local](#t%C3%B4i-ch%E1%BB%89-lo%E1%BA%A1i-b%E1%BB%8F-c%C3%A1c-thay-%C4%91%E1%BB%95i-ch%C6%B0a-stage-tr%C3%AAn-local)
    - [Tôi muốn loại bỏ tất cả các file chưa được theo dõi (track)](#t%C3%B4i-mu%E1%BB%91n-lo%E1%BA%A1i-b%E1%BB%8F-t%E1%BA%A5t-c%E1%BA%A3-c%C3%A1c-file-ch%C6%B0a-%C4%91%C6%B0%E1%BB%A3c-theo-d%C3%B5i-track)
    - [Tôi muốn untage một file cụ thể đã stage](#t%C3%B4i-mu%E1%BB%91n-untage-m%E1%BB%99t-file-c%E1%BB%A5-th%E1%BB%83-%C4%91%C3%A3-stage)
  - [Nhánh](#nh%C3%A1nh)
    - [Tôi muốn liệt kê tất cả các nhánh](#t%C3%B4i-mu%E1%BB%91n-li%E1%BB%87t-k%C3%AA-t%E1%BA%A5t-c%E1%BA%A3-c%C3%A1c-nh%C3%A1nh)
    - [Tạo một nhánh từ một commit](#t%E1%BA%A1o-m%E1%BB%99t-nh%C3%A1nh-t%E1%BB%AB-m%E1%BB%99t-commit)
    - [Tôi đã pull vào sai nhánh](#t%C3%B4i-%C4%91%C3%A3-pull-t%E1%BB%AB--v%C3%A0o-sai-nh%C3%A1nh)
    - [Tôi muốn loại bỏ các commit trên local đến nhánh của tôi giống như một nhánh trên server](#t%C3%B4i-mu%E1%BB%91n-lo%E1%BA%A1i-b%E1%BB%8F-c%C3%A1c-commit-tr%C3%AAn-local-%C4%91%E1%BB%83n-nh%C3%A1nh-c%E1%BB%A7a-t%C3%B4i-gi%E1%BB%91ng-nh%C6%B0-m%E1%BB%99t-nh%C3%A1nh-tr%C3%AAn-server)
    - [Tôi đã commit đến master thay vì một nhánh mới](#t%C3%B4i-%C4%91%C3%A3-commit-%C4%91%E1%BA%BFn-master-thay-v%C3%AC-m%E1%BB%99t-nh%C3%A1nh-m%E1%BB%9Bi)
    - [Tôi muốn giữ toàn bộ file từ một ref-ish khác](#t%C3%B4i-mu%E1%BB%91n-gi%E1%BB%AF-to%C3%A0n-b%E1%BB%99-file-t%E1%BB%AB-m%E1%BB%99t-ref-ish-kh%C3%A1c)
    - [Tôi đã thực hiện một số commit trên một nhánh duy nhất nó nên ở trên các nhánh khác nhau](#t%C3%B4i-%C4%91%C3%A3-th%E1%BB%B1c-hi%E1%BB%87n-m%E1%BB%99t-s%E1%BB%91-commit-tr%C3%AAn-m%E1%BB%99t-nh%C3%A1nh-duy-nh%E1%BA%A5t-n%C3%B3-n%C3%AAn-%E1%BB%9F-tr%C3%AAn-c%C3%A1c-nh%C3%A1nh-kh%C3%A1c-nhau)
    - [Tôi muốn xóa các nhánh local đã bị xóa luồng phía trước](#t%C3%B4i-mu%E1%BB%91n-x%C3%B3a-c%C3%A1c-nh%C3%A1nh-local-%C4%91%C3%A3-b%E1%BB%8B-x%C3%B3a-lu%E1%BB%93ng-ph%C3%ADa-tr%C6%B0%E1%BB%9Bc)
    - [Tôi vô tình xóa nhánh của tôi](#t%C3%B4i-v%C3%B4-t%C3%ACnh-x%C3%B3a-nh%C3%A1nh-c%E1%BB%A7a-t%C3%B4i)
    - [Tôi muốn xoá một nhánh](#t%C3%B4i-mu%E1%BB%91n-xo%C3%A1-m%E1%BB%99t-nh%C3%A1nh)
    - [Tôi muốn xoá nhiều nhánh](#t%C3%B4i-mu%E1%BB%91n-xo%C3%A1-nhi%E1%BB%81u-nh%C3%A1nh)
    - [Tôi muốn đổi tên một nhánh](#t%C3%B4i-mu%E1%BB%91n-%C4%91%E1%BB%95i-t%C3%AAn-m%E1%BB%99t-nh%C3%A1nh)
    - [Tôi muốn checkout đến một nhánh remote mà người khác đang làm việc trên đó](#t%C3%94i-mu%E1%BB%91n-checkout-%C4%91%E1%BA%BFn-m%E1%BB%99t-nh%C3%A1nh-remote-m%C3%A0-ng%C6%B0%E1%BB%9Di-kh%C3%A1c-%C4%91ang-l%C3%A0m-vi%E1%BB%87c-tr%C3%AAn-%C4%91%C3%B3)
    - [Tôi muốn tạo một nhánh remote mới từ một nhánh local hiện tại](#t%C3%B4i-mu%E1%BB%91n-t%E1%BA%A1o-m%E1%BB%99t-nh%C3%A1nh-remote-m%E1%BB%9Bi-t%E1%BB%AB-m%E1%BB%99t-nh%C3%A1nh-local-hi%E1%BB%87n-t%E1%BA%A1i)
    - [Tôi muốn thiết lập một nhánh remote giống như upstream cho một nhánh local](#t%C3%B4i-mu%E1%BB%91n-thi%E1%BA%BFt-l%E1%BA%ADp-m%E1%BB%99t-nh%C3%A1nh-remote-gi%E1%BB%91ng-nh%C6%B0-upstream-cho-m%E1%BB%99t-nh%C3%A1nh-local)
    - [Tôi muốn thiết lập HEAD của tôi để theo dõi nhánh remote mặc định](#t%C3%B4i-mu%E1%BB%91n-thi%E1%BA%BFt-l%E1%BA%ADp-head-c%E1%BB%A7a-t%C3%B4i-%C4%91%E1%BB%83-theo-d%C3%B5i-nh%C3%A1nh-remote-m%E1%BA%B7c-%C4%91%E1%BB%8Bnh)
    - [Tôi đã thực hiện thay đổi trên nhánh sai](#t%C3%B4i-%C4%91%C3%A3-th%E1%BB%B1c-hi%E1%BB%87n-thay-%C4%91%E1%BB%95i-tr%C3%AAn-nh%C3%A1nh-sai)
  - [Rebasing và Merging](#rebasing-v%C3%A0-merging)
    - [Tôi muốn huỷ bỏ rebase/merge](#t%C3%B4i-mu%E1%BB%91n-hu%E1%BB%B7-b%E1%BB%8F-rebasemerge)
    - [Tôi đã rebase, nhưng tôi không muốn force push](#t%C3%B4i-%C4%91%C3%A3-rebase-nh%C6%B0ng-t%C3%B4i-kh%C3%B4ng-mu%E1%BB%91n-force-push)
    - [Tôi cần kết hợp các commit](#t%C3%B4i-c%E1%BA%A7n-k%E1%BA%BFt-h%E1%BB%A3p-c%C3%A1c-commit)
      - [Chiến lược merge an toàn](#chi%E1%BA%BFn-l%C6%B0%E1%BB%A3c-merge-an-to%C3%A0n)
      - [Tôi cần merge một nhánh vào một commit duy nhất](#t%C3%B4i-c%E1%BA%A7n-merge-m%E1%BB%99t-nh%C3%A1nh-v%C3%A0o-m%E1%BB%99t-commit-duy-nh%E1%BA%A5t)
      - [Tôi chỉ muốn kết hợp các commit chưa push](#t%C3%B4i-ch%E1%BB%89-mu%E1%BB%91n-k%E1%BA%BFt-h%E1%BB%A3p-c%C3%A1c-commit-ch%C6%B0a-push)
      - [Tôi cần huỷ việc merge](#t%C3%B4i-c%E1%BA%A7n-hu%E1%BB%B7-vi%E1%BB%87c-merge)
    - [Tôi cần cập nhật commit cha của nhánh của tôi](#t%C3%B4i-c%E1%BA%A7n-c%E1%BA%ADp-nh%E1%BA%ADt-commit-cha-c%E1%BB%A7a-nh%C3%A1nh-c%E1%BB%A7a-t%C3%B4i)
    - [Kiểm tra xem tất cả commit trên một nhánh đã được merge](#ki%E1%BB%83m-tra-xem-t%E1%BA%A5t-c%E1%BA%A3-commit-tr%C3%AAn-m%E1%BB%99t-nh%C3%A1nh-%C4%91%C3%A3-%C4%91%C6%B0%E1%BB%A3c-merge)
    - [Các vấn đề có thể xảy ra với interactive rebases](#c%C3%A1c-v%E1%BA%A5n-%C4%91%E1%BB%81-c%C3%B3-th%E1%BB%83-x%E1%BA%A3y-ra-v%E1%BB%9Bi-interactive-rebases)
      - [Màn hình chỉnh sửa rebase cho biết 'noop'](#m%C3%A0n-h%C3%ACnh-ch%E1%BB%89nh-s%E1%BB%ADa-rebase-cho-bi%E1%BA%BFt-noop)
      - [Có một vài xung đột](#c%C3%B3-m%E1%BB%99t-v%C3%A0i-xung-%C4%91%E1%BB%99t)
  - [Stash](#stash)
    - [Stash tất cả chỉnh sửa](#stash-t%E1%BA%A5t-c%E1%BA%A3-ch%E1%BB%89nh-s%E1%BB%ADa)
    - [Stash các file cụ thể](#stash-c%C3%A1c-file-c%E1%BB%A5-th%E1%BB%83)
    - [Stash với message](#stash-v%E1%BB%9Bi-message)
    - [Apply một stash cụ thể từ danh sách](#apply-m%E1%BB%99t-stash-c%E1%BB%A5-th%E1%BB%83-t%E1%BB%AB-danh-s%C3%A1ch)
  - [Finding](#finding)
    - [Tôi muốn tìm một chuỗi trong bất kỳ commit nào](#t%C3%B4i-mu%E1%BB%91n-t%C3%ACm-m%E1%BB%99t-chu%E1%BB%97i-trong-b%E1%BA%A5t-k%E1%BB%B3-commit-n%C3%A0o)
    - [Tôi muốn tìm tác giả hoặc người commit](#t%C3%B4i-mu%E1%BB%91n-t%C3%ACm-t%C3%A1c-gi%C3%A1c-ho%E1%BA%B7c-ng%C6%B0%E1%BB%9Di-commit)
    - [Tôi muốn liệt kê các commit chứa các file cụ thể](#t%C3%B4i-mu%E1%BB%91n-li%E1%BB%87t-k%C3%AA-c%C3%A1c-commit-ch%E1%BB%A9a-c%C3%A1c-file-c%E1%BB%A5-th%E1%BB%83)
    - [Tìm một tag nơi một commit đã tham chiếu](#t%C3%ACm-m%E1%BB%99t-tag-n%C6%A1i-m%E1%BB%99t-commit-%C4%91%C3%A3-tham-chi%E1%BA%BFu)
  - [Submodules](#submodules)
    - [Clone tất cả submodules](#clone-t%E1%BA%A5t-c%E1%BA%A3-submodules)
    - [Xoá một submodule](#xo%C3%A1-m%E1%BB%99t-submodule)
  - [Miscellaneous Objects](#miscellaneous-objects)
    - [Khôi phục một file đã xoá](#kh%C3%B4i-ph%E1%BB%A5c-m%E1%BB%99t-file-%C4%91%C3%A3-xo%C3%A1)
    - [Xoá tag](#xo%C3%A1-tag)
    - [Khôi phục một tag đã xoá](#kh%C3%B4i-ph%E1%BB%A5c-m%E1%BB%99t-tag-%C4%91%C3%A3-xo%C3%A1)
    - [Deleted Patch](#deleted-patch)
    - [Xuất một repository ra một file Zip](#xu%E1%BA%A5t-m%E1%BB%99t-repository-ra-m%E1%BB%99t-file-zip)
    - [Push một nhánh và một tag có tên giống nhau](#push-m%E1%BB%99t-nh%C3%A1nh-v%C3%A0-m%E1%BB%99t-tag-c%C3%B3-t%C3%AAn-gi%E1%BB%91ng-nhau)
  - [Tracking các file](#tracking-c%C3%A1c-file)
    - [Tôi muốn thay đổi cách viết hoa của tên tệp mà không thay đổi nội dung của tệp](#t%C3%B4i-mu%E1%BB%91n-thay-%C4%91%E1%BB%95i-c%C3%A1ch-vi%E1%BA%BFt-hoa-c%E1%BB%A7a-t%C3%AAn-t%E1%BB%87p-m%C3%A0-kh%C3%B4ng-thay-%C4%91%E1%BB%95i-n%E1%BB%99i-dung-c%E1%BB%A7a-t%E1%BB%87p)
    - [Tôi muốn ghi đè lên các tệp local khi thực hiện lệnh git pull](#t%C3%B4i-mu%E1%BB%91n-ghi-%C4%91%C3%A8-l%C3%AAn-c%C3%A1c-t%E1%BB%87p-local-khi-th%E1%BB%B1c-hi%E1%BB%87n-l%E1%BB%87nh-git-pull)
    - [Tôi muốn xóa một tệp khỏi Git nhưng vẫn giữ tệp](#t%C3%B4i-mu%E1%BB%91n-x%C3%B3a-m%E1%BB%99t-t%E1%BB%87p-kh%E1%BB%8Fi-git-nh%C6%B0ng-v%E1%BA%ABn-gi%E1%BB%AF-t%E1%BB%87p)
    - [Tôi muốn revert tệp về bản sửa đổi cụ thể](#t%C3%B4i-mu%E1%BB%91n-revert-t%E1%BB%87p-v%E1%BB%81-b%E1%BA%A3n-s%E1%BB%ADa-%C4%91%E1%BB%95i-c%E1%BB%A5-th%E1%BB%83)
    - [Tôi muốn liệt kê các thay đổi của một tệp cụ thể giữa các commit hoặc các nhánh](#t%C3%B4i-mu%E1%BB%91n-li%E1%BB%87t-k%C3%AA-c%C3%A1c-thay-%C4%91%E1%BB%95i-c%E1%BB%A7a-m%E1%BB%99t-t%E1%BB%87p-c%E1%BB%A5-th%E1%BB%83-gi%E1%BB%AFa-c%C3%A1c-commit-ho%E1%BA%B7c-c%C3%A1c-nh%C3%A1nh)
    - [Tôi muốn Git bỏ qua những thay đổi đối với một tệp cụ thể](#t%C3%B4i-mu%E1%BB%91n-git-b%E1%BB%8F-qua-nh%E1%BB%AFng-thay-%C4%91%E1%BB%95i-%C4%91%E1%BB%91i-v%E1%BB%9Bi-m%E1%BB%99t-t%E1%BB%87p-c%E1%BB%A5-th%E1%BB%83)
  - [Cấu hình](#c%E1%BA%A5u-h%C3%ACnh)
    - [Tôi muốn thêm bí danh cho một số lệnh Git](#t%C3%B4i-mu%E1%BB%91n-th%C3%AAm-b%C3%AD-danh-cho-m%E1%BB%99t-s%E1%BB%91-l%E1%BB%87nh-git)
    - [Tôi muốn thêm một thư mục trống vào repository của tôi](#t%C3%B4i-mu%E1%BB%91n-th%C3%AAm-m%E1%BB%99t-th%C6%B0-m%E1%BB%A5c-tr%E1%BB%91ng-v%C3%A0o-repository-c%E1%BB%A7a-t%C3%B4i)
    - [Tôi muốn cache một username và password cho một repository](#t%C3%B4i-mu%E1%BB%91n-cache-m%E1%BB%99t-username-v%C3%A0-password-cho-m%E1%BB%99t-repository)
    - [Tôi muốn làm cho Git bỏ qua các quyền và thay đổi về filemode](#t%C3%B4i-mu%E1%BB%91n-l%C3%A0m-cho-git-b%E1%BB%8F-qua-c%C3%A1c-quy%E1%BB%81n-v%C3%A0-thay-%C4%91%E1%BB%95i-v%E1%BB%81-filemode)
    - [Tôi muốn đặt user toàn cục](#t%C3%B4i-mu%E1%BB%91n-%C4%91%E1%BA%B7t-user-to%C3%A0n-c%E1%BB%A5c)
    - [Tôi muốn thêm màu cho command Git](#t%C3%B4i-mu%E1%BB%91n-th%C3%AAm-m%C3%A0u-cho-command-git)
  - [Tôi không nghĩ mình đã làm gì sai](#t%C3%B4i-kh%C3%B4ng-ngh%C4%A9-m%C3%ACnh-%C4%91%C3%A3-l%C3%A0m-g%C3%AC-sai)
- [Tài nguyên khác](#t%C3%A0i-nguy%C3%AAn-kh%C3%A1c)
  - [Sách](#s%C3%A1ch)
  - [Hướng dẫn](#h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn)
  - [Scripts và các công cụ](#scripts-v%C3%A0-c%C3%A1c-c%C3%B4ng-c%E1%BB%A5)
  - [GUI Clients](#gui-clients)


## Repositories

### Tôi muốn bắt đầu một repository cục bộ

Để khởi tạo cho một Git repository trên thư mục đã tồn tại:

```sh
(my-folder) $ git init
```

### Tôi muốn clone một remote repository

Để clone (copy) một remote repository, copy đường dẫn url cho repository, và chạy:

```sh
$ git clone [url]
```

Việc này sẽ lưu lại nó vào một thư mục có tên như tên của remote repository. Hãy chắc chắn rằng bạn có kết nối đến remote server khi bạn đang clone về (hầu hết các các việc này cần đảm bảo bạn được kết nối với internet).

Để clone vào một thư mực với tên khác với tên mặc định của repository:

```sh
$ git clone [url] name-of-new-folder
```

## Chỉnh sửa Commit
### Bạn vừa commit điều gì ?

Giả sử bạn vừa commit thay đổi một cách mù quáng với lệnh `git commit -a` và bạn không chắc chắn nội dung thực sự là của commit vừa thực hiện. Bạn có thể hiển thị ra commit gần nhất trên con trỏ HEAD hiện tại của bạn với lệnh:

```sh
(master)$ git show
```

Hoặc

```sh
$ git log -n1 -p
```

Nếu bạn muốn xem một file tại một commit cụ thể, bạn có thể cũng có thể làm được điều này ( `<commitid>`  là commit mà bạn quan tâm):

```sh
$ git show <commitid>:filename
```

### Tôi đã viết sai vài thứ trong message của commit

Nếu bạn đã viết sai thứ gì đó và commit chưa được push lên, bạn có thể làm theo cách sau để thay đổi message của commit mà không làm thay đổi commit:

```sh
$ git commit --amend --only
```
Câu lệnh đó sẽ mở trình soạn thảo mặc định của bạn, nơi bạn có thể chỉnh sửa message. Ngoài ra, bạn có thể làm tất cả điều này với một câu lệnh sau:

```sh
$ git commit --amend --only -m 'xxxxxxx'
```

Nếu bạn đã đẩy message lên, bạn có thể chỉnh sửa commit và force push, nhưng điều đó không được khuyến khích.

### Tôi đã commit với tên và email cấu hình sai

Nếu đó là một commit độc lập, chỉnh sửa nó:

```sh
$ git commit --amend --no-edit --author "Tên tác giả mới <authoremail@mydomain.com>"
```

Một cách khác để cấu hình đúng tác giả là cài đặt trong `git config --global author.(name|email)` và sau đó sử dụng

```sh
$ git commit --amend --reset-author --no-edit
```

Nếu bạn cần thay đổi tất cả lịch sử, hãy xem trang đó với `git filter-branch`.

### Tôi muốn xoá một file từ commit trước

Để xoá các thay đổi đối với một file khỏi commit trước đó, hãy làm như sau:

```sh
$ git checkout HEAD^ myfile
$ git add myfile
$ git commit --amend --no-edit
```

Trong trường hợp file mới được thêm vào commit và bạn muốn xoá nó (từ Git), hãy thực hiện:

```sh
$ git rm --cached myfile
$ git commit --amend --no-edit
```

Điều này đăc biệt hữu ích khi bạn có một bản patch mở và bạn đã commit một file không cần thiết và force push để cập nhật bản patch trên remote. Tuỳ chọn `--no-edit` được sử dụng để giữ message cho commit hiện tại.

### Tôi muốn xoá hoặc loại bỏ commit cuối cùng của tôi

Nếu bạn muốn xoá các commit đã push, bạn có thể dụng cách sau. Tuy nhiên, nó sẽ không thể phục hồi thay đổi của lịch sử, và làm hỏng lịch sử của bất kỳ ai khác đã pull từ repository. Tóm lại, nếu bạn không chắc chắn, bạn không nên làm điều này.

```sh
$ git reset HEAD^ --hard
$ git push --force-with-lease [remote] [branch]
```

Nếu bạn chưa push, để reset Git về trạng thái trước khi bạn thực hiện commit cuối (trong khi vẫn giữ các thay đổi của giai đoạn) sử dụng:

```
(my-branch*)$ git reset --soft HEAD@{1}

```

Điều này chỉ hoạt động nếu bạn chưa push. Nếu bạn đã push, điều thực sự an toàn nhất cần làm là `git revert SHAofBadCommit`. Điều đó sẽ tạo một commit mới để quay trở lại thay đổi của commit trước đó. Hoặc nếu nhánh bạn đã push là rebase-safe (các dev khác không định pull từ nó về), bạn chỉ có thể sử dụng `git push --force-with-lease`. Để biết thêm, hãy xem [phần trên](#deleteremove-last-pushed-commit).

### Xoá/loại bỏ commit tuỳ ý

Lưu ý như trên. Không bao giờ làm điều này nếu có thể.

```sh
$ git rebase --onto SHA1_OF_BAD_COMMIT^ SHA1_OF_BAD_COMMIT
$ git push --force-with-lease [remote] [branch]
```

Hoặc thực hiện một [interactive rebase](#interactive-rebase) và loại bỏ các dòng tương ứng cho các commit bạn muốn loại bỏ.

### Tôi đã cố gắng push commit đã sửa đổi lên remote, nhưng tôi gặp một thông báo lỗi

```sh
To https://github.com/yourusername/repo.git
! [rejected]        mybranch -> mybranch (non-fast-forward)
error: failed to push some refs to 'https://github.com/tanay1337/webmaker.org.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Lưu ý rằng, như với rebase (xem bên dưới), sử đổi thay thể commit cũ với một commit mới, nên bạn phải force push (`--force-with-lease`) thay đổi của bạn nếu bạn đã push commit đã sửa đổi trược lên remote của bạn. Hãy cẩn thận khi bạn làm điều này &ndash; *luôn luôn* đảm bảo rằng bạn đã chỉ định một nhánh!

```sh
(my-branch)$ git push origin mybranch --force-with-lease
```

Nói chung, **tránh force push**. Tốt nhất là tạo và push một commit mới thay vì force-push commit đã sửa đổi vì nó sẽ gây xung đột trong lịch sử của resource cho bất kỳ developer nào người mà đã tương tác với nhánh được đề cập hoặc bất kỳ nhánh con nào. `--force-with-lease` sẽ vẫn thất bại, nếu ai đó cũng đang làm việc trên cùng một nhánh với bạn, và việc push lên sẽ ghi đè lên những thay đổi đó.

Nếu bạn hoàn toàn chắc chắn rằng không ai đang làm việc trên cùng một nhánh hoặc bạn muốn cập nhật phần đầu của một nhánh *vô điều kiện*, bạn có thể sử dụng `--force` (`-f`), nhưng điều này nói chung nên tránh.

### Tôi đã vô tình thực hiện hard reset, và tôi muốn các thay đổi của tôi trở lại trước đó.

Nếu vô tình bạn thực hiện `git reset --hard`, bạn có thể vẫn nhận được commit trước của bạn, vì git giữ một bản log cho tất cả mọi thứ trong 1 vài ngày.

Chú ý: Điều này chỉ hợp lệ nếu luồng làm việc của bạn đã được sao lưu, tức là được commit hoặc được stash. `git reset --hard` _sẽ loại bỏ_ các thay đổi không được commit, vì vậy hãy sử dụng nó một cách thận trọng. (Một tuỳ chọn an toàn là `git reset --keep`.)

```sh
(master)$ git reflog
```

Bạn sẽ thấy danh sách các commit gần đây, và một commit cho reset. Chọn SHA của commit và muốn quay trở lại và reset lại:

```sh
(master)$ git reset --hard SHA1234
```

Và sẽ tốt hơn nếu bạn tiếp tục thực hiện tiếp

### Tôi vô tình commit và đẩy lên một merge

Nếu bạn vô tình merge một nhánh tính năng vào nhánh phát triển chính trước khi nó sẵn sàng để merge, bạn vẫn có thể undo merge. Nhưng có một điểm phải nắm được: Một commit merge có một hoặc nhiều hơn một parent (thường là 2).

Câu lệnh để sử dụng:
```sh
(feature-branch)$ git revert -m 1 <commit>
```
Chỗ tuỳ chọn -m 1 cho biết số parent là 1 (nhánh mà merge được thực hiện) làm parent để revert.

Chú ý: Số parent không phải là số để xác định commit. Thay vào đó, một commit merge có một dòng `Merge: 8e2ce2d 86ac2e7`. Số parent là một index trên 1 của một parent trên dòng này, số nhận dạng đầu tiên là 1, thứ 2 là số 2 và tiếp tục.

### Tôi vô tình commit và đẩy các file chứa các dữ liệu nhạy cảm

Nếu bạn vô tình push lên các file chứa dữ liệu nhạy cảm (passwords, keys, etc.), bạn có thể sửa đổi commit trước.. Lưu ý rằng khi bạn đã đẩy một commit, bạn nên xem xét bất kỳ dữ liệu nào nó chứa dữ liệu để bị xâm nhập. Các bước này có thể xoá dữ liệu nhạy cảm từ repo public hoặc bản sao cục bộ của repository, nhưng bạn không thể xóa dữ liệu nhạy cảm khỏi các bản sao được kéo về của người khác. Nếu bạn đã commit mật khẩu, hãy thay đổi mật khẩu ngay lập tức. Nếu bạn đã commit một key, hãy tạo lại key đó ngay lập tức. Việc sửa đổi commit đã đẩy là không đủ, vì bất kỳ ai cũng có thể đã pull commit ban đầu chứa dữ liệu nhạy cảm của bạn trong thời gian chờ đợi. Việc sửa đổi commit đã đẩy là không đủ, vì bất kỳ ai cũng có thể đã kéo cam kết ban đầu chứa dữ liệu nhạy cảm của bạn trong thời gian chờ đợi.

Nếu bạn chỉnh sửa tệp và xóa dữ liệu nhạy cảm, hãy chạy
```sh
(feature-branch)$ git add edited_file
(feature-branch)$ git commit --amend --no-edit
(feature-branch)$ git push --force-with-lease origin [branch]
```

Nếu bạn muốn xóa toàn bộ tệp (nhưng giữ nguyên tệp cục bộ), hãy chạy
```sh
(feature-branch)$ git rm --cached sensitive_file
echo sensitive_file >> .gitignore
(feature-branch)$ git add .gitignore
(feature-branch)$ git commit --amend --no-edit
(feature-branch)$ git push --force-with-lease origin [branch]
```
Ngoài ra, lưu trữ dữ liệu nhạy cảm của bạn trong các biến môi trường cục bộ.

Nếu bạn muốn xóa hoàn toàn toàn bộ tệp (và không giữ nguyên tệp cục bộ), sau đó chạy

```sh
(feature-branch)$ git rm sensitive_file
(feature-branch)$ git commit --amend --no-edit
(feature-branch)$ git push --force-with-lease origin [branch]
```

Nếu bạn đã thực hiện các commit khác trong thời gian chờ đợi (tức là dữ liệu nhạy cảm nằm trong commit trước commit trước đó), bạn sẽ phải rebase.

## Stagging

### Tôi cần thêm các thay đổi đã stage cho commit trước đó

```sh
(my-branch*)$ git commit --amend
```

Nếu bạn đã biết bạn không muốn thay đổi message commit, bạn có thể yêu cầu git sử dụng lại commit message:

```sh
(my-branch*)$ git commit --amend -C HEAD
```

### Tôi muốn stage một phần của một file mới, nhưng không phải toàn bộ file

Thông thường, nếu bạn muốn stage một phần của một file, bạn chạy lệnh này:

```sh
$ git add --patch filename.x
```

`-p` sẽ hoạt động trong ngắn hạn. Việc này sẽ mở chế độ tương tác. Bạn sẽ có thể sử dụng tuỳ chọn `s` để cắt commit - tuy nhiên, nếu là file mới, bạn sẽ không có tuỳ chọn này. Để thêm một file mới, làm như sau:

```sh
$ git add -N filename.x
```

Sau đó, bạn sẽ cần sử dụng tuỳ chọn `e` để dùng cách thủ công thêm dòng. Đang chạy `git diff --cached` hoặc
`git diff --staged` sẽ cho bạn thấy những dòng bạn đã stage so với những dòng vẫn lưu ở cục bộ.

### Tôi muốn thêm các thay đổi trong một file vào 2 commit khác nhau

`git add` sẽ thêm toàn bộ file vào một commit. `git add -p` sẽ cho phép tương tác chọn những thay đổi bạn muốn thêm.

### Tôi muốn stage các chỉnh sửa chưa được stage, và unstage các chỉnh sửa đã stage

Điều này là khó khăn. Cách tốt nhất là bạn nên stash các chỉnh sửa chưa stage. Sau đó, reset. Sau đó, hãy bật lại các chỉnh sửa đã được lưu trữ của bạn và thêm lại chúng.

```sh
$ git stash -k
$ git reset --hard
$ git stash pop
$ git add -A
```

## Chỉnh sửa Unstaged
### Tôi muốn di chuyển các chỉnh sửa Unstaged đến một nhánh mới

```sh
$ git checkout -b my-branch
```

### Tôi muốn di chuyển các chỉnh sửa unstaged của tôi đến một nhánh khác đã tồn tại

```sh
$ git stash
$ git checkout my-branch
$ git stash pop
```

### Tôi muốn bỏ các thay đổi chưa commit trên local (staged and unstaged)

Nếu bạn muốn bỏ tất cả các thay đổi đã stage và chưa stage trên local của bạn, bạn có thể làm như thế này:

```sh
(my-branch)$ git reset --hard
# or
(master)$ git checkout -f
```

Nó sẽ unstage tất cả các file bạn đã stage với `git add`:

```sh
$ git reset
```

Nó sẽ revert tất cả các thay đổi chưa commit trên local (nên thực hiện trong thư mục gốc repo):

```sh
$ git checkout .
```

Bạn cũng có thể revert các thay đổi chưa commit đối với một file hoặc một thư mục cụ thể:

```sh
$ git checkout [some_dir|file.txt]
```

Tuy nhiên, một cách khác để revert tất cả các thay đổi chưa commit (dài hơn để nhập, những hoạt động từ bất kỳ thư mục con nào):

```sh
$ git reset --hard HEAD
```

Thao tác này sẽ xoá tất cả các file chưa theo dõi(untrack) trên local, do đó, chỉ các file đã theo dõi (tracked) được git giữ:

```sh
$ git clean -fd
```

`-x` cũng sẽ xoá tất cả các file đã ignore.

### Tôi muốn loại bỏ các thay đổi unstaged cụ thể

Khi bạn muốn loại bỏ một số, nhưng không phải tất cả các thay đổi trong bản sao làm việc của bạn.

Checkout các thay đổi không mong muốn, giữa các thay đổi tốt.

```sh
$ git checkout -p
# Trả lời y cho tất cả các đoạn trích bạn muốn thả
```

Một cách khác liên quan đến việc sử dụng `stash`. Stash tất cả các thay đổi tốt, reset bản sao làm việc và chấp nhận lại các thay đổi tốt.

```sh
$ git stash -p
# Select all of the snippets you want to save
$ git reset --hard
$ git stash pop
```

Ngoài ra, stash những thay đổi không mong muốn của bạn và sau đó drop stash.

```sh
$ git stash -p
# Select all of the snippets you don't want to save
$ git stash drop
```

### Tôi muốn loại bỏ các file unstaged cụ thể

Khi bạn muốn loại bỏ một file cụ thể trong bản sao đang làm việc của bạn.

```sh
$ git checkout myFile
```

Ngoài ra, dể loại bỏ nhiều file trong bản sao làm việc của bạn, hãy liệt kê tất cả chúng.

```sh
$ git checkout myFirstFile mySecondFile
```

### Tôi chỉ loại bỏ các thay đổi unstaged cục bộ

Khi bạn muốn loại bỏ tất cả các thay đổi chưa commit mà unstaged  cục bộ

```sh
$ git checkout .
```
### Tôi muốn loại bỏ tất cả các file chưa được theo dõi (track)

Khi bạn muốn loại bỏ tất cả các file chưa được theo dõi

```sh
$ git clean -f
```

### Tôi muốn unstaged một file cụ thể đã stage

Đôi khi, chúng ta có một hoặc nhiều file đã vô tình bị kết thúc và các file này chưa được commit trước đó. Để unstage chúng:

```sh
$ git reset -- <filename>
```

Điều này dẫn đến việc các file đang unstaged và làm cho nó giống như chưa được theo dõi.

## Nhánh

### Tôi muốn liệt kê tất cả các nhánh

Liệt kê các nhánh trên local

```sh
$ git branch
```

Liệt kê các nhánh trên remote

```sh
$ git branch -r
```

Liệt kê tất cả các nhánh (cả local và remote)

```sh
$ git branch -a
```

### Tạo một nhánh từ một commit
```sh
$ git checkout -b <branch> <SHA1_OF_COMMIT>
```

### Tôi đã pull vào sai nhánh

Đây là một cơ hội khác để sử dụng `git reflog` để xem nơi con trỏ HEAD đã trỏ trước khi pull sai.

```sh
(master)$ git reflog
ab7555f HEAD@{0}: pull origin wrong-branch: Fast-forward
c5bc55a HEAD@{1}: checkout: checkout message goes here
```

Chỉ cần đặt lại nhánh trước đó của bạn để commit theo mong muốn:

```sh
$ git reset --hard c5bc55a
```

Xong.

### Tôi muốn loại bỏ các commit trên local đển nhánh của tôi giống như một nhánh trên server

Xác nhận rằng bạn chưa push các thay đổi của mình đến server.

`git status` sẽ hiển thị số lượng các commit bạn đang ở phía trước của origin:

```sh
(my-branch)$ git status
# On branch my-branch
# Your branch is ahead of 'origin/my-branch' by 2 commits.
#   (use "git push" to publish your local commits)
#
```

Một cách khác để reset lại phù hợp với origin (để có các nhánh giống như trên remote) là thực hiện điều này:

```sh
(master)$ git reset --hard origin/my-branch
```

### Tôi đã commit đến master thay vì một nhánh mới

Tạo nhánh mới trong khi giữ master:

```sh
(master)$ git branch my-branch
```

Reset nhánh master đến commit trước đó:

```sh
(master)$ git reset --hard HEAD^
```

`HEAD^` là viết tắt của `HEAD^1`. Điều này là viết tắt của parent  `HEAD`, tương tự `HEAD^2` là viết tắt của parent thứ hai của commit (merge có thể có 2 parent).

Chú ý rằng `HEAD^2`  **không** giống như `HEAD~2` (xem [link này](http://www.paulboxley.com/blog/2011/06/git-caret-and-tilde) để có thêm thông tin).

Ngoài ra, nếu bạn không muốn sử dụng `HEAD^`, tìm mã hash của commit để thiết lập nhánh master của bạn (`git log` là một thủ thuật). Sau đó đặt lại mã hash. `git push` sẽ đảm bảo rằng thay đổi này được thể hiển trên remote của bạn.

Ví dụ, nếu hash của commit mà nhánh master của bạn được cho là  `a13b85e`:

```sh
(master)$ git reset --hard a13b85e
HEAD is now at a13b85e
```

Checkout một nhánh mới để tiếp tục làm việc:

```sh
(master)$ git checkout my-branch
```

### Tôi muốn giữ toàn bộ file từ một ref-ish khác

Giả sử bạn có tăng đột biến mức độ làm việc (xem lưu ý), với hàng trăm thay đổi. Mọi thứ đang hoạt động. Bây giờ, bạn commit vào một nhánh khác để lưu công việc đó:

```sh
(solution)$ git add -A && git commit -m "Adding all changes from this spike into one big commit."
```

Khi bạn muốn đặt nó vào một nhánh (có thể feature, có thể `develop`), bạn quan tâm đến việc giữ toàn bộ file. Bạn muốn chia commit lớn của bạn thành những cái nhỏ hơn.

Giả sử bạn có:

  * nhánh `solution`, với solution để tăng đột biến của bạn. Phía trước `develop`.
  * nhánh `develop`, nơi bạn muốn thêm các thay đổi của bạn.

Bạn có thể giải quyết nó mang nội dung đến nhánh của bạn:

```sh
(develop)$ git checkout solution -- file1.txt
```

Điều này sẽ lấy nội dung của tập tin đó trong nhánh `solution` đến nhánh `develop` của bạn:

```sh
# On branch develop
# Your branch is up-to-date with 'origin/develop'.
# Changes to be committed:
#  (use "git reset HEAD <file>..." to unstage)
#
#        modified:   file1.txt
```

Sau đó, commit như bình thường.

Lưu ý: Các giải pháp tăng đột biến được thực hiện để phân tích hoặc giải quyết vấn đề. Các giải pháp này được sử dụng để ước tính và loại bỏ sau khi mọi người hiểu rõ vấn đề. ~ [Wikipedia](https://en.wikipedia.org/wiki/Extreme_programming_practices).

### Tôi đã thực hiện một số commit trên một nhánh duy nhất nó nên ở trên các nhánh khác nhau

Giả sử bạn đang ở trên nhánh master của bạn. Chạy `git log`, bạn thấy bạn đã thực hiện 2 commit:

```sh
(master)$ git log

commit e3851e817c451cc36f2e6f3049db528415e3c114
Author: Alex Lee <alexlee@example.com>
Date:   Tue Jul 22 15:39:27 2014 -0400

    Bug #21 - Added CSRF protection

commit 5ea51731d150f7ddc4a365437931cd8be3bf3131
Author: Alex Lee <alexlee@example.com>
Date:   Tue Jul 22 15:39:12 2014 -0400

    Bug #14 - Fixed spacing on title

commit a13b85e984171c6e2a1729bb061994525f626d14
Author: Aki Rose <akirose@example.com>
Date:   Tue Jul 21 01:12:48 2014 -0400

    First commit
```

Hãy lưu ý các hash commit của chúng ta cho mỗi lỗi (`e3851e8` cho #21, `5ea5173` cho #14).

Trước tiên, hãy đặt lại nhánh master của chúng ta về commit chính xác (`a13b85e`):

```sh
(master)$ git reset --hard a13b85e
HEAD is now at a13b85e
```

Bây giờ, chúng ta có thể tạo ra một nhánh mới cho lỗi của chúng ta #21:

```sh
(master)$ git checkout -b 21
(21)$
```

Bây giờ, hãy *cherry-pick* commit cho bug #21 trên dầu của nhánh. Điều này có ý nghĩa là chúng ta sẽ áp dụng commit đó, và chỉ commit đó, trực tiếp trên đầu của bất cứ head nào của chúng ta.

```sh
(21)$ git cherry-pick e3851e8
```

Tại thời điểm này, có khả năng có thể có xung đột. Hãy xem phần [**There were conflicts**](#merge-conflict) trong [phầnn interactive rebasing trên](#interactive-rebase) để làm thế nào giải quyết xung đột.

Bây giờ chúng ta hãy tạo một nhánh mới cho bug # 14, cũng dựa trên master

```sh
(21)$ git checkout master
(master)$ git checkout -b 14
(14)$
```

Và cuối cùng, hãy cherry-pick commit cho bug #14:

```sh
(14)$ git cherry-pick 5ea5173
```

<a name="delete-stale-local-branches"></a>
### Tôi muốn xóa các nhánh local đã bị xóa luồng phía trước

Khi bạn kết hợp một request pull trên GitHub, nó sẽ cho bạn tùy chọn để xóa nhánh đã merge trong fork của bạn. Nếu bạn không có kế hoạch tiếp tục làm việc trên nhánh, nó sạch hơn nếu xóa các bản sao local của nhánh, do đó bạn không kết thúc lộn xộn lên checkout luồng làm việc của bạn với rất nhiều nhánh cũ.

```sh
$ git fetch -p upstream
```

nơi, `upstream` là remote bạn muốn fetch từ đó.

### Tôi vô tình xóa nhánh của tôi

Nếu bạn thường xuyên push lên remote, bạn sẽ an toàn phần lớn thời gian. Nhưng đôi khi bạn có thể sẽ xóa các nhánh của bạn. Giả sử chúng ta tạo một nhánh và tạo một tệp mới:

```sh
(master)$ git checkout -b my-branch
(my-branch)$ git branch
(my-branch)$ touch foo.txt
(my-branch)$ ls
README.md foo.txt
```

Hãy thêm nó và commit.

```sh
(my-branch)$ git add .
(my-branch)$ git commit -m 'foo.txt added'
(my-branch)$ foo.txt added
 1 files changed, 1 insertions(+)
 create mode 100644 foo.txt
(my-branch)$ git log

commit 4e3cd85a670ced7cc17a2b5d8d3d809ac88d5012
Author: siemiatj <siemiatj@example.com>
Date:   Wed Jul 30 00:34:10 2014 +0200

    foo.txt added

commit 69204cdf0acbab201619d95ad8295928e7f411d5
Author: Kate Hudson <katehudson@example.com>
Date:   Tue Jul 29 13:14:46 2014 -0400

    Fixes #6: Force pushing after amending commits
```

Bây giờ chúng ta đang chuyển về master và 'vô tình' xóa nhánh của chúng ta

```sh
(my-branch)$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
(master)$ git branch -D my-branch
Deleted branch my-branch (was 4e3cd85).
(master)$ echo oh noes, deleted my branch!
oh noes, deleted my branch!
```

Tại thời điểm này, bạn nên làm quen với 'reflog', một logger được nâng cấp. Nó lưu trữ lịch sử của tất cả các hành động trong repo.

```
(master)$ git reflog
69204cd HEAD@{0}: checkout: moving from my-branch to master
4e3cd85 HEAD@{1}: commit: foo.txt added
69204cd HEAD@{2}: checkout: moving from master to my-branch
```

Như bạn có thể thấy chúng ta đã có commit hash từ nhánh đã xóa của chúng tôi. Hãy xem liệu chúng ta có thể khôi phục nhánh đã xóa của chúng ta hay không.

```sh
(master)$ git checkout -b my-branch-help
Switched to a new branch 'my-branch-help'
(my-branch-help)$ git reset --hard 4e3cd85
HEAD is now at 4e3cd85 foo.txt added
(my-branch-help)$ ls
README.md foo.txt
```

Và đấy! Chúng ta đã xoá file trước của chúng ta. `git reflog` cũng hữu ích khi rebase đi sai lầm lớn.

### Tôi muốn xoá một nhánh

Để xoá một nhánh remote:

```sh
(master)$ git push origin --delete my-branch
```

Bạn cũng có thể làm :

```sh
(master)$ git push origin :my-branch
```

Để xoá nhánh local:

```sh
(master)$ git branch -d my-branch
```

Để xoá một nhánh local *không được* merge đến nhánh hiện tại hoặc một upstream:

```sh
(master)$ git branch -D my-branch
```

### Tôi muốn xoá nhiều nhánh

Giả sử bạn muốn xoá tất cả nhánh bắt đầu với `fix/`:

```sh
(master)$ git branch | grep 'fix/' | xargs git branch -d
```

### Tôi muốn đổi tên một nhánh

Để đổi tên nhánh local hiện tại:

```sh
(master)$ git branch -m new-name
```

Để đổi tên nhánh local khác:

```sh
(master)$ git branch -m old-name new-name
```

### TÔi muốn checkout đến một nhánh remote mà người khác đang làm việc trên đó

Đầu tiên, fetch tất cả nhánh từ remote:

```sh
(master)$ git fetch --all
```

Giả sử bạn muốn checkout sang `daves` từ remote.

```sh
(master)$ git checkout --track origin/daves
Branch daves set up to track remote branch daves from origin.
Switched to a new branch 'daves'
```

(`--track` là viết tắt của `git checkout -b [branch] [remotename]/[branch]`)

Điều này sẽ cung cấp cho bạn một bản sao cục bộ của nhánh `daves`, và mọi cập nhật đã được push cũng sẽ được hiển thị từ remote.

### Tôi muốn tạo một nhánh remote mới từ một nhánh local hiện tại

```sh
$ git push <remote> HEAD
```

Nếu bạn cũng muốn đặt nhánh từ remote như upstream cho nhánh hiện tại, sử dụng:

```sh
$ git push -u <remote> HEAD
```

Với chế độ `upstream` và `simple` (mặc định trong Git 2.0) của cấu hình `push.default`, command sau sẽ push nhánh hiện tại liên quan đến nhánh remote được đăng ký trước đó với `-u`:

```sh
$ git push
```

Các hành động khác của chế độ `git push` được mô tả trong [doc of `push.default`](https://git-scm.com/docs/git-config#git-config-pushdefault).

### Tôi muốn thiết lập một nhánh remote giống như upstream cho một nhánh local

Bạn có thể thiết lập một nhánh remote như upstream cho nhánh local hiện tại bằng cách sử dụng:

```sh
$ git branch --set-upstream-to [remotename]/[branch]
# or, using the shorthand:
$ git branch -u [remotename]/[branch]
```

Để thiết lập nhánh upstream remote cho nhánh local khác:

```sh
$ git branch -u [remotename]/[branch] [local-branch]
```

### Tôi muốn thiết lập HEAD của tôi để theo dõi nhánh remote mặc định

Bằng cách kiểm tra các nhánh remote của bạn, bạn có thể thấy rằng các nhánh remote mà HEAD của bạn đang theo dõi. Trong một số trường hợp, đây không phải là nhánh mong muốn.

```sh
$ git branch -r
  origin/HEAD -> origin/gh-pages
  origin/master
```

Để thay đổi  `origin/HEAD` để theo dõi `origin/master`, bạn có thể chạy command này:

```sh
$ git remote set-head origin --auto
origin/HEAD set to master
```

### Tôi đã thực hiện thay đổi trên nhánh sai

Bạn đã thực hiện các thay đổi chưa được commit và nhận ra bạn đang ở nhánh sai. Stash các thay đổi và apply chúng vào nhánh bạn muốn:

```sh
(wrong_branch)$ git stash
(wrong_branch)$ git checkout <correct_branch>
(correct_branch)$ git stash apply
```

## Rebasing và Merging

### Tôi muốn huỷ bỏ rebase/merge

Bạn có thể đã merge hoặc rebase nhánh hiện tại của bạn với một nhánh sai hoặc bạn không thể tìm ra cách hoàn thành quá trình rebase/merge. Git lưu con trỏ original HEAD trong một biến được gọi là ORIG_HEAD trước khi làm các hành động nguy hiểm, vì vậy nó giống như một nhánh khôi phục ở một trạng thái trước khi rebase/merge.

```sh
(my-branch)$ git reset --hard ORIG_HEAD
```

### Tôi đã rebase, nhưng tôi không muốn force push

Thật không may, bạn phải bắt buộc push, nếu bạn muốn những thay đổi đó được ánh xạ trên nhánh remote. Điều này là do bạn đã thay đổi lịch sử. Nhánh remote sẽ không chấp nhận thay đổi trừ khi bạn ép buộc. Đây là một trong những lý do chính khiến nhiều người sử dụng một luồng merge, thay vì một luồng rebasing - các nhóm lớn có thể gặp rắc rối với các developer bắt buộc push. Sử dụng điều này một cách thận trọng. Một cách an toàn hơn để sử dụng rebase không phải là để ánh xạ các thay đổi của bạn trên nhánh remte, và thay vào đó thực hiện các thao tác sau:

```sh
(master)$ git checkout my-branch
(my-branch)$ git rebase -i master
(my-branch)$ git checkout master
(master)$ git merge --ff-only my-branch
```

Để biết thêm hãy xem [this SO thread](https://stackoverflow.com/questions/11058312/how-can-i-use-git-rebase-without-requiring-a-forced-push).

### Tôi cần kết hợp các commit

Giả sử bạn đang làm việc trong một nhánh có / sẽ trở thành một pull-request  trái với `master`. Trong trường hợp đơn giản nhất khi tất cả những gì bạn muốn làm là kết hợp tất cả các commit thành một commit và bạn không quan tâm đến timestamp commit, bạn có thể đặt lại và commit lại. Đảm bảo rằng nhánh master được cập nhật và tất cả các thay đổi của bạn được commit, sau đó:

```sh
(my-branch)$ git reset --soft master
(my-branch)$ git commit -am "New awesome feature"
```

Nếu bạn muốn kiểm soát nhiều hơn, và cũng để bảo vệ timestamp, bạn cần phải làm  một vài thứ được gọi là một interactive rebase:

```sh
(my-branch)$ git rebase -i master
```

Nếu bạn không làm việc với một nhánh khác, bạn phải rebase liên quan tới `HEAD` của bạn. Nếu bạn muốn squash 2 commit cuối, bạn sẽ phải rebase lại `HEAD~2`. Cho commit cuối 3, `HEAD~3`,...

```sh
(master)$ git rebase -i HEAD~2
```

Sau khi bạn chạy lệnh rebase interactive, bạn sẽ thấy một cái gì đó như thế này trong trình soạn thảo của bạn:

```vim
pick a9c8a1d Some refactoring
pick 01b2fd8 New awesome feature
pick b729ad5 fixup
pick e3851e8 another fix

# Rebase 8074d12..b729ad5 onto 8074d12
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Tất cả các dòng bắt đầu bằng một `#` là các comment, chúng sẽ không ảnh hưởng đến rebase của bạn.

Sau đó bạn thay thể câu lệnh `pick` với những thứ trong danh sách trên, và bạn cũng có thể loại bỏ các commit bằng cách xoá các dòng tương ứng.

Ví dụ, nếu bạn muốnn **di chuyển một mình commit cũ nhất(đầu tiên) và kết với với tất cả commit sau với commit cũ thứ 2**, bạn nên chỉnh sửa chữ cái bên cạnh mỗi commit ngoại trừ chữ cái đầu tiên và chữ cái thứ hai `f`:

```vim
pick a9c8a1d Some refactoring
pick 01b2fd8 New awesome feature
f b729ad5 fixup
f e3851e8 another fix
```

Nếu bạn muốn kết hợp các commit và **đổi tên commit**, bạn nên thêm một chữ cái `r` bên cạnh commit thứ 2 hoặc đơn giản sử dụng `s` thay vì `f`:

```vim
pick a9c8a1d Some refactoring
pick 01b2fd8 New awesome feature
s b729ad5 fixup
s e3851e8 another fix
```

Bạn có thể đổi tên commit sau trong đoạn text bật lên.

```vim
Newer, awesomer features

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# rebase in progress; onto 8074d12
# You are currently editing a commit while rebasing branch 'master' on '8074d12'.
#
# Changes to be committed:
#   modified:   README.md
#

```

Nếu mọi thứ thành công, bạn sẽ thấy một cái gì đó như thế này:

```sh
(master)$ Successfully rebased and updated refs/heads/master.
```

#### Chiến lược merge an toàn
`--no-commit` thực hiện merge nhưng giả vờ hợp nhất không thành công và không tự động, cho phép người dùng có cơ hội kiểm tra và tinh chỉnh thêm kết quả merge trước khi commit. `no-ff` duy trì bằng chứng rằng một nhánh tính năng đã từng tồn tại, giữ cho lịch sử dự án nhất quán.

```sh
(master)$ git merge --no-ff --no-commit my-branch
```

#### Tôi cần merge một nhánh vào một commit duy nhất

```sh
(master)$ git merge --squash my-branch
```

#### Tôi chỉ muốn kết hợp các commit chưa push

Đôi khi bạn có một số công việc đang tiến hành commit bạn muốn kết hợp trước khi bạn đẩy chúng lên upstream. Bạn không muốn vô tình kết hợp bất kỳ commit nào đã được push lên upstream vì một người khác có thể đã thực hiện các commit tham chiếu đến chúng.

```sh
(master)$ git rebase -i @{u}
```

Điều này sẽ làm một interactive rebase mà chỉ liệt kê các commit mà bạn chưa push, vì vậy nó sẽ được an toàn để sắp xếp lại / sửa chữa / squash bất cứ điều gì trong danh sách

#### Tôi cần huỷ việc merge

Đôi khi việc merge có thể gây ra sự cố trong một số file nhất định, trong những trường hợp đó, chúng ta có thể sử dụng tùy  `abort` để hủy bỏ quá trình giải quyết xung đột hiện tại và cố gắng xây dựng lại trạng thái merge trước.

```sh
(my-branch)$ git merge --abort
```

Lệnh này có sẵn kể từ phiên bản Git >= 1.7.4

### Tôi cần cập nhật commit cha của nhánh của tôi

Giả sử tôi có một nhánh master, một nhánh feature-1 tách nhánh từ master và một nhánh feature-2 tách nhánh từ feature-1. Nếu tôi thực hiện commit đối với feature-1, thì commit của feature-2 không còn chính xác nữa (nó phải là phần đầu của feature-1, vì chúng ta đã phân nhánh nó). Chúng ta có thể sửa điều này với `git rebase --onto`.

```sh
(feature-2)$ git rebase --onto feature-1 <the first commit in your feature-2 branch that you don't want to bring along> feature-2
```

Điều này giúp trong các trường hợp khó nơi bạn có thể có một feature được xây dựng trên một feature khác chưa được merge và một bugfix trên nhánh feature-1 cần được phản ánh trong nhánh feature-2 của bạn.

### Kiểm tra xem tất cả commit trên một nhánh đã được merge

Để kiểm cháu tất cả commit trên một nhánh được merge vào nhánh khác, bạn nên diff giữa các head (hoặc mọi commit) của những nhánh đó:

```sh
(master)$ git log --graph --left-right --cherry-pick --oneline HEAD...feature/120-on-scroll
```

Điều này sẽ cho bạn biết nếu bất kỳ commit trong một nhưng không phải là nhánh khác, và sẽ cung cấp cho bạn một danh sách của bất kỳ nonshared giữa các nhánh. Một lựa chọn khác là làm điều này:

```sh
(master)$ git log master ^feature/120-on-scroll --no-merges
```

### Các vấn đề có thể xảy ra với interactive rebases

#### Màn hình chỉnh sửa rebase cho biết 'noop'

Nếu bạn thấy điều này:
```
noop
```

Điều này có nghĩa bạn đang cố rebase lại một nhánh mà là một commit giống hệt nhau hoặc là *ahead* của nhánh hiện tại. Bạn có thể thử:

* đảm bảo nhánh master của bạn là nơi nó cần
* rebase lại `HEAD~2` hoặc sớm hơn

#### Có một vài xung đột

Nếu bạn không thể hoàn tất thành việc rebase, bạn có thể phải giải quyết xung đột.

Đầu tiên chạy `git status` để xem tệp nào có xung đột trong chúng:

```sh
(my-branch)$ git status
On branch my-branch
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  both modified:   README.md
```

Trong ví dụ đó,, `README.md` có xung đột. Mở tệp đó và tìm kiếm những điều sau:

```vim
   <<<<<<< HEAD
   some code
   =========
   some code
   >>>>>>> new-commit
```

Bạn sẽ cần phải giải quyết sự khác biệt giữa code đã được thêm vào trong commit mới của bạn (trong ví dụ, mọi thứ từ dòng ở giữa  `new-commit`) và `HEAD` của bạn.

Nếu bạn muốn giữ phiên bản code của một nhánh, bạn có thể sử dụng `--ours` hoặc `--theirs`:

```sh
(master*)$ git checkout --ours README.md
```

- Khi *đang merge*, sử dụng `--ours` để giữa các thay đổi từ nhánh local, hoặc `--theirs` để giữ các thay đổi từ nhánh khác.
- Khi *đang rebase*, sử dụng `--theirs` để giữ các thay đổi từ nhánh local, hoặc `--ours` để giữ các thay đổi từ nhánh khác. Để giải thích về sự hoán đổi này, hãy xem [chú ý này trong tài liệu Git](https://git-scm.com/docs/git-rebase#git-rebase---merge).

Nếu việc merge phức tạp hơn, bạn có thể sử dụng trình chỉnh sửa khác biệt trực quan:

```sh
(master*)$ git mergetool -t opendiff
```

Sau khi bạn đã giải quyết tất cả xung đột và đã kiểm tra code của mình, `git add` các file đã thay đổi và sau đó tiếp tục rebase với `git rebase --continue`

```sh
(my-branch)$ git add README.md
(my-branch)$ git rebase --continue
```

Nếu sau khi giải quyết tất cả các xung đột bạn kết thúc bằng một cây giống hệt với cái trước khi thực hiện, bạn cần `git rebase --skip` thay thế.

Nếu bất kỳ lúc nào bạn muốn dừng toàn bộ quá trình rebase và quay trở lại trạng thái ban đầu nhánh của bạn, bạn có thể làm như thế này:


```sh
(my-branch)$ git rebase --abort
```

## Stash

### Stash tất cả chỉnh sửa

Để stash tất cả các chỉnh sửa trong thư mục làm việc

```sh
$ git stash
```

Nếu bạn cũng muốn stash các file chưa được theo dõi, sử dụng tuỳ chọn `-u`.

```sh
$ git stash -u
```

### Stash các file cụ thể

Để stash chỉ một file từ thư mục làm việc

```sh
$ git stash push working-directory-path/filename.ext
```

Để stash nhiều file từ thư mục làm việc

```sh
$ git stash push working-directory-path/filename1.ext working-directory-path/filename2.ext
```

### Stash với message

```sh
$ git stash save <message>
```

### Apply một stash cụ thể từ danh sách

Đầu tiên kiểm tra danh sách các stash với message sử dụng

```sh
$ git stash list
```

Sau đó apply một stash cụ thể từ danh sách sử dụng

```sh
$ git stash apply "stash@{n}"
```

Ở đây, 'n' cho biết vị trí của stash trong stack. Stash trên cùng sẽ là vị trí 0.

## Finding

### Tôi muốn tìm một chuỗi trong bất kỳ commit nào

Để tìm một chuỗi nhất định được cho vào trong bất kỳ commit nào, bạn có thể sử dụng cấu trúc sau:

```sh
$ git log -S "string to find"
```

Các thông số chung:

* `--source` có nghĩa là hiển thị tên ref được đưa ra trên dòng lệnh mà mỗi lần commit đã đạt được.

* `--all` nghĩa là bắt đầu từ mọi nhánhmeans to start from every branch.

* `--reverse` in theo thứ tự ngược lại, nó có nghĩa là sẽ hiển thị commit đầu tiên đã thực hiện thay đổi.

### Tôi muốn tìm tác giả hoặc người commit

Tìm tất cả commit từ tác giả hoặc người commit bạn có thể sử dụng:

```sh
$ git log --author=<name or email>
$ git log --committer=<name or email>
```

Hãy nhớ rằng tác giả và người commit không giống. `--author` là người ban đầu đã viết code; mặt khác,  `--committer`, là người đã commit code thay mặc tác giả gốc.

### Tôi muốn liệt kê các commit chứa các file cụ thể

Để tìm tất cả các commit chưa một file cụ thể bạn có thể sử dụng:

```sh
$ git log -- <path to file>
```

Bạn thường sẽ chỉ định một đường dẫn chính xác, nhưng bạn cũng có thể sử dụng các ký tự đại diện trong đường dẫn và tên tệp:

```sh
$ git log -- **/*.js
```

Trong khi sử dụng ký tự đại diện, nó hữu ích để thông báo `--name-status` để xem danh sách các tệp đã commit:

```sh
$ git log --name-status -- **/*.js
```

### Tìm một tag nơi một commit đã tham chiếu

Để tìm tất cả các tag có chứa một commit cụ thể

```sh
$ git tag --contains <commitid>
```

## Submodules

### Clone tất cả submodules

```sh
$ git clone --recursive git://github.com/foo/bar.git
```

Nếu đã clone:

```sh
$ git submodule update --init --recursive
```

### Xoá một submodule

Tạo một submodule là khá nhanh, nhưng xóa chúng ít hơn như vậy. Các lệnh bạn cần là:

```sh
$ git submodule deinit submodulename
$ git rm submodulename
$ git rm --cached submodulename
$ rm -rf .git/modules/submodulename
```

## Các đối tượng khác
### Khôi phục một file đã xoá

Đầu tiên tìm commit nơi mà lần cuối file đã tồn tại:

```sh
$ git rev-list -n 1 HEAD -- filename
```

Sau đó checkout file:

```
git checkout deletingcommitid^ -- filename
```

### Xoá tag

```sh
$ git tag -d <tag_name>
$ git push <remote> :refs/tags/<tag_name>
```

### Khôi phục một tag đã xoá

Nếu bạn muốn khôi phục tag đã bị xóa, bạn có thể làm được vậy bằng cách làm theo các bước sau: Trước tiên, bạn cần phải tìm tag không thể truy cập

```sh
$ git fsck --unreachable | grep tag
```

Ghi lại mã hash của tag. Sau đó, khôi phục tag đã xóa theo cách sau, sử  [`git update-ref`](https://git-scm.com/docs/git-update-ref):

```sh
$ git update-ref refs/tags/<tag_name> <hash>
```

Tag của bạn bây giờ đã được khôi phục.

### Xóa Patch

Nếu ai đó đã gửi cho bạn một pull request trên GitHub, nhưng sau đó đã xoá chúng trên fork gốc, bạn sẽ không thể clone repository của họ hoặc sử dụng `git am` như [.diff, .patch](https://github.com/blog/967-github-secrets) url không khả dụng. Nhưng bạn có thể checkout chính PR bằng cách sử dụng [GitHub's special refs](https://gist.github.com/piscisaureus/3342247). Để fetch nội dung của PR#1 vào một nhánh được gọi là pr_1:

```sh
$ git fetch origin refs/pull/1/head:pr_1
From github.com:foo/bar
 * [new ref]         refs/pull/1/head -> pr_1
```

### Xuất một repository ra một file Zip

```sh
$ git archive --format zip --output /full/path/to/zipfile.zip master
```
### Push một nhánh và một tag có tên giống nhau

Nếu có một tag trên một remote repository mà có tên giống với một nhánh bạn sẽ gặp phải lỗi khi cố push nhanh với một commad chuẩn `$ git push <remote> <branch>`.

```sh
$ git push origin <branch>
error: dst refspec same matches more than one.
error: failed to push some refs to '<git server>'
```

Sửa lỗi này bằng cách chỉ định bạn muốn đẩy tham chiếu head.

```sh
$ git push origin refs/heads/<branch-name>
```

Nếu bạn muốn đẩy một tag vào một repository từ remote có cùng tên với một nhánh, bạn có thể sử dụng một lệnh tương tự.

```sh
$ git push origin refs/tags/<tag-name>
```

## Tracking các file

### Tôi muốn thay đổi cách viết hoa của tên tệp mà không thay đổi nội dung của tệp

```sh
(master)$ git mv --force myfile MyFile
```

### Tôi muốn ghi đè lên các tệp local khi thực hiện lệnh git pull

```sh
(master)$ git fetch --all
(master)$ git reset --hard origin/master
```

### Tôi muốn xóa một tệp khỏi Git nhưng vẫn giữ tệp

```sh
(master)$ git rm --cached log.txt
```

### Tôi muốn revert tệp về bản sửa đổi cụ thể

Giả sử mã hash của commit bạn muốn c5f567:

```sh
(master)$ git checkout c5f567 -- file1/to/restore file2/to/restore
```

Nếu bạn muốn revert các thay đổi được thực hiện chỉ 1 commit trước c5f567, vượt qua commit hash như c5f567~1:

```sh
(master)$ git checkout c5f567~1 -- file1/to/restore file2/to/restore
```

### Tôi muốn liệt kê các thay đổi của một tệp cụ thể giữa các commit hoặc các nhánh

Giả sử bạn muốn so sánh commit cuối cùng với tệp từ commit c5f567:

```sh
$ git diff HEAD:path_to_file/file c5f567:path_to_file/file
```

Cùng đi cho các nhánh:

```sh
$ git diff master:path_to_file/file staging:path_to_file/file
```

### Tôi muốn Git bỏ qua những thay đổi đối với một tệp cụ thể

Điều này hoạt động tốt cho các mẫu cấu hình hoặc các tệp khác yêu cầu thêm thông tin đăng nhập cục bộ mà không được commit

```sh
$ git update-index --assume-unchanged file-to-ignore
```

Lưu ý rằng điều này không xóa tệp khỏi kiểm soát source - nó chỉ bị bỏ qua cục bộ. Để hoàn tác thao tác này và yêu cầu Git lưu ý các thay đổi một lần nữa, điều này sẽ xóa ignore flag:

```sh
$ git update-index --no-assume-unchanged file-to-stop-ignoring
```

## Cấu hình

### Tôi muốn thêm bí danh cho một số lệnh Git

Trên OS X và Linux, file cấu hình git được lưu trong ```~/.gitconfig```.  Tôi đã thêm một số bí danh mẫu mà tôi sử dụng làm shortcut (và một số lỗi chính tả phổ biến của tôi) trong phần ```[alias]``` được hiển thị như dưới đây:

```vim
[alias]
    a = add
    amend = commit --amend
    c = commit
    ca = commit --amend
    ci = commit -a
    co = checkout
    d = diff
    dc = diff --changed
    ds = diff --staged
    extend = commit --amend -C HEAD
    f = fetch
    loll = log --graph --decorate --pretty=oneline --abbrev-commit
    m = merge
    one = log --pretty=oneline
    outstanding = rebase -i @{u}
    reword = commit --amend --only
    s = status
    unpushed = log @{u}
    wc = whatchanged
    wip = rebase -i @{u}
    zap = fetch -p
```

### Tôi muốn thêm một thư mục trống vào repository của tôi

Bạn không thể! Git không hỗ trợ điều này, nhưng có một cách hack. Bạn có thể tạo tệp .gitignore trong thư mục với các nội dung sau:

```
 # Ignore everything in this directory
 *
 # Except this file
 !.gitignore
```

Một quy ước chung khác là tạo một tệp trống trong thư mục có tiêu đề .gitkeep.


```sh
$ mkdir mydir
$ touch mydir/.gitkeep
```

Bạn cũng có thể đặt tên tệp là .keep, trong trường hợp dòng thứ hai ở trên sẽ ```touch mydir/.keep```

### Tôi muốn cache một username và password cho một repository

Bạn có thể có một repository yêu cầu xác thực.  Trong trường hợp này bạn có thể cache một username và password vì vậy bạn không phải nhập nó vào mỗi lần push / pull. Việc xác thực có thể làm điều này cho bạn. 

```sh
$ git config --global credential.helper cache
# Set git to use the credential memory cache
```

```sh
$ git config --global credential.helper 'cache --timeout=3600'
# Set the cache to timeout after 1 hour (setting is in seconds)
```

### Tôi muốn làm cho Git bỏ qua các quyền và thay đổi về filemode

```sh
$ git config core.fileMode false
```

Nếu bạn muốn đặt hành vi này là hành vi mặc định cho người dùng đã đăng nhập, thì hãy sử dụng:

```sh
$ git config --global core.fileMode false
```

### Tôi muốn đặt user toàn cục

Để cấu hình thông tin người dùng được sử dụng trên tất cả các repository cục bộ và để đặt tên có thể nhận dạng khi xem lại phiên bản lịch sử:

```sh
$ git config --global user.name “[firstname lastname]”
```
Để đặt địa chỉ email sẽ được liên kết với mỗi điểm đánh dấu lịch sử:

```sh
git config --global user.email “[valid-email]”
```

### Tôi muốn thêm màu cho command Git

Để thiết lập màu command tự động cho Git để dễ dàng xem lại:

```sh
$ git config --global color.ui auto
```

## Tôi không nghĩ mình đã làm gì sai

Vì vậy, bạn đang say - bạn `reset` vài thứ, hoặc bạn merge sai nhánh, hoặc bạn force push và bây giờ bạn không thể tìm thấy các commit của bạn. Bạn biết, tại một số thời điểm, bạn đã làm tốt, và bạn muốn quay trở lại trạng thái bạn đang ở đó.

Đây là những gì `git reflog` cho. `reflog` theo dõi bất kỳ thay đổi nào đối với mẹo của nhánh, ngay cả khi mẹo đó không được tham chiếu bởi nhánh hoặc tag. Về cơ bản, mỗi lần HEAD thay đổi, một mục mới được thêm vào reflog. Điều này chỉ hoạt động đối với các repository cục bộ, thật đáng buồn, và nó chỉ theo dõi các chuyển động (ví dụ: không thay đổi một tệp không được ghi ở bất kỳ đâu).

```sh
(master)$ git reflog
0a2e358 HEAD@{0}: reset: moving to HEAD~2
0254ea7 HEAD@{1}: checkout: moving from 2.2 to master
c10f740 HEAD@{2}: checkout: moving from master to 2.2
```

Các reflog ở trên cho thấy một checkout từ master đến nhánh 2.2 và trở lại. Từ đó, có một thiết lập cứng để một commit cũ hơn. Hoạt động mới nhất được thể hiện ở đầu được gắn nhãn `HEAD@{0}`.

Nếu nó chỉ ra rằng bạn vô tình di chuyển trở lại, các reflog sẽ chứa commit master chỉ đến (0254ea7) trước khi bạn vô tình giảm 2 commit

```sh
$ git reset --hard 0254ea7
```

Sử dụng `git reset` sau đó nó có thể thay đổi master trở về commit trước đó. Điều này cung cấp sự an toàn trong trường hợp lịch sử đã vô tình thay đổi.

(đã sao chép và chỉnh sửa từ [Source](https://www.atlassian.com/git/tutorials/rewriting-history/git-reflog)).

# Tài nguyên khác

## Sách

* [Learn Enough Git to Be Dangerous](https://www.learnenough.com/git-tutorial) - A book by Michael Hartl covering Git from basics
* [Pro Git](https://git-scm.com/book/en/v2) - Scott Chacon and Ben Straub's excellent book about Git
* [Git Internals](https://github.com/pluralsight/git-internals-pdf) - Scott Chacon's other excellent book about Git

## Hướng dẫn

* [19 mẹo sử dụng GIT hàng ngày](https://www.alexkras.com/19-git-tips-for-everyday-use) - Một danh sách các mẹo dùng GIT hữu ích
* [Atlassian's Git tutorial](https://www.atlassian.com/git/tutorials) Sử dụng Git đúng với các hướng dẫn từ cơ bản đến nâng cao.
* [Learn Git branching](https://learngitbranching.js.org/) Hướng dẫn phân nhánh / merging / rebasing dựa trên web interactive
* [Getting solid at Git rebase vs. merge](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa)
* [Git Commands and Best Practices Cheat Sheet](https://zeroturnaround.com/rebellabs/git-commands-and-best-practices-cheat-sheet) - Một Git cheat sheet trong một bài đăng trên blog với nhiều giải thích hơn
* [Git from the inside out](https://codewords.recurse.com/issues/two/git-from-the-inside-out) - Hướng dẫn đi sâu vào vào Git
* [git-workflow](https://github.com/asmeurer/git-workflow) - [Aaron Meurer](https://github.com/asmeurer) của cách sử dụng Git để đóng góp vào repository mã nguồn mở
* [GitHub as a workflow](https://hugogiraudel.com/2015/08/13/github-as-a-workflow/) - Một điều thú vị khi sử dụng GitHub như một quy trình làm việc, đặc biệt với các PR trống
* [Githug](https://github.com/Gazler/githug) - Một trò chơi để học thêm về luồng làm việc chung của Git

## Scripts và các công cụ

* [firstaidgit.io](http://firstaidgit.io/) Một lựa chọn có thể tìm kiếm các câu hỏi thường gặp nhất về Git
* [git-extra-commands](https://github.com/unixorn/git-extra-commands) - tập hợp các script Git mở rộng hữu ích
* [git-extras](https://github.com/tj/git-extras) - Các tiện ích GIT -- repo tóm tắt, thay thế, số lượng thay đổi, tỷ lệ phần trăm của tác giả và nhiều nữa
* [git-fire](https://github.com/qw3rtman/git-fire) - git-fire là một plugin Git để giúp trong trường hợp khẩn cấp bằng cách thêm tất cả các tệp hiện tại, commit và push vào một nhánh mới (để ngăn xung đột khi merge).
* [git-tips](https://github.com/git-tips/tips) - Các mẹo Git nhỏ
* [git-town](https://github.com/Originate/git-town) - Hỗ trợ luồng làm việc Git chung, mức cao! http://www.git-town.com

## GUI Clients
* [GitKraken](https://www.gitkraken.com/) - Client sang trọng cho Windows, Mac & Linux
* [git-cola](https://git-cola.github.io/) - Git client khác cho Windows và OS X
* [GitUp](https://github.com/git-up/GitUp) - Một GUI mới mẻ mà có một số cách rất quan tâm để giải quyết các việc khó chịu của Git
* [gitx-dev](https://rowanj.github.io/gitx/) - Một Git client đồ hoạ khác cho OS X
* [Sourcetree](https://www.sourcetreeapp.com/) - Sự đơn giản nhưng mạnh mẽ cho giao diện Git đẹp và miễn. Cho Windows and Mac.
* [Tower](https://www.git-tower.com/) - Git client đồ hoạ cho OS X (trả phí)
* [tig](https://jonas.github.io/tig/) - terminal text-mode interface cho Git
* [Magit](https://magit.vc/) - Interface cho Git thực hiện như một gói Emacs .
* [GitExtensions](https://github.com/gitextensions/gitextensions) - Một shell extension, một Visual Studio 2010-2015 plugin và một công cụ Git repository độc lập.
* [Fork](https://git-fork.com/) - Một Git client nhanh và thân thiện cho Mac (beta)
* [gmaster](https://gmaster.io/) - Một Git client cho Windows với 3 cách merge, analyze refactors, semantic diff và merge (beta)
* [gitk](https://git-scm.com/docs/gitk) - Một Git client cho linux để cho phép xem đơn giản cho trạng thái repo.
* [SublimeMerge](https://www.sublimemerge.com/) - Client nhanh, mở rộng, cung cấp 3 cách merge, tìm kiếm mạnh mẽ và làm nổi bật cú pháp, đang phát triển tích cực.
