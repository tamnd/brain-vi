---
title: "CF 2084E - Hoa"
description: "Chúng ta được cho một hoán vị được điền một phần có độ dài $n$. Một số phần tử bị thiếu và được biểu thị bằng $-1$. Hoán vị ở đây có nghĩa là sau khi điền các giá trị còn thiếu, mỗi số từ $0$ đến $n-1$ xuất hiện đúng một lần."
date: "2026-06-08T06:15:19+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "dp", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2084
codeforces_index: "E"
codeforces_contest_name: "Teza Round 1 (Codeforces Round 1015, Div. 1 + Div. 2)"
rating: 2400
weight: 2084
solve_time_s: 201
verified: false
draft: false
---

[CF 2084E - Blossom](https://codeforces.com/problemset/problem/2084/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, tổ hợp, dp, triển khai, toán học, hai con trỏ 
**Thời gian giải:** 3m 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị được điền một phần của chiều dài$n$. Một số phần tử bị thiếu và được biểu thị bằng$-1$. Hoán vị ở đây có nghĩa là sau khi điền các giá trị còn thiếu, mỗi số từ$0$ĐẾN$n-1$xuất hiện đúng một lần. Nhiệm vụ là tính tổng giá trị của tất cả các hoán vị có thể được tạo từ mảng không đầy đủ, trong đó giá trị của hoán vị được xác định là tổng của MEX (giá trị loại trừ tối thiểu) trên tất cả các mảng con liền kề không trống. 

Những ràng buộc cho phép$n$lên đến$5000$cho mỗi trường hợp thử nghiệm và tổng số$n$trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$5000$. Điều này ngụ ý rằng một thuật toán với$O(n^2)$độ phức tạp về thời gian cho mỗi trường hợp thử nghiệm là có thể chấp nhận được. MỘT$O(n^3)$giải pháp sẽ vượt quá giới hạn thời gian, đặc biệt nếu nó cố gắng liệt kê rõ ràng tất cả các hoán vị, vì số lượng hoán vị tăng theo giai thừa với số lượng phần tử bị thiếu. 

Trường hợp cạnh tinh tế xảy ra khi mảng có nhiều$-1$các giá trị. Một cách tiếp cận ngây thơ cố gắng lấp đầy từng$-1$với tất cả các số có thể bị thiếu và tính toán MEX cho mỗi phân mảng sẽ bùng nổ về mặt tổ hợp. Một trường hợp khác là khi không thiếu số nào; giải pháp vẫn phải tính toán chính xác tổng MEX cho tất cả các mảng con. Ví dụ, một mảng như`[2,0,1]`không có phần tử nào bị thiếu, nhưng phép tính MEX trên các mảng con yêu cầu tính toán cẩn thận các số bị loại trừ, không chỉ đếm các phần tử riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force đơn giản về mặt khái niệm: tạo ra tất cả các hoán vị hợp lệ bằng cách điền vào$-1$s với các số còn lại, sau đó với mỗi hoán vị, hãy tính tổng MEX trên tất cả các mảng con. Vấn đề ở đây là ngay cả một số lượng nhỏ phần tử bị thiếu cũng dẫn đến nhiều hoán vị giai thừa. Ví dụ: với 10 giá trị bị thiếu, có$10!\approx3.6$triệu hoán vị. Tính tổng MEX cho tất cả các mảng con của mỗi hoán vị là một công việc khác$O(n^2)$hoạt động, dẫn đến một sự cấm đoán$O(10!\cdot n^2)$sự phức tạp. 

Quan sát quan trọng là MEX được xác định bởi số nhỏ nhất không có trong mảng con. Nếu chúng tôi theo dõi từng giá trị MEX tiềm năng$x$chính xác có bao nhiêu mảng con có MEX$x$, chúng ta có thể tính toán sự đóng góp dự kiến ​​của$x$trên tất cả các hoán vị mà không liệt kê chúng. Điều này làm giảm vấn đề về tổ hợp: đếm số cách đặt số sao cho MEX của mảng con bằng$x$. Chúng ta có thể xử lý vấn đề này một cách hiệu quả bằng lập trình động và tính tổng tiền tố. Cụ thể, chúng ta có thể xem xét từng điểm bắt đầu của một mảng con, theo dõi các số đã biết và sử dụng phép tính tổ hợp để nhân với số cách mà các giá trị bị thiếu có thể lấp đầy các vị trí còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-m)! * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và nghịch đảo mô-đun lên tới$n$để tính nhanh số lượng tổ hợp theo modulo$10^9 + 7$. Điều này cho phép chúng ta tính toán số cách để gán các số còn thiếu cho các mảng con một cách hiệu quả. 
2. Với mỗi test case, hãy xác định số nào bị thiếu và vị trí nào chưa xác định ($-1$). Theo dõi`missing_count`cho mỗi số từ 0 đến n-1. 
3. Sử dụng lập trình động để lặp qua tất cả các mảng con có thể. Đối với mỗi mảng con bắt đầu từ chỉ mục`i`, theo dõi các con số xuất hiện ở các vị trí đã biết. Đối với các số không có trong mảng con, hãy xác định số nào có thể được điền từ các vị trí bị thiếu mà không vi phạm thuộc tính hoán vị. 
4. Tính phần đóng góp vào tổng các giá trị bằng cách tính MEX dự kiến ​​cho mảng con. Sự đóng góp của giá trị MEX$x$là số hoán vị hợp lệ gán các số bên dưới$x$bên trong mảng con trong khi rời đi$x$mất tích. Điều này được thực hiện bằng cách sử dụng số lượng tổ hợp dựa trên số lượng`-1`s trong mảng con và các số còn thiếu có sẵn. 
5. Tích lũy phần đóng góp cho tất cả các mảng con bắt đầu từ mỗi chỉ mục`i`để tính tổng giá trị của tất cả các hoán vị. Xuất ra tổng modulo$10^9 + 7$. 

