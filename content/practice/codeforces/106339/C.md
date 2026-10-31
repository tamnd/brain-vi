---
title: "CF 106339C - Đường Xa Lộ Mờ"
description: "Chúng ta được cung cấp một đường cao tốc được thể hiện dưới dạng một chuỗi các điểm đánh dấu. Mỗi điểm đánh dấu có một giá trị chiều cao cố định và một chỉ mục trong mảng."
date: "2026-06-19T08:50:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106339
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 1-28-2026"
rating: 0
weight: 106339
solve_time_s: 49
verified: true
draft: false
---

[CF 106339C - Đường cao tốc băng giá](https://codeforces.com/problemset/problem/106339/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường cao tốc được thể hiện dưới dạng một chuỗi các điểm đánh dấu. Mỗi điểm đánh dấu có một giá trị chiều cao cố định và một chỉ mục trong mảng. Thao tác chính không trực tiếp về độ cao mà về mối quan hệ giữa từng vị trí và giá trị của nó: cho mọi vị trí`i`, ta xét ước chung lớn nhất của`h[i]`Và`i`. 

Từ đó, chúng ta xác định một mảng nhị phân dẫn xuất trên cùng các chỉ số. Giá trị tại vị trí`i`là`1`nếu như`gcd(h[i], i) = 1`, Và`0`nếu không thì. Sau đó, mỗi truy vấn sẽ yêu cầu tổng số vị trí trong một khoảng nhất định`[l, r]`nơi điều kiện này xảy ra. 

Do đó, cấu trúc đầu vào là một mảng tĩnh theo sau là nhiều truy vấn tổng phạm vi trên một mảng chỉ báo dẫn xuất. Vì độ cao không bao giờ thay đổi nên tất cả quá trình tiền xử lý có thể được thực hiện một lần trước khi trả lời truy vấn. 

Các ràng buộc ngụ ý rằng cả kích thước mảng và số lượng truy vấn đều đủ lớn để việc tính toán lại gcds hoặc phạm vi quét cho mỗi truy vấn sẽ quá chậm. Giới hạn trên điển hình như`n, q up to 2e5`có nghĩa là một`O(nq)`Cách tiếp cận này sẽ thực hiện theo thứ tự 40 tỷ hoạt động, điều này không khả thi. Thậm chí`O(n sqrt(max h))`lặp lại cho mỗi truy vấn sẽ thất bại. 

Một trường hợp phức tạp xuất phát từ việc lập chỉ mục. Vì gcd được tính toán bằng chỉ mục, không chỉ giá trị mảng, nên rất dễ xuất hiện các lỗi riêng lẻ. Ví dụ, ở vị trí`i = 1`,`gcd(h[1], 1)`luôn luôn là`1`, nên vị trí đầu tiên luôn được tính bất kể chiều cao. Bất kỳ việc triển khai nào quên lập chỉ mục dựa trên 1 hoặc dịch chuyển mảng không chính xác sẽ âm thầm tạo ra kết quả sai. 

Một trường hợp cạnh khác phát sinh khi chiều cao lớn hoặc bằng chỉ số. Ví dụ, nếu`h[i] = i`, sau đó`gcd(h[i], i) = i`, vậy chỉ`i = 1`đóng góp`1`. Điều này kiểm tra tính đúng đắn của điều kiện gcd và đảm bảo chúng ta không vô tình kiểm tra sự bằng nhau thay vì gcd. 

## Phương pháp tiếp cận 

Cách trực tiếp để giải quyết vấn đề là xử lý từng truy vấn một cách độc lập. Đối với một truy vấn`[l, r]`, chúng tôi lặp lại tất cả các chỉ số trong phạm vi và tính toán`gcd(h[i], i)`mỗi lần, tăng bộ đếm khi kết quả là`1`. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa của mảng dẫn xuất. 

Tuy nhiên, chi phí tính toán của mỗi gcd`O(log min(h[i], i))`và thực hiện điều này cho mọi vị trí trong mọi truy vấn sẽ dẫn đến khoảng`O(nq log max h)`hoạt động. Với những ràng buộc lớn, điều này nhanh chóng trở nên quá chậm. 

Quan sát quan trọng là không có gì thay đổi sau khi tiền xử lý. Mọi vị trí đều được đánh giá độc lập theo một trong hai`0`hoặc`1`và giá trị này không bao giờ thay đổi trong các truy vấn. Điều này có nghĩa là chúng ta có thể tính toán trước toàn bộ mảng dẫn xuất một lần. Sau đó, mỗi truy vấn sẽ trở thành một bài toán tổng phạm vi đơn giản trên một mảng tĩnh. 

Khi vấn đề được giảm xuống thành các truy vấn tổng phạm vi trên một mảng cố định, tổng tiền tố sẽ trở thành công cụ tự nhiên. Bằng cách lưu trữ số lượng tích lũy các vị trí hợp lệ, mỗi truy vấn có thể được trả lời trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(n log h) cho mỗi truy vấn | O(1) | Quá chậm | 
| Tiền xử lý tổng tiền tố | O(n log h + n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng chiều cao và lưu ý rằng các chỉ số được cố định và sử dụng trực tiếp trong tính toán gcd. Điều này chứng tỏ rằng việc xử lý trước là có thể thực hiện được vì không có cập nhật nào xảy ra. 
2. Xây dựng mảng trợ giúp`good`, trong đó mỗi vị trí`i`được giao`1`nếu như`gcd(h[i], i) == 1`, nếu không thì`0`. Điều này chuyển đổi vấn đề thành một mảng nhị phân. Phép biến đổi hợp lệ vì mọi truy vấn chỉ phụ thuộc vào việc đếm xem có bao nhiêu vị trí thỏa mãn điều kiện. 
3. Tính mảng tổng tiền tố`pref`, Ở đâu`pref[i]`lưu trữ tổng số`1`ở trong`good[1..i]`. Điều này cho phép bất kỳ tổng khoảng nào được tính bằng phép trừ. 
4. Đối với mỗi truy vấn`[l, r]`, trở lại`pref[r] - pref[l - 1]`. Điều này trực tiếp đếm có bao nhiêu vị trí hợp lệ nằm trong khoảng mà không cần quét nó. 
5. Xuất ra tất cả các câu trả lời truy vấn. 

### Tại sao nó hoạt động 

Mỗi vị trí đóng góp độc lập vào việc nó có được tính hay không và đóng góp đó được xác định đầy đủ trước khi xử lý bất kỳ truy vấn nào. Mảng tổng tiền tố duy trì tổng số chính xác của những đóng góp này. Bất kỳ truy vấn tổng phạm vi nào trên một mảng có tổng tiền tố đều có thể phân tách thành hai trạng thái tích lũy, do đó phép trừ sẽ tách chính xác phân đoạn cần thiết mà không cần tính toán lại hoặc chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

def solve():
    n, q = map(int, input().split())
    h = list(map(int, input().split()))
    
    good = [0] * (n + 1)
    
    for i in range(1, n + 1):
        if gcd(h[i - 1], i) == 1:
            good[i] = 1
    
    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + good[i]
    
    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append(str(pref[r] - pref[l - 1]))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thuật toán chính xác. Việc chuyển đổi từ lập chỉ mục Python dựa trên 0 sang lập chỉ mục logic dựa trên 1 được xử lý bằng cách truy cập`h[i - 1]`. Mảng tiền tố có kích thước`n + 1`để có thể`pref[0]`hỗ trợ các truy vấn bắt đầu từ chỉ mục một cách tự nhiên`1`không có trường hợp đặc biệt. Mỗi truy vấn được trả lời theo thời gian không đổi bằng cách sử dụng phép trừ tổng tiền tố. 

Một lỗi triển khai phổ biến là quên rằng chỉ mục`i`bắt đầu từ 1 trong điều kiện gcd. sử dụng`h[i]`thay vì`h[i - 1]`thay đổi tất cả các tính toán và phá vỡ sự chính xác một cách âm thầm. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ trong đó`n = 5`Và`h = [2, 3, 4, 5, 6]`với các truy vấn`[1,3]`Và`[2,5]`. 

Đầu tiên chúng ta tính toán`good`mảng: 

| tôi | h[i] | gcd(h[i], i) | tốt[i] | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 
| 3 | 4 | 1 | 1 | 
| 4 | 5 | 1 | 1 | 
| 5 | 6 | 1 | 1 | 

Bây giờ tổng tiền tố: 

| tôi | trước[i] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 
| 5 | 5 | 

Truy vấn`[1,3]`cho`pref[3] - pref[0] = 3`. 

Truy vấn`[2,5]`cho`pref[5] - pref[1] = 5 - 1 = 4`. 

Ví dụ này cho thấy trường hợp rõ ràng nhất trong đó tất cả các vị trí đều thỏa mãn điều kiện, xác minh rằng tổng tiền tố tích lũy chính xác. 

Bây giờ hãy xem xét một trường hợp tương phản:`n = 5`,`h = [1, 2, 3, 4, 5]`. 

| tôi | h[i] | gcd(h[i], i) | tốt[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 0 | 
| 3 | 3 | 3 | 0 | 
| 4 | 4 | 4 | 0 | 
| 5 | 5 | 5 | 0 | 

Tổng tiền tố trở thành`[0,1,1,1,1,1]`. Truy vấn`[1,5]`trả lại`1`, xác nhận rằng chỉ số duy nhất`1`đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log h + q) | Mỗi phép tính gcd tốn thời gian logarit, được thực hiện một lần cho mỗi chỉ mục và mỗi truy vấn là O(1) | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng nhị phân và tổng tiền tố | 

Quá trình tiền xử lý chiếm ưu thế trong thời gian chạy, nhưng trong thực tế nó là tuyến tính ngoài các lệnh gọi gcd. Với các ràng buộc điển hình, điều này phù hợp thoải mái trong giới hạn vì cả hai`n`Và`q`được xử lý trong tổng công việc tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        n, q = map(int, input().split())
        h = list(map(int, input().split()))
        good = [0] * (n + 1)
        for i in range(1, n + 1):
            if math.gcd(h[i - 1], i) == 1:
                good[i] = 1
        pref = [0] * (n + 1)
        for i in range(1, n + 1):
            pref[i] = pref[i - 1] + good[i]
        out = []
        for _ in range(q):
            l, r = map(int, input().split())
            out.append(str(pref[r] - pref[l - 1]))
        return "\n".join(out)

    return solve()

# sample-style checks
assert run("5 2\n2 3 4 5 6\n1 3\n2 5") == "3\n4"

# minimum input
assert run("1 1\n7\n1 1") == "1"

# all equal values
assert run("4 2\n2 2 2 2\n1 4\n2 3") == "2\n1"

# increasing values
assert run("5 1\n1 2 3 4 5\n1 5") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | Tính chính xác của việc lập chỉ mục trường hợp cơ sở | 
| tất cả đều bình đẳng | 2, 1 | ổn định hành vi gcd | 
| tăng giá trị | 1 | đảm bảo chỉ tính gcd=1 trường hợp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`n = 1`. Đầu vào có thể có một chiều cao duy nhất và chỉ mục duy nhất là`1`. Từ`gcd(h[1], 1)`luôn luôn là`1`, đầu ra phải luôn là`1`cho bất kỳ truy vấn hợp lệ. Thuật toán xử lý việc này một cách chính xác vì mảng tiền tố có kích thước`2`, với`pref[1] = good[1]`và các truy vấn giảm xuống còn`pref[1] - pref[0]`. 

Một trường hợp cạnh khác là khi tất cả các độ cao là bội số của chỉ số của chúng. Ví dụ`h[i] = 2 * i`. Sau đó`gcd(2i, i) = i`, vậy chỉ`i = 1`đóng góp. Tổng tiền tố trở thành`[0,1,1,1,...]`và bất kỳ truy vấn nào cũng chỉ tính chính xác vị trí đầu tiên. 

Trường hợp tinh tế cuối cùng là khi độ cao rất lớn nhưng chỉ số vẫn nhỏ. Vì gcd được tính toán bằng chỉ mục nên các giá trị lớn không ảnh hưởng đáng kể đến độ chính xác hoặc hiệu suất. Thuật toán vẫn chạy trong`O(log h)`trên mỗi phần tử và tổng tiền tố vẫn không bị ảnh hưởng bởi độ lớn.
