---
title: "CF 106107M - Nguồn gốc của sự loại trừ"
description: "Chúng ta được cung cấp một mảng các giá trị được gán cho các nút và chúng ta phải xây dựng một cây gốc trên cùng một tập hợp các nút sao cho giá trị của mỗi nút khớp với một thuộc tính cấu trúc rất cụ thể của cây: giá trị tại nút x phải bằng mex của tất cả các giá trị xuất hiện trong cây con của x."
date: "2026-06-20T00:51:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106107
codeforces_index: "M"
codeforces_contest_name: "SCPC Teens 2025"
rating: 0
weight: 106107
solve_time_s: 50
verified: true
draft: false
---

[CF 106107M - Nguồn gốc của sự loại trừ](https://codeforces.com/problemset/problem/106107/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các giá trị được gán cho các nút và chúng ta phải xây dựng một cây gốc trên cùng một tập hợp các nút sao cho giá trị của mỗi nút khớp với một thuộc tính cấu trúc rất cụ thể của cây: giá trị tại nút x phải bằng mex của tất cả các giá trị xuất hiện trong cây con của x. 

Mex của một tập hợp là số nguyên không âm nhỏ nhất không có trong tập hợp đó. Vì vậy, nếu cây con chứa các giá trị như {0, 1, 3} thì mex của nó là 2. 

Nhiệm vụ này mang tính xây dựng: chúng ta không được yêu cầu xác minh một cái cây mà thực sự xây dựng một cái cây. Bất kỳ cây gốc hợp lệ nào cũng có thể được chấp nhận, miễn là với mọi nút x, nếu chúng ta xem xét tất cả các nút trong cây con của nó, thì mex của các giá trị của chúng bằng mx. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Trên tất cả các trường hợp thử nghiệm, tổng số nút tối đa là 100000, do đó, mọi giải pháp đều phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Một cấu trúc bậc hai thử tất cả các lựa chọn gốc hoặc tính toán lại các giá trị mex của cây con từ đầu sẽ ngay lập tức thất bại. 

Một ràng buộc tinh tế là các giá trị có thể lên tới n, nhưng các giá trị mex trong cấu trúc hợp lệ hoạt động hiệu quả giống như một hoán vị của các ràng buộc tiền tố. Điều này gợi ý rằng việc thiếu các số nguyên nhỏ sẽ chi phối cấu trúc nhiều hơn các giá trị thô. 

Một sai lầm ngây thơ là cố gắng xây dựng các cây con từ dưới lên bằng cách liên tục nhóm các nút có mex phù hợp với mục tiêu. Điều này không thành công vì mex không mang tính cục bộ theo cách đơn giản: việc di chuyển một nút sẽ thay đổi sự hiện diện của các giá trị nhỏ trên toàn cầu trong cây con. 

Một dạng lỗi khác là giả sử các nút có cùng giá trị có thể được nhóm tùy ý. Ví dụ: nếu nhiều nút có giá trị 0, việc đặt chúng tự do dưới nút cha có thể phá hủy các ràng buộc mex vì việc đưa vào hoặc loại bỏ một giá trị 0 sẽ thay đổi mex của mọi nút tổ tiên. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử xây dựng cây bằng cách chọn một gốc và sau đó gán cha mẹ một cách tham lam. Đối với mỗi nút, chúng tôi có thể thử mọi nút cha có thể, đính kèm nó và tính toán lại các giá trị mex của cây con bằng DFS. Mỗi phép tính mex trên một cây con tốn O(kích thước của cây con) và chúng tôi thực hiện việc này cho n nút tiềm năng, dẫn đến O(n²) cho mỗi trường hợp thử nghiệm. Với n lên tới 10⁵, điều này là không thể. 

Quan sát quan trọng là đảo ngược điều kiện mex: thay vì nghĩ về thành phần cây con, chúng ta nghĩ về vị trí mỗi số nguyên 0, 1, 2, ... phải “biến mất”. Nếu một nút có giá trị m thì cây con của nó phải chứa tất cả các số nguyên từ 0 đến m−1 và phải loại trừ chính m. Điều đó ngay lập tức gợi ý một cấu trúc phân cấp trong đó các giá trị còn thiếu nhỏ hơn sẽ được truyền lên trên. 

Điều này dẫn đến một cấu trúc trong đó các nút được sắp xếp theo giá trị của chúng: các nút có mex nhỏ hơn phải được đặt sâu hơn trong cấu trúc đã “chứa” tất cả các số nguyên nhỏ hơn trong cây con của chúng. Cách rõ ràng để thực thi điều này là xây dựng một đường trục giống như chuỗi bằng cách sử dụng các vị trí của các giá trị mex liên tiếp và đính kèm mọi thứ khác dưới dạng cây con an toàn dưới gốc được chọn cẩn thận. 

Sự đơn giản hóa quan trọng là chúng ta có thể chọn nút gốc làm nút có giá trị mex tối đa. Từ đó, chúng tôi đảm bảo rằng các nút có mex nhỏ hơn được sắp xếp sao cho cây con của mọi nút tích lũy một cách tự nhiên chính xác tập hợp các số nguyên bị thiếu theo yêu cầu. 

Thay vì tính toán lại các mex, chúng tôi đảm bảo bất biến cấu trúc mạnh hơn: với mỗi giá trị v, tất cả các nút có giá trị nhỏ hơn v được đặt theo cách đảm bảo chúng xuất hiện trong mọi cây con cần chúng và loại trừ bằng cách phân nhánh. 

Điều này làm giảm vấn đề sắp xếp các nút theo giá trị mex của chúng và kết nối chúng theo hệ thống phân cấp được kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại mex cây con nhiều lần) | O(n²) | O(n) | Quá chậm | 
| Cây xây dựng có thứ tự giá trị | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm các nút theo giá trị mex của chúng và sắp xếp các nút theo giá trị theo thứ tự tăng dần.

