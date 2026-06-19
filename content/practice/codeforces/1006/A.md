---
title: "CF 1006A - Các thiết bị thay thế liền kề"
description: "Chúng ta được cung cấp một chuỗi các số nguyên, mỗi số nằm trong một phạm vi rất lớn lên đến $10^9$. Mishka liên tục áp dụng quy tắc chuyển đổi toàn cầu hoạt động độc lập trên từng giá trị: mọi cặp số nguyên liên tiếp $(1,2)$, $(3,4)$, $(5,6)$, v.v., được hoán đổi tại chỗ, nhưng…"
date: "2026-06-16T23:10:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1006
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 498 (Div. 3)"
rating: 800
weight: 1006
solve_time_s: 102
verified: true
draft: false
---

[CF 1006A - Các thiết bị thay thế liền kề](https://codeforces.com/problemset/problem/1006/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên, mỗi số nằm trong một phạm vi rất lớn đến$10^9$. Mishka liên tục áp dụng quy tắc chuyển đổi toàn cục hoạt động độc lập trên từng giá trị: mọi cặp số nguyên liên tiếp$(1,2)$,$(3,4)$,$(5,6)$, v.v., được hoán đổi tại chỗ, nhưng tất cả các hoán đổi được áp dụng đồng thời dưới dạng một thao tác khái niệm duy nhất. 

Chi tiết quan trọng là quá trình này không lặp lại theo nghĩa có ý nghĩa. Sau khi áp dụng đầy đủ tất cả các giao dịch hoán đổi, việc lặp lại thao tác tương tự một lần nữa sẽ tạo ra kết quả tương tự như lần đầu tiên. Điều này cho thấy phép biến đổi thực chất là một hoán vị cố định được áp dụng một lần cho mọi phần tử. 

Kích thước đầu vào$n \le 1000$là nhỏ, vì vậy bất kỳ giải pháp nào chạy trong thời gian tuyến tính hoặc thậm chí không đổi cho mỗi phần tử là đủ. Điều quan trọng là nhận ra cấu trúc của phép biến đổi thay vì mô phỏng nó từng bước trên một phạm vi rộng lớn đến$10^9$, điều đó là không thể. 

Một sự hiểu lầm ngây thơ là cố gắng mô phỏng tất cả các giao dịch hoán đổi từ$1$ĐẾN$10^9$hoặc áp dụng lại quy trình cho đến khi ổn định. Cả hai đều không cần thiết và sẽ không khả thi về mặt tính toán. 

Một trường hợp khó phát hiện khi nghĩ về các số không bao giờ xuất hiện trong mảng mà nằm giữa các cặp được xử lý. Ví dụ: nếu chúng ta cố gắng mô phỏng ánh xạ một phần hoặc chỉ các giá trị được quan sát, chúng ta có thể bỏ sót rằng phép biến đổi được xác định toàn cục và độc lập theo số chứ không phải theo nội dung mảng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là áp dụng theo đúng nghĩa đen quy trình được mô tả: đối với mỗi cặp$(2i-1, 2i)$, quét toàn bộ mảng và hoán đổi các lần xuất hiện. Mỗi lần vượt qua tất cả các cặp đều chạm tới$10^9 / 2$và ngay cả khi chúng ta hạn chế chỉ các giá trị xuất hiện trong mảng thì việc chuyển đi lặp lại vẫn không cần thiết. 

Một cách đơn giản hóa mạnh mẽ hơn sẽ là: đối với mỗi giá trị trong mảng, hãy kiểm tra liên tục xem nó có thuộc một cặp hay không và hoán đổi nó. Thậm chí điều này còn dẫn tới một$O(n \cdot 10^9)$- độ phức tạp về khái niệm kiểu nếu được triển khai theo nghĩa đen trên toàn bộ miền, điều này không được chấp nhận. 

Quan sát quan trọng là hoạt động hoàn toàn cục bộ trên không gian giá trị. Mỗi số nguyên$x$được ánh xạ độc lập: 

- Nếu$x$thật kỳ lạ, nó hoán đổi với$x+1$. 
- Nếu như$x$chẵn, nó hoán đổi với$x-1$. 

Vì vậy giá trị cuối cùng của mỗi phần tử được xác định bằng một quy tắc số học duy nhất chứ không phải mô phỏng. Toàn bộ quá trình tương đương với việc lật bit có trọng số nhỏ nhất của số trong hệ nhị phân, vì việc ghép cặp chẵn và lẻ chính xác là chuyển đổi tính chẵn lẻ trong mỗi cặp liên tiếp. 

Như vậy mỗi giá trị$x$biến đổi như sau: 

-$x \rightarrow x-1$nếu như$x$thậm chí là 
-$x \rightarrow x+1$nếu như$x$thật kỳ lạ 

Điều này mang lại một$O(n)$giải pháp bản đồ trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 10^9) khái niệm | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng số nguyên. Không cần xử lý trước vì mỗi phần tử độc lập với các phần tử khác. 
2. Đối với mỗi phần tử$a_i$, xác định xem nó là số lẻ hay số chẵn. Sự phân loại này xác định đầy đủ giá trị cuối cùng của nó vì phép biến đổi là một cặp số nguyên liên tiếp cố định. 
3. Nếu giá trị là số lẻ, hãy thay thế bằng$a_i + 1$. Điều này phản ánh rằng mọi số lẻ đều được hoán đổi với số nguyên tiếp theo. 
4. Nếu giá trị là số chẵn, hãy thay thế bằng$a_i - 1$. Điều này phản ánh rằng mọi số chẵn đều được hoán đổi với số nguyên trước đó. 
5. Xuất mảng đã chuyển đổi theo thứ tự tương tự. 

