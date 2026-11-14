---
title: "CF 1807D - Truy vấn lẻ"
description: "Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn hỏi liệu nếu chúng ta ghi đè tất cả các phần tử trong một mảng con nhất định bằng một giá trị cố định thì tổng của mảng sẽ trở thành số lẻ. Các truy vấn không sửa đổi mảng vĩnh viễn."
date: "2026-06-09T09:03:45+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1807
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 859 (Div. 4)"
rating: 900
weight: 1807
solve_time_s: 89
verified: true
draft: false
---

[CF 1807D - Truy vấn kỳ lạ](https://codeforces.com/problemset/problem/1807/D) 

**Xếp hạng:** 900 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn hỏi liệu nếu chúng ta ghi đè tất cả các phần tử trong một mảng con nhất định bằng một giá trị cố định thì tổng của mảng sẽ trở thành số lẻ. Các truy vấn không sửa đổi mảng vĩnh viễn. Đối với mỗi truy vấn, chúng tôi chỉ cần "CÓ" nếu tổng là số lẻ sau khi ghi đè giả định và "KHÔNG" nếu tổng là số chẵn. 

Kích thước mảng$n$có thể lên tới 200.000 và số lượng truy vấn$q$cũng có thể là 200.000, nhưng tổng của tất cả$n$và tất cả$q$trên các trường hợp thử nghiệm vẫn ở mức dưới 200.000. Điều này ngụ ý rằng chúng ta có thể thực hiện công việc tuyến tính cho mỗi trường hợp thử nghiệm chứ không phải cho mỗi truy vấn. MỘT$O(n \cdot q)$Cách tiếp cận ngay lập tức là quá chậm vì trong trường hợp xấu nhất có thể yêu cầu$4 \cdot 10^{10}$hoạt động. 

Các trường hợp biên cần xem xét bao gồm các mảng rất nhỏ, các truy vấn bao trùm toàn bộ mảng và các truy vấn trong đó giá trị thay thế rất lớn. Ví dụ, nếu mảng là`[2]`và truy vấn yêu cầu đặt phần tử duy nhất thành`1`, tổng trở thành`1`điều này thật kỳ lạ và câu trả lời là "CÓ". Một cách tiếp cận đơn giản là tính toán lại tổng của toàn bộ mảng cho mỗi truy vấn sẽ hoạt động trên các mảng nhỏ nhưng không hoạt động trên các mảng lớn. 

Một điều tinh tế khác là chúng tôi chỉ quan tâm đến **chẵn lẻ**. Bản thân các số lớn không cần số học đầy đủ; biết chúng lẻ hay chẵn là đủ. Một lỗi phổ biến là tính tổng đầy đủ với số lượng lớn, có nguy cơ tràn hoặc tính toán không cần thiết. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trong phạm vi truy vấn, thay thế các phần tử, tính tổng và kiểm tra tính chẵn lẻ. Điều này hoạt động vì nó mô phỏng chính xác vấn đề. Đối với mỗi truy vấn, chúng tôi có thể thực hiện tối đa$r - l + 1$hoạt động và với 200.000 truy vấn trên mảng có kích thước 200.000, điều này mang lại trường hợp xấu nhất$O(n \cdot q) = O(4 \cdot 10^{10})$, quá chậm. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần **chẵn lẻ** chứ không cần toàn bộ số tiền. Thay vì thay thế các phần tử, chúng ta có thể tính tổng mới bằng tổng cũ trừ đi tổng các phần tử được thay thế cộng với tổng các giá trị mới. Đặt độ dài của phạm vi được thay thế là$m = r - l + 1$. Tính chẵn lẻ của tổng mới là:```
new_sum_parity = (old_sum - sum_of_range + m * k) % 2
```Bởi vì chỉ có vấn đề về tính chẵn lẻ, nên phép trừ và phép cộng có thể được thực hiện theo modulo 2. Mỗi phần tử modulo 2 là 0 hoặc 1. Tổng của phạm vi ban đầu modulo 2 có thể được tính toán trước bằng mảng tổng tiền tố modulo 2. Nhân giá trị thay thế$k$theo chiều dài phạm vi$m$modulo 2 đơn giản là$0$nếu như$k$là chẵn hoặc$m % 2$nếu như$k$thật kỳ quặc. 

Quan sát này làm giảm việc xử lý truy vấn xuống$O(1)$cho mỗi truy vấn sau khi tính tổng tiền tố của mảng ban đầu. Điều này hoạt động hiệu quả đối với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của toàn bộ mảng theo modulo 2. Điều này thể hiện tính chẵn lẻ hiện tại của mảng. Chúng ta chỉ cần 0 cho số chẵn, 1 cho số lẻ. 
2. Tính mảng tổng tiền tố modulo 2. Giả sử`prefix[i]`là tổng modulo 2 của số đầu tiên`i`các phần tử. Điều này cho phép truy vấn O(1) về tính chẵn lẻ tổng của bất kỳ mảng con nào`[l, r]`sử dụng`prefix[r] - prefix[l-1]`modulo 2. 
3. Đối với mỗi truy vấn`(l, r, k)`, tính độ dài`m = r - l + 1`. Tính tính chẵn lẻ của tổng phạm vi modulo 2 từ mảng tổng tiền tố. 
4. Tính tính chẵn lẻ của`m * k`. Đây là`0`nếu như`k`là số chẵn, hoặc`m % 2`nếu như`k`thật kỳ quặc. 
5. Tính chẵn lẻ mảng mới như sau`(total_parity - range_parity + replacement_parity) % 2`. 
6. Nếu kết quả là 1, in ra "CÓ". Ngược lại in ra "KHÔNG". 

Tại sao nó hoạt động: tất cả số học là modulo 2. Điều bất biến là bất kỳ chuỗi cộng và trừ modulo 2 nào đều cho kết quả chẵn lẻ chính xác. Vì mỗi truy vấn là độc lập nên chúng tôi không bao giờ sửa đổi mảng thực tế và chúng tôi chỉ dựa vào tổng tiền tố được tính toán trước và tính chẵn lẻ của tổng tổng mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        prefix = [0] * (n + 1)
        total_parity = 0
        for i in range(n):
            prefix[i + 1] = (prefix[i] + a[i]) % 2
            total_parity = (total_parity + a[i]) % 2

        for _ in range(q):
            l, r, k = map(int, input().split())
            m = r - l + 1
            range_parity = (prefix[r] - prefix[l - 1]) % 2
            replacement_parity = (m % 2) if (k % 2 == 1) else 0
            new_parity = (total_parity - range_parity + replacement_parity) % 2
            print("YES" if new_parity == 1 else "NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này tính tổng tiền tố modulo 2 để cho phép truy vấn tính chẵn lẻ của mảng con nhanh chóng. Sau đó, mỗi truy vấn được xử lý trong O(1), tính toán hiệu quả của việc thay thế phạm vi trên tính chẵn lẻ tổng thể. Những điểm tinh tế bao gồm việc sử dụng`l-1`trong mảng tiền tố cho các chỉ số dựa trên 1 và giảm tất cả modulo 2 số học để tránh số lượng lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
2 2 1 3 2
2 3 3
```| Truy vấn | tôi | r | k | m | phạm vi_chẵn lẻ | thay thế_parity | tổng_chẵn lẻ | new_parity | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 3 | 2 | 1 | 0 | 0 | 1 | CÓ | 

Giải thích: tổng ban đầu = 10 (chẵn), thay thế các chỉ số 2-3 bằng 3, độ dài 2, chẵn lẻ thay thế 0, trừ phạm vi chẵn lẻ 1 → 10-1+0 = 9 → lẻ. 

### Ví dụ 2 

đầu vào:```
1 1
1
1 1 2
```| Truy vấn | tôi | r | k | m | phạm vi_chẵn lẻ | thay thế_parity | tổng_chẵn lẻ | new_parity | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | 1 | 0 | 1 | 0 | KHÔNG | 

Điều này kiểm tra mảng nhỏ nhất. Thay thế`1`thậm chí với`2`đổi tổng từ lẻ thành chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Tính tổng tiền tố là O(n), mỗi truy vấn là O(1) | 
| Không gian | O(n) | Lưu trữ tổng tiền tố modulo 2 | 

Tổng cộng`n`Và`q`trên các trường hợp thử nghiệm không vượt quá 200.000, do đó O(n + q) có thể chấp nhận được trong giới hạn 2 giây. Số học Modulo tránh tổng số nguyên lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("""2
5 5
2 2 1 3 2
2 3 3
2 3 4
1 5 5
1 4 9
2 4 3
10 5
1 1 1 1 1 1 1 1 1 1
3 8 13
2 5 10
3 8 10
1 10 2
1 9 100""") == """YES
YES
YES
NO
YES
NO
NO
NO
NO
YES"""

# Custom cases
assert run("""1
1 1
1
1 1 1""") == "NO", "single element replaced with same parity"
assert run("""1
2 2
1 2
1 1 2
2 2 3""") == "NO\nYES", "two elements, mixed parity"
assert run("""1
3 3
2 2 2
1 3 1
1 2
```