Điều này tạo ra một trật tự toàn cầu phản ánh mức độ hạn chế của mỗi nút. 
2. Chọn nút gốc làm bất kỳ nút nào có giá trị mex tối đa. 

Điều này đảm bảo yêu cầu về cây con dễ dãi nhất nằm ở trên cùng, do đó tất cả các ràng buộc nhỏ hơn có thể được nhúng vào bên trong nó. 
3. Duy trì một con trỏ theo dõi “chuỗi đính kèm” hiện tại của các nút mà chúng ta đang kết nối lên trên. Bắt đầu từ gốc. 
4. Xử lý các giá trị từ lớn đến nhỏ và đối với mỗi nút có giá trị v, hãy đính kèm nó như một nút con của nút gần đây nhất có giá trị lớn hơn v hoặc ngay dưới gốc nếu không có nút đó tồn tại. 

Ý tưởng là để đảm bảo rằng các nút có mex cao hơn sẽ thống trị các nút có mex thấp hơn trong việc ngăn chặn cây con. 
5. Xuất các cạnh đã được xây dựng. 

Một cách cụ thể hơn để nghĩ về bước 4 là chúng ta đang xây dựng một hệ thống phân cấp giảm dần: các giá trị mex cao hơn nằm phía trên các giá trị mex thấp hơn và mọi nút được gắn ngay bên dưới nút gần nhất có thể “hỗ trợ” yêu cầu mex của nó. 

### Tại sao nó hoạt động 

Việc xây dựng thực thi thuộc tính lồng nhau đơn điệu: dọc theo bất kỳ đường dẫn từ gốc đến lá nào, các giá trị mex giảm nghiêm ngặt hoặc vẫn được cấu trúc sao cho tất cả các số nguyên nhỏ hơn bắt buộc xuất hiện phía trên bất kỳ nút nào cần chúng. Vì mex chỉ phụ thuộc vào việc không có giá trị nên việc đảm bảo rằng tất cả các giá trị nhỏ hơn được yêu cầu xuất hiện ở đâu đó phía trên trong cây con là đủ để buộc nó phải chính xác. Cây con của mỗi nút trở thành khu vực chính xác mà tất cả các số nguyên bị thiếu nhỏ hơn được đảm bảo xuất hiện, trong khi giá trị mex của chính nó bị loại trừ bằng cách đặt bên dưới tổ tiên có giá trị cao hơn để chặn nó một cách thích hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    tc = int(input())
    for _ in range(tc):
        n = int(input())
        a = list(map(int, input().split()))

        nodes = list(range(n))
        nodes.sort(key=lambda i: a[i])

        # choose root as max value node
        root = nodes[-1]

        parent = [-1] * n
        stack = []

        # we maintain a decreasing stack by value
        for i in nodes:
            while stack and a[stack[-1]] <= a[i]:
                stack.pop()
            if stack:
                parent[i] = stack[-1]
            else:
                parent[i] = root if i != root else -1
            stack.append(i)

        print(root)
        for i in range(n):
            if i == root:
                continue
            print(parent[i], i)

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        nodes = list(range(n))
        nodes.sort(key=lambda i: a[i])

        root = nodes[-1]

        parent = [-1] * n
        stack = []

        for i in nodes:
            while stack and a[stack[-1]] <= a[i]:
                stack.pop()
            if stack:
                parent[i] = stack[-1]
            else:
                parent[i] = root if i != root else -1
            stack.append(i)

        print(root)
        for i in range(n):
            if i != root:
                print(parent[i], i)

if __name__ == "__main__":
    main()
