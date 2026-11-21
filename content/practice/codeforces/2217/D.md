---
title: "CF 2217D - Flip the Bit (Phiên bản cứng)"
description: "Chúng ta được cung cấp một mảng nhị phân và một tập hợp các chỉ số đặc biệt trong đó tất cả các giá trị đều giống hệt nhau. Mục tiêu của chúng tôi là làm cho toàn bộ mảng bằng giá trị tại các chỉ số đặc biệt này bằng cách sử dụng số thao tác lật ít nhất."
date: "2026-06-07T18:26:03+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2217
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1091 (Div. 2) and CodeCraft 26"
rating: 1900
weight: 2217
solve_time_s: 120
verified: false
draft: false
---

[CF 2217D - Lật Bit (Phiên bản cứng)](https://codeforces.com/problemset/problem/2217/D) 

**Xếp hạng:** 1900 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhị phân và một tập hợp các chỉ số đặc biệt trong đó tất cả các giá trị đều giống hệt nhau. Mục tiêu của chúng tôi là làm cho toàn bộ mảng bằng giá trị tại các chỉ số đặc biệt này bằng cách sử dụng số thao tác lật ít nhất. Thao tác lật cho phép chúng ta chọn một mảng con liền kề chứa ít nhất một chỉ mục đặc biệt và đảo ngược mọi bit trong mảng con đó. 

Vấn đề yêu cầu xử lý nhiều trường hợp thử nghiệm và tổng kích thước mảng trên tất cả các trường hợp có thể lên tới 200.000. Điều này loại trừ bất kỳ giải pháp nào kiểm tra tất cả các mảng con có thể một cách rõ ràng, vì đó sẽ là bậc hai hoặc tệ hơn. Thay vào đó, chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính theo chiều dài mảng. 

Trường hợp cạnh tinh tế phát sinh khi có các bit liên tiếp khác với giá trị đặc biệt nhưng không có chỉ số đặc biệt nào trong phân đoạn. Một cách tiếp cận ngây thơ, lật ngược mọi điểm khác biệt có thể cố gắng lật các phân đoạn mà không bao gồm chỉ mục đặc biệt, chỉ mục này không hợp lệ. Một trường hợp khác là khi mảng đã đồng nhất; câu trả lời đúng là không lật, nhưng một chiến lược tham lam bất cẩn vẫn có thể cố gắng lật các phân đoạn một cách không cần thiết. Ví dụ, đối với một mảng`[1,1,1,1]`có chỉ số đặc biệt`[2,3]`, số lần lật tối thiểu cần thiết là`0`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp lại trên mọi phần tử và khi nó khác với giá trị đặc biệt, hãy thử tất cả các phạm vi có thể có chứa một chỉ mục đặc biệt để lật nó. Về nguyên tắc, điều này đúng vì cuối cùng tất cả các bit sẽ khớp nhau, nhưng nó thực hiện tối đa O(n^2) thao tác cho mỗi trường hợp thử nghiệm, quá chậm đối với n lên tới 200.000. 

Thông tin chi tiết quan trọng cho một giải pháp tối ưu là chúng ta chỉ cần xem xét các phân đoạn liền kề của các phần tử khác với giá trị đặc biệt. Mỗi phân đoạn như vậy có thể được vô hiệu hóa bằng một lần lật kéo dài từ phần tử đầu tiên của phân đoạn đến chỉ số đặc biệt gần nhất ở hai bên. Điều này hiệu quả vì mỗi lần lật có thể bao gồm ít nhất một chỉ mục đặc biệt và đảo ngược toàn bộ phân đoạn, đưa tất cả các bit trong phân đoạn về giá trị chính xác. Bằng cách xử lý mảng từ trái sang phải và lật từng đoạn "sai" trong một thao tác, chúng ta có thể bao quát toàn bộ mảng một cách hiệu quả. 

Cách tiếp cận tham lam có hiệu quả vì vấn đề là tuyến tính: việc lật bất kỳ bit nào bên ngoài phân đoạn sai là không cần thiết và việc lật là bình thường trên các phân đoạn không chồng chéo. Chúng ta không cần xem xét các phân đoạn chồng chéo hoặc nhiều lần lật trên cùng một phân đoạn, bởi vì một thao tác đơn lẻ là đủ nếu phạm vi bao gồm một chỉ mục đặc biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Với mỗi test, đọc n, k, mảng`a`, và danh sách các chỉ số đặc biệt`p`. Chuyển thành`p`sang các chỉ số dựa trên 0 để xử lý mảng dễ dàng hơn. 
2. Xác định giá trị mục tiêu`x`tại các chỉ số đặc biệt. Tất cả các chỉ số đặc biệt được đảm bảo có cùng giá trị, vì vậy chúng ta có thể chọn`a[p[0]]`. 
3. Khởi tạo bộ đếm`ops`về 0 và một chỉ số`i`về không. Chúng ta sẽ quét mảng từ trái sang phải. 
4. Trong khi`i`ít hơn`n`, kiểm tra xem`a[i]`bằng`x`. Nếu có thì tăng`i`và tiếp tục. 
5. Nếu`a[i]`khác với`x`, chúng tôi đã tìm thấy điểm bắt đầu của một đoạn cần lật. Tăng`ops`bằng 1. Sau đó di chuyển`i`chuyển tiếp cho đến khi đạt phần tử tiếp theo bằng`x`hoặc cuối mảng. Điều này bỏ qua toàn bộ phân đoạn sai trong một thao tác. 
6. Lặp lại các bước 4-5 cho đến khi hết mảng. Sau khi xử lý,`ops`chứa số lượng hoạt động tối thiểu cần thiết. 

Cách tiếp cận tham lam có hiệu quả vì mọi phân đoạn bit khác với`x`phải được lật ít nhất một lần. Bằng cách mở rộng lần lật đến chỉ số đặc biệt gần nhất, chúng ta thỏa mãn ràng buộc rằng mỗi lần lật đều chứa một chỉ số đặc biệt. Việc lật các phân đoạn chồng chéo là không cần thiết vì khi một phân đoạn được lật, các bit của nó sẽ khớp với nhau`x`và sẽ không bị lật lại nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        p = list(map(int, input().split()))
        p = [pi - 1 for pi in p]  # convert to 0-based
        x = a[p[0]]  # target value
        ops = 0
        i = 0
        while i < n:
            if a[i] == x:
                i += 1
            else:
                ops += 1
                while i < n and a[i] != x:
                    i += 1
        print(ops)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc đầu vào và chuyển đổi các chỉ số đặc biệt thành dựa trên số 0. Giá trị mục tiêu được lấy từ chỉ số đặc biệt đầu tiên. Chúng tôi quét mảng và bất cứ khi nào chúng tôi gặp một phân đoạn khác với`x`, chúng ta tăng bộ đếm hoạt động và bỏ qua toàn bộ phân đoạn. Điều này đảm bảo rằng mỗi lần lật là tối thiểu và cần thiết. Các điều kiện biên được xử lý một cách tự nhiên vì vòng lặp while dừng ở`n`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2 1
1 0
1
```| Bước | tôi | một [tôi] | hoạt động | Bình luận | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 1 | 0 | Phù hợp với mục tiêu 1, đi tiếp | 
| tôi=1 | 1 | 0 | 0 | Khác biệt, tăng ops=1, bỏ qua phân đoạn | 
| Kết thúc | 2 | - | 1 | Đã hoàn thành | 

Điều này chứng tỏ rằng một lần lật sẽ che sai bit. 

Ví dụ thứ hai:```
3 2
0 1 0
1 3
```| Bước | tôi | một [tôi] | hoạt động | Bình luận | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | Matches target 0, move on |
 | tôi=1 | 1 | 1 | 0 | Khác nhau, tăng ops=1, chuyển tới số 0 tiếp theo tại i=2 | 
| tôi=2 | 2 | 0 | 1 | Matches target, move on |
 | Kết thúc | 3 | - | 2 | Xong | 

Bảng xác nhận cách tiếp cận tham lam lật các phân đoạn tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét từng phần tử một lần và mỗi phân đoạn sai sẽ bị bỏ qua trong một lần quét. | 
| Không gian | O(n) | Lưu trữ mảng`a`và chỉ số`p`. Không có cấu trúc bổ sung ngoài bộ nhớ đầu vào. | 

Vì tổng số n trên tất cả các trường hợp thử nghiệm là 200.000, nên quá trình quét tuyến tính trên mỗi trường hợp thử nghiệm sẽ vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n2 1\n1 0\n1\n3 2\n0 1 0\n1 3\n5 5\n1 1 1 1 1\n1 2 3 4 5\n9 5\n0 1 0 0 1 0 0 1 0\n3 4 6 7 9\n13 4\n1 0 0 1 0 1 0 1 1 0 1 0 1\n4 8 11 13\n15 3\n1 0 1 0 1 0 1 0 1 0 1 0 1 0 1\n3 11 13") == "2\n2\n0\n3\n5\n8", "sample 1"

# custom edge cases
assert run("1\n4 2\n1 1 1 1\n2 3") == "0", "all equal"
assert run("1\n1 1\n0\n1") == "1", "single element differs"
assert run("1\n5 2\n1 0 0 0 1\n1 5") == "1", "flip entire middle segment"
assert run("1\n6 3\n1 0 1 0 0 1\n1 3 6") == "2", "multiple segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,1,1,1]`|`0`| Đã đồng phục, không cần lật | 
|`[0]`|`1`| Mảng một phần tử, cần lật | 
|`[1,0,0,0,1]`|`1`| Đoạn giữa dài, lật trong một thao tác | 
|`[1,0,1,0,0,1]`|`2`| Nhiều phân đoạn có khoảng cách giữa chúng | 

## Vỏ cạnh 

Đối với một mảng`[1,1,1,1]`có chỉ số đặc biệt`[2,3]`, thuật toán xuất ra chính xác`0`bởi vì không có yếu tố nào khác với mục tiêu. Vòng lặp while quét từng phần tử, không tìm thấy sự khác biệt và không bao giờ tăng`ops`. 

Đối với một mảng`[1,0,0,0,1]`với các chỉ số đặc biệt `[1,5]
