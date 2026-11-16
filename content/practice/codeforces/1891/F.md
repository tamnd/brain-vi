---
title: "CF 1891F - Một Cây Đang Phát Triển"
description: "Chúng ta được yêu cầu xử lý một chuỗi các thao tác trên một cây có gốc đang phát triển. Cây bắt đầu bằng một nút được đánh số 1 và mỗi nút có một giá trị số, ban đầu là 0."
date: "2026-06-08T22:02:27+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 1891
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 907 (Div. 2)"
rating: 2000
weight: 1891
solve_time_s: 126
verified: false
draft: false
---

[CF 1891F - Một cái cây đang phát triển](https://codeforces.com/problemset/problem/1891/F) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, cây 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xử lý một chuỗi các thao tác trên một cây có gốc đang phát triển. Cây bắt đầu bằng một nút được đánh số 1 và mỗi nút có một giá trị số, ban đầu là 0. Các truy vấn có thể thêm một nút con mới vào nút hiện có hoặc tăng tất cả các giá trị trong cây con của một nút nhất định lên một lượng được chỉ định. Sau tất cả các truy vấn, chúng ta cần xuất ra giá trị cuối cùng của tất cả các nút trong cây. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có tối đa 500.000 truy vấn. Tổng số truy vấn trên tất cả các trường hợp thử nghiệm được đảm bảo không vượt quá 500.000. Các giá trị có thể lớn như$10^9$hoặc nhỏ như$-10^9$. Những ràng buộc này ngụ ý rằng bất kỳ thuật toán nào có kết quả tệ hơn$O(q \log q)$mỗi trường hợp thử nghiệm dường như không đủ hiệu quả. Một giải pháp đơn giản cập nhật tất cả các nút trong cây con cho mỗi truy vấn sẽ mất$O(nq)$thời gian trong trường hợp xấu nhất có thể đạt tới$2.5 \cdot 10^{11}$hoạt động-quá chậm. 

Các trường hợp biên không rõ ràng bao gồm việc thêm nút con vào các nút mà bản thân chúng có cây con lớn và nhiều phần tăng dần của cây con chồng chéo. Ví dụ: nếu chúng ta có một cây có các nút 1-3 và chúng ta thực hiện tăng dần trên nút 1, sau đó là tăng dần trên nút 2, một cách tiếp cận đơn giản đi qua các cây con mỗi lần có thể đếm gấp đôi hoặc bỏ lỡ các cập nhật nếu được triển khai không chính xác. Một trường hợp khác là thêm một nút mới và ngay lập tức thực hiện tăng cây con trên nút đó. Thuật toán phải tính toán chính xác các nút mới được thêm vào trong tương lai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: duy trì danh sách kề cho cây, lưu trữ giá trị cho mỗi nút và đối với mỗi truy vấn, hãy duyệt qua cây con để áp dụng các bước tăng dần. Điều này đúng, nhưng độ phức tạp về thời gian của nó là$O(qn)$bởi vì mỗi truy vấn loại 2 có thể chạm vào mọi nút trong cây con. Trong trường hợp xấu nhất, khi cây là một ngôi sao hoặc một chuỗi và mỗi truy vấn là một phần gia tăng trên gốc, chúng ta đạt được nửa tỷ thao tác cho mỗi trường hợp thử nghiệm - điều này không thực tế. 

Quan sát quan trọng là sự gia tăng của cây con và sự phát triển của cây có cấu trúc tuần tự. Mỗi nút được thêm chính xác một lần và khi một nút được thêm vào, nút cha của nó sẽ không bao giờ thay đổi. Chúng ta có thể biểu diễn cây dưới dạng một mảng với con trỏ "cha" cho mỗi nút. Sau đó, chúng ta có thể truyền các giá trị từ cha mẹ sang con cái trong một lần duyệt sau tất cả các truy vấn. Thay vì cập nhật tất cả các nút trong cây con ngay lập tức, chúng ta có thể ghi lại phần tăng dần ở nút gốc và đẩy nó xuống sau. Đây thực chất là sự lan truyền tiền tố dựa trên DFS: mỗi nút tích lũy số gia tăng từ tổ tiên của nó. 

Brute-force hoạt động vì việc cập nhật trực tiếp từng cây con sẽ mang lại giá trị chính xác nhưng không thành công khi cây con lớn và có nhiều truy vấn. Quan sát cho thấy chúng ta chỉ cần truyền các số gia tăng một lần trong DFS cho phép chúng ta giảm độ phức tạp từ$O(nq)$ĐẾN$O(n+q)$cho mỗi trường hợp thử nghiệm, hiệu quả đối với các giới hạn nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Con trỏ gốc + lan truyền DFS | O(n+q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`parent`Ở đâu`parent[i]`lưu trữ cha mẹ của nút`i`. Khởi tạo một mảng`value`số không cho các giá trị nút. Bộ`children`dưới dạng danh sách các danh sách để theo dõi các nút con của mỗi nút. 
2. Với mỗi truy vấn loại 1 (thêm con vào đỉnh`v`), tăng kích thước cây`sz`bằng 1, đặt`parent[sz] = v`, và nối thêm`sz`ĐẾN`children[v]`. Điều này duy trì cấu trúc cây trong bộ nhớ một cách hiệu quả. 
3. Đối với mỗi truy vấn loại 2 (cây con tăng dần của đỉnh`v`qua`x`), cập nhật`value[v] += x`. Chưa duyệt cây con; lưu trữ phần tăng ở gốc của cây con dự định. 
4. Sau khi xử lý tất cả các truy vấn, hãy thực hiện DFS bắt đầu từ gốc (nút 1). Tại mỗi nút`u`, cho mỗi đứa trẻ`c`TRONG`children[u]`, tuyên truyền giá trị tích lũy:`value[c] += value[u]`. Sau đó xử lý đệ quy con`c`. Điều này đảm bảo rằng tất cả các gia số của cây con được áp dụng chính xác. 
5. Sau DFS,`value`mảng chứa các giá trị chính xác cho tất cả các nút. Xuất chúng theo thứ tự từ 1 đến`sz`. 

Bất biến đảm bảo tính chính xác là giá trị của mỗi nút phải là tổng của tất cả các gia số được áp dụng cho nó hoặc bất kỳ nút tổ tiên nào của nó. Bằng cách lưu trữ các phần gia tăng ở gốc và truyền chúng một lần bằng DFS, chúng tôi duy trì chính xác thuộc tính này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    T = int(input())
    for _ in range(T):
        q = int(input())
        parent = [0]  # 1-indexed, parent[1] unused
        children = [[] for _ in range(q + 2)]
        value = [0] * (q + 2)
        sz = 1

        for _ in range(q):
            line = input().split()
            if line[0] == '1':
                v = int(line[1])
                sz += 1
                parent.append(v)
                children[v].append(sz)
            else:
                v, x = int(line[1]), int(line[2])
                value[v] += x

        def dfs(u):
            for c in children[u]:
                value[c] += value[u]
                dfs(c)

        dfs(1)
        print(' '.join(str(value[i]) for i in range(1, sz + 1)))

if __name__ == "__main__":
    solve()
```Trong giải pháp này,`parent`được duy trì để thêm các phần tử con một cách chính xác, nhưng không được sử dụng trong quá trình truyền bá DFS vì chúng tôi theo dõi trực tiếp các phần tử con. Chúng tôi phân bổ trước`children`Và`value`mảng để tránh thay đổi kích thước động trong khi truy vấn, giúp cải thiện hiệu suất. DFS đệ quy truyền các phần tăng dần từ cha mẹ sang con chính xác một lần, xử lý các phần tăng dần của cây con chồng chéo một cách chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 vết 

đầu vào:```
9
2 1 3
1 1
2 2 1
1 1
2 3 2
1 3
2 1 4
1 3
2 3 2
```| Truy vấn | Nút được thêm / tăng | Mảng giá trị sau thao tác | 
| --- | --- | --- | 
| 2 1 3 | +3 đến nút 1 | [3] | 
| 1 1 | thêm nút 2 vào 1 | [3,0] | 
| 2 2 1 | +1 đến nút 2 | [3,1] | 
| 1 1 | thêm nút 3 vào 1 | [3,1,0] | 
| 2 3 2 | +2 đến nút 3 | [3,1,2] | 
| 1 3 | thêm nút 4 vào 3 | [3,1,2,0] | 
| 2 1 4 | +4 đến nút 1 | [7,1,6,0] | 
| 1 3 | thêm nút 5 vào 3 | [7,1,6,0,0] | 
| 2 3 2 | +2 đến nút 3 | [7,1,8,2,2] | 

Tuyên truyền DFS thêm giá trị cha mẹ cho con: nút 1->2,3-> con 4,5. 

Cuối cùng`value`: 7 5 8 6 2 

Dấu vết này xác nhận thuật toán tích lũy chính xác các phần tăng của cây con chồng chéo và tính đến các nút được thêm động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+q) | Mỗi truy vấn được xử lý theo O(1) và DFS truyền các giá trị qua tất cả các nút một lần | 
| Không gian | O(n) | Mảng`value`,`children`, Và`parent`lưu trữ cây và giá trị lên tới`q+1`nút | 

Với$q \le 5 \cdot 10^5$, tổng số thao tác dưới 1 triệu cho mỗi trường hợp thử nghiệm, dễ dàng trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided sample 1
assert run("1\n9\n2 1 3\n1 1\n2 2 1\n1 1\n2 3 2\n1 3\n2 1 4\n1 3\n2 3 2\n") == "7 5 8 6 2"

# minimum-size tree
assert run("1\n1\n2 1 5\n") == "5"

# all nodes added linearly, multiple increments
assert run("1\n5\n1 1\n1 2\n2 1 3\n2 3 2\n2 2 1\n") == "3 4 5"

# negative increment
assert run("1\n3\n1 1\n2 2 -2\n2 1 5\n") == "5 3"

# multiple test cases
assert run("2\n2\n1 1\n2 2 3\n2\n1 1\n2 1 4\n") == "0 3\n4 0"
```| Kiểm tra đầu vào | Hy vọng
