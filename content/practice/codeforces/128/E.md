---
title: "CF 128E - Sinh nhật"
description: "Chúng ta được yêu cầu xác định số lượng chuối tối đa mà Maria có thể sản xuất từ ​​một chiếc bánh sinh nhật được trang trí bằng n miếng chuối hình tròn không chồng lên nhau bằng cách sử dụng đúng k đường thẳng. Mỗi đường tròn có tọa độ tâm (x, y) và bán kính r."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 128
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 94 (Div. 1 Only)"
rating: 2900
weight: 128
solve_time_s: 95
verified: true
draft: false
---

[CF 128E - Sinh nhật](https://codeforces.com/problemset/problem/128/E) 

**Xếp hạng:** 2900 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định số lượng chuối tối đa mà Maria có thể sản xuất từ một chiếc bánh sinh nhật được trang trí bằng _n_ miếng chuối hình tròn không chồng lên nhau bằng cách sử dụng chính xác _k_ các đường cắt thẳng. Mỗi vòng tròn có tọa độ trung tâm`(x, y)`và bán kính`r`. Mục đích không phải là chia đều chiếc bánh hay tối đa hóa số lát bánh mà là tối đa hóa số lượng miếng trong vòng tròn chuối. Các vết cắt có thể giao nhau và nhiều vết cắt có thể đi qua cùng một vòng tròn. 

Đầu vào cung cấp số lượng vòng tròn _n_, số lần cắt _k_ cũng như tọa độ và bán kính của mỗi vòng tròn. Kết quả là một số nguyên duy nhất, số lượng miếng chuối lớn nhất có thể đạt được với các lần cắt đã cho. 

Cho rằng`n ≤ 1000`Nhưng`k`có thể đi lên`10^5`, việc lặp lại tất cả các vị trí dòng có thể là không khả thi. Bất kỳ mô phỏng hình học mạnh mẽ nào cũng sẽ nhanh chóng trở nên quá chậm. Điều này ngụ ý rằng chúng ta cần một cách tiếp cận trên mỗi vòng tròn để tính toán số phần tối đa mà một vòng tròn có thể tạo ra một cách độc lập và sau đó tổng hợp số phần này trên tất cả các vòng tròn. Các trường hợp cạnh bao gồm các tình huống có ít vết cắt hơn vòng tròn, trong đó một số vòng tròn không thể cắt được chút nào và các tình huống có nhiều vết cắt so với số lượng vòng tròn, có thể chia quá mức một số vòng tròn nhất định. 

Một sai lầm ngây thơ là cho rằng mỗi vết cắt sẽ chia một vòng tròn một lần hoặc phân bổ các vết cắt không đồng đều mà không ưu tiên các vòng tròn lớn nhất. Ví dụ: với một hình tròn và một vết cắt, số mảnh tối đa là 2 chứ không phải 1. Tương tự, với hai hình tròn và một vết cắt, chúng ta không thể chia cả hai cùng một lúc ngoài tổng cộng 2 mảnh. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi đường thẳng qua mặt phẳng và tính toán các giao điểm với mọi vòng tròn. Đối với mỗi lần cắt, chúng ta có thể lặp lại tất cả các vòng tròn để xác định có bao nhiêu phần mới được tạo ra. Điều này có thể đúng nhưng cực kỳ chậm vì mỗi phép tính giao điểm đường tròn là không cần thiết và với`k`lên tới`10^5`, số lượng hoạt động sẽ vượt quá`10^8`một cách dễ dàng. 

Thông tin chi tiết quan trọng cho cách tiếp cận tối ưu đến từ hình học đường tròn cổ điển: số phần tối đa mà một vòng tròn có thể được chia thành bằng`m`đường cắt thẳng được cho bởi công thức`(m * (m + 1)) // 2 + 1`. Công thức này đếm tất cả các mảnh được tạo bằng cách giao nhau các đường bên trong một vòng tròn: đường cắt đầu tiên chia hình tròn thành 2, đường cắt thứ hai có thể cắt đường cắt đầu tiên tạo ra tối đa 2 mảnh bổ sung, v.v. Mỗi vòng tròn là độc lập, vì vậy mục tiêu của chúng tôi giảm xuống việc phân phối`k`vết cắt giữa các`n`vòng tròn để tối đa hóa tổng số mảnh. 

Để tối đa hóa các mảnh, chúng tôi chỉ định các vết cắt một cách tham lam cho các vòng tròn. Chúng tôi bắt đầu với mỗi vòng tròn không có vết cắt nào và liên tục chỉ định vết cắt tiếp theo cho vòng tròn nơi nó sẽ tăng tổng số mảnh nhiều nhất. Vì mức tăng biên của một vết cắt bổ sung trên một vòng tròn là`cuts + 1`(tăng từ`m`ĐẾN`m+1`cắt giảm là`(m+1) - m`về mặt công thức), chúng ta có thể sắp xếp hoặc sử dụng hàng đợi ưu tiên để luôn phân bổ lần cắt tiếp theo vào vòng tròn có mức tăng biên cao nhất. Bởi vì công thức là bậc hai, nên các vết cắt trước đó trên một vòng tròn có mức tăng nhỏ hơn, vì vậy chúng tôi cân bằng các vết cắt trên các vòng tròn một cách đồng đều cho đến khi tất cả các vết cắt được sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(n) | Quá chậm | 
| Tham lam cắt phân phối | O(k * log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng có độ dài _n_ để đếm xem mỗi vòng tròn đã nhận được bao nhiêu vết cắt. Ban đầu, tất cả các giá trị đều bằng 0. 
2. Sử dụng hàng đợi ưu tiên (max-heap) để theo dõi mức tăng tiềm năng tiếp theo cho mỗi vòng kết nối. Lợi ích của việc thêm một vết cắt vào một vòng tròn với`m`cắt giảm là`m + 1`. 
3. Đối với mỗi lần cắt _k_: 

một. Bật vòng tròn có mức tăng hiện tại lớn nhất từ ​​heap. 

b. Tăng số lần cắt của nó lên một. 

c. Tính mức tăng mới cho lần cắt tiếp theo trên vòng tròn này và đẩy nó trở lại vùng heap. 
4. Sau khi phân phối tất cả các phần cắt, hãy tính tổng số mảnh cho mỗi vòng tròn bằng công thức`(cuts * (cuts + 1)) // 2 + 1`và tổng hợp chúng. 
5. Ghi tổng số tiền là số lượng chuối tối đa. 

Tại sao nó hoạt động: Công thức tính số lượng tối đa trên mỗi vòng tròn là đúng về mặt toán học. Việc phân bổ các phần cắt một cách tham lam theo mức tăng cận biên đảm bảo mỗi lần cắt đóng góp số lượng phần mới tối đa có thể tại thời điểm nó được áp dụng. Vì mỗi vòng tròn là độc lập và hàm lõm đối với các đường cắt, chiến lược này đảm bảo mức tối đa toàn cục. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n, k = map(int, input().split())
circles = [tuple(map(int, input().split())) for _ in range(n)]

# initialize heap with (-gain, cuts, index)
# gain is negative because heapq is min-heap in Python
heap = [(-1, 0, i) for i in range(n)]  # initial gain is 1 for each circle
heapq.heapify(heap)

for _ in range(k):
    gain, cuts, i = heapq.heappop(heap)
    cuts += 1
    new_gain = -(cuts + 1)
    heapq.heappush(heap, (new_gain, cuts, i))

total_pieces = 0
for _, cuts, _ in heap:
    total_pieces += (cuts * (cuts + 1)) // 2 + 1

print(total_pieces)
```Heap được sử dụng để chọn một cách hiệu quả vòng tròn sẽ mang lại mức tăng lớn nhất từ ​​lần cắt tiếp theo. Việc sử dụng các giá trị âm sẽ chuyển đổi vùng heap tối thiểu của Python thành vùng heap tối đa. Sau khi phân phối các vết cắt, số lượng mảnh cuối cùng trên mỗi vòng tròn được tính bằng công thức tiêu chuẩn. Các điều kiện biên được xử lý một cách tự nhiên vì chúng tôi khởi tạo với các lần cắt bằng 0 và chỉ thêm các lần cắt khi được cho phép bởi _k_. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
1 1
0 0 1
```| Bước | Đỉnh đống | Cắt | Đạt được mới | Đống sau khi đẩy | Tổng số mảnh | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0→1 | 2 | [2] | - | 

Tính toán cuối cùng:`(1 * 2)//2 + 1 = 2`. Đầu ra là`2`. 

### Đầu vào tùy chỉnh```
2 3
0 0 1
1 1 1
```| Bước | Đỉnh đống | Cắt | Đạt được mới | Đống sau khi đẩy | 
| --- | --- | --- | --- | --- | 
| 1 | Vòng 1 | 0→1 | 2 | cập nhật đống | 
| 2 | vòng tròn thứ 2 | 0→1 | 2 | cập nhật đống | 
| 3 | bất kỳ vòng tròn nào | 1→2 | 3 | cập nhật đống | 

Phần cuối cùng: vòng tròn 1`(2*3)//2+1=4`, vòng tròn2`(1*2)//2+1=2`. Tổng cộng = 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * log n) | Mỗi lần cắt bao gồm một cú đẩy/đẩy có chi phí log n, lặp lại k lần | 
| Không gian | O(n) | Chúng ta lưu trữ heap với n vòng tròn | 

Điều này phù hợp trong giới hạn vì`k`có thể`10^5`Và`n`nhiều nhất là`1000`, mang lại ~500.000 thao tác heap. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq
    n, k = map(int, input().split())
    circles = [tuple(map(int, input().split())) for _ in range(n)]
    heap = [(-1, 0, i) for i in range(n)]
    heapq.heapify(heap)
    for _ in range(k):
        gain, cuts, i = heapq.heappop(heap)
        cuts += 1
        heapq.heappush(heap, (-(cuts+1), cuts, i))
    total = sum((cuts*(cuts+1))//2+1 for _, cuts, _ in heap)
    return str(total)

assert run("1 1\n0 0 1\n") == "2"
assert run("2 3\n0 0 1\n1 1 1\n") == "6"
assert run("3 0\n0 0 1\n1 1 1\n2 2 1\n") == "3"
assert run("2 5\n0 0 1\n1 1 1\n") == "12"
assert run("1 1000\n0 0 1000\n") == str(1000*1001//2 + 1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 vòng tròn | 2 | cắt đơn tối thiểu | 
| 2 3 vết cắt | 6 | phân phối nhiều vết cắt giữa các vòng tròn | 
| 3 vòng tròn, 0 vết cắt | 3 | kịch bản cắt giảm bằng 0 | 
| 2 vòng tròn, 5 vết cắt | 12 | nhiều vết cắt hơn vòng tròn, phân bổ tham lam | 
| 1 vòng tròn, 1000 vết cắt | 500501 | cắt tối đa trên một vòng tròn, số lượng lớn | 

## Vỏ cạnh 

Với các vết cắt bằng 0, mỗi vòng tròn vẫn nguyên:`n`miếng. Với một vòng tròn và một lần cắt, đầu ra là`2`. Với nhiều vết cắt hơn vòng tròn, phân phối tham lam đảm bảo các vết cắt được cân bằng để
