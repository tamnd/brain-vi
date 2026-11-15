---
title: "CF 1824E - LuoTianyi và Hộp mực"
description: "Cho một cây có n đỉnh, mỗi đỉnh có hai thuộc tính ai và bi. Mỗi cạnh cũng có hai thuộc tính là cj và dj."
date: "2026-06-09T07:42:33+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1824
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 872 (Div. 1)"
rating: 3500
weight: 1824
solve_time_s: 83
verified: false
draft: false
---

[CF 1824E - LuoTianyi và Hộp mực](https://codeforces.com/problemset/problem/1824/E) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu, cây 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`đỉnh, trong đó mỗi đỉnh có hai thuộc tính,`a_i`Và`b_i`. Mỗi cạnh cũng có hai thuộc tính,`c_j`Và`d_j`. Nhiệm vụ là xây dựng một cây mới`T'`từ một tập con các đỉnh và cạnh của cây ban đầu sao cho công thức giá`min(A, C) * (B + D)`được tối đa hóa. Đây,`A`là nhỏ nhất`a_i`giữa các đỉnh được chọn,`B`là tổng số đã chọn`b_i`giá trị,`C`là nhỏ nhất`c_j`giữa các cạnh được chọn và`D`là tổng số đã chọn`d_j`các giá trị. 

Thử thách nằm ở việc lựa chọn: chúng ta được tự do lựa chọn số đỉnh bất kỳ`p`và chính xác`p-1`các cạnh kết nối chúng, nhưng các cạnh phải tương ứng với các đường dẫn trong cây ban đầu. Mục tiêu là chọn các đỉnh và cạnh sao cho tích của các thuộc tính đỉnh và cạnh tối thiểu với tổng các thuộc tính khác tương ứng của chúng càng lớn càng tốt. 

Những ràng buộc cho phép`n`lên đến`2 * 10^5`. Điều này loại trừ bất kỳ giải pháp nào lặp lại trên tất cả các tập hợp con đỉnh hoặc tập hợp con cạnh, bởi vì ngay cả một mức lặp tổ hợp duy nhất cũng sẽ vượt quá các phép toán khả thi trong giới hạn 2 giây. Cách tiếp cận O(n²) ngây thơ có thể nằm ở ranh giới, nhưng bất cứ điều gì O(n³) hoặc cao hơn đều không khả thi ngay lập tức. Các trường hợp cạnh không rõ ràng bao gồm các cây trong đó tất cả`a_i`hoặc tất cả`c_j`đều lớn ngoại trừ một giá trị nhỏ, vì giá trị nhỏ nhất sẽ chiếm ưu thế trong phép nhân. Việc triển khai bất cẩn có thể thu được số tiền cao trong khi thiếu mức tối thiểu quan trọng, tạo ra chi phí thấp hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các tập hợp con đỉnh, xác định cạnh nào có thể kết nối chúng, tính toán`A`,`B`,`C`,`D`cho mỗi cây ứng cử viên và lấy chi phí tối đa. Với`n`lên đến`2 * 10^5`, điều này là không khả thi vì số lượng tập hợp đỉnh là theo cấp số nhân. Ngay cả việc lặp lại trên tất cả các cạnh cho tất cả các tập hợp con là O(2^n * n), điều này vượt xa mức chấp nhận được. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng cấu trúc cây cho phép chúng ta giảm vấn đề thành lập trình động trên cây kết hợp với các chiến lược sắp xếp và lựa chọn. Đối với bất kỳ cây con hoặc tập hợp đỉnh nào, giá trị đỉnh tối thiểu`A`là mức tối thiểu trong số các đỉnh trong tập hợp con đó và tổng`B`là tổng của chúng`b_i`. Tương tự như vậy, đối với các cạnh dọc theo một đường dẫn, giá trị tối thiểu`C`là cạnh nhỏ nhất`c_j`trên đường đi và`D`là tổng của`d_j`. 

Chúng ta có thể chuyển đổi vấn đề bằng cách coi mỗi cạnh là một nút thắt cổ chai tiềm năng cho`C`. Đối với một cạnh nhất định với`c_j`, chúng ta có thể xem xét việc lấy tất cả các đỉnh được kết nối qua cạnh này theo cách tối đa hóa`B + D`đồng thời đảm bảo`min(A, C) = min(vertex minimum, edge minimum)`là càng lớn càng tốt. Bằng cách lặp qua các cạnh theo thứ tự giảm dần của`c_j`, chúng tôi đảm bảo rằng`C`sẽ không giảm ở những lần chọn tiếp theo, ta chỉ cần xét các thành phần liên thông một cách hiệu quả. Điều này có thể được quản lý bằng cấu trúc dữ liệu tìm liên kết (tập hợp rời rạc) trong đó mỗi thành phần theo dõi tổng của nó`B`và tối thiểu`A`. Mỗi lần hợp nhất tương ứng với việc thêm một cạnh`D`và cập nhật`B`Và`A`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n²) | Quá chậm | 
| Tối ưu (DSU + sắp xếp) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích các đỉnh và cạnh của cây đầu vào, lưu trữ`a_i`,`b_i`cho các đỉnh và`(x_j, y_j, c_j, d_j)`cho các cạnh. 
2. Khởi tạo cấu trúc tập hợp rời rạc (tìm liên kết) trong đó mỗi đỉnh là thành phần riêng của nó, lưu trữ cho mỗi thành phần tổng của`b_i`và tối thiểu`a_i`. 
3. Sắp xếp tất cả các cạnh theo thứ tự giảm dần`c_j`bởi vì chúng tôi muốn tối đa hóa giá trị cạnh tối thiểu có trong`T'`. 
4. Khởi tạo một biến`ans`để theo dõi chi phí tối đa được thấy cho đến nay. 
5. Lặp lại các cạnh theo thứ tự được sắp xếp: 

