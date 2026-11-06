---
title: "CF 1250N - Dây điện"
description: "Mỗi dây kết nối hai điểm tiếp xúc và chúng tôi nói rằng hai dây có liên quan nếu chúng có chung ít nhất một điểm cuối hoặc nếu có một chuỗi dây trong đó các dây liên tiếp có chung điểm cuối."
date: "2026-06-18T17:35:07+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1250
codeforces_index: "N"
codeforces_contest_name: "2019-2020 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2000
weight: 1250
solve_time_s: 120
verified: false
draft: false
---

[CF 1250N - Dây điện](https://codeforces.com/problemset/problem/1250/N) 

**Xếp hạng:** 2000 
**Tags:** dfs và tương tự, đồ thị, tham lam 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi dây kết nối hai điểm tiếp xúc và chúng tôi nói rằng hai dây có liên quan nếu chúng có chung ít nhất một điểm cuối hoặc nếu có một chuỗi dây trong đó các dây liên tiếp có chung điểm cuối. Điều này tạo ra khái niệm về khả năng kết nối qua chính các dây: dây là các nút và việc chia sẻ một điểm tiếp xúc sẽ tạo ra mối quan hệ vô hướng giữa chúng. 

Mục tiêu là sửa đổi càng ít điểm cuối dây càng tốt để tất cả các dây được kết nối theo mối quan hệ này, nghĩa là từ bất kỳ dây nào bạn có thể tiếp cận bất kỳ dây nào khác thông qua các điểm cuối được chia sẻ. 

Việc sửa đổi bao gồm lấy một điểm cuối của dây và thay đổi nó thành bất kỳ số nguyên nào trong phạm vi cho phép, miễn là hai điểm cuối của dây vẫn khác nhau. Mỗi thay đổi như vậy tốn một đơn vị thời gian. 

Ràng buộc rằng tổng của n trên tất cả các trường hợp thử nghiệm nhiều nhất là 100000 ngụ ý rằng mọi giải pháp đều phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Cách tiếp cận O(n log n) hoặc O(n α(n)) có thể được chấp nhận, nhưng mọi phương pháp bậc hai về số lượng dây trên mỗi trường hợp thử nghiệm sẽ không thành công. Điều này gợi ý rõ ràng rằng chúng ta nên nén cấu trúc của thể hiện thành một biểu đồ có các thành phần được kết nối có thể được tính toán một cách hiệu quả. 

Một nỗ lực ngây thơ có thể cố gắng hợp nhất nhiều lần các thành phần bằng cách quét tất cả các cặp dây và kiểm tra xem chúng có thể được kết nối với những sửa đổi tối thiểu hay không. Điều đó không thành công vì việc kiểm tra kết nối sau mỗi lần sửa đổi giả định đã tốn O(n) và việc thực hiện việc này nhiều lần sẽ dẫn đến O(n²) hoặc tệ hơn. 

Ý tưởng ngây thơ thứ hai là xử lý từng dây một cách độc lập và gắn nó vào một cấu trúc đang phát triển. Điều này cũng thất bại một cách tinh vi vì việc một dây đã được kết nối hay chưa còn phụ thuộc vào chuỗi gián tiếp thông qua các điểm cuối được chia sẻ chứ không chỉ cấu trúc cục bộ. 

Một trường hợp nhỏ nhưng quan trọng xuất hiện khi tất cả các dây đã tạo thành một thành phần được kết nối duy nhất. Trong trường hợp đó, không cần sửa đổi. Một trường hợp khác là khi không có điểm cuối nào được chia sẻ. Sau đó, mọi dây đều được cách ly và chúng tôi phải kết nối tất cả các thành phần bằng cách hàn lại các điểm cuối một cách rõ ràng. 

## Phương pháp tiếp cận 

Chúng tôi diễn giải lại hệ thống dưới dạng biểu đồ trong đó mỗi dây là một nút và hai dây liền kề nhau nếu chúng có chung một điểm tiếp xúc. Nếu chúng ta xây dựng biểu đồ này một cách rõ ràng thì nhiệm vụ sẽ giảm xuống còn làm cho nó được kết nối bằng cách sửa đổi các điểm cuối, trong đó mỗi sửa đổi có thể hợp nhất một thành phần bị ngắt kết nối này thành một thành phần khác. 

Cách tiếp cận bạo lực sẽ tính toán lại kết nối một cách rõ ràng sau mỗi lần thay đổi điểm cuối giả định. Mỗi lần thử yêu cầu xây dựng lại vùng lân cận hoặc chạy BFS/DFS qua dây dẫn, tốn O(n) cho mỗi lần kiểm tra. Vì cần có các sửa đổi O(n) tiềm năng, điều này dẫn đến O(n²), quá chậm đối với 100000 dây. 

Quan sát quan trọng là khả năng kết nối giữa các dây được xác định hoàn toàn bởi các điểm cuối dùng chung và việc sửa đổi điểm cuối tương đương với việc “gán lại” một dây vào một thành phần khác bằng cách buộc nó chia sẻ một điểm tiếp xúc đã chọn. Thay vì mô phỏng các thay đổi một cách linh hoạt, trước tiên chúng tôi tính toán các thành phần được kết nối của dây trong cấu hình ban đầu. Mỗi thành phần đều đạt mức tối đa theo các điểm cuối được chia sẻ, do đó không cần thay đổi nội bộ nào để kết nối nội bộ. 

Khi đã biết các thành phần, vấn đề sẽ giảm xuống việc kết nối các thành phần này với nhau. Nếu có c thành phần, chúng ta cần ít nhất c − 1 sự hợp nhất, bởi vì mỗi sửa đổi có thể kết nối một thành phần riêng biệt trước đó với một cấu trúc đã được kết nối. Chiến lược tối ưu là chọn một thành phần làm gốc và kết nối mọi thành phần khác với nó bằng cách sửa đổi chính xác một điểm cuối của một dây đại diện duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Thành phần + Hợp nhất tham lam | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi mô hình hóa các dây dưới dạng các nút trong biểu đồ nơi tồn tại các cạnh nếu hai dây có chung điểm cuối. Thay vì xây dựng rõ ràng tất cả các cạnh theo cặp, chúng tôi sử dụng ánh xạ từ các điểm cuối đến danh sách các dây chạm vào chúng. 

1. Xây dựng một cấu trúc tập hợp rời rạc trống trên dây. Đối với mỗi điểm tiếp xúc, thu thập tất cả các dây nối tới nó và nối chúng thành một bộ phận. Bước này xây dựng các thành phần được kết nối theo thời gian tuyến tính trên tổng số lần xuất hiện của dây. 
2. Sau khi thực hiện các thao tác kết hợp, hãy trích xuất các thành phần được kết nối của dây. Mỗi thành phần đại diện cho một nhóm tối đa có thể truy cập được lẫn nhau thông qua các điểm cuối được chia sẻ. 
3. Nếu chỉ có một thành phần thì không cần sửa đổi nên câu trả lời là 0. 
4. Nếu không, hãy chọn một thành phần làm thành phần cơ sở. Từ thành phần này, chọn bất kỳ dây nào và chọn một trong các điểm cuối của nó làm điểm neo chung. 
5. Đối với mọi thành phần khác, chọn một dây tùy ý bên trong nó. Đối với dây đó, hãy quyết định điểm cuối nào cần sửa đổi. Nếu một điểm cuối bằng điểm neo đã chọn, chúng tôi sẽ sửa đổi điểm cuối còn lại; nếu không chúng tôi sẽ sửa đổi điểm cuối một cách tùy ý. Chúng tôi đặt điểm cuối đã chọn thành điểm neo, gắn thành phần này vào đế một cách hiệu quả. 
6. Mỗi thao tác như vậy làm giảm số thành phần đi một, vì vậy chúng ta thực hiện chính xác c − 1 thao tác. 

Lý do điều này hoạt động là vì khi một dây từ một thành phần chia sẻ điểm neo, mọi dây trong thành phần đó sẽ được kết nối thông qua kết nối bên trong ban đầu của thành phần, do đó toàn bộ thành phần sẽ hợp nhất vào cấu trúc chính. 

### Tại sao nó hoạt động 

Mỗi thành phần của biểu đồ ban đầu được đóng dưới kết nối chia sẻ điểm cuối, nghĩa là không có dây nào bên ngoài thành phần chia sẻ điểm cuối với bất kỳ dây nào bên trong nó. Bằng cách sửa đổi một điểm cuối của bất kỳ dây nào trong một thành phần để khớp với điểm neo, chúng tôi giới thiệu một điểm cuối dùng chung mới kết nối toàn bộ thành phần với thành phần gốc. Vì các thành phần rời rạc trước khi sửa đổi nên mỗi thao tác sẽ hợp nhất chính xác một thành phần mà không ảnh hưởng đến giá trị nội tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        edges = []
        mp = {}

        for i in range(n):
            x, y = map(int, input().split())
            edges.append((x, y))
            if x not in mp:
                mp[x] = []
            if y not in mp:
                mp[y] = []
            mp[x].append(i)
            mp[y].append(i)

        dsu = DSU(n)

        for lst in mp.values():
            if len(lst) > 1:
                first = lst[0]
                for v in lst[1:]:
                    dsu.union(first, v)

        comp = {}
        for i in range(n):
            r = dsu.find(i)
            comp.setdefault(r, []).append(i)

        comps = list(comp.values())

        if len(comps) == 1:
            print(0)
            continue

        base_wire = comps[0][0]
        bx, by = edges[base_wire]

        ops = []
        for ci in range(1, len(comps)):
            w = comps[ci][0]
            x, y = edges[w]

            if x == bx or (y != bx):
                ops.append((w + 1, x, bx))
            else:
                ops.append((w + 1, y, bx))

        print(len(ops))
        for w, a, b in ops:
            print(w, a, b)

if __name__ == "__main__":
    solve()
```DSU nén biểu đồ ngầm của các dây được kết nối thông qua các điểm cuối được chia sẻ. Mỗi điểm cuối hoạt động như một trung tâm liên kết tất cả các dây chạm vào nó và kết hợp các dây đó để đảm bảo tìm thấy từng thành phần được kết nối mà không cần xây dựng danh sách kề bậc hai một cách rõ ràng. 

Việc lựa chọn dây cơ sở sẽ cung cấp điểm cuối neo ổn định. Mỗi thành phần khác đóng góp chính xác một thao tác buộc một trong các dây của nó phải chia sẻ điểm cuối đó, đảm bảo sự hợp nhất. 

Việc lựa chọn có điều kiện điểm cuối nào để thay thế đảm bảo chúng ta không bao giờ tạo ra một dây không hợp lệ có điểm cuối giống hệt nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2
2 3
4 5
5 6
```Chúng tôi bắt đầu với hai thành phần: {1,2,3} và {4,5,6} về kết nối dây. 

| Bước | Dây được chọn | Điểm cuối trước | Neo | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | dây 3 | (4,5) | 1 | (4 → 1) | 

Sau khi sửa đổi dây 3, nó chia sẻ điểm cuối 1 với thành phần đầu tiên, hợp nhất mọi thứ thành một cấu trúc được kết nối. 

Đầu ra:```
1
3 4 1
```Điều này xác nhận rằng một sửa đổi duy nhất là đủ khi có chính xác hai thành phần. 

### Ví dụ 2 

đầu vào:```
3
10 20
20 30
40 50
```Ở đây chúng ta có hai thành phần: {wire1, wire2} và {wire3}. 

| Bước | Dây được chọn | Điểm cuối trước | Neo | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | dây 3 | (40,50) | 10 | (40 → 10) | 

Sau thao tác này, dây 3 kết nối vào thành phần chính. 

Đầu ra:```
1
3 40 10
```Điều này chứng tỏ rằng ngay cả khi các điểm cuối hoàn toàn rời rạc, một sửa đổi được lựa chọn cẩn thận cũng đủ cho mỗi thành phần riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Liên minh DSU qua danh sách điểm cuối và nhóm một lượt | 
| Không gian | O(n) | lưu trữ cho các cạnh, DSU và lân cận điểm cuối | 

Giải pháp này vẫn tuyến tính trong thực tế, điều này là cần thiết vì tổng số dây trong tất cả các trường hợp thử nghiệm có thể lên tới 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    backup = sys.stdout
    sys.stdout = output

    # call solution
    solve()

    sys.stdout = backup
    return output.getvalue().strip()

# sample 1
assert run("""2
1
4 7
4
1 2
2 3
4 5
5 6
""") == """0
1
2 3 5"""

# single wire
assert run("""1
1
1 2
""") == """0"""

# disjoint pairs
assert run("""1
2
1 2
3 4
""").split()[0] == "1"

# chain already connected
assert run("""1
3
1 2
2 3
3 4
""") == """0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dây đơn | 0 | trường hợp cơ sở | 
| hai dây rời nhau | 1 | cần hợp nhất một lần | 
| chuỗi dây | 0 | cấu trúc đã được kết nối | 
| trường hợp mẫu | 0/1 lần | tính đúng đắn của việc phân nhóm DSU | 

## Vỏ cạnh 

Khi tất cả các dây đã chia sẻ điểm cuối một cách bắc cầu, DSU sẽ tạo ra một thành phần duy nhất và thuật toán ngay lập tức trả về các hoạt động bằng 0. Điều này bao gồm các chuỗi dài trong đó kết nối là gián tiếp, chẳng hạn như một chuỗi các dây, mỗi dây chia sẻ chính xác một điểm cuối với điểm tiếp theo. 

Khi không có điểm cuối nào được chia sẻ, mỗi dây sẽ tạo thành thành phần riêng của nó. Thuật toán chọn một dây cơ sở và sau đó kết nối trực tiếp từng dây còn lại với điểm cuối neo, tạo ra chính xác n − 1 thao tác. Điều này là tối ưu vì mỗi thao tác chỉ có thể gộp một thành phần vào cấu trúc chính. 

Khi nhiều dây chia sẻ cùng một điểm cuối nhưng bị cô lập, DSU sẽ hợp nhất chúng một cách chính xác thành một thành phần duy nhất trước khi lên kế hoạch cho bất kỳ hoạt động nào. Điều này tránh việc đếm quá mức các kết hợp cần thiết và đảm bảo rằng chúng tôi không thực hiện việc hàn lại không cần thiết.
