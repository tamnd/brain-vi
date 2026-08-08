---
title: "CF 102503J - Gandhi bị kích thích nhẹ"
description: "Các hòn đảo và cây cầu tạo thành một đa đồ thị vô hướng được kết nối. Gandhi muốn loại bỏ càng nhiều cây cầu càng tốt trong khi vẫn giữ cho biểu đồ được kết nối."
date: "2026-08-07T20:41:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "J"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 69
verified: true
draft: false
---

[CF 102503J - Gandhi bị kích thích nhẹ](https://codeforces.com/problemset/problem/102503/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Các hòn đảo và cây cầu tạo thành một đa đồ thị vô hướng được kết nối. Gandhi muốn loại bỏ càng nhiều cây cầu càng tốt trong khi vẫn giữ cho biểu đồ được kết nối. Loại bỏ số lượng cầu tối đa có nghĩa là biểu đồ còn lại phải chứa chính xác số cạnh tối thiểu cần thiết để kết nối, do đó các cầu còn lại tạo thành một cây bao trùm. 

Trong số tất cả các cây bao trùm có thể, chỉ những cây có tổng giá trị văn hóa tối đa có thể là hợp lệ. Đối với mỗi truy vấn, chúng ta được cung cấp một tập hợp các chỉ số cầu nối và phải đếm xem có bao nhiêu cầu nối đó xuất hiện trong ít nhất một cây bao trùm tối ưu. 

Kích thước đầu vào loại trừ việc cố gắng xây dựng cây bao trùm một cách rõ ràng. Biểu đồ có thể chứa khoảng 133000 cây cầu, do đó, ngay cả một thuật toán gần với bậc hai cũng không thể thực hiện được. Chúng ta cần một giải pháp dựa trên việc sắp xếp và xử lý đồ thị gần tuyến tính. 

Những trường hợp phức tạp đều xuất phát từ những giá trị văn hóa bình đẳng. Nếu tất cả các giá trị đều khác nhau thì cây bao trùm tối đa sẽ là duy nhất nhưng các giá trị bằng nhau có thể tạo ra nhiều cây hợp lệ. Một sai lầm phổ biến khác là coi mọi cạnh được chọn bởi một lần chạy Kruskal là câu trả lời khả thi duy nhất. Truy vấn hỏi liệu một cạnh có thể xuất hiện trong bất kỳ cây bao trùm tối đa nào hay không. 

Ví dụ:```
1
3 3
1 2 5
2 3 5
1 3 5
1
3 1 2 3
```Câu trả lời là:```
3
```Việc triển khai bất cẩn bằng cách sử dụng một lần chạy Kruskal sẽ chỉ giữ lại hai cạnh và trả lời sai 2. Vì tất cả các cạnh đều có giá trị bằng nhau, nên bất kỳ hai cạnh nào cũng tạo thành một cây bao trùm tối đa, vì vậy mọi cạnh đều có thể xảy ra. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ có thể liệt kê tất cả các cây bao trùm, tính toán giá trị văn hóa của chúng, giữ giá trị tối đa và sau đó kiểm tra mọi cạnh truy vấn dựa trên tất cả các cây tối ưu. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa, nhưng số lượng cây bao trùm có thể theo cấp số nhân. Ngay cả việc chỉ tạo các tập hợp con của các cạnh cũng mang lại khả năng (2^b), điều này là không thể đối với các đồ thị lớn nhất. 

Quan sát hữu ích là cây bao trùm tối đa có cấu trúc tham lam. Thuật toán của Kruskal xử lý các cạnh từ giá trị văn hóa lớn hơn đến giá trị nhỏ hơn. Đối với một giá trị văn hóa, các cạnh có giá trị lớn hơn đã được sửa. Sau khi thu nhỏ các thành phần được hình thành bởi các cạnh lớn hơn, mọi cạnh của giá trị hiện tại kết nối hai thành phần khác nhau có thể được đưa vào một số cây bao trùm tối ưu. Các cạnh có điểm cuối đã được kết nối không bao giờ có thể xuất hiện vì đã tồn tại kết nối chu trình tốt hơn. 

Vì vậy, vấn đề trở thành tìm mọi cạnh được thuật toán Kruskal cho phép, sau đó trả lời các truy vấn bằng cách đếm các chỉ số được phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(b log b) | O(n + b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các cây cầu theo giá trị văn hóa giảm dần. Cây bao trùm tối đa sử dụng thứ tự tham lam giống như Kruskal, nhưng có trọng số lớn nhất trước tiên. 
2. Duy trì cấu trúc tập hợp rời rạc chứa các thành phần được kết nối được tạo bởi tất cả các giá trị văn hóa lớn hơn đã được xử lý trước đó. 
3. Xử lý cầu nối theo nhóm có cùng giá trị văn hóa. Đối với mỗi cây cầu trong nhóm, hãy kiểm tra xem hai điểm cuối của nó hiện có thuộc về các thành phần DSU khác nhau hay không. Nếu có, hãy đánh dấu cây cầu càng tốt. Lý do là bên trong lớp có giá trị bằng nhau này, bất kỳ cạnh không vòng lặp nào cũng có thể được chọn làm một phần của rừng bao trùm. 
4. Sau khi kiểm tra toàn bộ nhóm, hợp nhất tất cả các điểm cuối của các cây cầu trong nhóm đó kết nối các thành phần hiện tại khác nhau. Những sự hợp nhất này thể hiện việc thêm giá trị văn hóa này vào cấu trúc trước khi chuyển sang các giá trị nhỏ hơn. 
5. Đối với mỗi truy vấn, hãy đếm xem có bao nhiêu chỉ số cầu được liệt kê được đánh dấu là có thể. 

Tại sao nó hoạt động: DSU luôn đại diện cho các thành phần đã được kết nối bởi các giá trị văn hóa lớn hơn. Một cạnh nối hai thành phần khác nhau với trọng số riêng của nó có thể thay thế một cạnh khác có cùng trọng số trong cây bao trùm cực đại nào đó, do đó nó thuộc về ít nhất một nghiệm tối ưu. Một cạnh bên trong một thành phần DSU sẽ tạo ra một chu trình sử dụng các cạnh không kém hơn, do đó, việc đưa nó vào không thể cải thiện cây bao trùm và không thể yêu cầu nó theo bất kỳ mức tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n + 1))

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a != b:
            self.p[b] = a

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, b = map(int, input().split())
        edges = []

        for i in range(1, b + 1):
            x, y, c = map(int, input().split())
            edges.append((c, x, y, i))

        edges.sort(reverse=True)

        possible = [False] * (b + 1)
        dsu = DSU(n)

        i = 0
        while i < b:
            j = i
            while j < b and edges[j][0] == edges[i][0]:
                j += 1

            for k in range(i, j):
                _, x, y, idx = edges[k]
                if dsu.find(x) != dsu.find(y):
                    possible[idx] = True

            for k in range(i, j):
                _, x, y, _ = edges[k]
                dsu.union(x, y)

            i = j

        q = int(input())
        for _ in range(q):
            data = list(map(int, input().split()))
            cnt = 0
            for idx in data[1:]:
                if possible[idx]:
                    cnt += 1
            ans.append(str(cnt))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các bước sắp xếp nhóm các giá trị văn hóa bằng nhau vì việc xử lý từng cạnh một sẽ không chính xác cho phép một cạnh có trọng số bằng nhau ảnh hưởng đến một cạnh khác trong cùng một nhóm. 

