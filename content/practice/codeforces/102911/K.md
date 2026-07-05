---
title: "CF 102911K - Kallipolis"
description: "Chúng ta được cung cấp một danh sách các triết gia, mỗi người được gắn với một giá trị nguyên dương đại diện cho “sức mạnh thống trị” của họ."
date: "2026-07-04T08:06:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102911
codeforces_index: "K"
codeforces_contest_name: "2021 Ateneo de Manila Senior High School Dagitab Programming Contest (Mirror)"
rating: 0
weight: 102911
solve_time_s: 34
verified: true
draft: false
---

[CF 102911K - Kallipolis](https://codeforces.com/problemset/problem/102911/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một danh sách các triết gia, mỗi người được gắn với một giá trị nguyên dương đại diện cho “sức mạnh thống trị” của họ. Một triết gia được cho là thống trị người khác nếu giá trị của họ chia hết cho giá trị của người kia, nghĩa là giá trị lớn hơn phải là bội số chính xác của giá trị nhỏ hơn. 

Nhiệm vụ là xác định liệu có tồn tại một triết gia thống trị mọi triết gia khác trong danh sách hay không. Nếu có nhiều ứng viên thỏa mãn điều kiện này thì ta chọn ứng viên có chỉ số nhỏ nhất. 

Đầu vào chỉ đơn giản là số lượng các triết gia theo sau là giá trị của họ. Đầu ra là chỉ số dựa trên 1 của một “vua” hợp lệ hoặc -1 nếu không có triết gia nào như vậy tồn tại. 

Các ràng buộc cho phép các giá trị lên tới 10^9 và tối đa 2·10^5 triết gia. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào kiểm tra trực tiếp từng cặp triết gia, vì đó sẽ là O(n^2), có thể đạt tới 4·10^10 phép tính trong trường hợp xấu nhất và rõ ràng vượt quá giới hạn thời gian. 

Một trường hợp phức tạp xuất hiện khi nhiều triết gia có cùng giá trị. Vì bất kỳ số nào cũng tự chia, nên các giá trị giống hệt nhau tạo thành sự thống trị lẫn nhau và chỉ số sớm nhất trong số chúng sẽ trở thành câu trả lời nếu giá trị đó cũng lấn át tất cả các số khác. Ví dụ: nếu tất cả các giá trị là 224 thì mọi triết gia đều chiếm ưu thế hơn nhau, vì vậy câu trả lời là 1. 

Một trường hợp góc khác là khi không có một giá trị nào chia hết cho tất cả các giá trị khác, ngay cả khi nó rất lớn. Ví dụ: trong mảng [6, 10, 15], không có số nào chia hết cả hai số còn lại, vì vậy kết quả đầu ra đúng là -1 mặc dù tồn tại nhiều giá trị “lớn”. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mỗi triết gia với tư cách là một vị vua ứng cử viên và kiểm tra xem giá trị của họ có chia hết cho mọi giá trị khác hay không. Đối với mỗi ứng cử viên i, chúng tôi quét tất cả j và xác minh Ai % Aj == 0. Điều này tốn tổng cộng O(n^2) việc kiểm tra tính chia hết. Mỗi lần kiểm tra có thời gian không đổi, nhưng với n lên tới 2·10^5 thì điều này trở nên không khả thi. 

Quan sát quan trọng là vua hợp lệ phải là giá trị lớn nhất trong mảng, bởi vì nếu Ai thống trị tất cả những người khác thì Ai phải chia hết cho mọi Aj, ngụ ý Ai ≥ Aj cho tất cả j. Vì vậy, bất kỳ ứng cử viên nào cũng phải đến từ tập hợp các giá trị tối đa. 

Điều này làm giảm đáng kể không gian tìm kiếm. Thay vì kiểm tra tất cả các chỉ số, chúng tôi chỉ kiểm tra những chỉ số có giá trị lớn nhất. Vì tất cả các giá trị cực đại bằng nhau đều hành xử giống hệt nhau về mặt thống trị, nên chúng ta chỉ cần kiểm tra một trong số chúng, lý tưởng nhất là lần xuất hiện đầu tiên. 

Sau đó, chúng tôi xác minh xem giá trị tối đa này có chia hết mọi phần tử trong mảng hay không. Nếu có thì đó là một vị vua hợp pháp. Nếu không thì không có ứng cử viên nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Kiểm tra ứng viên tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn vấn đề thành việc kiểm tra xem liệu một giá trị có thể chia hết tất cả những giá trị khác hay không. 

1. Quét mảng một lần để tìm giá trị lớn nhất và chỉ số đầu tiên của nó. Lý do là bất kỳ vị vua hợp lệ nào cũng phải có kích thước ít nhất bằng tất cả những vị vua khác, vì vậy chỉ những giá trị tối đa mới đủ điều kiện. 
2. Duyệt mảng một lần nữa và kiểm tra tính chia hết của từng phần tử cho giá trị lớn nhất. Nếu tìm thấy yếu tố nào không chia hết được, chúng ta có thể loại bỏ ngay ứng cử viên này vì ưu thế đòi hỏi phải bao trùm mọi triết gia. 
3. Nếu tất cả các lần kiểm tra đều đạt, hãy trả về chỉ mục được lưu trữ của phần tử tối đa đầu tiên. 
4. Nếu bất kỳ kiểm tra nào không thành công, hãy trả về -1. 

Quyết định quan trọng là hạn chế sự chú ý đến phần tử tối đa, loại bỏ nhu cầu so sánh theo cặp. 

### Tại sao nó hoạt động

Nếu một giá trị A_k lấn át mọi giá trị khác thì với mỗi i chúng ta phải có A_k = m · A_i cho một số nguyên m, suy ra A_k ≥ A_i với mọi i. Điều này buộc A_k phải đạt mức tối đa toàn cầu. Trong số các cực đại bằng nhau thì khả năng chia hết là như nhau nên việc chọn chỉ số nhỏ nhất đảm bảo tính đúng đắn theo quy tắc tie-break. Do đó, thuật toán kiểm tra trực tiếp ứng cử viên cấu trúc có thể duy nhất và xác minh điều kiện chia hết cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    mx = max(a)
    idx = -1
    
    for i in range(n):
        if a[i] == mx:
            idx = i + 1
            break
    
    for x in a:
        if mx % x != 0:
            print(-1)
            return
    
    print(idx)

if __name__ == "__main__":
    solve()
```Lần đầu tiên tính giá trị lớn nhất và ghi lại vị trí sớm nhất của nó. Điều này là cần thiết vì đầu ra phụ thuộc vào chỉ số chứ không chỉ giá trị. 

Bước thứ hai thực thi điều kiện thống trị bằng cách kiểm tra khả năng chia hết từ mức tối đa ứng cử viên cho mọi phần tử. Điều kiện mx % x == 0 là bản dịch trực tiếp của yêu cầu mx là bội số của mọi x. 

Việc thoát khỏi lỗi sớm sẽ ngăn chặn những lần kiểm tra không cần thiết, giữ cho giải pháp luôn tuyến tính. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào: 

đầu vào: 

4 

1 215 43 5 

Tối đa là 215, xảy ra ở chỉ số 2. 

| Bước | Giá trị hiện tại | giá trị 215 % | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| kiểm tra 1 | 1 | 0 | vâng | 
| kiểm tra 2 | 215 | 0 | vâng | 
| kiểm tra 3 | 43 | 0 | vâng | 
| kiểm tra 4 | 5 | 0 | vâng | 

Tất cả các giá trị đều chia hết cho 215, vì vậy câu trả lời là 2. Điều này xác nhận rằng ứng viên chiếm ưu thế chính xác ở mọi phần tử. 

Bây giờ hãy xem xét: 

đầu vào: 

4 

201 202 227 228 

Tối đa là 228 ở chỉ số 4. 

| Bước | Giá trị hiện tại | giá trị 228 % | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| kiểm tra 1 | 201 | khác không | không | 

Lỗi xảy ra ngay lập tức ở 201, vì 228 không chia hết cho 201. Điều này chứng tỏ sự kết thúc sớm và cho thấy tại sao không phải mọi mức tối đa đều hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tìm giá trị lớn nhất và một lượt để kiểm tra tính chia hết | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Với n lên tới 2·10^5, quét tuyến tính dễ dàng phù hợp với giới hạn thời gian và bộ nhớ không đổi đảm bảo không có vấn đề về chi phí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp))

# Re-implement wrapper for clarity in testing context
def solve_output(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:1+n]))
    mx = max(a)
    idx = a.index(mx) + 1
    for x in a:
        if mx % x != 0:
            return "-1"
    return str(idx)

# samples
assert solve_output("4\n1 215 43 5\n") == "2"
assert solve_output("4\n201 202 227 228\n") == "-1"

# custom cases
assert solve_output("1\n7\n") == "1"
assert solve_output("5\n224 224 224 224 224\n") == "1"
assert solve_output("3\n2 3 4\n") == "-1"
assert solve_output("4\n1 2 4 8\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp tối thiểu | 
| tất cả đều bình đẳng | 1 | xử lý cà vạt | 
| chuỗi không chia hết | -1 | trường hợp thất bại | 
| dây chuyền hoàn hảo | chỉ mục cuối cùng | vua hợp lệ mạnh mẽ | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, giá trị tối đa sẽ xuất hiện nhiều lần. Thuật toán chọn lần xuất hiện đầu tiên và vẫn vượt qua kiểm tra tính chia hết vì mọi số đều tự chia. Ví dụ: [224, 224, 224] tạo ra mx = 224 và không có lỗi nào trong lần vượt qua thứ hai, do đó chỉ số 1 được trả về. 

Khi giá trị lớn nhất xuất hiện nhiều lần nhưng các giá trị khác phá vỡ tính chia hết thì kết quả vẫn là -1. Ví dụ: [10, 10, 3] có mx = 10, nhưng 10% 3 thất bại nên không có ứng viên nào sống sót. 

Khi mảng chứa 1, nó không tự động trở thành câu trả lời trừ khi tất cả các giá trị đều bằng 1, vì 1 chỉ chiếm ưu thế. Ví dụ: [1, 2, 3] không thành công vì 1 % 2 không hợp lệ theo hướng ưu thế ngược lại.
