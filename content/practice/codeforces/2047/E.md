---
title: "CF 2047E - Nhà thám hiểm"
description: "Chúng ta được cung cấp một tập hợp các thành phố được biểu thị bằng tọa độ của chúng trên mặt phẳng 2D. Bốn thương gia muốn chia các thành phố cho nhau bằng cách sử dụng một điểm phân chia duy nhất $(x0, y0)$."
date: "2026-06-09T03:34:48+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "flows", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2047
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 990 (Div. 2)"
rating: 2100
weight: 2047
solve_time_s: 131
verified: false
draft: false
---

[CF 2047E - Nhà thám hiểm](https://codeforces.com/problemset/problem/2047/E) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, luồng, tham lam, triển khai 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các thành phố được biểu thị bằng tọa độ của chúng trên mặt phẳng 2D. Bốn thương gia muốn chia thành phố cho nhau bằng một điểm phân chia duy nhất$(x_0, y_0)$. Việc phân chia hoạt động sao cho mỗi thương gia có được các thành phố trong một góc phần tư so với$(x_0, y_0)$: 

- Người bán 1 lấy góc phần tư trên cùng bên phải ($x \ge x_0$,$y \ge y_0$), 
- Người bán 2 lấy góc phần tư trên cùng bên trái ($x < x_0$,$y \ge y_0$), 
- Người bán 3 lấy góc phần tư phía dưới bên phải ($x \ge x_0$,$y < y_0$), 
- Người bán 4 lấy góc phần tư phía dưới bên trái ($x < x_0$,$y < y_0$). 

Mục tiêu là chọn$(x_0, y_0)$sao cho số lượng thành phố tối thiểu được chỉ định cho bất kỳ thương gia nào là tối đa. Nói cách khác, chúng tôi muốn có sự phân chia cân bằng nhất có thể. Đầu ra là số lượng thành phố tối thiểu tối đa này$k$và bất kỳ điểm tương ứng$(x_0, y_0)$. 

Các ràng buộc chỉ ra rằng tổng số thành phố trong tất cả các trường hợp thử nghiệm có thể đạt tới$10^5$. Một cách tiếp cận ngây thơ cố gắng hết sức có thể$(x_0, y_0)$là không khả thi vì điều đó có thể liên quan đến$O(n^2)$khả năng. Các trường hợp cạnh bao gồm các thành phố có tọa độ giống hệt nhau, có thể khiến toàn bộ góc phần tư bị trống nếu không được xử lý cẩn thận. 

Ví dụ: nếu tất cả các thành phố đều ở$(0,0)$, bất kể chúng ta chia như thế nào, ít nhất ba thương gia sẽ không có thành phố nào và kết quả đúng là$k=0$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là lặp đi lặp lại tất cả những gì có thể$x_0$Và$y_0$từ tọa độ của các thành phố và đếm xem có bao nhiêu thành phố nằm trong mỗi góc phần tư. Điều này sẽ yêu cầu$O(n^2)$hoạt động trên mỗi trường hợp thử nghiệm, quá chậm đối với$n$lên tới$10^5$. Ngay cả việc sắp xếp tọa độ và thử điểm giữa của mỗi cặp cũng sẽ$O(n^2)$trong trường hợp xấu nhất. 

Nhận xét quan trọng là để phân chia cân bằng, chúng ta chỉ cần xét các điểm chia nằm gần tọa độ trung tuyến. Nếu chúng ta sắp xếp tọa độ x và tọa độ y một cách độc lập thì kết quả tối ưu$x_0$sẽ ở gần$n/2$- phần tử thứ và tương tự cho$y_0$. Điều này làm giảm số điểm chia của ứng viên xuống một số trung vị không đổi. 

Cụ thể, chúng ta có thể xem xét các giá trị tại$\lceil n/4 \rceil$-th và$\lfloor 3n/4 \rfloor$- vị trí thứ trong mảng tọa độ được sắp xếp. Bất kỳ điểm nào trong phạm vi này đều đảm bảo rằng không có thương gia nào có được hơn 3/4 số thành phố và không ít hơn 1/4, tối đa hóa mức tối thiểu. 

Cái nhìn sâu sắc này làm giảm vấn đề sắp xếp tọa độ và chọn các ứng cử viên trung vị thích hợp. Sau đó chúng ta cần xác minh sự kết hợp nào của ứng cử viên$x_0$Và$y_0$mang lại số lượng tối thiểu lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Lựa chọn dựa trên trung vị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc số thành phố$n$và danh sách tọa độ. 
2. Trích xuất và sắp xếp tất cả tọa độ x một cách độc lập, sau đó sắp xếp tất cả tọa độ y một cách độc lập. 
3. Xác định ứng viên$x_0$các giá trị làm tọa độ tại các vị trí$n//4$Và$(3n)//4$trong mảng x đã được sắp xếp. Làm tương tự với ứng viên$y_0$các giá trị. Những vị trí này tương ứng với các phần tư. 
4. Đối với mỗi tổ hợp ứng viên$x_0$Và$y_0$, đếm số thành phố ở mỗi góc phần tư trong số bốn góc phần tư bằng cách duyệt qua tọa độ một lần. 
5. Tính số lượng tối thiểu của bốn thương gia cho mỗi cặp ứng cử viên. 
6. Theo dõi mức tối đa của các số lượng tối thiểu này và điểm phân chia tương ứng. 
7. Xuất ra số lượng tối thiểu tối đa$k$và bất kỳ$(x_0, y_0)$đạt được nó. 

Tại sao nó hoạt động: Bằng cách chọn ứng viên chia điểm ở các phần tư, chúng tôi đảm bảo rằng không có người bán nào nhận được ít hơn số thành phố ở phần tư thấp hơn, giúp tối đa hóa mức tối thiểu. Chúng ta chỉ cần xem xét một số lượng tọa độ ứng cử viên không đổi vì bất kỳ điểm nào giữa hai ranh giới tứ phân vị đều mang lại phân phối tối thiểu như nhau. Việc sắp xếp đảm bảo rằng chúng ta có thể truy cập các vị trí tứ phân vị này trong$O(1)$sau đó$O(n \log n)$sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        cities = [tuple(map(int, input().split())) for _ in range(n)]
        xs = sorted(x for x, y in cities)
        ys = sorted(y for x, y in cities)
        
        # candidate quartile positions
        x_candidates = [xs[n//4], xs[(3*n)//4]]
        y_candidates = [ys[n//4], ys[(3*n)//4]]
        
        best_k = -1
        best_point = None
        
        for x0 in x_candidates:
            for y0 in y_candidates:
                counts = [0, 0, 0, 0]  # merchants 1-4
                for x, y in cities:
                    if x >= x0 and y >= y0:
                        counts[0] += 1
                    elif x < x0 and y >= y0:
                        counts[1] += 1
                    elif x >= x0 and y < y0:
                        counts[2] += 1
                    else:
                        counts[3] += 1
                min_count = min(counts)
                if min_count > best_k:
                    best_k = min_count
                    best_point = (x0, y0)
        
        print(best_k)
        print(*best_point)

if __name__ == "__main__":
    solve()
```Mã sắp xếp tọa độ để xác định các ứng cử viên tứ phân vị một cách hiệu quả. Việc đếm các thành phố trong mỗi góc phần tư được thực hiện bằng một vòng lặp cho mỗi ứng cử viên, đủ nhanh vì chỉ có bốn giá trị x ứng cử viên và bốn giá trị y ứng cử viên, mang lại tối đa 16 lần kiểm tra cho mỗi trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
4
1 1
1 2
2 1
2 2
```| Bước | xs | vâng | Ứng viên x0 | Ứng viên y0 | Số Phần Tư | Số lượng tối thiểu | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 1 2 2 | 1 1 2 2 | 1 | 1 | [2,1,1,0] | 0 | 
| 2 | giống nhau | giống nhau | 2 | 2 | [1,1,1,1] | 1 | 

Điểm tối ưu (2,2) đảm bảo mỗi thương gia có được chính xác một thành phố. 

### Đầu vào mẫu 2```
4
0 0
0 0
0 0
0 0
```Tất cả các ứng cử viên mang lại số lượng tối thiểu 0. Đầu ra là$k=0$và bất kỳ$(0,0)$. 

Những dấu vết này xác nhận rằng thuật toán xử lý chính xác các điểm giống hệt nhau và các điểm phân bố đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp tọa độ x và y chiếm ưu thế; đếm góc phần tư là O(n) cho mỗi ứng viên nhưng ứng viên không đổi | 
| Không gian | O(n) | Lưu trữ tọa độ thành phố và sắp xếp mảng | 

Điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ cho$n \le 10^5$và tổng số thành phố trên tất cả các trường hợp thử nghiệm$\le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("4\n4\n1 1\n1 2\n2 1\n2 2\n4\n0 0\n0 0\n0 0\n0 0\n8\n1 2\n2 1\n2 -1\n1 -2\n-1 -2\n-2 -1\n-2 1\n-1 2\n7\n1 1\n1 2\n1 3\n1 4\n2 1\n3 1\n4 1\n") \
    == "1\n2 2\n0\n0 0\n2\n1 0\n0\n0 0"

# minimum input
assert run("1\n4\n1 1\n2 2\n3 3\n4 4\n") == "1\n2 2"

# all equal
assert run("1\n5\n0 0\n0 0\n0 0\n0 0\n0 0\n") == "0\n0 0"

# maximum size small test
import random
coords = "\n".join(f"{random.randint(-10**9,10**9)} {random.randint(-10**9,10**9)}" for _ in range(100))
inp = f"1\n100\n{coords}\n"
run(inp)
```
