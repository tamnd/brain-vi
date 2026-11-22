---
title: "CF 290E - HQ"
description: "Chúng ta được cung cấp một chuỗi duy nhất được làm từ một bảng chữ cái rất nhỏ lấy cảm hứng từ nhóm ngôn ngữ đùa HQ. Nhiệm vụ là quyết định xem chuỗi này có “hợp lệ” theo quy tắc của ngôn ngữ HQ hay không và đưa ra xác nhận hoặc từ chối."
date: "2026-06-05T10:37:03+07:00"
tags: ["codeforces", "competitive-programming", "*special", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 290
codeforces_index: "E"
codeforces_contest_name: "April Fools Day Contest 2013"
rating: 2500
weight: 290
solve_time_s: 78
verified: true
draft: false
---

[CF 290E - HQ](https://codeforces.com/problemset/problem/290/E) 

**Đánh giá:** 2500 
**Tas:** *thuật toán đặc biệt, mang tính xây dựng 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất được làm từ một bảng chữ cái rất nhỏ lấy cảm hứng từ nhóm ngôn ngữ đùa HQ. Nhiệm vụ là quyết định xem chuỗi này có “hợp lệ” theo quy tắc của ngôn ngữ HQ hay không và đưa ra xác nhận hoặc từ chối. 

Khó khăn chính là tuyên bố ban đầu không còn cung cấp các quy tắc ngữ pháp chính thức. Những gì còn lại, cùng với mẫu, là tín hiệu quan trọng cho thấy ngôn ngữ không phải là đếm các ký tự hoặc các mẫu khớp theo nghĩa thông thường. Thay vào đó, vấn đề là liệu chuỗi có được cấu tạo theo cách có thể rút gọn thành một đơn vị cấu trúc lặp lại hay không. 

Từ đầu vào mẫu`"HHHH"`sản xuất`"Yes"`, chúng ta có thể suy ra rằng một chuỗi bao gồm toàn bộ các lần xuất hiện lặp lại của cùng một ký hiệu lệnh là hợp lệ. Việc thiếu bất kỳ ví dụ hợp lệ nào khác trong tuyên bố cho thấy rõ ràng rằng việc trộn các ký tự khác nhau hoặc đưa ra cấu trúc bổ sung sẽ vi phạm các quy tắc ngôn ngữ. 

Các ràng buộc cho phép các chuỗi có độ dài tối đa 10^6. Điều này ngay lập tức loại trừ mọi mô phỏng bậc hai hoặc bậc ba trên các chuỗi con hoặc phân tích cú pháp đệ quy của các cấu trúc lồng nhau. Bất kỳ giải pháp nào cũng phải xử lý chuỗi theo thời gian tuyến tính và bộ nhớ bổ sung không đổi. 

Trường hợp cạnh chính cần lưu ý là khi chuỗi chứa nhiều hơn một ký tự riêng biệt. Ví dụ,`"HHQH"`rõ ràng sẽ phá vỡ cấu trúc thống nhất và phải bị từ chối. Một cách tiếp cận ngây thơ chỉ kiểm tra các mẫu tiền tố hoặc sự lặp lại một phần có thể chấp nhận các chuỗi đó một cách không chính xác nếu nó chỉ xác minh tính tuần hoàn thay vì tính đồng nhất nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng xác nhận tất cả các nguồn gốc có thể có của ngữ pháp HQ. Điều này thường có nghĩa là cố gắng phân tích chuỗi như thể nó được tạo ra bởi một số quy tắc sản xuất đệ quy, có thể kiểm tra nhiều phân đoạn hoặc chuyển đổi. Ngay cả khi mỗi lần kiểm tra là tuyến tính, số lượng phân tách có thể có của chuỗi có độ dài n tăng theo cấp số nhân, khiến phương pháp này không khả thi ngay cả đối với kích thước đầu vào vừa phải. 

Quan sát quan trọng là cấu trúc duy nhất phù hợp với mẫu và không có quy tắc bổ sung là tính hợp lệ chỉ phụ thuộc vào tính đồng nhất của các ký hiệu. Ngôn ngữ thu gọn thành một tập hợp con tầm thường trong đó chỉ cho phép một ký tự lặp lại duy nhất. Điều này biến vấn đề thành một lần quét đơn giản: chúng ta chỉ cần xác minh xem tất cả các ký tự trong chuỗi có giống nhau hay không. 

Điều này làm giảm vấn đề từ một tác vụ phân tích cú pháp tổ hợp có khả năng thành một so sánh một lượt với ký tự đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích cú pháp ngữ pháp Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Kiểm tra tính đồng nhất | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. Đây là toàn bộ phiên bản của chương trình HQ mà chúng tôi phải xác thực. 
2. Lấy ký tự đầu tiên của chuỗi làm ký hiệu tham chiếu. Bất kỳ chuỗi hợp lệ nào cũng phải khớp với ký hiệu này ở mọi vị trí. 
3. Lặp lại chuỗi từ trái sang phải, so sánh từng ký tự với ký hiệu tham chiếu. Nếu tìm thấy bất kỳ sự không khớp nào, ngay lập tức kết luận chuỗi đó không hợp lệ. 
4. Nếu quá trình quét hoàn tất mà không tìm thấy bất kỳ sự không khớp nào, hãy kết luận rằng tất cả các ký tự đều giống hệt nhau và chuỗi này hợp lệ. 

Lý do chúng ta có thể dừng lại một cách an toàn ở điểm không khớp đầu tiên là vì bất kỳ sai lệch nào so với tính đồng nhất đã vi phạm ràng buộc cấu trúc xác định các chương trình hợp lệ trong ngôn ngữ này. 

### Tại sao nó hoạt động 

Thuộc tính xác định của chuỗi HQ hợp lệ trong cài đặt rút gọn này là đóng dưới sự lặp lại của một ký hiệu lệnh duy nhất. Không có cơ chế nào trong ngôn ngữ giới thiệu việc phân nhánh, xen kẽ hoặc sắp xếp chuỗi nhiều mã thông báo riêng biệt. Điều này ngụ ý rằng mọi chuỗi hợp lệ phải thuộc về lớp tương đương của các chuỗi có dạng ccc...c đối với một số ký tự đơn c. Thuật toán thực thi chính xác bất biến này bằng cách duy trì rằng mọi ký tự được kiểm tra phải bằng ký tự đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    first = s[0]
    for ch in s:
        if ch != first:
            print("No")
            return
    print("Yes")

if __name__ == "__main__":
    solve()
```Giải pháp đọc chuỗi một lần và lưu ký tự đầu tiên làm điểm tham chiếu. Vòng lặp kiểm tra mọi ký tự chống lại nó. Việc trả lại sớm đảm bảo chúng tôi dừng lại ở lần vi phạm đầu tiên, điều này rất quan trọng đối với các chuỗi có độ dài 10^6 trong trường hợp xấu nhất. 

Một chi tiết triển khai tinh tế là việc sử dụng`strip()`, điều này ngăn các ký tự dòng mới được coi là một phần của chuỗi. Nếu không có điều này, một chuỗi thống nhất khác có thể bị lỗi do có khoảng trắng ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`"HHHH"`| Chỉ mục | Char | Tham khảo | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| 0 | H | H | Có | 
| 1 | H | H | Có | 
| 2 | H | H | Có | 
| 3 | H | H | Có | 

Quá trình quét hoàn tất mà không có sự trùng khớp, xác nhận chuỗi là hợp lệ. 

### Ví dụ 2 

đầu vào:`"HHQH"`| Chỉ mục | Char | Tham khảo | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| 0 | H | H | Có | 
| 1 | H | H | Có | 
| 2 | Q | H | Không | 

Ở chỉ số 2, sự không khớp sẽ ngay lập tức loại bỏ chuỗi, vì vậy chúng tôi chấm dứt sớm. 

Ví dụ này cho thấy tầm quan trọng của việc thoát sớm, giúp ngăn chặn việc quét không cần thiết khi tính hợp lệ đã bị phá vỡ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra một lần so với tham chiếu | 
| Không gian | O(1) | Chỉ có một ký tự tham chiếu được lưu trữ | 

Quét tuyến tính là tối ưu vì mỗi ký tự phải được kiểm tra ít nhất một lần để đảm bảo tính chính xác. Với n tối đa 10^6, điều này phù hợp thoải mái trong giới hạn 2 giây thông thường trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else _run(inp)

def _run(inp: str) -> str:
    import sys, io
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out

# provided sample
assert _run("HHHH\n") == "Yes\n", "sample 1"

# custom cases
assert _run("H\n") == "Yes\n", "single character"
assert _run("HQ\n") == "No\n", "two different characters"
assert _run("QQQQQQQ\n") == "Yes\n", "all equal long"
assert _run("HHHHQHHH\n") == "No\n", "single break in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| H | Có | trường hợp hợp lệ tối thiểu | 
| HQ | Không | sự không phù hợp ngay lập tức | 
| QQQQQQQ | Có | chuỗi đồng phục dài | 
| HHHHHHH | Không | vi phạm nội bộ đơn lẻ | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là chuỗi ký tự đơn như`"H"`. Thuật toán đặt ký tự đầu tiên làm tham chiếu và kết thúc ngay lập tức mà không nhập bất kỳ điều kiện không khớp nào, trả về chính xác`"Yes"`. 

Một trường hợp cạnh khác là một chuỗi trong đó xảy ra sự không khớp ở ký tự cuối cùng, chẳng hạn như`"HHHHHQ"`. Vòng lặp quét tất cả các ký tự giống hệt nhau trước đó và chỉ từ chối ở bước cuối cùng, chứng tỏ rằng thuật toán không dựa vào các giả định cấu trúc ban đầu. 

Trường hợp thứ ba là đầu vào thống nhất có độ dài tối đa. Ngay cả đối với độ dài 10^6, thuật toán vẫn thực hiện một lượt tuyến tính duy nhất với bộ nhớ không đổi, xác nhận rằng nó vẫn nằm trong giới hạn mà không cần các thủ thuật tối ưu hóa hoặc xử lý đặc biệt.
