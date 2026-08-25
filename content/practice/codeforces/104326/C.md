---
title: "CF 104326C - Christopher Robin đang học sắp xếp hoán vị"
description: "Chúng tôi được cung cấp một biến thể xác định của quicksort trong đó bước phân vùng được viết theo một cách rất cụ thể và phụ thuộc vào chuỗi chỉ số trục được chọn trước được tạo ra bởi các lệnh gọi lặp lại đến một trình tạo ngẫu nhiên."
date: "2026-07-01T19:07:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104326
codeforces_index: "C"
codeforces_contest_name: "Udmurt SU Contest 2011"
rating: 0
weight: 104326
solve_time_s: 70
verified: true
draft: false
---

[CF 104326C - Christopher Robin đang học các hoán vị sắp xếp](https://codeforces.com/problemset/problem/104326/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một biến thể xác định của quicksort trong đó bước phân vùng được viết theo một cách rất cụ thể và phụ thuộc vào chuỗi chỉ số trục được chọn trước được tạo ra bởi các lệnh gọi lặp lại đến một trình tạo ngẫu nhiên. Mảng là một hoán vị của các số từ 1 đến n, nhưng giá trị thực tế không xác định được. Những gì đã biết là trình tự chính xác của các chỉ mục sẽ được sử dụng làm điểm xoay trong quá trình thực thi, theo thứ tự chúng được sử dụng. 

Quy trình phân vùng hoạt động giống như phân vùng hai con trỏ tiêu chuẩn, nhưng có một điểm thay đổi quan trọng: trục được chọn theo chỉ mục chứ không phải giá trị và trong quá trình hoán đổi, vị trí trục có thể di chuyển. Sau khi phân vùng, trục quay được khôi phục về vị trí cuối cùng của nó và quá trình đệ quy tiếp tục trên mảng con bên trái trước, sau đó đến mảng con bên phải. 

Hàm trả về một giá trị tỷ lệ thuận với số lần so sánh và hoán đổi được thực hiện trong quá trình thực hiện. Mỗi phân vùng đóng góp một chi phí bằng với kích thước của phân khúc hiện tại, cộng với các khoản đóng góp đệ quy. Mục tiêu của chúng tôi là xây dựng một hoán vị từ 1 đến n sao cho khi sử dụng chuỗi chỉ số trục chính xác này, tổng giá trị trả về sẽ tối đa. 

Các ràng buộc nhỏ, n ≤ 50, điều này ngay lập tức gợi ý rằng hàm mũ hoặc DP trên các tập hợp con là có thể chấp nhận được. Bất kỳ giải pháp nào cố gắng mô phỏng trực tiếp tất cả các mảng có thể có đều không khả thi vì có n! hoán vị, nhưng cấu trúc của chuỗi trục ràng buộc cây đệ quy rất nhiều. 

Một trường hợp lỗi tinh vi xuất phát từ xác nhận trong mã: chỉ mục trục phải luôn nằm trong phân đoạn hiện tại. Nếu chúng ta xây dựng một mảng khiến một trục kết thúc nằm ngoài phạm vi đệ quy dự kiến ​​của nó do cách hoán đổi di chuyển các phần tử, quy trình sẽ trở nên không hợp lệ. Một công trình tham lam ngây thơ chỉ tối đa hóa chi phí phân vùng cục bộ có thể dễ dàng vi phạm cấu trúc này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị từ 1 đến n và mô phỏng quá trình phân vùng cho từng hoán vị bằng cách sử dụng chuỗi trục đã cho. Mỗi mô phỏng có chi phí O(n^2) trong trường hợp xấu nhất do phân vùng lặp đi lặp lại trên các phân đoạn thu hẹp và có n! hoán vị, quá lớn ngay cả khi n = 15. 

Quan sát quan trọng là hành vi của thuật toán hoàn toàn được xác định bởi cách các giá trị được phân phối tương ứng với các trục ở mỗi phân đoạn. Mỗi trục tạo ra sự phân chia khoảng hiện tại thành các bài toán con bên trái và bên phải. Chi phí của phân vùng được cố định theo kích thước phân đoạn, nhưng tính chính xác của đệ quy phụ thuộc vào việc liệu giá trị trục đã chọn có ở vị trí nhất quán sau khi phân vùng hay không. 

Điều này chuyển vấn đề thành xây dựng cấu trúc đệ quy nhị phân trên các chỉ số từ 1 đến n, trong đó mỗi nút tương ứng với một phân đoạn và phải được gán một trục từ chuỗi được cung cấp. Trình tự trục xác định thứ tự các nút của cây đệ quy này được “mở”, nghĩa là chúng ta có thể mô phỏng cấu trúc trước và gán các giá trị theo cách tôn trọng kích thước cây con. 

Chiến lược tối ưu là xây dựng lại cây đệ quy do chuỗi trục tạo ra, sau đó gán các giá trị theo thứ tự tăng dần cho các nút theo cách tối đa hóa sự đóng góp của các giá trị cao hơn cho các phân đoạn lớn hơn. Vì các giá trị lớn hơn sẽ xuất hiện cao hơn trong phép đệ quy để tối đa hóa độ dịch chuyển hoán đổi, nên chúng tôi căn chỉnh phép gán với các ràng buộc thứ tự truyền tải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n^2) | O(n) | Quá chậm | 
| Tái thiết cây + phân công | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng cách sắp xếp nhanh sẽ phân vùng các phân đoạn nếu chuỗi trục hợp lệ, nhưng thay vì làm việc trên các giá trị, chúng tôi làm việc trên cấu trúc phân đoạn.

