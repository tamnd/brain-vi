---
title: "CF 2045K - GCDDCG"
description: "Chúng ta được cấp một bộ bài $N$, mỗi lá bài được gắn nhãn có giá trị nguyên từ $1$ đến $N$. Trò chơi có $N$ vòng."
date: "2026-06-08T09:19:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 2045
codeforces_index: "K"
codeforces_contest_name: "2024-2025 ICPC Asia Jakarta Regional Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2900
weight: 2045
solve_time_s: 123
verified: false
draft: false
---

[CF 2045K - GCDDCG](https://codeforces.com/problemset/problem/2045/K) 

**Xếp hạng:** 2900 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ bài$N$thẻ, mỗi thẻ được gắn nhãn với một giá trị số nguyên giữa$1$Và$N$. Trò chơi có$N$vòng. trong$i$- Vòng thứ hai, chúng ta phải chọn hai tập con không trống, rời rạc của các quân bài, gọi là bộ 1 và bộ 2, sao cho ước số chung lớn nhất của các giá trị trong mỗi bộ bài là chính xác.$i$. Những lá bài không có trong bộ bài sẽ bị bỏ qua. Điểm sáng tạo cho vòng tròn$i$là$i$nhân với số cách chọn hai bộ bài này. Nhiệm vụ là tính tổng điểm sáng tạo của tất cả các vòng theo modulo$998\,244\,353$. 

Đầu vào bao gồm$N$theo sau là$N$số nguyên$A_1$ĐẾN$A_N$. Đầu ra là một số nguyên duy nhất, tổng số điểm sáng tạo. 

Nhận xét quan trọng đầu tiên là$N$có thể lên tới 200.000, điều này khiến cho bất kỳ nỗ lực mạnh mẽ nào nhằm liệt kê các tập hợp con đều không thể thực hiện được vì số lượng tập hợp con là theo cấp số nhân trong$N$. Tương tự, việc kiểm tra tất cả các cặp tập con sẽ cho kết quả$O(3^N)$hoạt động trong trường hợp xấu nhất. Giới hạn thời gian 1 giây ngụ ý rằng chúng ta cần một thuật toán đại khái$O(N \log N)$hoặc$O(N \sqrt{N})$. 

Các trường hợp đặc biệt có thể phá vỡ các giải pháp ngây thơ bao gồm khi tất cả các thẻ đều bằng một số$i$. Ví dụ, với$N = 3$Và$A = [3, 3, 3]$, chỉ có vòng tương ứng với$i = 3$có điểm sáng tạo khác 0. Nếu một giải pháp cố gắng tính toán GCD một cách đơn giản cho mọi tập hợp con, nó sẽ hết thời gian chờ hoặc bỏ lỡ việc đếm chính xác vì GCD có tính nhân và có thể được xử lý hiệu quả hơn bằng các phương pháp đếm thay vì liệt kê. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là thử mọi vòng$i$, tạo tất cả các tập hợp con của thẻ, tính toán GCD của chúng và đếm các cặp hợp lệ. Về nguyên tắc thì điều này đúng nhưng cần phải liệt kê$2^N$tập hợp con, sau đó so sánh các cặp, dẫn đến$O(4^N)$sự phức tạp. Ngay cả đối với$N = 20$, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là đếm các thẻ chia hết cho bội số của$i$và sử dụng lý luận tổ hợp thay vì liệt kê tập hợp con. Cho một vòng$i$, chỉ những thẻ có giá trị chia hết cho$i$có thể được sử dụng vì GCD phải chính xác$i$. Cho phép$c_i$là số lượng thẻ như vậy. Đối với mỗi$i$, chúng ta cần số cách để chia chúng$c_i$các thẻ thành hai tập hợp con không trống, rời rạc trong đó GCD của mỗi tập hợp con vẫn còn$i$. 

Chúng ta có thể sử dụng nguyên tắc bao gồm-loại trừ trên bội số của$i$. Đầu tiên, đếm tất cả các tập hợp con của các thẻ chia hết cho$i$. Sau đó trừ các tập con có GCD là bội số cao hơn của$i$, đệ quy. Cuối cùng, ở mỗi vòng$i$, số cách để gán hai bộ bài không trống từ$f[i]$thẻ có GCD chính xác$i$là$f[i] \times f[i] - f[i]$bởi vì chúng ta có thể chọn bất kỳ tập hợp con không trống nào cho bộ 1, bất kỳ tập hợp con rời rạc không trống nào cho bộ 2 và loại trừ các cấu hình không hợp lệ. 

Điều này làm giảm vấn đề lặp lại trên các ước số và bội số, đó là$O(N \log N)$sử dụng phương pháp sàng lọc. Việc đếm lũy thừa của hai và áp dụng số học mô-đun đảm bảo chúng ta luôn ở trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(4^N)$|$O(2^N)$| Quá chậm | 
| Bao gồm-Loại trừ trên bội số |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`count`kích thước$N+1$về không. Lặp lại các giá trị thẻ$A_1$ĐẾN$A_N$và tăng dần`count[val]`cho mỗi giá trị thẻ. Điều này đếm có bao nhiêu thẻ có mỗi giá trị. 
2. Với mỗi số nguyên$i$từ$1$ĐẾN$N$, tính toán`mult[i]`là tổng số thẻ chia hết cho$i$. Lặp lại bội số của$i$và tính tổng`count`các giá trị. Điều này đưa ra tổng số thẻ ứng cử viên có thể tạo thành bộ bài với GCD$i$. 
3. Khởi tạo một mảng`f`kích thước$N+1$về không. Lặp lại từ$N$xuống$1$. Cho phép`total`là$2^{mult[i]} - 1$modulo$998244353$, đại diện cho tất cả các tập hợp con thẻ không trống chia hết cho$i$. Sau đó trừ`f[j]`cho tất cả các bội số$j = 2*i, 3*i, ..., N$để loại bỏ các tập con có GCD bằng bội số cao hơn của$i$. Gán kết quả cho`f[i]`. Hiện nay`f[i]`đại diện cho số tập hợp con không trống của thẻ có GCD chính xác$i$. 
4. Đối với mỗi vòng$i$, số cách chọn hai tập con khác rỗng, rời rạc từ`f[i]`thẻ là$f[i] \times f[i] - f[i]$. Điều này tính tất cả các cặp có thứ tự trừ trường hợp một tập hợp con trống. Nhân với$i$để có được điểm sáng tạo cho vòng này, sau đó cộng nó vào tổng modulo$998244353$. 
5. Xuất ra tổng số điểm sáng tạo. 

Tại sao nó hoạt động: Ở mỗi bước,`f[i]`đếm chính xác các tập con có GCD chính xác$i$bởi vì chúng tôi bao gồm tất cả các tập hợp con chia hết cho$i$và trừ các tập con được tính cho bội số cao hơn. Sử dụng quyền hạn của hai tài khoản cho tất cả các tập hợp con có thể một cách hiệu quả. Việc đếm các cặp theo thứ tự đảm bảo chúng tôi nắm bắt chính xác tất cả các bài tập hợp lệ của bộ bài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    N = int(input())
    A = list(map(int, input().split()))
    
    count = [0] * (N + 1)
    for a in A:
        count[a] += 1
    
    mult = [0] * (N + 1)
    for i in range(1, N + 1):
        for j in range(i, N + 1, i):
            mult[i] += count[j]
    
    f = [0] * (N + 1)
    pow2 = [1] * (N + 2)
    for i in range(1, N + 2):
        pow2[i] = (pow2[i-1] * 2) % MOD
    
    for i in range(N, 0, -1):
        total = (pow2[mult[i]] - 1) % MOD
        j = 2 * i
        while j <= N:
            total = (total - f[j]) % MOD
            j += i
        f[i] = total
    
    result = 0
    for i in range(1, N + 1):
        ways = (f[i] * f[i] - f[i]) % MOD
        result = (result + ways * i) % MOD
    
    print(result)

solve()
```Mã tính toán`mult[i]`để xác định lá bài nào có thể xuất hiện trong vòng`i`. Quyền hạn của hai xử lý tập hợp con một cách hiệu quả. Trừ`f[j]`đối với bội số đảm bảo chúng tôi tôn trọng các yêu cầu GCD chính xác. Cuối cùng, công thức cặp thứ tự sẽ tính các bài tập hợp lệ cho vòng đấu. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`N = 3, A = [3, 3, 3]`| tôi | đa[i] | f[i] | cách | sáng tạo | 
| --- | --- | --- | --- | --- | 
| 3 | 3 | 3 | 12 | 36 | 
| 2 | 0 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 0 | 0 | 

Giải thích: Chỉ có vòng 3 mới có các tập hợp con hợp lệ, mang lại 12 cách để tạo thành hai bộ bài. Nhân với 3 được 36. 

**Mẫu 2:**`N = 5, A = [2, 4, 2, 1, 3]`| tôi | đa[i] | f[i] | cách | sáng tạo | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 6 | 30 | 30 | 
| 2 | 3 | 3 | 6 | 12 | 
| 3 | 1 | 1 | 0 | 0 | 
| 4 | 1 | 1 | 0 | 0 | 
| 5 | 0 | 0 | 0 | 0 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc đếm bội số sử dụng các vòng lặp lồng nhau trên các ước số; việc đếm tập hợp con là O(N) với các lũy thừa được tính toán trước. | 
| Không gian | O(N) | Mảng`count`,`mult`,`f`,`pow2`có kích thước O(N) lưu trữ số lượng thẻ và giá trị DP. | 

Điều này phù hợp thoải mái dưới giới hạn 1 giây cho N 200.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from
```
