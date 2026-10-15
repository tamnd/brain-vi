---
title: "CF 105450D - Trick or Treat"
description: "Chúng ta được cung cấp một tập hợp các điểm trên lưới 2D và chúng ta bắt đầu từ gốc tọa độ (0, 0). Chúng ta muốn chọn một hướng và đi theo một đường thẳng đi qua điểm gốc. Trong khi di chuyển dọc theo đường đó, chúng tôi thu thập tất cả các viên kẹo nằm chính xác trên đó."
date: "2026-06-23T17:32:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105450
codeforces_index: "D"
codeforces_contest_name: "UTPC x WiCS Contest 10-25-24 (UT Internal)"
rating: 0
weight: 105450
solve_time_s: 84
verified: false
draft: false
---

[CF 105450D - Trick or Treat](https://codeforces.com/problemset/problem/105450/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm trên lưới 2D và chúng ta bắt đầu từ gốc tọa độ (0, 0). Chúng ta muốn chọn một hướng và đi theo một đường thẳng đi qua điểm gốc. Trong khi di chuyển dọc theo đường đó, chúng tôi thu thập tất cả các viên kẹo nằm chính xác trên đó. Mục tiêu là chọn đường đi qua gốc tọa độ và chứa số điểm cho trước lớn nhất. 

Mỗi viên kẹo chỉ là một điểm cố định trên mặt phẳng. Các dòng hợp lệ duy nhất là những dòng đi qua gốc tọa độ, vì vậy mọi giải pháp ứng cử viên đều được xác định đầy đủ bởi một vectơ chỉ hướng từ (0, 0) đến một số điểm kẹo (x, y). Tất cả các điểm nằm trên cùng một đường như vậy đều có cùng độ dốc, hay chính xác hơn là cùng một tỷ lệ hướng giảm. 

Kích thước đầu vào lên tới 100.000 điểm, điều này ngay lập tức loại trừ việc so sánh từng cặp điểm hoặc kiểm tra tất cả các cặp hệ số góc. Bất kỳ giải pháp nào cố gắng thực hiện hành vi O(n²) sẽ thực hiện tối đa 10¹⁰ so sánh trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong một giây. Điều này đẩy chúng ta tới cách tiếp cận O(n log n) hoặc O(n). 

Một vài trường hợp tế nhị quan trọng ở đây. Đầu tiên, các điểm trong góc phần tư đối diện có thể nằm trên cùng một đường thẳng qua gốc tọa độ, ví dụ (2, 3) và (-4, -6). Phép tính độ dốc đơn giản y/x sẽ coi chúng là các dấu khác nhau trừ khi được chuẩn hóa cẩn thận. 

Thứ hai, các đường thẳng đứng phải được xử lý. Một điểm như (0, 5) xác định một đường hợp lệ, nhưng biểu diễn độ dốc sẽ liên quan đến việc chia cho 0 nếu không được xử lý riêng. 

Thứ ba, bình thường hóa là rất quan trọng. Nếu không giảm các vectơ chỉ hướng thành dạng chính tắc, cùng một dòng có thể được đếm nhiều lần dưới các biểu diễn số nguyên khác nhau như (2, 4) và (1, 2), phải được coi là giống hệt nhau. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét từng cặp điểm và tính đường thẳng đi qua gốc tọa độ và hướng của cặp điểm đó. Đối với mỗi đường như vậy, chúng ta có thể đếm được có bao nhiêu điểm nằm trên đó bằng cách kiểm tra tính cộng tuyến thông qua tích chéo. Điều này hiệu quả vì hai điểm xác định một hướng và mọi điểm khác có thể được kiểm tra theo hướng đó. 

Tuy nhiên, điều này dẫn đến các hướng ứng cử viên O(n²) và đối với mỗi hướng, chúng tôi có thể quét tất cả n điểm, đưa ra O(n³) ở dạng xấu nhất hoặc tốt nhất là O(n²) nếu chúng tôi tối ưu hóa việc đếm. Với n = 10⁵ thì ngay cả O(n²) cũng không thể. 

Quan sát quan trọng là mọi đường thẳng hợp lệ đi qua gốc tọa độ có thể được biểu diễn duy nhất bằng vectơ chỉ hướng từ gốc đến bất kỳ điểm nào trên đường thẳng đó. Thay vì liệt kê các cặp điểm, chúng ta chỉ cần phân loại từng điểm theo hướng chuẩn hóa của nó. Các điểm có cùng hướng rút gọn nằm trên cùng một đường thẳng, do đó nhiệm vụ giảm xuống việc đếm tần số của các vectơ chỉ hướng chuẩn hóa. 

Điều này biến bài toán thành bài toán băm trên các cặp số nguyên rút gọn. Mỗi điểm đóng góp chính xác một hướng và chúng tôi duy trì bản đồ tần số để tìm số lượng tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) đến O(n³) | O(1) | Quá chậm | 
| Băm hướng | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi điểm (x, y), hãy tính biểu diễn chuẩn hóa của hướng của nó tính từ gốc tọa độ. Điều này đảm bảo rằng tất cả các điểm trên cùng một đường đều ánh xạ tới cùng một khóa. Việc chuẩn hóa là cần thiết vì các tỷ lệ thô như y/x không ổn định khi thay đổi tỷ lệ số nguyên hoặc thay đổi dấu. 
2. Nếu x bằng 0, hãy coi hướng là một lớp dọc đặc biệt. Tất cả các điểm có x = 0 đều thuộc cùng một đường thẳng đi qua gốc tọa độ, bất kể y dương hay âm. 
3. Nếu x khác 0, hãy tính ước số chung lớn nhất của x và y, sau đó chia cả hai cho x và y. Điều này làm giảm cặp thành biểu diễn số nguyên nhỏ nhất của nó. Bước này loại bỏ các trùng lặp như (2, 4) và (1, 2). 
4. Chuẩn hóa dấu sao cho bản đồ chỉ đường tương đương giống hệt nhau. Một quy tắc chuẩn là đảm bảo rằng x dương hoặc nếu x bằng 0 thì y dương. Nếu x âm, nhân cả hai thành phần với -1. 
5. Lưu trữ từng hướng chuẩn hóa trong bản đồ băm và tăng tần suất của nó. 
6. Sau khi xử lý tất cả các điểm, trả về tần số tối đa nhìn thấy trong số tất cả các lớp hướng. 