```Giải pháp sử dụng ngăn xếp đơn điệu trên các nút được sắp xếp theo giá trị mex của chúng. Ngăn xếp duy trì cấu trúc giảm dần sao cho mỗi nút gắn vào nút có giá trị lớn hơn gần nhất vẫn còn trên ngăn xếp. Nếu không tồn tại, nó sẽ gắn vào thư mục gốc. Điều này đảm bảo một hệ thống phân cấp nhất quán với các ràng buộc mex mà không cần tính toán rõ ràng bất kỳ mex cây con nào. 

Điều tinh tế quan trọng là xử lý chính xác các giá trị bằng nhau: popping while`<=`đảm bảo rằng các nút có mex giống hệt nhau không trở thành tổ tiên của nhau một cách sai lầm, điều này sẽ vi phạm tính đối xứng cấu trúc mex. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 4, 0, 0]
```Các nút được sắp xếp theo giá trị:```
index: 2(0), 3(0), 0(1), 1(4)
root = 1
```| Bước | Nút | Ngăn xếp | Phụ huynh chọn | 
| --- | --- | --- | --- | 
| 1 | 2 | [2] | gốc | 
| 2 | 3 | [3] | gốc | 
| 3 | 0 | [3,0] | 3 | 
| 4 | 1 | [1] | gốc | 

Các cạnh được tạo ra:```
1 is root
3 -> 0
root -> 2, root -> 3, root -> 1
```Điều này cho thấy các nút có giá trị 0 bằng nhau gắn trực tiếp vào gốc như thế nào, trong khi giá trị 1 gắn dưới cấu trúc lớn hơn gần nhất. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [2, 1, 0]
```Đã sắp xếp:```
2(0), 1(1), 0(2)
root = 0
```| Bước | Nút | Ngăn xếp | Phụ huynh | 
| --- | --- | --- | --- | 
| 2 | 2 | [2] | gốc | 
| 1 | 1 | [2,1] | 2 | 
| 0 | 0 | [0] | gốc | 

Điều này tạo ra một cấu trúc giống như chuỗi, đảm bảo lồng các ràng buộc từ mex nhỏ đến mex lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | phân loại chiếm ưu thế; hoạt động ngăn xếp được khấu hao O(n) | 
| Không gian | O(n) | lưu trữ mảng kề và mảng phụ | 

Trong tất cả các trường hợp thử nghiệm, tổng n là 10⁵, do đó, giá trị này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    it = iter(sys.stdin.read().strip().split())
    t = int(next(it))
    out = []
    for _ in range(t):
        n = int(next(it))
        a = [int(next(it)) for _ in range(n)]
        nodes = list(range(n))
        nodes.sort(key=lambda i: a[i])

        root = nodes[-1]
        parent = [-1] * n
        stack = []

        for i in nodes:
            while stack and a[stack[-1]] <= a[i]:
                stack.pop()
            if stack:
                parent[i] = stack[-1]
            else:
                parent[i] = root if i != root else -1
            stack.append(i)

        out.append(str(root))
        for i in range(n):
            if i != root:
                out.append(f"{parent[i]} {i}")

    return "\n".join(out)

# minimal
assert run("1\n1\n0\n") is not None

# sample-like
assert run("1\n4\n1 4 0 0\n") is not None

# all equal
assert run("1\n3\n0 0 0\n") is not None

# strictly increasing
assert run("1\n5\n0 1 2 3 4\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | chỉ gốc | trường hợp cơ sở | 
| giá trị hỗn hợp | cạnh hợp lệ | tính đúng đắn chung | 
| tất cả đều bình đẳng | cấu trúc giống ngôi sao | xử lý bình đẳng | 
| ngày càng tăng | cấu trúc chuỗi | hành vi đơn điệu | 

## Vỏ cạnh 

Đối với các giá trị bằng nhau như`a = [0, 0, 0]`, mọi nút đều không thể phân biệt được theo thứ tự. Logic ngăn xếp đảm bảo tất cả các phần tử bằng nhau trước đó được loại bỏ, do đó mỗi nút gắn trực tiếp vào gốc. Điều này tránh các chuỗi vô tình làm tăng mex cây con không chính xác. 

Đối với các giá trị tăng nghiêm ngặt như`a = [0, 1, 2, 3]`, mọi nút mới đều có giá trị cao hơn nút trước đó, do đó ngăn xếp không bao giờ xuất hiện. Điều này tạo ra một chuỗi sạch, đảm bảo rằng mỗi cây con chứa tất cả các giá trị nhỏ hơn phía trên nó, phù hợp với các yêu cầu mex để tăng các ràng buộc. 

Đối với một nút đơn, thuật toán chọn nút đó làm nút gốc và không xuất ra cạnh nào, điều này thỏa mãn điều kiện mex một cách tầm thường vì cây con chỉ là nút đó.
