---
title: "CF 1302C - Cây phân đoạn hay Fenwick?"
description: "Chúng tôi được yêu cầu duy trì một mảng các số nguyên bắt đầu bằng tất cả các số 0 và trả lời một loạt truy vấn. Mỗi truy vấn là một phép gán, đặt một phần tử cụ thể thành một giá trị hoặc một truy vấn tổng phạm vi, yêu cầu tổng của một mảng con liền kề."
date: "2026-06-11T18:10:10+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1302
codeforces_index: "C"
codeforces_contest_name: "AIM Tech Poorly Prepared Contest (unrated, funny, Div. 1 preferred)"
rating: 0
weight: 1302
solve_time_s: 106
verified: true
draft: false
---

[CF 1302C - Cây phân đoạn hay Fenwick?](https://codeforces.com/problemset/problem/1302/C) 

**Đánh giá:** - 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu duy trì một mảng các số nguyên bắt đầu bằng tất cả các số 0 và trả lời một loạt truy vấn. Mỗi truy vấn là một phép gán, đặt một phần tử cụ thể thành một giá trị hoặc một truy vấn tổng phạm vi, yêu cầu tổng của một mảng con liền kề. Có nhiều trường hợp thử nghiệm độc lập, mỗi trường hợp có mảng và truy vấn riêng. Đầu ra chỉ đơn giản là kết quả của các truy vấn tính tổng theo thứ tự. 

Các ràng buộc rất chặt chẽ: độ dài mảng và số lượng truy vấn trên mỗi trường hợp thử nghiệm có thể lên tới$10^5$và tổng số trên tất cả các trường hợp thử nghiệm có thể đạt tới$10^6$. Điều này loại trừ các cách tiếp cận ngây thơ quét phạm vi cho mọi truy vấn tổng. Một giải pháp ngây thơ sẽ mất$O(n)$cho mỗi truy vấn tổng, dẫn đến tối đa$10^{11}$hoạt động trong trường hợp xấu nhất, vượt xa mọi thời hạn hợp lý. 

Một trường hợp phức tạp phát sinh khi một truy vấn yêu cầu tổng của toàn bộ mảng trước bất kỳ cập nhật nào. Trong trường hợp đó, việc triển khai bất cẩn giả định các giá trị được khởi tạo có thể cố đọc bộ nhớ chưa được khởi tạo hoặc tính toán sai, nhưng tổng đúng là bằng 0. Một trường hợp khác là các bản cập nhật lặp lại ở cùng một vị trí, phải ghi đè lên các giá trị trước đó thay vì tích lũy. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Chúng tôi duy trì mảng một cách rõ ràng và xử lý từng truy vấn một cách rõ ràng. Các bài tập được thực hiện trong$O(1)$thời gian, nhưng các truy vấn tổng yêu cầu lặp lại từ chỉ mục bên trái sang chỉ mục bên phải, đó là$O(n)$trong trường hợp xấu nhất. Với$q$các truy vấn, độ phức tạp tổng thể có thể đạt tới$O(nq)$, quá chậm đối với đầu vào lớn. 

Thông tin chi tiết quan trọng để cải thiện hiệu suất là chúng tôi chỉ cần hỗ trợ cập nhật điểm và truy vấn tổng phạm vi. Điều này hoàn toàn phù hợp với khả năng của Cây Fenwick (Cây được lập chỉ mục nhị phân) hoặc Cây phân đoạn. Cả hai cấu trúc dữ liệu đều cho phép cập nhật điểm trong$O(\log n)$và truy vấn tổng phạm vi trong$O(\log n)$. Cây Fenwick dễ triển khai hơn và thực tế nhanh hơn một chút đối với loại truy vấn này. 

Quan sát quan trọng là các giá trị mảng chỉ được thay thế chứ không tăng lên. Đối với Cây Fenwick, chúng ta có thể lưu trữ các giá trị hiện tại trong một mảng riêng biệt để tính toán sự khác biệt trên mỗi phép gán, sau đó truyền sự khác biệt đó vào cây. Điều này cho phép mỗi truy vấn được xử lý trong$O(\log n)$, giúp duy trì tổng số hoạt động ở mức$10^7$đối với những hạn chế của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây Fenwick | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng cho mỗi trường hợp thử nghiệm: mảng chính để lưu trữ các giá trị hiện tại và mảng Cây Fenwick để tính tổng tiền tố hiệu quả. Điều này thiết lập một cấu trúc có thể xử lý cả phép gán và truy vấn phạm vi một cách hiệu quả. 
2. Đối với truy vấn loại 1,`1 x y`, tính sự khác biệt giữa giá trị mới`y`và giá trị hiện tại tại vị trí`x`. Cập nhật mảng chính tại vị trí`x`với giá trị mới. Sau đó nhân rộng sự khác biệt này thông qua Cây Fenwick. Việc truyền bá sự khác biệt đảm bảo rằng các truy vấn tổng phạm vi trong tương lai phản ánh giá trị được cập nhật mà không cần tính lại tổng từ đầu. 
3. Đối với truy vấn loại 2,`2 l r`, tính tổng tiền tố lên tới`r`trong Cây Fenwick và trừ tổng tiền tố lên tới`l-1`. Điều này đưa ra tổng của mảng con`[l, r]`. Xuất giá trị này. 
4. Lặp lại các bước trên cho từng truy vấn và cho từng trường hợp kiểm thử một cách độc lập. 

Tại sao nó hoạt động: Cây Fenwick duy trì tính bất biến là mỗi nút lưu trữ một phần tổng của mảng. Khi chúng tôi cập nhật một giá trị, sự khác biệt sẽ được truyền đến tất cả các nút có liên quan, đảm bảo rằng mọi truy vấn tổng tiền tố đều trả về tổng số chính xác. Khi đó, các truy vấn tổng phạm vi chỉ là hiệu của hai tổng tiền tố, tương đương về mặt toán học với tổng trực tiếp của các phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def add(bit, i, delta):
    n = len(bit)
    while i < n:
        bit[i] += delta
        i += i & -i

def prefix_sum(bit, i):
    result = 0
    while i > 0:
        result += bit[i]
        i -= i & -i
    return result

def main():
    T = int(input())
    for _ in range(T):
        n, q = map(int, input().split())
        arr = [0] * (n + 1)
        bit = [0] * (n + 1)
        for _ in range(q):
            query = input().split()
            if query[0] == '1':
                x, y = int(query[1]), int(query[2])
                delta = y - arr[x]
                arr[x] = y
                add(bit, x, delta)
            else:
                l, r = int(query[1]), int(query[2])
                result = prefix_sum(bit, r) - prefix_sum(bit, l - 1)
                print(result)

if __name__ == "__main__":
    main()
```Giải pháp trước tiên xác định các hàm trợ giúp để cập nhật Cây Fenwick và tính tổng tiền tố. các`add`hàm truyền delta qua tất cả các nút có liên quan.`prefix_sum`đi lùi qua cây để tính tổng các giá trị liên quan. Trong vòng lặp chính, chúng tôi đọc từng trường hợp thử nghiệm, khởi tạo mảng và cây, đồng thời xử lý từng truy vấn theo loại của nó. Sự tinh tế quan trọng là tính toán sự khác biệt`delta`cho các bài tập để duy trì tính chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Bước | Truy vấn | mảng | BIT | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 1 6 | [0,0,0,0,0,0,0] | [0,0,0,0,0,0,0] | 0 | 
| 2 | 1 3 2 | [0,0,2,0,0,0,0] | [0,0,2,2,0,0,0] | | 
| 3 | 2 2 4 | [0,0,2,0,0,0,0] | [0,0,2,2,0,0,0] | 2 | 
| 4 | 1 6 3 | [0,0,2,0,0,0,3] | [0,0,2,2,0,0,3] | | 
| 5 | 2 1 6 | [0,0,2,0,0,0,3] | [0,0,2,2,0,0,3] | 5 | 

Điều này chứng tỏ rằng thuật toán cập nhật chính xác các giá trị và tính tổng trên các phạm vi tùy ý. 

**Mẫu 2** 

| Bước | Truy vấn | mảng | BIT | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 3 7 | [0,0,7,0,0,0] | [0,0,7,7,0,0] | | 
| 2 | 1 1 4 | [0,4,7,0,0,0] | [0,4,7,11,0,0] | | 
| 3 | 2 1 5 | [0,4,7,0,0,0] | [0,4,7,11,0,0] | 11 | 

Ở đây, chúng ta thấy rằng nhiều bản cập nhật tích lũy chính xác trong Cây Fenwick và phép tính tổng phạm vi sử dụng chênh lệch của tổng tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q) log n) | Mỗi truy vấn yêu cầu O(log n) để cập nhật hoặc tính tổng; mảng khởi tạo là O(n) cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng và Cây Fenwick cho mỗi trường hợp thử nghiệm. | 

Cho rằng tổng của`n`Và`q`trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^6$, tổng số hoạt động dưới đây$2 \cdot 10^6 \cdot \log 10^5 \approx 2 \cdot 10^6 \cdot 17$, vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided samples
assert run("2\n6 5\n2 1 6\n1 3 2\n2 2 4\n1 6 3\n2 1 6\n5 3\n1 3 7\n1 1 4\n2 1 5\n") == "0\n2\n5\n11", "sample 1"

# Minimum-size input
assert run("1\n1 2\n2 1 1\n1 1 5\n") == "0", "min size"

# Maximum-size input: all zeros, sum query entire array
n = 100000
q = 1
inp = f"1\n{n} {q}\n2 1 {n}\n"
assert run(inp) == "0", "max size, all zeros"

# Repeated updates
assert run("1\n3 5\n1 1 5\n1 1 10\n2 1 1\n
```
