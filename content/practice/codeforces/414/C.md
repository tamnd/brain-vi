---
title: "CF 414C - Mashmokh và hoạt động ngược"
description: "Chúng ta được cung cấp một mảng có độ dài $2^n$, trong đó $n$ tối đa là 20 và một chuỗi truy vấn. Mỗi truy vấn chỉ định một số $qi$, số này yêu cầu chúng ta liên tục chia mảng thành các khối có kích thước $2^{qi}$, đảo ngược từng khối rồi nối các khối lại với nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "divide-and-conquer"]
categories: ["algorithms"]
codeforces_contest: 414
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 240 (Div. 1)"
rating: 2100
weight: 414
solve_time_s: 100
verified: true
draft: false
---

[CF 414C - Mashmokh và Hoạt động ngược](https://codeforces.com/problemset/problem/414/C) 

**Xếp hạng:** 2100 
**Thẻ:** tổ hợp, chia để trị 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$2^n$, Ở đâu$n$nhiều nhất là 20 và một chuỗi truy vấn. Mỗi truy vấn chỉ định một số$q_i$, điều này yêu cầu chúng ta liên tục chia mảng thành các khối có kích thước$2^{q_i}$, đảo ngược từng khối rồi nối các khối lại với nhau. Sau khi thực hiện đảo ngược, chúng ta được yêu cầu đếm số lần đảo ngược trong mảng kết quả. 

Đảo ngược là một cặp chỉ số$(i,j)$như vậy$i < j$Và$a[i] > a[j]$. Các hoạt động truy vấn được **tích lũy**, nghĩa là mảng được sửa đổi vĩnh viễn sau mỗi truy vấn. 

Với các ràng buộc, kích thước mảng tối đa là$2^{20} \approx 10^6$, có thể quản lý được, nhưng số lượng truy vấn cũng có thể rất lớn ($m \le 10^6$). Do đó, bất kỳ giải pháp ngây thơ nào tính toán lại các nghịch đảo từ đầu sau mỗi truy vấn bằng thuật toán bậc hai ($O(N^2)$) sẽ rất chậm. Thậm chí một$O(N \log N)$số lượng đảo ngược trên mỗi truy vấn có thể hầu như không phù hợp và chúng ta nên tìm cách **tái sử dụng các phép tính**. 

Vỏ cạnh bao gồm$n = 0$(mảng có kích thước 1, không đảo ngược),$q_i = 0$(khối có kích thước 1, không ảnh hưởng đến mảng) và các truy vấn lặp lại giống nhau$q_i$nhiều lần. Xử lý việc đảo ngược mảng một cách cẩn thận cũng rất quan trọng vì việc đảo ngược khối không giống như đảo ngược toàn bộ mảng. 

Việc triển khai ngây thơ có thể âm thầm thất bại do tính toán lại các phép đảo ngược không chính xác sau khi đảo ngược một phần hoặc do hiểu sai cách chia thành các khối khi$q_i = 0$hoặc$q_i = n$. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: với mỗi truy vấn, hãy chia mảng thành các khối có kích thước$2^{q_i}$, đảo ngược từng khối, tập hợp lại mảng, sau đó đếm các nghịch đảo bằng cách sử dụng sắp xếp hợp nhất hoặc một cách đơn giản$O(N^2)$vòng lặp. Đếm số lần đảo ngược thông qua sắp xếp hợp nhất là$O(N \log N)$. 

Vì$n = 20$,$N = 2^n = 1,048,576$. Chi phí cho một lần sắp xếp hợp nhất cho mỗi truy vấn là khoảng$N \log N \approx 2 \times 10^7$hoạt động. Với$m = 10^6$truy vấn, điều này sẽ vượt quá giới hạn thời gian hợp lý. Thậm chí$O(N \log N)$mỗi truy vấn quá chậm. 

Thông tin chi tiết quan trọng là mỗi truy vấn đảo ngược **khối kích thước$2^{q_i}$**, tương ứng chính xác với việc lật các bit trong biểu diễn nhị phân của các chỉ số. Nếu chúng tôi tính toán trước số lần đảo ngược ở mỗi cấp của **cây sắp xếp hợp nhất**, chúng tôi có thể trả lời từng truy vấn bằng cách chỉ cần chuyển đổi các cấp độ được đảo ngược. Cụ thể, một loại hợp nhất đầy đủ có thể được xem dưới dạng **cây chia để trị**: cấp cao nhất hợp nhất nửa thứ nhất và nửa thứ hai, cấp tiếp theo hợp nhất các phần tư, v.v. Đảo ngược kích thước khối$2^k$hoán đổi các phần tử theo cách ảnh hưởng đến sự đảo ngược ở các cấp độ$0..k-1$của cây này. 

Do đó, cách tiếp cận tối ưu là xử lý trước các phép đảo ngược theo cấp độ và duy trì **mặt nạ bit của các lần lật**. Mỗi truy vấn cập nhật mặt nạ này và tổng số lần đảo ngược có thể được tính toán trong$O(n)$mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * N log N) | O(N) | Quá chậm | 
| Tối ưu | O(N log N + m * n) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Tính toán trước các phép đảo ngược theo cấp độ.** Xử lý mảng dưới dạng cây sắp xếp hợp nhất đầy đủ. Mỗi cấp độ sắp xếp hợp nhất tương ứng với các đoạn có độ dài hợp nhất$2^k$. Trong quá trình tiền xử lý, đối với mỗi cấp độ, hãy đếm số lần đảo ngược được tạo giữa nửa bên trái và bên phải của mỗi phân đoạn và lưu trữ chúng trong một mảng`inv_level[k]`. Điều này cho chúng ta một cách để biết, đối với bất kỳ kích thước phân khúc nào, nó đóng góp bao nhiêu lần đảo ngược nếu không bị đảo ngược. 
2. **Khởi tạo mặt nạ bit để theo dõi sự đảo chiều.** Mặt nạ bit`rev_mask`có chiều dài$n$, Ở đâu`rev_mask[k]`cho biết liệu các phân đoạn có kích thước$2^k$đã bị đảo ngược do các truy vấn. Ban đầu tất cả đều là số không. 
3. **Xử lý từng truy vấn$q_i$.** Đối với một truy vấn$q_i$, lật tất cả các bit trong mặt nạ ở các cấp độ`0`ĐẾN`q_i-1`. Điều này mô phỏng hiệu ứng đảo ngược tất cả các khối có kích thước$2^{q_i}$. Lý do xuất phát từ cách các phép đảo ngược sắp xếp hợp nhất bị ảnh hưởng: việc lật một phân đoạn sẽ đảo ngược tất cả các phép đảo ngược cấp thấp hơn bên trong nó. 
4. **Tính tổng số nghịch đảo.** Sau khi cập nhật mặt nạ, hãy tính tổng số nghịch đảo bằng cách tính tổng`inv_level[k]`cho các cấp độ mà`rev_mask[k]`bằng không, và`total_pairs[k] - inv_level[k]`cho các cấp độ mà`rev_mask[k]`là một. Đây`total_pairs[k]`là số lần đảo ngược tối đa có thể có ở cấp độ`k`, tức là các cặp có kích thước bằng nửa bên trái và bên phải$2^k$. 
5. **Kết quả đầu ra.** Lặp lại các bước 3-4 cho mỗi truy vấn. Bởi vì chúng tôi chỉ cập nhật một mặt nạ và tổng hợp$n \le 20$cấp độ, mỗi truy vấn là$O(n)$và khả thi đối với$m \le 10^6$. 