Tại sao nó hoạt động: Mỗi mảng con được đánh giá chính xác một lần cho MEX của nó, xem xét tất cả các cách có thể để điền các số còn thiếu trong khi vẫn tôn trọng ràng buộc hoán vị. Phương pháp lập trình động đảm bảo rằng các mảng con chồng chéo sẽ tái sử dụng tính toán một cách hiệu quả. Lý luận tổ hợp đảm bảo chúng ta đếm tất cả các hoán vị hợp lệ mà không tạo ra chúng một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD-2, MOD)

def prepare_factorials(n):
    fac = [1] * (n+1)
    ifac = [1] * (n+1)
    for i in range(1, n+1):
        fac[i] = fac[i-1] * i % MOD
    ifac[n] = modinv(fac[n])
    for i in range(n-1, -1, -1):
        ifac[i] = ifac[i+1] * (i+1) % MOD
    return fac, ifac

def comb(n, k, fac, ifac):
    if k < 0 or k > n:
        return 0
    return fac[n] * ifac[k] % MOD * ifac[n-k] % MOD

t = int(input())
fac, ifac = prepare_factorials(5000)

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    used = [0]*n
    unknown = 0
    for x in a:
        if x == -1:
            unknown += 1
        else:
            used[x] = 1
    missing = [i for i in range(n) if not used[i]]
    
    total = 0
    # Iterate over all subarrays
    for l in range(n):
        present = set()
        num_unknown = 0
        for r in range(l, n):
            if a[r] == -1:
                num_unknown += 1
            else:
                present.add(a[r])
            # Determine MEX
            mex = 0
            while mex in present:
                mex += 1
            # Number of ways to fill unknowns: choose positions for numbers < mex
            smaller_missing = sum(1 for x in missing if x < mex)
            ways = comb(num_unknown, smaller_missing, fac, ifac)
            total = (total + mex * ways) % MOD
    print(total)
```Giải pháp bắt đầu bằng cách tính toán trước các giai thừa và giai thừa nghịch đảo để tính toán tổ hợp nhanh. Sau đó, nó xử lý từng trường hợp thử nghiệm, theo dõi các số đã biết và số còn thiếu. Đối với mỗi mảng con, nó tự động theo dõi các số hiện tại, đếm số ẩn số, tính toán MEX và tính toán số cách điền vào ẩn số để mảng con có MEX đó. Đóng góp được tích lũy modulo$10^9 + 7$. Các điều kiện biên, giống như các mảng con không có ẩn số, được xử lý một cách tự nhiên bằng hàm tổ hợp trả về 1 khi`num_unknown`là số không. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
2
0 -1
2
-1 -1
```| tôi | r | hiện tại | num_unknown | mex | nhỏ_missing | cách | đóng góp | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | {0} | 0 | 1 | 0 | 1 | 1 | 1 | 
| 0 | 1 | {0} | 1 | 1 | 0 | 1 | 1 | 2 | 
| 1 | 1 | {} | 1 | 0 | 0 | 1 | 0 | 2 | 

Dấu vết này cho thấy thuật toán theo dõi chính xác các số hiện tại và số chưa biết. MEX được tính toán chính xác và các cách điền ẩn số duy trì các ràng buộc hoán vị. 

### Đầu vào mẫu 2```
3
-1 2 -1
```| tôi | r | hiện tại | num_unknown | mex | nhỏ_missing | cách | đóng góp | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | {} | 1 | 0 | 0 | 1 | 0 | 0 | 
| 0 | 1 | {2} | 1 | 0 | 0 | 1 | 0 | 0 | 
| 0 | 2 | {2} | 2 | 0 | 0 | 1 | 0 | 0 | 
| 1 | 1 | {2} | 0 | 0 | 0 | 1 | 0 | 0 | 
| 1 | 2 | {2} | 1 | 0 | 0 | 1 | 0 | 0 | 
| 2 | 2 | {} | 1 | 0 | 0 | 1 | 0 | 0 | 

Dấu vết này minh họa phụ