Tại sao nó hoạt động: mỗi điểm thuộc về chính xác một đường đi qua gốc tọa độ và mỗi đường như vậy tương ứng với chính xác một vectơ chỉ phương rút gọn. Việc chuẩn hóa đảm bảo ánh xạ một-một giữa các đường hình học và khóa băm, do đó việc đếm tần số tương đương với việc đếm số điểm nằm trên mỗi đường hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd
from collections import defaultdict

def solve():
    n = int(input())
    freq = defaultdict(int)

    for _ in range(n):
        x, y = map(int, input().split())

        if x == 0:
            # vertical line through origin
            key = (0, 1)
        else:
            g = gcd(x, y)
            x //= g
            y //= g

            if x < 0:
                x = -x
                y = -y

            key = (x, y)

        freq[key] += 1

    print(max(freq.values()))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng điểm một cách độc lập và chuyển đổi nó thành một vectơ chỉ hướng chính tắc. Bước gcd đảm bảo rằng tất cả các vectơ tỷ lệ đều thu gọn về cùng một biểu diễn. Việc chuẩn hóa dấu đảm bảo rằng các hướng ngược nhau trên cùng một đường không bị chia thành các nhóm riêng biệt. 

Chi tiết triển khai tinh tế là xử lý x = 0 trước khi thử chuẩn hóa gcd. Không có bộ bảo vệ này, logic chia vẫn hợp lệ nhưng quy ước về dấu trở nên không nhất quán, đặc biệt đối với (0, y) và (0, -y). Việc sửa các đường thẳng đứng một cách rõ ràng sẽ tránh được sự mơ hồ và đơn giản hóa việc ánh xạ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 2
4 4
-3 -3
1 0
0 5
```Chúng tôi theo dõi các hướng được chuẩn hóa: 

| Điểm | Chuẩn hóa (x, y) | Trạng thái bản đồ tần số | 
| --- | --- | --- | 
| (2,2) | (1,1) | {(1,1):1} | 
| (4,4) | (1,1) | {(1,1):2} | 
| (-3,-3) | (1,1) | {(1,1):3} | 
| (1,0) | (1,0) | {(1,1):3, (1,0):1} | 
| (0,5) | (0,1) | {(1,1):3, (1,0):1, (0,1):1} | 

Tần số lớn nhất là 3, tương ứng với đường chéo y=x. Điều này xác nhận rằng các điểm tỷ lệ được nhóm chính xác ngay cả khi thay đổi dấu hiệu. 

### Ví dụ 2 

đầu vào:```
4
0 1
0 -2
3 6
-2 -4
```| Điểm | Chuẩn hóa (x, y) | Trạng thái bản đồ tần số | 
| --- | --- | --- | 
| (0,1) | (0,1) | {(0,1):1} | 
| (0,-2) | (0,1) | {(0,1):2} | 
| (3,6) | (1,2) | {(0,1):2, (1,2):1} | 
| (-2,-4) | (1,2) | {(0,1):2, (1,2):2} | 

Đường tốt nhất chứa 2 điểm và cả đường thẳng đứng và đường dốc đều được xử lý nhất quán thông qua quá trình chuẩn hóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi điểm yêu cầu tính toán gcd, có tính logarit theo kích thước tọa độ | 
| Không gian | O(n) | Trong trường hợp xấu nhất mọi điểm tạo thành một hướng riêng biệt | 

Các ràng buộc cho phép lên tới 100.000 điểm và giải pháp này thực hiện một lượng số học không đổi trên mỗi điểm cộng với tính toán gcd. Điều này nằm trong giới hạn thông thường cho việc thực hiện 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        n = int(input())
        freq = defaultdict(int)

        for _ in range(n):
            x, y = map(int, input().split())

            if x == 0:
                key = (0, 1)
            else:
                g = gcd(x, y)
                x //= g
                y //= g
                if x < 0:
                    x = -x
                    y = -y
                key = (x, y)

            freq[key] += 1

        return str(max(freq.values()))

    return solve()

# provided sample
assert run("""5
2 2
4 4
-3 -3
1 0
0 5
""") == "3"

# all same line
assert run("""3
1 2
2 4
-3 -6
""") == "3"

# vertical only
assert run("""3
0 1
0 -5
0 10
""") == "3"

# mixed directions
assert run("""4
1 0
0 1
-1 0
0 -1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bội số thẳng hàng | 3 | gcd chuẩn hóa chính xác | 
| trùng lặp dọc | 3 | nhóm đường thẳng đứng | 
| trục hỗn hợp | 2 | chuẩn hóa dấu và tách trục | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các điểm nằm trên cùng một đường hình học nhưng xuất hiện ở các tỷ lệ và dấu số nguyên khác nhau. Ví dụ: (1, 2), (2, 4) và (-3, -6). Thuật toán chuyển đổi tất cả chúng thành (1, 2) bằng cách chia cho gcd và sửa dấu, để chúng được tính cùng nhau. Nếu không có sự chuẩn hóa này, chúng sẽ được coi là các hướng khác nhau một cách không chính xác. 

Một trường hợp cạnh khác là căn chỉnh theo chiều dọc. Đối với đầu vào như (0, 1), (0, -1) và (0, 5), việc nhóm dựa trên độ dốc sẽ không thành công do chia cho 0 hoặc biểu diễn không nhất quán. Ánh xạ rõ ràng tới một khóa chính tắc duy nhất (0, 1) đảm bảo tất cả các điểm như vậy được hợp nhất và số tần số phản ánh chính xác rằng chúng nằm trên cùng một đường xuyên qua điểm gốc.