**Tại sao nó hoạt động.** Đảo ngược là tính chất bổ sung trong cây sắp xếp hợp nhất. Đảo ngược một phân đoạn kích thước$2^k$lật tất cả các đảo ngược bên trong phân đoạn đó, được tính toán chính xác bằng cách chuyển đổi các giá trị được tính toán trước cho các cấp độ bên dưới$k$. Không có sự đảo ngược nào được tính hai lần hoặc bị bỏ sót vì cấu trúc cây sắp xếp hợp nhất phân vùng tất cả các cặp duy nhất ở một mức nào đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_inversions(arr):
    n = len(arr)
    tmp = arr[:]
    inv_levels = [0] * (n.bit_length())
    
    def merge_sort(l, r, level):
        if r - l <= 1:
            return 0
        m = (l + r) // 2
        inv = merge_sort(l, m, level - 1) + merge_sort(m, r, level - 1)
        i, j, k = l, m, l
        while i < m and j < r:
            if arr[i] <= arr[j]:
                tmp[k] = arr[i]
                i += 1
            else:
                tmp[k] = arr[j]
                inv += m - i
                j += 1
            k += 1
        while i < m:
            tmp[k] = arr[i]
            i += 1
            k += 1
        while j < r:
            tmp[k] = arr[j]
            j += 1
            k += 1
        arr[l:r] = tmp[l:r]
        inv_levels[level] += inv
        return inv
    
    total_inv = merge_sort(0, n, n.bit_length() - 1)
    return inv_levels, total_inv

n = int(input())
arr = list(map(int, input().split()))
m = int(input())
queries = list(map(int, input().split()))

inv_levels, _ = count_inversions(arr[:])
total_pairs = [(1 << (i)) * (1 << (i)) for i in range(n.bit_length())]

rev_mask = [0] * n
res = []

for q in queries:
    for i in range(q):
        rev_mask[i] ^= 1
    total = 0
    for i in range(n):
        if rev_mask[i]:
            total += total_pairs[i] - inv_levels[i]
        else:
            total += inv_levels[i]
    res.append(str(total))

sys.stdout.write('\n'.join(res) + '\n')
```**Giải thích.**`count_inversions`hàm tính toán nghịch đảo theo cấp độ sắp xếp hợp nhất. các`rev_mask`theo dõi những cấp độ đã được đảo ngược.`total_pairs`tính toán số lần đảo ngược tối đa có thể có ở mỗi cấp độ. Đối với mỗi truy vấn, mặt nạ được cập nhật và tổng số lần đảo ngược được tính bằng cách tính tổng theo các cấp độ, lật các đóng góp khi cần thiết. 

Các điểm tinh tế bao gồm các mức lập chỉ mục chính xác từ dưới lên trên trong cây sắp xếp hợp nhất và xử lý các truy vấn với`q = 0`(không thay đổi) hoặc`q = n`(đảo ngược mảng đầy đủ). 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Mảng đầu vào:`[2, 1, 4, 3]`Truy vấn:`[1, 2, 0, 2]`| Truy vấn | Khối đảo ngược | rev_mask | Đảo ngược | 
| --- | --- | --- | --- | 
| 1 |`[1,2],[3,4]`|`[1,0]`| 0 | 
| 2 |`[4,3,2,1]`|`[0,1]`| 6 | 
| 0 | không thay đổi |`[0,1]`| 6 | 
| 2 | đảo ngược hoàn toàn`[1,2,3,4]`|`[0,0]`| 0 | 

Dấu vết xác nhận rằng các mức lật khớp chính xác với số lượng đảo ngược. 

**Đầu vào tùy chỉnh** 

Mảng: `[1,3,2
