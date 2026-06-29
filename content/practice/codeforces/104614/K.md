---
title: "CF 104614K - Hai biểu đồ trở thành một"
description: "Chúng ta được cung cấp hai mô tả bằng văn bản về các cấu trúc phân cấp gốc rễ. Mỗi cấu trúc xác định các phòng ban được gắn nhãn bằng số nguyên, trong đó mỗi phòng ban có thể có một số phòng ban trực tiếp."
date: "2026-06-29T21:31:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104614
codeforces_index: "K"
codeforces_contest_name: "2022-2023 ICPC East Central North America Regional Contest (ECNA 2022)"
rating: 0
weight: 104614
solve_time_s: 60
verified: true
draft: false
---

[CF 104614K - Hai biểu đồ trở thành một](https://codeforces.com/problemset/problem/104614/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai mô tả bằng văn bản về các cấu trúc phân cấp gốc rễ. Mỗi cấu trúc xác định các phòng ban được gắn nhãn bằng số nguyên, trong đó mỗi phòng ban có thể có một số phòng ban trực tiếp. Định dạng này là đệ quy: một số duy nhất đại diện cho một bộ phận lá và một số theo sau là một số nhóm trong ngoặc đơn đại diện cho một bộ phận có nhiều con, mỗi con là một hệ thống phân cấp đầy đủ. 

Nhiệm vụ là xác định xem hai hệ thống phân cấp đã cho có đại diện cho cùng một cấu trúc cây gốc hay không, bỏ qua thứ tự của các cây con. Hai phòng ban được coi là giống hệt nhau giữa hai biểu đồ nếu cấu trúc cây gốc đẳng cấu dưới các nút con không có thứ tự và nhãn phòng ban phải khớp chính xác tại các nút tương ứng. 

Khó khăn chính đến từ việc cùng một cấu trúc có thể được viết theo nhiều cách khác nhau vì các chữ cái có thể xuất hiện theo bất kỳ thứ tự nào và khoảng cách là tùy ý. Ngoài ra, kích thước đầu vào có thể cực kỳ lớn, lên tới 100.000 nút, do đó, bất kỳ phương pháp nào liên tục phân tích hoặc so sánh các cây con theo cách đơn giản sẽ thất bại. 

Các ràng buộc ngụ ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến việc băm cây con được thực hiện lặp đi lặp lại trên mỗi nút có nối chuỗi sẽ chuyển sang hành vi bậc hai trong trường hợp xấu nhất do lồng sâu (độ sâu lên tới 1000) và phân nhánh lớn. 

Một số tình huống khó khăn rất khó xảy ra. 

Một vấn đề là đặt hàng. Ví dụ, hai biểu diễn 

11 (10) (12 (13) (17) (28)) 

11 (12 (17) (28) (13)) (10) 

nên được coi là giống hệt nhau vì trẻ em không có thứ tự. 

Vấn đề thứ hai là tính linh hoạt của khoảng trắng. Đầu vào như 

11 ( 10 ) ( 12 ) 

11(10(12)) 

phải được phân tích cú pháp giống hệt nhau mặc dù có sự khác biệt về định dạng. 

Vấn đề thứ ba là cấu trúc không khớp ngay cả khi nhãn khớp cục bộ. Ví dụ 

11 (10) (12) 

11 (10) (13) 

khác nhau vì một gốc cây con khác nhau (12 so với 13), mặc dù cấu trúc trên cùng giống hệt nhau. 

Một sai lầm ngây thơ là thử chuẩn hóa chuỗi sau đó so sánh trực tiếp, vì việc sắp xếp lại dấu ngoặc đơn không bảo đảm sự bình đẳng từ vựng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là phân tích đầy đủ từng hệ thống phân cấp thành một cấu trúc cây và sau đó so sánh hai cây về tính đẳng cấu. Một so sánh đơn giản sẽ so khớp đệ quy các nút con của mỗi nút với nhau bằng cách thử tất cả các hoán vị. Điều đó ngay lập tức dẫn đến độ phức tạp giai thừa trên mỗi nút trong trường hợp xấu nhất khi một nút có nhiều nút con. Ngay cả với các ràng buộc cho biết mỗi nút có tối đa 100 nút con, việc so khớp lặp đi lặp lại giữa các cây con vẫn dẫn đến hành vi theo cấp số nhân nếu được triển khai trực tiếp. 

Một ý tưởng ngây thơ khác là chuyển đổi từng cây con thành một biểu diễn chuỗi chuẩn bằng cách sắp xếp đệ quy các biểu diễn con và nối chúng lại. Điều này đúng về mặt logic, nhưng nếu được triển khai bằng cách nối chuỗi lặp đi lặp lại thì quá trình này sẽ trở nên quá chậm vì việc xây dựng chuỗi qua đệ quy sâu sẽ dẫn đến việc sao chép lặp lại và độ phức tạp tổng thể có thể giảm xuống bậc hai trong tổng kích thước đầu vào. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xây dựng hoặc sắp xếp các chuỗi lớn một cách rõ ràng. Chúng ta chỉ cần một mã định danh ổn định cho mỗi cây con sao cho các cây con giống hệt nhau tạo ra các mã định danh giống hệt nhau bất kể thứ tự con nào. Điều này gợi ý việc tính toán hàm băm cấu trúc từ dưới lên. 

Chúng tôi phân tích biểu thức thành một cây, sau đó gán cho mỗi cây con một hàm băm chuẩn được tính từ nhãn của nó và nhiều tập hợp băm con của nó. Vì các phần tử con không có thứ tự nên chúng ta phải kết hợp các giá trị băm của chúng theo cách không phụ thuộc vào thứ tự. Việc sắp xếp các giá trị băm con có thể chấp nhận được vì tổng số con trên mỗi nút là nhỏ (nhiều nhất là 100) và độ phức tạp tổng thể vẫn tuyến tính trong phân nhánh nhưng tuyến tính trong tổng số nút. 

Khi mỗi cây con có một giá trị băm, việc so sánh hai biểu đồ sẽ không còn là so sánh các giá trị băm gốc.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp cây Brute Force | hàm mũ | O(n) | Quá chậm | 
| Cây con băm (dạng chuẩn) | O(n log k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi dòng đầu vào là một biểu thức cây có gốc đơn và phân tích nó thành các nút bằng cách sử dụng trình phân tích cú pháp dựa trên ngăn xếp. 

1. Quét chuỗi từ trái sang phải, trích xuất các số nguyên làm nhãn bộ phận và hiểu dấu ngoặc đơn là dấu phân cách cấu trúc. Bất cứ khi nào chúng ta đọc một số, chúng ta sẽ tạo một nút. Khi nhìn thấy dấu ngoặc đơn đóng, chúng ta sẽ hoàn thành bối cảnh cây con hiện tại và đính kèm nó với cây mẹ của nó. Bước này xây dựng cây một cách rõ ràng. 
2. Trong quá trình phân tích cú pháp, hãy duy trì một chồng các nút đại diện cho đường dẫn hoạt động hiện tại trong cây. Khi chúng tôi gặp một số mới theo sau là dấu ngoặc đơn mở hoặc cấu trúc con, chúng tôi sẽ đẩy số đó làm nút hiện tại. Khi chúng ta hoàn thành một cây con, chúng ta sẽ quay trở lại cây mẹ của nó. Điều này đảm bảo chúng tôi xây dựng lại hệ thống phân cấp chính xác. 
3. Sau khi xây dựng cây, hãy tính giá trị băm cho mỗi nút bằng cách sử dụng phép duyệt theo thứ tự sau. Đối với nút lá, hàm băm chỉ phụ thuộc vào nhãn của nó. Đối với một nút nội bộ, trước tiên chúng tôi tính toán giá trị băm của tất cả các giá trị băm con, sau đó sắp xếp danh sách các giá trị băm con. Việc sắp xếp là cần thiết vì trẻ em không có thứ tự trong định nghĩa bài toán. 
4. Kết hợp nhãn nút và các hàm băm con đã sắp xếp thành một biểu diễn tổng hợp duy nhất. Trong thực tế, chúng tôi ánh xạ từng kết hợp riêng biệt tới một ID số nguyên duy nhất bằng từ điển. Điều này tránh việc nối chuỗi đắt tiền và giữ cho việc so sánh luôn được duy trì liên tục. 
5. Thực hiện quy trình tương tự cho cả hai dòng đầu vào, tạo ra hai mã định danh gốc. 
6. So sánh hai mã định danh gốc. Nếu trùng nhau thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

Tại sao việc sắp xếp là đủ ở đây là việc nhận dạng cây con chỉ phụ thuộc vào cấu trúc nhiều tập hợp của các cây con chứ không phụ thuộc vào thứ tự của chúng. Bằng cách chuyển đổi các phần tử con thành một chuỗi được sắp xếp gồm các mã định danh ổn định, chúng tôi thực thi dạng chuẩn. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi cây con được gán một mã định danh duy nhất chỉ phụ thuộc vào cấu trúc và nhãn của nó chứ không phụ thuộc vào cách viết hoặc thứ tự các cây con của nó. Hai cây con tương đương khi và chỉ khi định danh được tính toán của chúng bằng nhau. Bởi vì các mã định danh được gán từ dưới lên nên bất kỳ sự khác biệt nào về cấu trúc ở bất kỳ độ sâu nào cũng sẽ lan truyền lên trên và làm thay đổi mã định danh gốc. Điều này đảm bảo rằng sự bình đẳng của các mã định danh gốc tương đương với sự tương đương về cấu trúc của hai hệ thống phân cấp đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def parse_tree(s):
    n = len(s)
    i = 0

    stack = []

    # Each element: (node_label, children list)
    nodes = []

    def new_node(val):
        return [val, []]

    root = None

    while i < n:
        if s[i].isspace():
            i += 1
            continue

        if s[i].isdigit():
            val = 0
            while i < n and s[i].isdigit():
                val = val * 10 + (ord(s[i]) - 48)
                i += 1
            node = new_node(val)
            nodes.append(node)
            if stack:
                stack[-1][1].append(node)
            stack.append(node)

        elif s[i] == '(':
            i += 1

        elif s[i] == ')':
            stack.pop()
            i += 1
        else:
            i += 1

    root = nodes[0]
    return root

def canonical_hash(root):
    from collections import defaultdict

    sys.setrecursionlimit(10**7)

    memo = {}
    counter = 1

    def dfs(node):
        nonlocal counter
        label, children = node

        child_ids = []
        for ch in children:
            child_ids.append(dfs(ch))

        child_ids.sort()

        key = (label, tuple(child_ids))
        if key not in memo:
            memo[key] = counter
            counter += 1
        return memo[key]

    return dfs(root)

def solve_one(line):
    root = parse_tree(line)
    return canonical_hash(root)

def main():
    s1 = input().strip()
    s2 = input().strip()

    id1 = solve_one(s1)
    id2 = solve_one(s2)

    print("Yes" if id1 == id2 else "No")

if __name__ == "__main__":
    main()
```Hàm phân tích cú pháp đọc số nguyên và xây dựng các nút tăng dần. Mỗi nút giữ một danh sách các nút con của nó. Ngăn xếp đảm bảo rằng việc lồng nhau được xác định bằng dấu ngoặc đơn sẽ được chuyển trực tiếp sang mối quan hệ cha-con. 

Hàm băm thực hiện DFS. Mỗi nút thu thập các giá trị băm của con nó, sắp xếp chúng và sử dụng từ điển để gán một ID chuẩn nhỏ gọn. Việc ghi nhớ đảm bảo rằng các cấu trúc cây con giống hệt nhau có cùng mã định danh ngay cả khi gặp phải nhiều lần. 

Một chi tiết tinh tế là việc sắp xếp được thực hiện trên các ID số nguyên, không phải trên các chuỗi cấu trúc, giúp so sánh nhanh chóng. Một điều nữa là chúng tôi dựa vào việc truyền tải thứ tự sau để mọi phần tử con đều được giải quyết hoàn toàn trước khi tính toán hàm băm gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
11 (10) (12 (13) (17) (28))
11 (12 (17) (28) (13)) (10)
```Chúng tôi phân tích cả hai thành cùng một cấu trúc nhưng với các thứ tự con khác nhau. 

| Bước | Nút | ID con trước khi sắp xếp | Đã sắp xếp tuple | ID được chỉ định | 
| --- | --- | --- | --- | --- | 
| 10 | 10 | [] | () | 1 | 
| 13 | 13 | [] | () | 2 | 
| 17 | 17 | [] | () | 3 | 
| 28 | 28 | [] | () | 4 | 
| 12 | 12 | [2,3,4] | [2,3,4] | 5 | 
| 11 | 11 | [1,5] | [1,5] | 6 | 

Cả hai cây đều tạo ra ID gốc là 6 nên kết quả là "Có". 

Dấu vết này cho thấy thứ tự con không ảnh hưởng đến bộ dữ liệu chuẩn cuối cùng. 

### Ví dụ 2 

đầu vào:```
11 (10) (12)
11 (10(12))
```Cây đầu tiên có 11 con với 2 con 10 và 12. Cây thứ 2 tổ 12 con dưới 10 con. 

| Bước | Nút | ID con | Đã sắp xếp tuple | ID được chỉ định | 
| --- | --- | --- | --- | --- | 
| 10 | 10 | [] | () | 1 | 
| 12 | 12 | [] | () | 2 | 
| 11 | 11 | [1,2] | [1,2] | 3 | 
| 10 | 10 | [2] | [2] | 4 | 
| 11 | 11 | [1,4] so với [1,2] không khớp | khác nhau | khác nhau | 

Các mã định danh gốc khác nhau nên kết quả đầu ra là "Không". 

Điều này chứng tỏ rằng các nhãn giống hệt nhau không bao hàm cấu trúc giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log k) | Mỗi nút sắp xếp tối đa k con, k ≤ 100, trên tất cả các nút tổng là tuyến tính khi phân nhánh | 
| Không gian | O(n) | Mỗi nút được lưu trữ một lần cộng với bảng ghi nhớ chữ ký cây con | 

Các ràng buộc cho phép lên tới 100.000 nút, do đó cần có giải pháp gần tuyến tính. Việc sắp xếp các phần tử con có mức độ giới hạn sẽ giữ cho tổng chi phí ở mức nhỏ và việc băm tránh việc so sánh cấu trúc lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    sys.setrecursionlimit(10**7)

    def parse_tree(s):
        n = len(s)
        i = 0
        stack = []
        nodes = []

        def new_node(v):
            return [v, []]

        while i < n:
            if s[i].isspace():
                i += 1
                continue
            if s[i].isdigit():
                v = 0
                while i < n and s[i].isdigit():
                    v = v * 10 + (ord(s[i]) - 48)
                    i += 1
                node = new_node(v)
                nodes.append(node)
                if stack:
                    stack[-1][1].append(node)
                stack.append(node)
            elif s[i] == '(':
                i += 1
            elif s[i] == ')':
                stack.pop()
                i += 1
            else:
                i += 1

        root = nodes[0]

        memo = {}
        counter = 1

        def dfs(node):
            nonlocal counter
            label, children = node
            ids = []
            for c in children:
                ids.append(dfs(c))
            ids.sort()
            key = (label, tuple(ids))
            if key not in memo:
                memo[key] = counter
                counter += 1
            return memo[key]

        return dfs(root)

    def solve():
        s1 = input().strip()
        s2 = input().strip()
        return "Yes" if run_tree(s1) == run_tree(s2) else "No"

    def run_tree(s):
        n = len(s)
        i = 0
        stack = []
        nodes = []

        def new_node(v):
            return [v, []]

        while i < n:
            if s[i].isspace():
                i += 1
                continue
            if s[i].isdigit():
                v = 0
                while i < n and s[i].isdigit():
                    v = v * 10 + (ord(s[i]) - 48)
                    i += 1
                node = new_node(v)
                nodes.append(node)
                if stack:
                    stack[-1][1].append(node)
                stack.append(node)
            elif s[i] == '(':
                i += 1
            elif s[i] == ')':
                stack.pop()
                i += 1
            else:
                i += 1

        root = nodes[0]

        memo = {}
        counter = 1

        def dfs(node):
            nonlocal counter
            label, children = node
            ids = []
            for c in children:
                ids.append(dfs(c))
            ids.sort()
            key = (label, tuple(ids))
            if key not in memo:
                memo[key] = counter
                counter += 1
            return memo[key]

        return dfs(root)

    s1 = input().strip()
    s2 = input().strip()

    print("Yes" if run_tree(s1) == run_tree(s2) else "No")

# provided samples
assert run("11 (10) (12 (13) (17) (28))\n11 (12 (17) (28) (13)) (10)\n") == "Yes"
assert run("11 ( 10 ) ( 12 )\n11(10(12))\n") == "No"

# custom cases
assert run("1\n1\n") == "Yes"
assert run("1\n2\n") == "No"
assert run("1 (2 3)\n1 (3 2)\n") == "Yes"
assert run("1 (2)\n1 (2 (3))\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 vs 1`| Có | cây giống nhau tối thiểu | 
|`1 vs 2`| Không | nguồn gốc khác nhau | 
| trẻ vô trật tự | Có | bất biến hoán vị | 
| thêm chiều sâu | Không | phát hiện sự không phù hợp về cấu trúc | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là cây một nút. Trình phân tích cú pháp phải xử lý chính xác một số đơn độc không có dấu ngoặc đơn như một cây hoàn chỉnh. Thuật toán gán cho nó một hàm băm lá chỉ dựa trên nhãn của nó, do đó hai nút đơn giống hệt nhau sẽ so sánh bằng nhau. 

Một trường hợp khác là phân nhánh nhiều, trong đó một nút có gần 100 nút con. Độ chính xác phụ thuộc vào việc sắp xếp các giá trị băm con và thuật toán vẫn hoạt động vì việc sắp xếp chỉ được áp dụng ở cấp nút đó. Đối với một đầu vào như```
1 (2) (3) (4) ... (100)
1 (100) (99) ... (2)
```cả hai bên đều tạo ra nhiều tập băm con giống hệt nhau, do đó hàm băm gốc khớp với nhau. 

Việc lồng sâu tới độ sâu 1000 được xử lý an toàn vì độ sâu DFS bằng độ sâu của cây và giới hạn đệ quy Python được tăng lên. Mỗi nút được truy cập đúng một lần, vì vậy ngay cả những chuỗi trong trường hợp xấu nhất như```
1(2(3(4(...))))
```vẫn tính toán công việc tuyến tính, tạo ra một chuỗi các phép tính băm mà không cần tính toán lại hoặc quay lui.