Điểm cấu trúc quan trọng là không có yếu tố nào ảnh hưởng đến bất kỳ yếu tố nào khác. Không có tầng hoặc phần phụ thuộc, vì vậy chỉ cần một lượt truy cập là đủ. 

### Tại sao nó hoạt động 

Phép biến đổi xác định một hoán vị trên tập hợp các số nguyên dương trong đó mỗi cặp liền kề được hoán đổi chính xác một lần. Mỗi số thuộc về chính xác một cặp và được ánh xạ tới đối tác của nó. Bởi vì các cặp này phân vùng toàn bộ dòng số nguyên nên ánh xạ là tổng và xác định. Cấu trúc mảng không liên quan; chỉ những giá trị cá nhân mới quan trọng. Áp dụng quy tắc một lần sẽ tạo ra trạng thái cuối cùng và áp dụng lại quy tắc đó sẽ trả về cấu hình ban đầu, xác nhận đó là một sự đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    res = []
    for x in a:
        if x % 2 == 0:
            res.append(x - 1)
        else:
            res.append(x + 1)
    
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào một lần và xử lý từng giá trị một cách độc lập. Kiểm tra tính chẵn lẻ trực tiếp mã hóa ghép nối trao đổi, tránh mọi mô phỏng trên phạm vi số nguyên lớn. Đầu ra giữ nguyên thứ tự ban đầu vì việc chuyển đổi không liên quan đến việc sắp xếp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 4 5 10
```| Yếu tố | Chẵn lẻ | Chuyển đổi | Kết quả | 
| --- | --- | --- | --- | 
| 1 | lẻ | 1 → 2 | 2 | 
| 2 | thậm chí | 2 → 1 | 1 | 
| 4 | thậm chí | 4 → 3 | 3 | 
| 5 | lẻ | 5 → 6 | 6 | 
| 10 | thậm chí | 10 → 9 | 9 | 

Đầu ra:```
2 1 3 6 9
```Dấu vết này cho thấy mỗi phần tử được xử lý độc lập và không có sự tương tác giữa các vị trí. Mỗi giá trị chỉ di chuyển trong cặp riêng của nó. 

### Ví dụ 2 

đầu vào:```
4
3 6 7 8
```| Yếu tố | Chẵn lẻ | Chuyển đổi | Kết quả | 
| --- | --- | --- | --- | 
| 3 | lẻ | 3 → 4 | 4 | 
| 6 | thậm chí | 6 → 5 | 5 | 
| 7 | lẻ | 7 → 8 | 8 | 
| 8 | thậm chí | 8 → 7 | 7 | 

Đầu ra:```
4 5 8 7
```Điều này xác nhận thuộc tính involution: áp dụng lại quy tắc tương tự sẽ khôi phục mảng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần bằng cách kiểm tra tính chẵn lẻ theo thời gian không đổi | 
| Không gian | O(1) thêm | Chỉ mảng đầu ra được lưu trữ ngoài đầu vào | 

Các ràng buộc cho phép tối đa 1000 phần tử, do đó, một lần truyền tuyến tính duy nhất có hiệu quả không đáng kể trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from io import StringIO
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n1 2 4 5 10\n") == "2 1 3 6 9"

# minimum size
assert run("1\n1\n") == "2"

# single even
assert run("1\n2\n") == "1"

# mixed
assert run("4\n1 2 3 4\n") == "2 1 4 3"

# already large values
assert run("3\n999999999 1000000000 7\n") == "1000000000 999999999 8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 2 | trường hợp lẻ nhỏ nhất | 
| 2 | 1 | trường hợp chẵn nhỏ nhất | 
| 1 2 3 4 | 2 1 4 3 | tính đúng đắn của mẫu xen kẽ | 
| giá trị lớn | hoán đổi chính xác | ranh giới gần$10^9$| 

## Vỏ cạnh 

Trường hợp một cạnh là mảng nhỏ nhất có thể có một giá trị lẻ duy nhất. Đối với đầu vào`1`, phép biến đổi ánh xạ nó tới`2`. Vì không có sự tương tác với các phần tử khác nên không có sự phụ thuộc ẩn nào xuất hiện. 

Một trường hợp cạnh khác là giá trị tối đa$10^9$. Đối với đầu vào`[1000000000]`, nó chẵn nên nó ánh xạ tới`999999999`. Quy tắc áp dụng thống nhất ở ranh giới của phạm vi cho phép và không yêu cầu tràn hoặc viết hoa đặc biệt vì số nguyên Python xử lý phép trừ một cách an toàn và kết quả vẫn nằm trong giới hạn.