Việc hợp nhất DSU xảy ra sau khi đánh dấu toàn bộ nhóm. Thứ tự này là chi tiết triển khai chính. Nếu việc hợp nhất được thực hiện ngay lập tức, các cạnh sau có cùng giá trị văn hóa có thể bị chặn một cách không chính xác. 

Mảng`possible`lưu trữ câu trả lời cho phần đồ thị của vấn đề. Vì tổng kích thước của tất cả các truy vấn bị giới hạn bởi số lượng cầu nối nên mọi truy vấn có thể chỉ cần quét các cạnh được liệt kê của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(b log b) | Việc sắp xếp chiếm ưu thế, trong khi hoạt động DSU có thời gian khấu hao gần như không đổi | 
| Không gian | O(n + b) | Lưu trữ các cạnh, cha mẹ DSU và các cờ có thể có | 

Các ràng buộc cho phép khoảng 133000 cây cầu, do đó việc sắp xếp cộng với xử lý DSU dễ dàng phù hợp với giới hạn thời gian. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
4 4
1 3 40
3 4 20
1 2 30
2 4 10
```Những cây cầu đã được sắp xếp theo văn hóa. 

| Bước | Cạnh hiện tại | Trạng thái DSU | Có thể | 
| --- | --- | --- | --- | 
| 40 | 1-3 | {1,3} đã hợp nhất | cạnh 1 | 
| 30 | 1-2 | {1,2,3} được hợp nhất | cạnh 3 | 
| 20 | 3-4 | {1,2,3,4} được hợp nhất | cạnh 2 | 
| 10 | 2-4 | cùng thành phần | không | 

Các cầu hợp lệ là 1, 2 và 3. Truy vấn đầu tiên chứa cạnh 2 và 4, vì vậy chỉ có thể có cạnh 2 và câu trả lời là 1. Truy vấn thứ hai chứa cạnh 2 và 3, cả hai đều có thể, vì vậy câu trả lời là 2. 

## Vỏ cạnh 

Các vòng lặp tự động bị từ chối vì cả hai điểm cuối luôn có cùng một đại diện DSU. 

Các cầu song song được xử lý chính xác vì các cầu được xác định theo chỉ số chứ không phải theo điểm cuối của chúng. Hai cây cầu nối cùng một hòn đảo có thể có câu trả lời khác nhau nếu giá trị văn hóa của chúng khác nhau. 

Một biểu đồ trong đó mọi cây cầu có cùng giá trị văn hóa cũng được xử lý chính xác. Quá trình DSU đầu tiên nhìn thấy mọi cạnh giữa các thành phần khác nhau càng tốt, điều này phù hợp với thực tế là mọi cạnh có thể thuộc về một số cây bao trùm.