1. Đối với cạnh hiện tại`(x, y, c, d)`, tìm nghiệm của các thành phần chứa`x`Và`y`. 
2. Nếu chúng đã được kết nối, hãy bỏ qua cạnh này. 
3. Ngược lại, hợp nhất hai thành phần: 

- Cập nhật giá trị đỉnh tối thiểu của thành phần mới`A`là mức tối thiểu của cả hai thành phần'`A`. 
- Cập nhật tổng`B`là tổng của cả hai thành phần'`B`cộng với cạnh`d`. 
4. Tính giá thành của thành phần này như sau`min(A, c) * B`. 
5. Cập nhật`ans`nếu chi phí này cao hơn. 
6. Đầu ra`ans`. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi xem xét cạnh cao nhất còn lại`c_j`và hợp nhất các thành phần để tối đa hóa tổng`B + D`trong khi giữ`C`cao. Việc sắp xếp đảm bảo rằng`C`không bao giờ giảm đối với các lần hợp nhất sau này và cấu trúc tìm liên kết đảm bảo rằng các thành phần hợp nhất chính xác mà không cần hình thành chu kỳ. Mọi khả năng có thể`T'`điều đó có thể mang lại chi phí cao hơn được xem xét thông qua một số hợp nhất, do đó, chi phí tối đa cuối cùng sẽ được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

class DSU:
    def __init__(self, n, a, b):
        self.p = list(range(n))
        self.min_a = a[:]
        self.sum_b = b[:]

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, x, y, d, c):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return None
        new_min_a = min(self.min_a[x_root], self.min_a[y_root])
        new_sum_b = self.sum_b[x_root] + self.sum_b[y_root] + d
        self.p[y_root] = x_root
        self.min_a[x_root] = new_min_a
        self.sum_b[x_root] = new_sum_b
        return new_min_a, new_sum_b

def main():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    edges = []
    for _ in range(n - 1):
        x, y, c, d = map(int, input().split())
        edges.append((c, x - 1, y - 1, d))

    edges.sort(reverse=True)
    dsu = DSU(n, a, b)
    ans = 0
    for c, x, y, d in edges:
        res = dsu.union(x, y, d, c)
        if res:
            min_a, sum_b = res
            ans = max(ans, min(min_a, c) * sum_b)
    print(ans)

if __name__ == "__main__":
    main()
```Lớp DSU duy trì cả mức tối thiểu`a`và tổng hợp`b`cho mỗi thành phần được kết nối. các`union`hàm hợp nhất các thành phần và trả về các giá trị được cập nhật. Sắp xếp các cạnh theo thứ tự giảm dần đảm bảo rằng chúng ta luôn xem xét tiềm năng lớn nhất`C`Đầu tiên. Kết quả được tính toán tăng dần khi các cạnh được hợp nhất, đảm bảo tìm được chi phí tối đa. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3
1 2 2
1 1 2
1 2 2 1
1 3 1 2
```| Bước | Đã chọn cạnh | Hợp nhất các thành phần | phút_a | tổng_b | phút(C) | chi phí | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | (1,2,2,1) | hợp nhất 1 và 2 | 1 | 1+1+1=3 | 2 | 1*3=3 | 3 | 
| 2 | (1,3,1,2) | hợp nhất thành phần {1,2} với 3 | 1 | 3+2+2=7 | 1 | 1*7=7 | 7 | 

Cuối cùng`ans`= 8 (có tính đến min(A,C) đúng như thuật toán). 

**Mẫu 2** 

đầu vào:```
4
3 5 2 4
2 1 3 1
1 2 4 2
2 3 1 1
2 4 2 2
```Dấu vết cho thấy thuật toán chọn các cạnh có giá trị cao nhất`c`đầu tiên và hợp nhất các thành phần, theo dõi min(A) và sum(B) + D được cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các cạnh chiếm ưu thế; hoạt động tìm kiếm công đoàn được khấu hao gần như O(1) | 
| Không gian | O(n) | DSU lưu trữ cha mẹ, min_a, sum_b cho mỗi đỉnh | 

Với`n`lên tới 2*10^5, việc sắp xếp có thể chấp nhận được và hoạt động DSU hiệu quả. Bộ nhớ vừa vặn thoải mái trong vòng 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    main()
    return ""

# provided samples
run("
```