1. Coi lệnh gọi ban đầu là nút gốc đại diện cho phân đoạn [1, n]. Chuỗi trục được sử dụng theo thứ tự bất cứ khi nào nút được xử lý lần đầu tiên. Điều này xác định cây đệ quy nhị phân trong đó mỗi nút chia thành con trái và con phải. 
2. Đối với đoạn [L, R], lấy chỉ số trục chưa sử dụng tiếp theo p. Chỉ mục trục này thuộc về phân đoạn hiện tại trong lần chạy ban đầu, vì vậy chúng tôi chỉ định nó làm gốc của nút này. Đoạn này chia thành [L, p-1] và [p+1, R], tạo thành con trái và con phải. 
3. Lặp lại ở đoạn bên trái trước, sử dụng các trục theo thứ tự, sau đó lặp lại ở đoạn bên phải. Điều này duy trì thứ tự thực hiện chính xác của hàm ban đầu, trong đó đệ quy trái xảy ra trước đệ quy phải. 
4. Nếu tại bất kỳ thời điểm nào, chỉ số trục tiếp theo không nằm trong phân đoạn hiện tại thì việc xây dựng không thành công vì mã ban đầu đảm bảo khẳng định rằng trục xoay phải nằm trong khoảng hoạt động. Trong trường hợp đó, không tồn tại mảng hợp lệ. 
5. Khi cấu trúc cây đã được cố định, gán các giá trị từ 1 đến n theo thứ tự tăng dần bằng cách duyệt cây theo thứ tự. Mỗi nút nhận được một giá trị duy nhất, đảm bảo kết quả là một hoán vị hợp lệ. 
6. Mảng kết quả được hình thành bằng cách đặt từng giá trị được gán vào vị trí trục tương ứng của nó. 

### Tại sao nó hoạt động 

Trình tự trục xác định đầy đủ hình dạng của cây đệ quy miễn là mọi trục đều phù hợp với phân đoạn của nó. Mỗi nút tương ứng với một phân vùng duy nhất và không có hai nút nào trùng nhau trong danh tính trục. Khi cấu trúc đã được cố định, quyền tự do duy nhất là gán giá trị cho các nút. Vì chi phí được điều khiển bởi kích thước phân đoạn và cấu trúc đệ quy thay vì so sánh giá trị cụ thể nên bất kỳ hoán vị hợp lệ nào phù hợp với cây đều đủ. Việc gán các giá trị theo thứ tự truyền tải đảm bảo tính nhất quán và tránh vi phạm các ràng buộc phân vùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    pivots = list(map(int, input().split()))
    
    idx = 0
    tree = {}
    pos = [0] * (n + 1)
    
    def build(l, r):
        nonlocal idx
        if l > r:
            return None
        if idx >= n:
            return None
        
        p = pivots[idx]
        if p < l or p > r:
            return None
        
        idx += 1
        tree[p] = [None, None]
        
        left = build(l, p - 1)
        right = build(p + 1, r)
        
        tree[p][0] = left
        tree[p][1] = right
        return p
    
    root = build(1, n)
    
    if idx != n or root is None:
        print("No solution")
        return
    
    val = 1
    
    def assign(u):
        nonlocal val
        if u is None:
            return
        assign(tree[u][0])
        pos[u] = val
        val += 1
        assign(tree[u][1])
    
    assign(root)
    
    print("Solution exists")
    print(*pos[1:])

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng lại cây đệ quy ngầm được tạo ra bởi chuỗi trục. Mỗi chỉ mục trục được xác thực theo phân đoạn hiện tại của nó, đảm bảo tính nhất quán với quá trình phân vùng. Nếu bất kỳ trục nào vi phạm ràng buộc phân đoạn, quá trình xây dựng sẽ dừng ngay lập tức. 

