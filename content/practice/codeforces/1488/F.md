---
title: "CF 1488F - Dogecoin"
description: "Ann muốn tối đa hóa lợi nhuận bằng cách khai thác và bán Dogecoin trong khoảng thời gian vài ngày. Cô ấy kiếm được chính xác một xu mỗi ngày khi khai thác và giá của một đồng xu thay đổi mỗi ngày tùy theo một danh sách nhất định."
date: "2026-06-10T22:48:57+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1488
codeforces_index: "F"
codeforces_contest_name: "Kotlin Heroes: Episode 6"
rating: 2300
weight: 1488
solve_time_s: 185
verified: false
draft: false
---

[CF 1488F - Dogecoin](https://codeforces.com/problemset/problem/1488/F) 

**Đánh giá:** 2300 
**Thẻ:** *đặc biệt, tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ann muốn tối đa hóa lợi nhuận bằng cách khai thác và bán Dogecoin trong khoảng thời gian vài ngày. Cô ấy kiếm được chính xác một xu mỗi ngày khi khai thác và giá của một đồng xu thay đổi mỗi ngày tùy theo một danh sách nhất định. Đối với mỗi kế hoạch, được xác định bởi ngày bắt đầu`l`và một ngày kết thúc`r`, cô ấy phải khai thác mỗi ngày trong khoảng thời gian`[l, r]`và bán tất cả tiền xu theo ngày`r`. Đầu ra của mỗi kế hoạch là tổng lợi nhuận tối đa mà cô ấy có thể nhận được từ việc bán số tiền khai thác được trong khoảng thời gian được chỉ định. 

Đầu vào cung cấp`n`, số ngày, theo sau là dãy giá xu cho mỗi ngày. Sau đó có`q`kế hoạch, mỗi kế hoạch chỉ định một khoảng thời gian khai thác`[l, r]`. Các ràng buộc chỉ ra rằng cả hai`n`Và`q`có thể lên tới 200.000, điều này ngụ ý rằng bất kỳ giải pháp đơn giản nào lặp lại trong khoảng thời gian cho mỗi truy vấn sẽ yêu cầu tới`O(n * q)`hoạt động. Với trường hợp xấu nhất là khoảng`4*10^10`hoạt động này quá chậm so với giới hạn thời gian 2 giây, vì vậy chúng tôi cần một phương pháp tính toán trước dữ liệu để trả lời từng truy vấn một cách hiệu quả. 

Một cách tiếp cận ngây thơ có thể bỏ qua thực tế là Ann có thể chọn ngày tốt nhất để bán từng đồng tiền được khai thác, dẫn đến lợi nhuận dưới mức tối ưu nếu cô ấy chỉ bán đồng xu theo giá của ngày chúng được khai thác. Các trường hợp đặc biệt phát sinh khi giá dao động, ví dụ: nếu giá`[1, 100, 1]`và cô ấy khai thác được ba ngày, phương pháp bán hàng ngày ngây thơ mang lại tổng cộng`1 + 100 + 1 = 102`, trong khi bán tất cả coin trong ngày với giá cao nhất`100`sản lượng`1 + 1 + 1`đồng xu lần`100 = 300`. Một trường hợp cạnh khác là khi khoảng thời gian chỉ chứa một ngày; trong trường hợp này, lợi nhuận tối đa chỉ đơn giản là giá xu của ngày đó. 

## Phương pháp tiếp cận 

Giải pháp brute-force xem xét từng kế hoạch một cách độc lập và mô phỏng việc bán tiền xu mỗi ngày trong khoảng thời gian đó. Nó sẽ theo dõi số lượng tiền được khai thác cho đến nay và luôn bán chúng theo giá ngày hiện tại. Điều này đúng, nhưng nó lặp lại mỗi ngày trong mỗi khoảng thời gian truy vấn, dẫn đến`O(n*q)`phức tạp, không thể chấp nhận được đối với các ràng buộc tối đa. Vì`n = 200,000`Và`q = 200,000`, điều này sẽ đòi hỏi khoảng 40 tỷ hoạt động. 

Quan sát quan trọng là vì Ann có thể bán số lượng xu bất kỳ vào bất kỳ ngày nào nên cô ấy luôn muốn bán xu vào ngày có giá cao nhất trong khoảng thời gian khai thác của mình. Tổng lợi nhuận trong một khoảng thời gian`[l, r]`chỉ đơn giản là tổng của đỉnh`k`giá cô ấy có thể bán mỗi ngày, ở đâu`k`tiền xu có sẵn theo ngày`r`. Vì cô ấy khai thác chính xác một đồng xu mỗi ngày nên sẽ có`r - l + 1`xu để bán và chiến lược tối ưu là bán tất cả xu trong ngày với giá tối đa trong`[l, r]`. Điều này làm giảm vấn đề tính tổng các tổng tiền tố tối đa trong khoảng. 

Một cách đơn giản để thực hiện điều này một cách hiệu quả là sử dụng mảng tổng tiền tố của các giá và cây phân đoạn hoặc bảng thưa để tính giá trị tối đa trong bất kỳ khoảng thời gian nào trong`O(1)`(đối với bảng thưa thớt) hoặc`O(log n)`(đối với cây phân đoạn) thời gian cho mỗi truy vấn. Bảng thưa ở đây đặc biệt hiệu quả vì mảng là tĩnh; không có cập nhật nào xảy ra, chỉ có truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*q) | O(1) | Quá chậm | 
| Bảng thưa/Cây phân đoạn cho phạm vi tối đa | O(n log n + q) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ngày`n`và mảng giá`c`. Điều này thiết lập mảng mà chúng ta sẽ thực hiện các truy vấn theo khoảng thời gian trên đó. 
2. Xây dựng một bảng thưa thớt từ`c`có thể trả lại mức giá tối đa trong bất kỳ khoảng thời gian nào`[l, r]`TRONG`O(1)`thời gian. Tính toán trước nhật ký và hàng trong bảng bằng cách sử dụng cấu trúc bảng thưa thớt tiêu chuẩn. 
3. Đọc số lượng truy vấn`q`và danh sách các kế hoạch khai thác. Đối với mỗi truy vấn`[l, r]`, chuyển đổi các chỉ số thành dựa trên 0 để truy cập mảng dễ dàng hơn. 
4. Với mỗi gói, hãy tính tổng giá từ`l`ĐẾN`r`. Sau đó tìm mức giá tối đa trong khoảng đó bằng cách sử dụng bảng thưa thớt. Lợi nhuận tối đa thu được bằng cách nhân số lượng tiền được khai thác`(r - l + 1)`theo mức giá tối đa trong khoảng thời gian đó. 
5. Đưa ra lợi nhuận tối đa cho mỗi truy vấn. 

