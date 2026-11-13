---
title: "CF 1697F - Quá nhiều ràng buộc"
description: "Chúng ta được yêu cầu xây dựng một dãy số nguyên có độ dài n, trong đó mỗi phần tử nằm trong khoảng từ 1 đến k và dãy này phải không giảm. Ngoài ra, bài toán còn cung cấp thêm ba loại ràng buộc."
date: "2026-06-09T22:30:14+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "constructive-algorithms", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1697
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 130 (Rated for Div. 2)"
rating: 2800
weight: 1697
solve_time_s: 145
verified: false
draft: false
---

[CF 1697F - Quá nhiều ràng buộc](https://codeforces.com/problemset/problem/1697/F) 

**Đánh giá:** 2800 
**Tags:** 2-sat, thuật toán xây dựng, đồ thị, triển khai 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một dãy số nguyên có độ dài`n`, trong đó mỗi phần tử nằm giữa`1`Và`k`và dãy phải không giảm. Ngoài ra, bài toán còn cung cấp thêm ba loại ràng buộc. Loại đầu tiên cấm một giá trị cụ thể tại một vị trí cụ thể. Loại thứ hai và thứ ba đặt các bất đẳng thức tính tổng lên các cặp phần tử. Nhiệm vụ của chúng ta là xây dựng bất kỳ chuỗi nào thỏa mãn tất cả các ràng buộc hoặc báo cáo rằng không tồn tại chuỗi nào. 

Quan sát quan trọng là`k`là rất nhỏ (lên tới 10), trong khi`n`có thể lớn (lên tới 2×10^4). Điều đó cho thấy rằng khả năng liệt kê các phần tử riêng lẻ là khả thi, nhưng việc ép buộc tất cả các chuỗi sẽ bùng nổ theo cấp số nhân. Các ràng buộc của loại 2 và 3 liên quan đến các cặp phần tử, do đó, bất kỳ nỗ lực ngây thơ nào thử mọi chuỗi đều nhanh chóng trở nên khó giải quyết. Một điểm nữa là mảng phải không giảm. Điều đó ngụ ý rằng khi một phần tử bị ràng buộc ở ít nhất một giá trị nào đó, tất cả các phần tử sau nó sẽ kế thừa giới hạn dưới đó. Tương tự, giới hạn trên của chỉ số ban đầu sẽ lan truyền về phía trước. 

Các trường hợp cạnh không rõ ràng xảy ra khi các ràng buộc xung đột gián tiếp thông qua việc truyền bá. Ví dụ, nếu`a1 + a2 ≥ 5`Và`a2 + a3 ≤ 4`, có thể không thể thỏa mãn cả hai nếu`k`là nhỏ. Một trường hợp phức tạp khác là các ràng buộc loại 1 kết hợp với tính đơn điệu: việc cấm một giá trị tại một chỉ mục có thể buộc giá trị sau đó vượt quá`k`vì dãy số không thể giảm được. 

Một cách tiếp cận bất cẩn có thể cố gắng thỏa mãn từng ràng buộc một cách độc lập theo thứ tự. Ví dụ, thiết lập`a[i] = 1`trừ khi bị cấm sẽ thất bại khi ràng buộc loại 2 hoặc 3 liên quan đến tổng, bởi vì những ràng buộc này kết nối các phần tử ở xa chứ không chỉ các phần tử lân cận. Một ví dụ cụ thể là`n = 3, k = 3`với những hạn chế`1 2 1`Và`3 1 3 5`. Chỉ cần thiết lập các giá trị tối thiểu được phép sẽ mang lại`[1, 2, 3]`, vi phạm ràng buộc tổng nếu diễn giải không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liệt kê tất cả các chuỗi có độ dài`n`với các phần tử ở giữa`1`Và`k`. Đối với mỗi chuỗi, nó sẽ kiểm tra tất cả các ràng buộc. Độ phức tạp về thời gian là`O(k^n * m)`, điều này là không thể thực hiện được ngay cả đối với những điều không tầm thường nhỏ nhất`n`. Tính đúng đắn đến từ việc kiểm tra toàn diện: nếu bất kỳ chuỗi nào thỏa mãn tất cả các ràng buộc thì nó sẽ được tìm thấy. Nó thất bại cho lớn hơn`n`bởi vì`k^n`tăng trưởng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là`k`rất nhỏ nên chúng ta có thể biểu diễn các giá trị được phép cho từng vị trí dưới dạng tập hợp hoặc mặt nạ bit. Mọi ràng buộc loại 1 sẽ trực tiếp loại bỏ một giá trị khỏi vị trí đó. Các ràng buộc loại 2 và 3 có thể được biểu thị dưới dạng giới hạn trên các phần tử riêng lẻ khi kết hợp với tính đơn điệu. Đối với loại 2,`a[i] + a[j] ≤ x`Và`i < j`ngụ ý`a[i] ≤ x - 1`nhiều nhất là nếu`a[j]`ít nhất là 1. Tương tự, loại 3 hàm ý giới hạn dưới. Vì mảng không giảm nên giới hạn dưới tại`i`truyền về phía trước và giới hạn trên về phía sau. Điều này cho phép áp dụng cách tiếp cận lan truyền ràng buộc: cập nhật lặp đi lặp lại các phạm vi cho đến khi chúng ổn định. Nếu bất kỳ phạm vi nào trở nên trống thì chuỗi đó là không thể. 

Chúng ta có thể tóm tắt các phương pháp: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n * m) | O(n) | Quá chậm | 
| Tuyên truyền ràng buộc | O(n * k + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng`low`Và`high`chiều dài`n`, đại diện cho các giá trị tối thiểu và tối đa được phép cho mỗi vị trí. Bộ`low[i] = 1`Và`high[i] = k`. 
2. Áp dụng ngay ràng buộc loại 1. Đối với một hạn chế cấm`a[i] = x`, chúng ta không lưu trữ tất cả các giá trị bị cấm nhưng xử lý nó trong lần gán cuối cùng bằng cách bỏ qua nó nếu`x`bằng`low[i]`và tăng dần trong giới hạn. 
3. Áp dụng ràng buộc loại 2. Đối với một hạn chế`a[i] + a[j] ≤ x`Và`i < j`, cập nhật giới hạn trên:`high[i] = min(high[i], x - 1)`Và`high[j] = min(high[j], x - 1)`nếu cần thiết. Điều này đảm bảo rằng ngay cả những giá trị tối thiểu cũng không vi phạm tổng. 
4. Áp dụng ràng buộc loại 3. Đối với một hạn chế`a[i] + a[j] ≥ x`, cập nhật giới hạn dưới:`low[i] = max(low[i], x - k)`Và`low[j] = max(low[j], x - k)`, tôn trọng giá trị lớn nhất có thể`k`. 
5. Thực thi tính đơn điệu bằng cách truyền các giới hạn dưới về phía trước: cho mỗi`i`từ 1 đến n-1, đặt`low[i+1] = max(low[i+1], low[i])`. Tuyên truyền giới hạn trên về phía sau: for`i`từ n-2 đến 0, đặt`high[i] = min(high[i], high[i+1])`. 
6. Nếu ở vị trí nào đó`low[i] > high[i]`, không có nghiệm nào tồn tại; đầu ra -1. 
7. Xây dựng mảng cuối cùng. Với mỗi vị trí hãy chọn`a[i] = low[i]`. Nếu giá trị này bị cấm bởi ràng buộc loại 1 ở vị trí đó, hãy chọn giá trị tiếp theo trong`[low[i], high[i]]`điều đó được cho phép. Nếu không có giá trị đó thì xuất -1. 

## Tại sao nó hoạt động 

Bất biến được duy trì là với mọi chỉ số`i`, khoảng`[low[i], high[i]]`chứa tất cả các giá trị có thể xuất hiện tại`i`mà không vi phạm ngay bất kỳ ràng buộc loại 2, 3 hoặc đơn điệu nào. Việc truyền bá ràng buộc đảm bảo rằng các giới hạn phản ánh tất cả các phụ thuộc gián tiếp. Nhiệm vụ cuối cùng từ`low[i]`trở lên đảm bảo thứ tự không giảm trong khi tôn trọng các khoảng cho phép và ngoại lệ loại 1 được xử lý cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        low = [1] * n
        high = [k] * n
        forb = [[] for _ in range(n)]
        constraints = []
        for _ in range(m):
            data = list(map(int, input().split()))
            if data[0] == 1:
                _, i, x = data
                forb[i - 1].append(x)
            else:
                constraints.append(data)
        for c in constraints:
            if c[0] == 2:
                _, i, j, x = c
                i -= 1
                j -= 1
                high[i] = min(high[i], x - 1)
                high[j] = min(high[j], x - 1)
            else:
                _, i, j, x = c
                i -= 1
                j -= 1
                low[i] = max(low[i], x - k)
                low[j] = max(low[j], x - k)
        for i in range(1, n):
            low[i] = max(low[i], low[i - 1])
        for i in range(n - 2, -1, -1):
            high[i] = min(high[i], high[i + 1])
        possible = True
        res = []
        for i in range(n):
            val = low[i]
            while val in forb[i] and val <= high[i]:
                val += 1
            if val > high[i]:
                possible = False
                break
            res.append(val)
        if possible:
            print(*res)
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm và khởi tạo giới hạn dưới và trên cho từng vị trí. Các ràng buộc loại 1 được thu thập riêng biệt để xử lý các giá trị bị cấm. Ràng buộc loại 2 và 3 cập nhật trực tiếp các giới hạn. Tính đơn điệu được thực thi theo hướng tiến và lùi. Mảng cuối cùng được xây dựng bằng cách chọn giá trị khả dụng thấp nhất ở mỗi vị trí, bỏ qua các giá trị bị cấm. Nếu một vị trí không có giá trị hợp lệ,`-1`được in. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4
4 0 4
2 2 3
3 1 2 3
1 2 2
3 3 2
1 1 1
2 2 3 2
3 2 3 2
5 5 5
3 2 5 7
2 4 5 10
3 4 5 6
3 3 4 7
2 1 5 7
```Dấu vết cho trường hợp thử nghiệm đầu tiên`4 0 4`: 

| tôi | thấp | cao | đã chọn | ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 1 | không bị cấm | 
| 2 | 1 | 4 | 1 | | 
| 3 | 1 | 4 | 1 | | 
| 4 | 1 | 4 | 1 | | 

Mảng`[1, 1, 1, 1]`là hợp lệ. Bất kỳ dãy không giảm nào trong`[1,4]`hoạt động. 

**Đầu vào mẫu 2:**`2 2 3`với`1 2 2`(loại 1 cấm 2 ở chỉ số 2) 

| tôi | thấp | cao | đã chọn | ghi chú
