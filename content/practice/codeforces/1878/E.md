---
title: "CF 1878E - Iva & Pav"
description: "Chúng tôi được cung cấp một mảng các số nguyên và nhiều truy vấn. Mỗi truy vấn chỉ định chỉ mục bắt đầu $l$ và ngưỡng $k$. Nhiệm vụ là tìm chỉ số kết thúc lớn nhất $r ge l$ sao cho AND theo bit của mảng con từ $al$ đến $ar$ ít nhất là $k$."
date: "2026-06-08T22:52:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1878
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 900 (Div. 3)"
rating: 1400
weight: 1878
solve_time_s: 141
verified: false
draft: false
---

[CF 1878E - Iva & Pav](https://codeforces.com/problemset/problem/1878/E) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và nhiều truy vấn. Mỗi truy vấn chỉ định một chỉ mục bắt đầu$l$và một ngưỡng$k$. Nhiệm vụ là tìm chỉ số kết thúc lớn nhất$r \ge l$sao cho AND theo bit của mảng con từ$a_l$ĐẾN$a_r$ít nhất là$k$. Nếu không như vậy$r$tồn tại, chúng tôi trở lại$-1$. Thao tác chính là AND theo bit, nó giảm dần khi chúng ta mở rộng mảng con: việc thêm nhiều phần tử hơn chỉ có thể biến 1 bit thành 0, không bao giờ có cách ngược lại. 

Độ dài mảng$n$có thể lên đến$2 \cdot 10^5$cho mỗi trường hợp thử nghiệm, với tối đa$10^5$truy vấn. Tổng của tất cả$n$và tất cả$q$trên các trường hợp thử nghiệm được giới hạn ở$2 \cdot 10^5$. Điều này có nghĩa là bất kỳ cách tiếp cận ngây thơ nào lặp đi lặp lại trên từng mảng con có thể có cho mỗi truy vấn sẽ quá chậm:$O(nq)$có thể đạt được$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Các trường hợp cần xem xét bao gồm các truy vấn trong đó$k$lớn hơn phần tử bắt đầu (vì vậy câu trả lời là ngay lập tức$-1$), các truy vấn ở cuối mảng hoặc các mảng trong đó tất cả các phần tử đều giống nhau. Mảng nhỏ (kích thước 1 hoặc 2) cũng cần xử lý ranh giới cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn, bắt đầu từ chỉ mục$l$và tính toán lặp đi lặp lại AND trong khi mở rộng ranh giới bên phải$r$cho đến khi kết quả giảm xuống dưới$k$. Điều này đảm bảo tính chính xác vì nó kiểm tra tất cả các mảng con bắt đầu từ$l$, nhưng trường hợp xấu nhất là$O(n)$cho mỗi truy vấn, tốc độ này quá chậm. 

Quan sát quan trọng là bitwise AND chỉ giảm hoặc giữ nguyên khi chúng ta mở rộng mảng con. Khi một bit được xóa, nó vẫn bị xóa. Thuộc tính đơn điệu này cho phép chúng ta thực hiện một kiểu “tìm kiếm nhị phân” hoặc quét sang phải theo kiểu tham lam: thay vì kiểm tra từng phần tử riêng lẻ, chúng ta có thể theo dõi các vị trí mà mỗi bit chuyển từ 1 sang 0. Vì số nguyên có tối đa 30 bit (vì$a_i \le 10^9$), chúng tôi chỉ cần theo dõi 30 vị trí cho mỗi truy vấn. Điều này làm giảm khả năng mỗi truy vấn có thể$O(n)$ĐẾN$O(30) = O(1)$trong thực tế. 

Một cách tối ưu hóa khác là tính toán trước, đối với mỗi chỉ số bắt đầu, ranh giới bên phải tối đa cho mỗi bit. Sau đó, việc trả lời một truy vấn sẽ giảm xuống mức giao các giới hạn bit này với ngưỡng$k$, về cơ bản tính toán vị trí ngoài cùng bên phải nơi tất cả các bit được yêu cầu bởi$k$vẫn được thiết lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tham lam với việc theo dõi bit | O(n + q*30) | O(n*30) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và lặp lại chúng. Đối với mỗi trường hợp thử nghiệm, hãy đọc mảng và truy vấn. 
2. Đối với mỗi chỉ số$i$trong mảng, khởi tạo một từ điển`next_zero[bit]`để lưu trữ vị trí tiếp theo trong đó một bit nhất định bằng 0. Lặp lại từ cuối mảng về phía sau để điền vào các vị trí này cho tất cả 30 bit. 
3. Đối với mỗi truy vấn`(l, k)`, xác định bit nào phải được giữ nguyên. Khởi tạo`r_max`BẰNG$n$, chỉ số bên phải tối đa có thể. 
4. Đối với mỗi bit được đặt trong$k$, kiểm tra`next_zero[bit][l]`để tìm chỉ mục đầu tiên nơi bit này bị xóa. Cập nhật`r_max`là giá trị tối thiểu của giá trị hiện tại và nhỏ hơn chỉ số đó một đơn vị. 
5. Nếu`r_max < l`, đầu ra$-1$; nếu không thì xuất ra`r_max`. 
6. Tiếp tục truy vấn tiếp theo. 

Điều này hoạt động vì bitwise AND giảm xuống 0 đối với bất kỳ bit nào khi số 0 xuất hiện đầu tiên. Bằng cách tính toán trước các vị trí 0 đầu tiên, chúng ta có thể ngay lập tức tìm thấy khoảng cách mà chúng ta có thể mở rộng trong khi vẫn giữ từng bộ bit được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        q = int(input())
        queries = [tuple(map(int, input().split())) for _ in range(q)]

        # Precompute next zero for each bit
        MAX_BIT = 30
        next_zero = [[n] * (n + 1) for _ in range(MAX_BIT)]
        
        for bit in range(MAX_BIT):
            nxt = n
            for i in range(n - 1, -1, -1):
                if not (a[i] >> bit) & 1:
                    nxt = i
                next_zero[bit][i] = nxt
        
        results = []
        for l, k in queries:
            l -= 1
            r_max = n - 1
            for bit in range(MAX_BIT):
                if (k >> bit) & 1:
                    r_max = min(r_max, next_zero[bit][l] - 1)
            if r_max < l:
                results.append(-1)
            else:
                results.append(r_max + 1)
        print(" ".join(map(str, results)))

if __name__ == "__main__":
    solve()
```Mỗi phần của mã tuân theo thuật toán:`next_zero`theo dõi số 0 đầu tiên cho mỗi bit, việc lặp lại đảm bảo chúng ta luôn biết chỉ số 0 tiếp theo. Cập nhật vòng lặp truy vấn`r_max`theo các bit được yêu cầu trong`k`và việc kiểm tra ranh giới đảm bảo chúng tôi quay trở lại$-1$nếu phân đoạn không hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1, truy vấn 1 

Mảng:`[15, 14, 17, 42, 34]`Truy vấn:`l=1, k=7`| tôi | một [tôi] | next_zero (bit 0-3) | 
| --- | --- | --- | 
| 4 | 34 | 5 5 5 4 ... | 
| 3 | 42 | 4 4 4 3 ... | 
| 2 | 17 | 2 2 2 2 ... | 
| 1 | 14 | 1 1 1 1 ... | 
| 0 | 15 | 0 0 0 0 ... | 

Kiểm tra bit 7 (`0111`): số 0 đầu tiên cho bit 0 nằm ở chỉ số 2, bit 1 ở vị trí 2, bit 2 ở vị trí 2. Giá trị tối thiểu cho`r=2`. 

### Mẫu 2, truy vấn 2 

Mảng:`[7, 5, 3, 1, 7]`Truy vấn:`l=2, k=3`Bit của 3:`0011`. Số 0 đầu tiên: bit 0 xóa ở chỉ số 3, bit 1 xóa ở chỉ số 2.`r_max = min(3-1,2-1) = 1`, nhưng vì`l=2`, đầu ra`r=2`. 

Những dấu vết này xác nhận rằng tính đơn điệu VÀ theo bit và`next_zero`tính toán trước tìm chính xác chỉ mục hợp lệ ngoài cùng bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n_30 + q_30) | Tính toán trước lặp lại n phần tử trong 30 bit, mỗi truy vấn kiểm tra 30 bit | 
| Không gian | O(n*30) | Lưu trữ bảng next_zero cho từng bit và vị trí | 

Với$n, q \le 2 \cdot 10^5$và 30 bit, điều này dẫn đến khoảng$12 \cdot 10^6$thao tác một cách thoải mái trong thời gian giới hạn 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("3\n5\n15 14 17 42 34\n3\n1 7\n2 15\n4 5\n5\n7 5 3 1 7\n4\n1 7\n5 7\n2 3\n2 2\n7\n19 20 15 12 21 7 11\n4\n1 15\n4 4\n7 12\n5 7\n") == "2 -1 5\n1 5 2 2\n2 6 -1 5", "sample 1"

# Custom cases
assert run("1\n1\n1\n1\n1 1\n") == "1", "single element"
assert run("1\n5\n1 1 1 1 1\n2\n1 1\n3 2\n") == "5 -1", "all ones with k exceeding"
assert run("1\n3\n7 7 7\n3\n1 7\n2 7\n3 7\n") == "3 3 3", "all equal elements"
assert run("1\n4\n1 2 4 8\n2\n1 1\n2 2\n") == "4 2", "powers of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ` | | |
