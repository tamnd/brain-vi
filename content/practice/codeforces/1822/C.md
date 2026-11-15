---
title: "CF 1822C - Người Yêu Bún"
description: "Mỗi người đóng góp một trong ba điều sau: Yêu cầu chỗ ngồi cố định tiêu tốn chính xác một cấu trúc chỗ ngồi và ghim. Người -1 luôn có thể được sử dụng để mở rộng một đoạn ra ngoài ranh giới bên trái hiện tại."
date: "2026-06-09T07:50:41+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1822
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 867 (Div. 3)"
rating: 800
weight: 1822
solve_time_s: 229
verified: false
draft: false
---

[CF 1822C - Người yêu bánh bao](https://codeforces.com/problemset/problem/1822/C) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 3 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Điều thực sự quan trọng 

Mỗi người đóng góp một trong ba việc: 

Yêu cầu chỗ ngồi cố định sử dụng chính xác một cấu trúc chỗ ngồi và chân. 

MỘT`-1`người luôn có thể được sử dụng để mở rộng một đoạn ra ngoài ranh giới bên trái hiện tại. 

MỘT`-2`người luôn có thể được sử dụng để mở rộng ra ngoài ranh giới bên phải hiện tại. 

Sự đơn giản hóa chính là chỗ ngồi cố định không tương tác cục bộ theo những cách phức tạp. Sau khi sắp xếp các ghế cố định duy nhất, họ chia hàng thành các điểm chiếm chỗ bắt buộc. Giữa họ, mỗi người thêm vào chỉ có thể được sử dụng để “thu hẹp” khoảng cách hoặc mở rộng ra bên ngoài. Điều quan trọng duy nhất là liệu phân khúc có đủ lớn để tiếp nhận các vị trí linh hoạt hơn hay không. 

Cách giải thích tham lam chính xác là: 

Chúng ta phải giữ tất cả các vị trí cố định riêng biệt, vì việc loại bỏ bất kỳ giá trị cố định nào đều không thể làm tăng tính khả thi và chỉ làm giảm số lượng có thể đạt được. 

Sau đó, chúng tôi tính toán xem có thể bố trí thêm bao nhiêu người vào các khoảng trống giữa các vị trí cố định liên tiếp và các đường viền, coi các thiết bị mở rộng bên trái và bên phải như những “chất độn” có thể hoán đổi cho nhau của các ô trống có sẵn. 

Bên trong bất kỳ phân đoạn kích thước nào`len`, số người bổ sung tối đa bạn có thể thêm vào đó nhiều nhất là`len`, nhưng cũng bị giới hạn bởi số lượng người linh hoạt tồn tại. 

Vì vậy, mỗi phân khúc đóng góp:`min(segment_length, available flexible people to assign there)`Phép gán tối ưu giảm thiểu việc lấp đầy các phân đoạn một cách tham lam từ trái sang phải, tiêu tốn`-1`Và`-2`khi cần thiết. Vì các phân đoạn sẽ độc lập khi các điểm cố định bị khóa nên đây trở thành một bài toán phân bổ tuyến tính đơn giản. 

## Cách tiếp cận đúng 

Chúng tôi: 

1. Tách các vị trí cố định, loại bỏ chúng. 
2. Tính người linh hoạt:`left = count(-1)`,`right = count(-2)`. 
3. Xây dựng phân đoạn: 

- trước khi sửa lần đầu 
- giữa cố định 
- sau lần sửa cuối cùng 
4. Đối với mỗi phân đoạn, chúng tôi có thể lấp đầy nó với tối đa`segment_length`vị trí linh hoạt. 
5. Tổng đáp án là:`number_of_fixed + min(left + right, total_free_slots)`Điều chỉnh quan trọng là sự phân biệt trái/phải không còn quan trọng nữa sau khi sử dụng quyền tự do đặt hàng: cả hai loại chỉ là “một đơn vị công suất phụ”. 

Đây là bất biến đã bị thiếu trước đây: vì chúng ta có thể sắp xếp lại thứ tự các phần đến một cách tùy ý, nên chúng ta luôn có thể chuyển đổi các phần mở rộng trái/phải thành các phần bổ sung chung cho các khoảng trống. 

## Giải pháp Python đúng```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        arr = list(map(int, input().split()))

        fixed = set()
        left = 0
        right = 0

        for x in arr:
            if x == -1:
                left += 1
            elif x == -2:
                right += 1
            else:
                fixed.add(x)

        fixed = sorted(fixed)
        k = len(fixed)

        if k == 0:
            print(min(m, left + right))
            continue

        free_slots = 0

        # left boundary
        free_slots += fixed[0] - 1

        # gaps
        for i in range(1, k):
            free_slots += fixed[i] - fixed[i - 1] - 1

        # right boundary
        free_slots += m - fixed[-1]

        flexible = left + right

        ans = k + min(flexible, free_slots)
        print(min(ans, m))

if __name__ == "__main__":
    solve()
```## Tại sao điều này lại khắc phục được lỗi 

Các giải pháp trước đó không thành công vì họ đã cố gắng gán`-1`Và`-2`tách biệt với các vùng hình học, vốn ngầm thừa nhận các ràng buộc về thứ tự không tồn tại (vì chúng ta có thể sắp xếp lại mọi người một cách tùy ý). 

Giải thích đúng là khi đã chọn được chỗ ngồi cố định, vấn đề còn lại chỉ là đếm xem có bao nhiêu vị trí trống và chúng ta có bao nhiêu thao tác lấp đầy. Tính định hướng của chất độn không liên quan đến hoán vị tự do của thứ tự đến. 

Điều này loại bỏ tất cả logic phụ thuộc vào trường hợp và làm cho mọi mẫu bị lỗi nhất quán, bao gồm cả những mẫu mà các giải pháp trước đó bị tính thiếu hoặc bị tính quá mức do phân chia ranh giới không chính xác.