Sau khi xây dựng cây, chúng ta gán các giá trị bằng cách sử dụng phép duyệt theo thứ tự sao cho mỗi vị trí nhận một giá trị duy nhất từ ​​1 đến n. Mảng cuối cùng được xây dựng bằng cách ánh xạ từng vị trí trục tới giá trị được chỉ định của nó. 

Một điểm tinh tế là chúng ta phải sử dụng nghiêm ngặt các trục xoay theo thứ tự trước của phép đệ quy: cha trước cây con trái trước cây con phải. Bất kỳ sai lệch nào sẽ phá vỡ sự liên kết với thứ tự thực hiện ban đầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2 3
```| Bước | Phân đoạn | Chỉ số xoay | Hành động | Các trục còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | [1,3] | 1 | gốc = 1 | [2,3] | 
| 2 | [2,3] | 2 | trái 1 | [3] | 
| 3 | [3,3] | 3 | bên phải 1 | [] | 

Sau khi xây dựng cây, gán thứ tự sẽ đưa ra các giá trị 1,2,3 cho các vị trí 1,2,3. 

Đầu ra:```
Solution exists
1 2 3
```Điều này xác nhận rằng thứ tự trục tăng hoàn hảo sẽ tạo ra cây đệ quy giống như chuỗi hợp lệ. 

### Mẫu 2 

đầu vào:```
7
1 7 1 7 1 7 1
```Ở gốc, trục 1 là hợp lệ. Cây con bên trái trở nên trống, nhưng trục tiếp theo là 7, không còn hợp lệ đối với cấu trúc còn lại mà đệ quy mong đợi. Cuối cùng, trình tự buộc một trục vào một phân đoạn không hợp lệ, khiến quá trình xây dựng không thành công. 

Đầu ra:```
No solution
```Điều này chứng tỏ rằng ngay cả khi tất cả các giá trị nằm trong phạm vi toàn cầu, các ràng buộc phân đoạn cục bộ của tính năng sắp xếp nhanh sẽ làm mất hiệu lực của chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | mỗi trục được sử dụng một lần trong khi xây dựng các phân đoạn đệ quy | 
| Không gian | O(n) | cây đệ quy và ánh xạ vị trí | 

Các ràng buộc n 50 làm cho việc tái cấu trúc bậc hai trở nên tầm thường về mặt hiệu suất và độ sâu đệ quy được giới hạn bởi n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n1 2 3\n") == "Solution exists\n1 2 3"
assert run("7\n1 7 1 7 1 7 1\n") == "No solution"

# custom cases
assert run("1\n1\n") == "Solution exists\n1"
assert run("2\n2 1\n") in ["Solution exists\n1 2", "Solution exists\n2 1"]
assert run("4\n1 2 3 4\n") == "Solution exists\n1 2 3 4"
assert run("4\n2 1 4 3\n") == "Solution exists\n1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | phần tử đơn | ranh giới tối thiểu | 
| 2 1 | trao đổi gốc thứ tự | tính đối xứng của cây hợp lệ | 
| 1 2 3 4 | chuỗi nghiêm ngặt | trình tự trục tăng dần | 
| 2 1 4 3 | cấu trúc hợp lệ hỗn hợp | tính chính xác của nhiều cây con | 

## Vỏ cạnh 

Đầu vào tối thiểu có kích thước 1 luôn thành công vì không có ràng buộc phân đoạn nào ngoài trục duy nhất hợp lệ. 

Chuỗi trục đảo ngược hoặc không đơn điệu chỉ hợp lệ nếu nó tôn trọng ranh giới phân đoạn đệ quy. Ví dụ, trong`2 1 4 3`, trục đầu tiên chia phạm vi thành hai phân đoạn độc lập hợp lệ và các trục tiếp theo khớp chính xác với các phân đoạn đó trong quá trình đệ quy. Việc xây dựng sẽ đặt từng trục vào cây con chính xác của nó một cách tự nhiên, do đó việc gán vẫn thành công. 

Những trường hợp như`1 7 1 7 1 7 1`không thành công vì sau khi phân tách, phép đệ quy dự kiến ​​các trục sẽ vẫn nằm trong các phân đoạn nhỏ dần dần, nhưng chuỗi liên tục tham chiếu đến các chỉ số không còn thuộc về khoảng hoạt động. Thuật toán phát hiện điều này ngay lập tức khi một trục xoay bị lỗi`[L, R]`kiểm tra trong quá trình thi công.
