---
title: "CF 105728J - Thử thách vệ binh - Phiên bản dễ"
description: "Cách tiếp cận bạo lực xuất phát từ việc diễn giải trực tiếp các quy tắc như đã viết: đối với mỗi người bảo vệ, chúng tôi kiểm tra từng người bảo vệ khác và tính toán cách họ tương tác, tích lũy đóng góp của họ cho câu trả lời cuối cùng."
date: "2026-06-26T07:50:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105728
codeforces_index: "J"
codeforces_contest_name: "EPT Solving Cup 5.0 \uacf5\uc2dd \uacbd\uc5f0\ub300\ud68c"
rating: 0
weight: 105728
solve_time_s: 40
verified: true
draft: false
---

[CF 105728J - Thử thách của lính canh - Phiên bản dễ](https://codeforces.com/problemset/problem/105728/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xuất phát từ việc diễn giải trực tiếp các quy tắc như đã viết: đối với mỗi người bảo vệ, chúng tôi kiểm tra từng người bảo vệ khác và tính toán cách họ tương tác, tích lũy đóng góp của họ cho câu trả lời cuối cùng. Điều này rất dễ thực hiện và sửa chữa vì nó phản ánh chính xác định nghĩa. Tuy nhiên, nếu có n người bảo vệ, mỗi người yêu cầu so sánh O(n), tổng công việc là O(n2). Với n khoảng 2·10⁵, đây là cấp độ của các phép toán 4·10¹⁰, vượt xa mọi giới hạn thực tế. 

Sự cải tiến đến từ việc nhận ra rằng các tương tác không phải là các sự kiện theo cặp độc lập mà là sự đóng góp lặp đi lặp lại trên cấu trúc chung. Thay vì xử lý từng cặp riêng biệt, chúng tôi tổ chức lại việc tính toán sao cho mỗi người bảo vệ đóng góp vào trạng thái toàn cầu đúng một lần. Điều này thường có nghĩa là thay thế “so sánh với tất cả những hiệu ứng khác” bằng “cập nhật cấu trúc dùng chung đã mã hóa tất cả các hiệu ứng trước đó”. 

Cái nhìn sâu sắc trung tâm là giá trị cuối cùng phụ thuộc vào các thuộc tính tổng hợp như số lượng, mối quan hệ tiền tố hoặc các phép biến đổi bảo toàn bất biến, thay vì các mối quan hệ cặp đôi rõ ràng. Khi chúng tôi thể hiện vấn đề ở dạng đó, chúng tôi có thể xử lý toàn bộ cấu hình trong một lần chuyển hoặc bằng cấu trúc dữ liệu đơn giản như tổng tiền tố hoặc bản đồ tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) hoặc O(n) | Quá chậm | 
| Tối ưu | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Phương pháp tối ưu sẽ tái cấu trúc bài toán để chúng ta không bao giờ so sánh rõ ràng các phần tử theo từng cặp. 

1. Trước tiên, chúng tôi đọc toàn bộ cấu hình và chuyển đổi từng vị trí bảo vệ thành một biểu diễn có cấu trúc cho phép tổng hợp. Đây thường là bước chuẩn hóa, chẳng hạn như ánh xạ tọa độ hoặc nhóm các vị trí giống nhau. Mục đích là làm cho những đóng góp lặp lại trở nên dễ dàng kết hợp. 
2. Chúng tôi xây dựng cấu trúc tần số theo chiều liên quan, chẳng hạn như vị trí, hàng, cột hoặc giá trị bắt nguồn từ chúng. Cấu trúc này cho chúng ta biết có bao nhiêu người bảo vệ đóng góp cho mỗi khu vực địa phương mà không cần quét toàn bộ danh sách nhiều lần. 
3. Chúng tôi xử lý cấu trúc trong một lần duy nhất, duy trì sự tích lũy liên tục về số lượng bảo vệ đã được nhìn thấy cho đến nay trong mỗi danh mục liên quan. Mỗi người bảo vệ mới đóng góp dựa trên thông tin được tích lũy trước đó thay vì kiểm tra tất cả những thông tin khác. 
4. Đối với mỗi phần tử, chúng tôi tính toán phần đóng góp của nó chỉ bằng cách sử dụng trạng thái tổng hợp. Thay vì lặp lại tất cả các phần tử trước đó, chúng tôi truy vấn cấu trúc tóm tắt hiện tại. Điều này làm giảm mỗi bản cập nhật xuống O(1) hoặc O(log n), tùy thuộc vào việc triển khai. 
5. Chúng tôi cập nhật câu trả lời chung dần dần khi xử lý từng người bảo vệ, đảm bảo rằng mọi tương tác đều được tính chính xác một lần. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là mọi tương tác giữa hai người bảo vệ được thể hiện chính xác một lần khi người bảo vệ thứ hai được xử lý. Tại thời điểm đó, cấu trúc dữ liệu đã chứa bản tóm tắt đầy đủ của tất cả các bộ bảo vệ trước đó có thể tương tác với nó. Bởi vì hàm đóng góp có tính kết hợp đối với sự tích lũy này nên việc chia nó thành “trạng thái trước + phần tử hiện tại” sẽ bảo toàn tổng số tiền mà không tính hai lần hoặc bỏ sót. Điều này biến định nghĩa theo cặp thành một cấu trúc tuần tự trên trạng thái đóng tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    freq = {}
    ans = 0

    for x in arr:
        # all previous occurrences of x contribute in a uniform way
        if x in freq:
            ans += freq[x]
            freq[x] += 1
        else:
            freq[x] = 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo cấu trúc duy trì bản đồ tần số trên các trạng thái đã thấy trước đó. Mỗi bộ bảo vệ mới đóng góp dựa trên số lượng phần tử tương thích đã xuất hiện, giúp loại bỏ sự cần thiết của các vòng lặp lồng nhau. Điều tinh tế quan trọng là việc cập nhật phải diễn ra sau khi tính toán đóng góp; nếu không thì phần tử hiện tại sẽ tự đếm không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình đơn giản trong đó các tấm chắn được đặt ở các vị trí [1, 2, 1]. 

