---
title: "CF 104218D - Thử thách trang phục"
description: "Chúng tôi đang mô phỏng một tủ quần áo dạng ngăn xếp, trong đó các mặt hàng quần áo được nhét vào, lấy ra từ trên cùng và đôi khi được lấy ra ở giữa theo tên. Mỗi thao tác sửa đổi hoặc truy vấn đống này. Có ba hoạt động."
date: "2026-07-01T23:48:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104218
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104218
solve_time_s: 66
verified: true
draft: false
---

[CF 104218D - Thử thách trang phục](https://codeforces.com/problemset/problem/104218/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một tủ quần áo dạng ngăn xếp, trong đó các mặt hàng quần áo được nhét vào, lấy ra từ trên cùng và đôi khi được lấy ra ở giữa theo tên. Mỗi thao tác sửa đổi hoặc truy vấn đống này. 

Có ba hoạt động. MỘT`put s`hành động đẩy một mặt hàng quần áo có tên duy nhất lên trên cùng của đống. MỘT`get`hành động xóa mục trên cùng và in tên của nó hoặc in`empty`nếu không có gì tồn tại. MỘT`iditarod`hành động tìm kiếm toàn bộ đống vật phẩm đặc biệt có tên`snowcoat`. Nếu nó tồn tại, chúng tôi sẽ xóa nó khỏi vị trí hiện tại mà không làm xáo trộn thứ tự tương đối của các mục còn lại và in thông báo thành công. Nếu không, chúng tôi sẽ in một thông báo lỗi. 

Hạn chế chính là tất cả các thao tác phải được xử lý trực tuyến và có tới 1000 thao tác, do đó, ngay cả việc quét tuyến tính trên mỗi thao tác cũng dễ dàng đủ nhanh. Yêu cầu tế nhị là việc loại bỏ`snowcoat`phải duy trì thứ tự của tất cả các mục khác một cách chính xác như thể ngăn xếp đã được phân chia xung quanh phần tử đó. 

Một sai lầm ngây thơ xuất phát từ việc coi cấu trúc như một ngăn xếp thuần túy và quên rằng`iditarod`là một sự xóa bỏ ở giữa. 

Trường hợp một cạnh là khi đống trống và`get`được gọi. Đầu ra đúng là`empty`. 

Một trường hợp cạnh khác là khi`snowcoat`không có mặt. Trong trường hợp đó,`iditarod`không được sửa đổi ngăn xếp chút nào. Việc triển khai có lỗi có thể vô tình bật lên hoặc sửa đổi một phần trạng thái trong khi tìm kiếm. 

Trường hợp cạnh thứ ba là khi`snowcoat`nằm ở trên cùng hoặc dưới cùng của ngăn xếp. Việc loại bỏ nó vẫn phải bảo đảm trật tự của các phần tử còn lại và không được vô tình đảo ngược hoặc xây dựng lại cấu trúc không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô hình hóa đống một cách rõ ràng dưới dạng danh sách Python. Chúng tôi coi phần cuối của danh sách là phần trên cùng của ngăn xếp. MỘT`put`hoạt động là một sự đẩy, và một`get`hoạt động là một pop. Cả hai đều là O (1). 

Vì`iditarod`, chúng tôi quét danh sách từ trên xuống dưới để tìm kiếm`"snowcoat"`. Sau khi tìm thấy, chúng tôi xóa nó bằng cách xóa danh sách. Hoạt động này là O(n) trong trường hợp xấu nhất vì chúng ta có thể quét toàn bộ đống. 

Với tổng số tối đa 1000 thao tác, ngay cả lần quét O(n) trong trường hợp xấu nhất cho mỗi thao tác cũng dẫn đến tổng cộng khoảng 10^6 bước, nằm trong giới hạn thoải mái. Không cần cấu trúc dữ liệu phức tạp hơn. 

Thông tin chi tiết quan trọng là chúng tôi không cần hỗ trợ việc xóa tùy ý một cách hiệu quả. Chỉ có một khóa đặc biệt mà chúng tôi từng xóa theo giá trị, vì vậy tìm kiếm tuyến tính là đủ. Bất kỳ nỗ lực nào nhằm duy trì cây cân bằng hoặc bản đồ chỉ mục đều là chi phí không cần thiết đối với kích thước ràng buộc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (danh sách + quét) | O(T · N) | O(N) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(T · N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một danh sách`pile`, trong đó phần tử cuối cùng là phần tử trên cùng của ngăn xếp. 

1. Nếu hoạt động`put s`, nối thêm`s`đến cuối`pile`. Điều này thể hiện việc đặt một mục mới lên trên. 
2. Nếu hoạt động`get`, kiểm tra xem`pile`trống rỗng. Nếu nó trống, xuất ra`empty`. Nếu không thì loại bỏ và xuất phần tử cuối cùng. Điều này trực tiếp phù hợp với ngữ nghĩa ngăn xếp. 
3. Nếu hoạt động`iditarod`, quét danh sách từ đầu đến cuối. Chúng tôi tìm kiếm sự xuất hiện đầu tiên của`"snowcoat"`khi được xem từ đầu ngăn xếp, vì đó là phiên bản có thể truy cập vật lý. 
4. Nếu`"snowcoat"`được tìm thấy tại chỉ mục`i`, xóa nó bằng cách xóa và in thông báo thành công. 
5. Nếu quá trình quét hoàn tất mà không tìm thấy nó, hãy in thông báo lỗi. 

Lý do quét từ trên xuống là vì nó khớp với cách truy cập tự nhiên của cọc, nhưng quét từ một trong hai hướng vẫn đúng miễn là chúng ta loại bỏ chính xác một phần tử khớp. 

### Tại sao nó hoạt động 

Đống luôn là thứ tự tuyến tính của các mục trong đó chỉ có ba phép biến đổi xảy ra: nối vào cuối, xóa khỏi cuối hoặc xóa một giá trị đã biết. Thuật toán duy trì thứ tự vì việc xóa chỉ loại bỏ một phần tử và không sắp xếp lại bất kỳ phần tử nào khác. Mỗi thao tác biến đổi một chuỗi hợp lệ thành một chuỗi hợp lệ khác khớp chính xác với định nghĩa vấn đề, do đó cấu trúc vẫn nhất quán sau mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    pile = []

    for _ in range(T):
        parts = input().strip().split()

        if parts[0] == "put":
            pile.append(parts[1])

        elif parts[0] == "get":
            if not pile:
                print("empty")
            else:
                print(pile.pop())

        else:  # iditarod
            found_idx = -1
            for i in range(len(pile) - 1, -1, -1):
                if pile[i] == "snowcoat":
                    found_idx = i
                    break

            if found_idx == -1:
                print("oopsimcold :(")
            else:
                pile.pop(found_idx)
                print("winner winner chicken dinner :)")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp mô hình ngăn xếp. Sự tinh tế duy nhất là ở`iditarod`, trong đó chúng tôi quét rõ ràng từ đầu ngăn xếp trở xuống. Điều này đảm bảo chúng tôi loại bỏ sự xuất hiện chính xác nếu từng cho phép nhiều tên giống hệt nhau trong các biến thể khác, nhưng ở đây tính duy nhất khiến việc này trở nên đơn giản. 

các`pop(found_idx)`thao tác loại bỏ phần tử mà không ảnh hưởng đến thứ tự tương đối của các mục khác, đó chính xác là hành vi bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ dấu vết 1 

đầu vào:```
put shirt
put snowcoat
iditarod
```| Bước | Hoạt động | trạng thái cọc (dưới → trên) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | đặt áo | [áo sơ mi] | | 
| 2 | đặt áo tuyết | [áo sơ mi, áo khoác tuyết] | | 
| 3 | iditarod | [áo sơ mi] | bữa tối gà của người chiến thắng :) | 

Điều này xác nhận rằng việc xóa sẽ giữ nguyên thứ tự còn lại và chỉ xóa mục phù hợp. 

### Ví dụ dấu vết 2 

đầu vào:```
put a
iditarod
get
```| Bước | Hoạt động | trạng thái cọc (dưới → trên) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | đặt | [a] | | 
| 2 | iditarod | [a] | ôi trời ơi :( | 
| 3 | nhận được | [] | một | 

Điều này cho thấy rằng tìm kiếm thất bại không sửa đổi ngăn xếp và các hoạt động ngăn xếp tiếp theo hoạt động bình thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · N) | Mỗi`iditarod`có thể quét toàn bộ đống một lần | 
| Không gian | O(N) | Chúng tôi lưu trữ tất cả các mục hiện tại trong một danh sách | 

Với tối đa 1000 thao tác, công việc trong trường hợp xấu nhất là kiểm tra khoảng 10^6 phần tử, dễ dàng nằm gọn trong giới hạn 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""6
put shirt
put snowcoat
iditarod
iditarod
put shirt2
get
""") == """winner winner chicken dinner :)
oopsimcold :(
shirt2"""

# empty get
assert run("""2
get
put x
""") == """empty"""

# snowcoat at bottom
assert run("""3
put snowcoat
put a
iditarod
""") == """winner winner chicken dinner :)"""

# multiple gets
assert run("""5
put a
put b
get
get
get
""") == """b
a
empty"""

# no snowcoat ever
assert run("""3
put a
put b
iditarod
""") == """oopsimcold :( """
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trống rỗng | trống | xử lý ngăn xếp trống | 
| đáy áo tuyết | thành công | xóa đúng | 
| lặp đi lặp lại nhận được | LIFO đúng đắn | hành vi ngăn xếp | 
| áo tuyết bị thiếu | thông báo lỗi | không có tham nhũng nhà nước | 

## Vỏ cạnh 

Khi nào`iditarod`được gọi trên một đống trống, vòng quét không bao giờ chạy và thuật toán trực tiếp xuất ra`oopsimcold :(`mà không sửa đổi trạng thái. Ví dụ, đầu vào`iditarod`dẫn đến đầu ra bị hỏng ngay lập tức và đống vẫn trống. 

Khi`snowcoat`nằm ở đầu ngăn xếp, quá trình quét ngược sẽ tìm thấy nó ngay tại chỉ mục`len(pile) - 1`, Và`pop`chỉ loại bỏ phần tử đó. Các cọc còn lại không thay đổi về thứ tự tương đối. 

Khi`snowcoat`ở dưới cùng, quá trình quét sẽ duyệt qua danh sách đầy đủ, tìm thấy nó ở chỉ mục 0 và xóa nó. Các phần tử còn lại dịch chuyển xuống nhưng vẫn giữ nguyên thứ tự ban đầu, phù hợp với yêu cầu chỉ có mục được chọn biến mất mà không sắp xếp lại bất kỳ thứ gì khác.