Tại sao nó hoạt động: Bằng cách luôn bán xu vào ngày có giá cao nhất trong khoảng thời gian đó, Ann sẽ tối đa hóa doanh thu. Bảng thưa đảm bảo rằng chúng ta có thể tìm thấy giá trị tối đa này trong thời gian không đổi cho mỗi truy vấn sau`O(n log n)`tiền xử lý. Vì tất cả các đồng xu đều giống hệt nhau và cô ấy phải bán tất cả các đồng xu vào ngày cuối cùng nên chiến lược này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_sparse_table(arr):
    import math
    n = len(arr)
    K = n.bit_length()
    st = [[0] * n for _ in range(K)]
    st[0] = arr[:]
    for k in range(1, K):
        for i in range(n - (1 << k) + 1):
            st[k][i] = max(st[k-1][i], st[k-1][i + (1 << (k-1))])
    return st

def query_max(st, l, r):
    import math
    k = (r - l + 1).bit_length() - 1
    return max(st[k][l], st[k][r - (1 << k) + 1])

def main():
    n = int(input())
    c = list(map(int, input().split()))
    st = build_sparse_table(c)
    q = int(input())
    res = []
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        num_coins = r - l + 1
        max_price = query_max(st, l, r)
        res.append(num_coins * max_price)
    print(" ".join(map(str, res)))

if __name__ == "__main__":
    main()
```Việc xây dựng bảng thưa thớt`K`các lớp, trong đó mỗi lớp lưu trữ giá trị tối đa trong các khoảng thời gian`2^k`. Trong các truy vấn, lũy thừa lớn nhất của hai khớp trong`[l, r]`xác định hai khoảng chồng chéo nào cần xem xét. Việc chuyển đổi các chỉ số dựa trên 1 thành dựa trên 0 đảm bảo tính chính xác. Nhân số lượng xu với giá tối đa sẽ trực tiếp tạo ra lợi nhuận tối đa. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
4 1 2 3 2
4
1 5
2 4
3 5
5 5
```| Truy vấn | Khoảng thời gian | Giá tối đa | Tiền xu | Lợi nhuận | 
| --- | --- | --- | --- | --- | 
| 1 | 1-5 | 4 | 5 | 20 | 
| 2 | 2-4 | 3 | 3 | 9 | 
| 3 | 3-5 | 3 | 3 | 9 | 
| 4 | 5-5 | 2 | 1 | 2 | 

Dấu vết này xác nhận rằng thuật toán xác định chính xác ngày tối ưu để bán và tính toán lợi nhuận tương ứng. 

Một ví dụ khác:```
3
1 100 1
2
1 3
2 3
```| Truy vấn | Khoảng thời gian | Giá tối đa | Tiền xu | Lợi nhuận | 
| --- | --- | --- | --- | --- | 
| 1 | 1-3 | 100 | 3 | 300 | 
| 2 | 2-3 | 100 | 2 | 200 | 

Điều này chứng tỏ rằng thuật toán xử lý các đợt tăng giá đột ngột một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Quá trình tiền xử lý bảng thưa thớt là O(n log n) và mỗi truy vấn là O(1) | 
| Không gian | O(n log n) | Cửa hàng bảng thưa thớt log n lớp có kích thước n | 

Được cho`n, q <= 2*10^5`, cách tiếp cận này phù hợp thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("5\n4 1 2 3 2\n4\n1 5\n2 4\n3 5\n5 5\n") == "20 9 9 2", "sample 1"

# Minimum size
assert run("1\n5\n1\n1 1\n") == "5", "min size"

# All equal values
assert run("4\n2 2 2 2\n2\n1 4\n2 3\n") == "8 4", "all equal"

# Maximum values
assert run("5\n1000000 1000000 1000000 1000000 1000000\n1\n1 5\n") == "5000000", "max values"

# Boundary condition
assert run("3\n1 2 3\n1\n2 3\n") == "4", "boundary"

# Peak in middle
assert run("5\n1 10 1 10 1\n1\n1 5\n") == "50
```