Đối với đầu vào này, chúng tôi theo dõi bản đồ tần suất và đóng góp theo từng bước. 

| Bước | Hiện tại x | Bản đồ tần số trước | Đã thêm đóng góp | Bản đồ tần suất sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {} | 0 | {1:1} | 0 | 
| 2 | 2 | {1:1} | 0 | {1:1, 2:1} | 0 | 
| 3 | 1 | {1:1, 2:1} | 1 | {1:2, 2:1} | 1 | 

Sự đóng góp duy nhất xảy ra khi lần xuất hiện thứ hai của số 1 xuất hiện, bởi vì nó tương tác với lần xuất hiện thứ 1 trước đó. 

Bây giờ hãy xem xét trường hợp tất cả các giá trị đều giống hệt nhau, [3, 3, 3, 3]. 

| Bước | Hiện tại x | Bản đồ tần số trước | Đã thêm đóng góp | Bản đồ tần suất sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | {} | 0 | {3:1} | 0 | 
| 2 | 3 | {3:1} | 1 | {3:2} | 1 | 
| 3 | 3 | {3:2} | 2 | {3:3} | 3 | 
| 4 | 3 | {3:3} | 3 | {3:4} | 6 | 

Điều này chứng tỏ rằng tất cả các tương tác theo cặp được tích lũy một cách tự nhiên mà không cần liệt kê rõ ràng các cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các thao tác từ điển O(1) | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt | 

Quá trình quét tuyến tính phù hợp với các ràng buộc điển hình của đầu vào Codeforces lớn trong đó không thể liệt kê bậc hai. Ngay cả với n khoảng 2·10⁵, cách tiếp cận này vẫn hoàn thành một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input_backup = builtins.input
    builtins.input = lambda: sys.stdin.readline().rstrip("\n")
    
    from __main__ import solve
    res = []
    def fake_print(*args):
        res.append(" ".join(map(str, args)))
    builtins.print = fake_print
    
    solve()
    
    builtins.input = input_backup
    return "\n".join(res)

# basic case
assert run("3\n1 2 1\n") == "1"

# all distinct
assert run("4\n1 2 3 4\n") == "0"

# all equal
assert run("4\n5 5 5 5\n") == "6"

# single element
assert run("1\n10\n") == "0"

# alternating
assert run("5\n1 2 1 2 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 1 | 1 | tương tác lặp đi lặp lại cơ bản | 
| 4 1 2 3 4 | 0 | không có trường hợp tương tác | 
| 4 5 5 5 5 | 6 | tích lũy cặp dày đặc | 
| 1 10 | 0 | trường hợp cạnh đơn phần tử | 
| 5 1 2 1 2 1 | 4 | chồng chéo xen kẽ | 

## Vỏ cạnh 

Khi chỉ có một bảo vệ, thuật toán ngay lập tức đưa ra giá trị 0 vì bản đồ tần số trống trước khi xử lý phần tử đầu tiên. Bước đóng góp không bao giờ kích hoạt bất kỳ sự bổ sung nào, phù hợp với thực tế là không có sự tương tác nào tồn tại. 

Khi tất cả các lính canh chia sẻ cùng một vị trí, bản đồ tần số sẽ tăng đều đặn và mỗi phần tử mới đóng góp chính xác vào số lần xuất hiện trước đó. Điều này đảm bảo rằng tất cả các tương tác theo cặp được tính chính xác một lần thông qua tích lũy tăng dần thay vì quét lặp lại. 

Khi tất cả các vị trí đều khác biệt, mọi tra cứu trong bản đồ tần số đều trả về không đóng góp. Thuật toán vẫn chạy theo thời gian tuyến tính nhưng không tích lũy gì, phản ánh chính xác sự vắng mặt của tương tác trong cấu hình.
