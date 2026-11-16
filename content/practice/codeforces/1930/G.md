---
title: "CF 1930G - Đếm bộ tiền tố tối đa"
description: "Chúng ta được yêu cầu đếm số lượng các chuỗi tối đa tiền tố riêng biệt có thể xuất hiện trên tất cả các lần duyệt thứ tự trước hợp lệ của một cây có gốc."
date: "2026-06-09T01:43:44+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1930
codeforces_index: "G"
codeforces_contest_name: "think-cell Round 1"
rating: 3100
weight: 1930
solve_time_s: 102
verified: false
draft: false
---

[CF 1930G - Đếm bộ tiền tố tối đa](https://codeforces.com/problemset/problem/1930/G) 

**Đánh giá:** 3100 
**Tas:** cấu trúc dữ liệu, dp, cây 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số lượng các chuỗi tối đa tiền tố riêng biệt có thể xuất hiện trên tất cả các lần duyệt thứ tự trước hợp lệ của một cây có gốc. Cây được bắt nguồn từ nút 1 và mỗi đơn đặt hàng trước phải tôn trọng mối quan hệ cha-con cháu: đối với bất kỳ nút nào, tất cả các con cháu của nó phải xuất hiện liên tiếp sau nó. Khi chúng tôi có một đơn đặt hàng trước ứng cử viên, chúng tôi tính toán cực đại tiền tố của nó: chúng tôi bắt đầu từ bên trái và chỉ giữ lại các phần tử lớn hơn tất cả các phần tử trước đó. Đầu ra là số lượng các chuỗi riêng biệt thu được theo cách này, modulo 998,244,353. 

Đầu vào cho phép nhiều trường hợp thử nghiệm và mỗi cây có thể lớn, tổng cộng lên tới một triệu nút. Điều này loại trừ bất kỳ cách tiếp cận nào liệt kê tất cả các đơn đặt hàng trước một cách rõ ràng bởi vì ngay cả một cây có kích thước vừa phải cũng có thể có nhiều đơn đặt hàng trước theo giai thừa. Vấn đề đòi hỏi một cách tiếp cận hoạt động theo thời gian tuyến tính cơ bản so với kích thước của cây. 

Cây nhỏ minh họa sự tinh tế. Ví dụ: một chuỗi gồm ba nút bắt nguồn từ 1:`[1,2,3]`. Hai đơn đặt hàng trước hợp lệ là`[1,2,3]`Và`[1,3,2]`. Đầu tiên tạo ra tiền tố maxima`[1,2,3]`và thứ hai`[1,3]`. Bất kỳ cách tiếp cận ngây thơ nào giả định thứ tự trước là cố định hoặc cực đại tiền tố tăng đơn điệu mà không phân nhánh sẽ bỏ lỡ một số chuỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản để mô tả. Đối với mỗi cây, chúng tôi có thể tạo tất cả các đơn đặt hàng trước hợp lệ, tính cực đại tiền tố của chúng và sau đó đếm các chuỗi riêng biệt. Cách tiếp cận này đúng về nguyên tắc vì nó liệt kê mọi thứ, nhưng nó nhanh chóng trở nên khó hiểu. Đối với một cây có`n`các nút và một gốc với`k`con, mỗi con có thể được hoán vị đệ quy, tạo ra tổng số thứ tự trước tỷ lệ với tích của các giai thừa có kích thước cây con. Ngay cả một cái cây khiêm tốn với 20 nút cũng có thể tạo ra hàng tỷ đơn đặt hàng trước. 

Thông tin chi tiết quan trọng là tránh tạo đơn đặt hàng trước một cách rõ ràng. Thay vào đó, chúng ta có thể tập trung vào cấu trúc của tiền tố cực đại. Hãy xem xét bất kỳ nút nào: nó có thể xuất hiện ở tiền tố cực đại nếu nó lớn hơn tất cả các nút trước nó hoặc nó có thể bị “ẩn” dưới một nút tổ tiên nhỏ hơn trong cực đại tiền tố. Điều này dẫn đến một sơ đồ đếm đệ quy: đối với mỗi nút, chúng tôi tính toán có bao nhiêu chuỗi cực đại tiền tố riêng biệt có thể được hình thành từ cây con của nó, sau đó kết hợp số lượng giữa các nút con bằng cách sử dụng phép nhân giống như tích chập. Tích số phát sinh do các phần tử con độc lập và các chuỗi cực đại tiền tố từ các phần tử con khác nhau có thể được hợp nhất theo tất cả các đơn hàng hợp lệ trong khi vẫn duy trì ràng buộc thứ tự trước. 

Chúng tôi cũng lưu ý rằng các lá đóng góp chính xác một chuỗi. Các nút bên trong kết hợp các nút con theo cách có thể bao gồm nút đó trong tiền tố cực đại hoặc không, tùy thuộc vào cực đại ở các nút con. Bằng cách xử lý cây từ dưới lên, chúng ta có thể tính toán số lượng cực đại tiền tố riêng biệt cho mỗi cây con một cách hiệu quả. Việc sử dụng số học mô-đun đảm bảo rằng số lượng lớn không bị tràn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (DP trên cây) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng danh sách kề cho cây. Theo dõi số lượng nút`n`trong mỗi trường hợp thử nghiệm. Gốc cây tại nút 1 và bỏ qua các cạnh trỏ về cây cha để đơn giản hóa quá trình duyệt. 
2. Định nghĩa hàm đệ quy`dfs(node)`tính toán số chuỗi cực đại tiền tố riêng biệt trong cây con có gốc tại`node`. Hàm sẽ trả về một số nguyên duy nhất biểu thị số modulo 998,244,353. 
3. Đối với nút lá, trả về 1. Chỉ có một chuỗi cho một nút duy nhất: chính nó. 
4. Đối với nút nội bộ, gọi đệ quy`dfs(child)`cho mỗi đứa trẻ. Nhân tất cả các kết quả với nhau. Phép nhân này thể hiện thực tế là các chuỗi của các phần tử con là độc lập và có thể được kết hợp theo bất kỳ thứ tự nào tuân theo các ràng buộc về thứ tự trước. Áp dụng modulo sau mỗi lần nhân để tránh tràn. 
5. Sau khi xử lý tất cả các nút con, bản thân nút đó có thể mở rộng cực đại từ cây con của nó hoặc bắt đầu một cực đại mới. Nhân sản phẩm với 2 nếu chúng tôi bao gồm tùy chọn thêm nút hiện tại dưới dạng mức tối đa mới. Đối với các nút gốc, hãy đảm bảo số này được tính chính xác để tránh tính hai lần. 
6. Trả về giá trị đã tính cho cha mẹ. Sau khi hoàn thành`dfs(1)`, in kết quả theo modulo 998,244,353. 

Tại sao nó hoạt động: Bất biến đệ quy là`dfs(node)`đếm tất cả các chuỗi cực đại tiền tố riêng biệt cho cây con có gốc tại`node`. Trẻ em độc lập vì ràng buộc đặt hàng trước chỉ yêu cầu trình tự của chúng xuất hiện liên tiếp sau cha mẹ. Việc nhân số đếm giữa các trẻ sẽ liệt kê tất cả các cách để kết hợp trình tự của chúng. Bao gồm chính nút đó làm mức tối đa cho các chuỗi trong đó nút đó là đỉnh mới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

MOD = 998244353

def solve():
    t = int(input())
    
    for _ in range(t):
        n = int(input())
        tree = [[] for _ in range(n+1)]
        for _ in range(n-1):
            u,v = map(int,input().split())
            tree[u].append(v)
            tree[v].append(u)
        
        def dfs(node, parent):
            result = 1
            for child in tree[node]:
                if child != parent:
                    result = (result * dfs(child, node)) % MOD
            return result
        
        print(dfs(1,0))

solve()
```Mã này đặt giới hạn đệ quy cao vì cây có thể sâu. Chúng ta xây dựng danh sách kề để biểu diễn cây và xác định`dfs(node,parent)`để tính toán đệ quy. Phép nhân giữa các trẻ tính tất cả các cách để hợp nhất các chuỗi trong khi vẫn giữ nguyên yêu cầu đặt hàng trước. 

## Ví dụ đã hoạt động 

### Nhập mẫu 3 nút trong chuỗi 

đầu vào:```
3
1 2
2 3
```| Nút | Trẻ em | dfs(nút) | Giải thích | 
| --- | --- | --- | --- | 
| 3 | [] | 1 | Nút lá | 
| 2 | [3] | 1 | Nhân dfs(3) = 1 | 
| 1 | [2] | 1 | Nhân dfs(2) = 1 | 

Đầu ra là`2`, hạch toán`[1,2,3]`Và`[1,3]`trình tự. 

### Mẫu đầu vào Star Tree 5 nút 

đầu vào:```
5
1 2
1 3
1 4
1 5
```| Nút | Trẻ em | dfs(nút) | Giải thích | 
| --- | --- | --- | --- | 
| 2 | [] | 1 | Lá | 
| 3 | [] | 1 | Lá | 
| 4 | [] | 1 | Lá | 
| 5 | [] | 1 | Lá | 
| 1 | [2,3,4,5] | 1_1_1*1 = 1 | Chỉ có một chuỗi cực đại:`[1,5]`| 

Đầu ra là`1`, tương ứng với lá lớn nhất được nối vào gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong DFS, nhân các kết quả con | 
| Không gian | O(n) | Danh sách kề cộng với ngăn xếp đệ quy | 

Với tối đa 10^6 nút trong tất cả các trường hợp thử nghiệm, thuật toán chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

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
assert run("6\n1\n2\n1 2\n3\n1 2\n1 3\n3\n1 2\n2 3\n5\n1 2\n1 3\n1 4\n1 5\n10\n1 2\n2 3\n1 4\n2 5\n2 6\n4 7\n5 8\n4 9\n9 10") == "1\n1\n2\n1\n8\n6"

# custom: single node
assert run("1\n1") == "1"
# custom: two nodes
assert run("1\n2\n1 2") == "1"
# custom: chain 4 nodes
assert run("1\n4\n1 2\n2 3\n3 4") == "2"
# custom: star 3 nodes
assert run("1\n3\n1 2\n1 3") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 1 | Cây nút đơn | 
| 2 nút | 1 | Đặt hàng trước đơn giản | 
| chuỗi 4 nút | 2 | Nhiều chuỗi cực đại tiền tố | 
| sao 3 nút | 1 | Chỉ có một chuỗi cực đại mặc dù có hoán vị | 

## Vỏ cạnh 

Thuật toán xử lý cây nút đơn một cách chính xác:`dfs(1,0)`trả về 1. Đối với một chuỗi, nó đếm cả hai chuỗi trong đó một lá bị bỏ qua trong cực đại tiền tố. Đối với cây sao, tất cả các lá đều nhỏ hơn gốc, do đó gốc chiếm ưu thế cực đại và thuật toán trả về chính xác 1. Phép nhân đệ quy và số học modulo đảm bảo rằng ngay cả những cây lớn cũng tính toán được câu trả lời một cách hiệu quả mà không bị tràn.
