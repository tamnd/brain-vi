---
title: "CF 978B - Tên tập tin"
description: "Chúng ta được cung cấp một chuỗi ngắn đại diện cho tên tệp. Hạn chế duy nhất quan trọng là chuỗi con \"xxx\" bị cấm ở bất kỳ đâu trong chuỗi cuối cùng."
date: "2026-06-17T01:22:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 978
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 481 (Div. 3)"
rating: 800
weight: 978
solve_time_s: 73
verified: true
draft: false
---

[CF 978B - Tên tệp](https://codeforces.com/problemset/problem/978/B) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn đại diện cho tên tệp. Ràng buộc duy nhất quan trọng là chuỗi con`"xxx"`bị cấm ở bất cứ đâu trong chuỗi cuối cùng. Chúng tôi được phép xóa các ký tự khỏi các vị trí tùy ý và chúng tôi muốn xóa càng ít ký tự càng tốt để không có ba ký tự liên tiếp`'x'`các ký tự vẫn còn trong chuỗi kết quả. 

Do đó, nhiệm vụ này là một vấn đề giảm thiểu trên một chuỗi: chúng ta phải ngắt mọi lần chạy liên tiếp`'x'`ký tự sao cho không có dòng nào có độ dài từ 3 ký tự trở lên, đồng thời xóa càng ít ký tự càng tốt. 

Kích thước đầu vào nhỏ, với độ dài của chuỗi nhiều nhất là 100. Điều này ngay lập tức cho thấy rằng bất kỳ thao tác quét tuyến tính nào hoặc thậm chí logic tham lam liên quan nhiều hơn một chút cũng sẽ đủ. Thậm chí một$O(n^2)$Cách tiếp cận này sẽ trôi qua một cách thoải mái, nhưng cấu trúc của vấn đề chỉ ra rõ ràng rằng chỉ cần một giải pháp tham lam là đủ. 

Một quan sát ngây thơ nhưng quan trọng là việc xóa bên ngoài các dòng`'x'`không bao giờ hữu ích cho hạn chế này. Chỉ có các chuỗi liên tiếp`'x'`các ký tự đều quan trọng vì mọi khuôn mẫu bị cấm đều phải hoàn toàn xuất phát từ chúng. 

Một số trường hợp đặc biệt quan trọng về mặt khái niệm. 

Một trường hợp là một chuỗi như`"xx"`hoặc`"axxa"`. Không cần xóa vì không có độ dài 3. Câu trả lời đúng là 0. 

Một trường hợp khác là`"xxx"`. Chúng ta phải xóa ít nhất một ký tự để ngắt chuỗi, vì vậy câu trả lời là 1. 

Một trường hợp dài hơn một chút như`"xxxx"`yêu cầu xóa ít nhất một ký tự để giảm bất kỳ lần chạy kết quả nào`'x'`tối đa là 2. Ví dụ: loại bỏ một ký tự sẽ mang lại`"xxx"`, nhưng điều đó vẫn không hợp lệ, vì vậy cần phải xóa ít nhất một lần nữa để tạo ra câu trả lời 2. 

Một cách tiếp cận bất cẩn thường thất bại khi chỉ kiểm tra xem`"xxx"`tồn tại và xóa một ký tự ở đâu đó mà không xem xét các lần chạy dài hơn. Điều đó hoạt động với độ dài chính xác là 3 nhưng bị hỏng ngay lập tức đối với độ dài 4 trở lên. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử xóa tất cả các tập hợp con của các vị trí, xây dựng lại chuỗi kết quả và kiểm tra xem nó có chứa`"xxx"`. Điều này đúng nhưng nói chung hoàn toàn không khả thi vì có$2^n$tập hợp con, cho$n = 100$có kích thước lớn về mặt thiên văn. 

Cấu trúc của ràng buộc làm cho vấn đề trở nên cục bộ: chỉ các khối liên tiếp của`'x'`vấn đề. Thay vì nghĩ đến việc xóa toàn cục, chúng ta có thể xử lý chuỗi từ trái sang phải và xử lý từng đoạn tối đa của chuỗi liên tiếp.`'x'`một cách độc lập. 

Bên trong một khối$k$liên tiếp`'x'`ký tự, chúng tôi muốn đảm bảo rằng chỉ còn lại tối đa 2 ký tự. Nếu như$k \le 2$, không có gì cần phải được loại bỏ. Nếu như$k \ge 3$, chúng ta phải xóa chính xác$k - 2$vì việc giữ nhiều hơn 2 ký tự sẽ vi phạm điều kiện và việc giữ ít hơn 2 ký tự sẽ chỉ làm tăng số lần xóa không cần thiết. 

Thông tin chi tiết quan trọng là việc xóa một khối sẽ không ảnh hưởng đến bất kỳ khối nào khác. Do đó, chiến lược tối ưu chỉ đơn giản là tính tổng số dư trên 2 cho mỗi lần chạy`'x'`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét chuỗi một lần trong khi theo dõi liên tiếp`'x'`nhân vật. 

1. Khởi tạo bộ đếm cho câu trả lời và một biến để theo dõi thời lượng chạy hiện tại của`'x'`. Độ dài chạy này biểu thị số lượng liên tiếp`'x'`những nhân vật chúng ta đã thấy cho đến nay. 
2. Lặp lại từng ký tự trong chuỗi. 
3. Nếu ký tự hiện tại là`'x'`, tăng chiều dài chạy. Điều này mở rộng khối hiện tại. 
4. Nếu ký tự hiện tại không`'x'`, chúng tôi hoàn thiện khối trước đó. Nếu thời lượng chạy ít nhất là 3, chúng tôi thêm`run_length - 2`để trả lời vì chỉ có hai`'x'`các ký tự có thể vẫn còn mà không vi phạm quy tắc. Sau đó đặt lại độ dài chạy về 0. 
5. Sau khi kết thúc vòng lặp, chúng ta phải xử lý khối cuối cùng nếu chuỗi kết thúc bằng`'x'`. Áp dụng quy tắc tương tự: bất kỳ lần chạy nào dài hơn 2 đều góp phần`run_length - 2`việc xóa. 

Tại sao nó hoạt động dựa trên sự bất biến về cấu trúc: tại mọi thời điểm, chúng tôi tính toán đầy đủ các thao tác xóa cần thiết trong tất cả các lần chạy liên tiếp đã hoàn thành`'x'`các ký tự và không có ký tự nào tương tác với ký tự khác. Mỗi khối là độc lập vì mẫu bị cấm chỉ phụ thuộc vào lân cận và việc xóa bên trong một khối không thể tạo hoặc sửa các vi phạm trong khối khác. Do đó, việc tính tổng các bản sửa lỗi tối ưu cục bộ mang lại kết quả tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = input().strip()

ans = 0
run = 0

for ch in s:
    if ch == 'x':
        run += 1
    else:
        if run >= 3:
            ans += run - 2
        run = 0

# finalize last run
if run >= 3:
    ans += run - 2

print(ans)
```Việc triển khai giữ một bộ đếm chạy duy nhất`run`liên tiếp`'x'`nhân vật. Chi tiết quan trọng là lần xóa cuối cùng sau vòng lặp, vì một chuỗi kết thúc bằng`'x'`nếu không sẽ để phân đoạn cuối cùng của nó chưa được xử lý. 

Một lỗi phổ biến là quên bước cuối cùng này, dẫn đến việc đếm thiếu khi chuỗi kết thúc bằng một chuỗi dài các chữ số.`'x'`. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`"xxxiii"`. 

Chúng tôi theo dõi hoạt động của`'x'`: 

| Chỉ mục | Char | Chiều dài chạy | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | x | 1 | tiếp tục | 0 | 
| 2 | x | 2 | tiếp tục | 0 | 
| 3 | x | 3 | vẫn mở | 0 | 
| 4 | tôi | 0 | cộng 3-2=1 | 1 | 
| 5 | tôi | 0 | tiếp tục | 1 | 
| 6 | tôi | 0 | tiếp tục | 1 | 

Câu trả lời cuối cùng là 1. 

Điều này cho thấy rằng chỉ cần một lần xóa để phá vỡ ba lần chạy duy nhất`'x'`nhân vật. 

Bây giờ hãy xem xét`"xxxx"`. 

| Chỉ mục | Char | Chiều dài chạy | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | x | 1 | tiếp tục | 0 | 
| 2 | x | 2 | tiếp tục | 0 | 
| 3 | x | 3 | tiếp tục | 0 | 
| 4 | x | 4 | kết thúc chuỗi kích hoạt tuôn ra | 4-2=2 | 

Câu trả lời cuối cùng là 2. 

Điều này xác nhận rằng các lần chạy dài hơn yêu cầu giảm tỷ lệ thay vì chỉ xóa một ký tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ký tự được xử lý một lần trong một lần chuyển | 
| Không gian |$O(1)$| Chỉ duy trì một số lượng quầy không đổi | 

Kích thước đầu vào tối đa là 100, do đó quá trình quét tuyến tính diễn ra nhanh chóng trong giới hạn và thậm chí chi phí hệ số không đổi cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input().strip())
    s = input().strip()

    ans = 0
    run_len = 0

    for ch in s:
        if ch == 'x':
            run_len += 1
        else:
            if run_len >= 3:
                ans += run_len - 2
            run_len = 0

    if run_len >= 3:
        ans += run_len - 2

    return str(ans)

# provided samples
assert run("6\nxxxiii\n") == "1"

# custom cases
assert run("3\nxxx\n") == "1"
assert run("4\nxxxx\n") == "2"
assert run("5\naxxxa\n") == "1"
assert run("2\nxx\n") == "0"
assert run("7\nxxxxxxx\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"xxx"`|`1`| trường hợp cấm tối thiểu | 
|`"xxxx"`|`2`| xử lý lâu hơn | 
|`"axxxa"`|`1`| xử lý khối nội bộ | 
|`"xx"`|`0`| không cần xóa | 
|`"xxxxxxx"`|`5`| độ chính xác lớn | 

## Vỏ cạnh 

Đối với một chuỗi như`"axxxxa"`, thuật toán cô lập hoạt động nội bộ của bốn`'x'`nhân vật. Khi quét, nó xây dựng thời lượng chạy là 4, sau đó đặt lại khi chạm vào`'a'`, thêm`4 - 2 = 2`để trả lời. Xung quanh`'a'`ký tự không ảnh hưởng đến tính toán. 

Vì`"xxxxxxx"`, lượt chạy tăng lên 7 và chỉ được xử lý ở cuối. Lần xả cuối cùng thêm vào`7 - 2 = 5`, xử lý chính xác khối liên tục trong trường hợp xấu nhất. 

Cả hai trường hợp đều xác nhận tính bất biến mà mỗi cực đại`'x'`phân đoạn được giảm độc lập xuống độ dài tối đa là 2, với số lần xóa tối thiểu.
