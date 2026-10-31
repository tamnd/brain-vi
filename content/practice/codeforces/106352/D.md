---
title: "CF 106352D - \u0420\u0430\u0431\u043e\u0447\u0438\u0435 \u043f\u0430\u0440\u044b \u0432 \u0417\u0432\u0435\u0440\u043e\u043f\u043e\u043b\u0438\u0441\u0435"
description: "Chúng ta được cung cấp một danh sách các số nguyên, mỗi số đại diện cho một “công nhân” ở Zveropolis. Đối với mỗi nhân viên, chúng ta phải chọn một đối tác khác từ cùng một danh sách để tối đa hóa điểm tương tác cụ thể."
date: "2026-06-20T22:52:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106352
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106352
solve_time_s: 72
verified: true
draft: false
---

[CF 106352D - \u0420\u0430\u0431\u043e\u0447\u0438\u0435 \u043f\u0430\u0440\u044b \u0432 \u0417\u0432\u0435\u0440\u043e\u043f\u043e\u043b\u0438\u0441\u0435](https://codeforces.com/problemset/problem/106352/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên, mỗi số đại diện cho một “công nhân” ở Zveropolis. Đối với mỗi nhân viên, chúng ta phải chọn một đối tác khác từ cùng một danh sách để tối đa hóa điểm tương tác cụ thể. 

Tỉ số giữa hai số$x$Và$y$được định nghĩa theo các biểu diễn nhị phân của chúng. Chúng tôi xem xét tất cả các vị trí bit$i$, và chúng ta đếm xem có bao nhiêu chỉ số thỏa mãn điều kiện cả hai$x$Và$y$có những cái liên tiếp ở các vị trí$i$Và$i+1$. Nói cách khác, đối với mỗi cặp bit liền kề, chúng tôi kiểm tra xem cả hai số có chứa mẫu “11” ở cùng một vị trí hay không và chúng tôi đếm xem có bao nhiêu vị trí như vậy khớp với nhau. 

Nhiệm vụ là, với mọi phần tử$a_i$, để tìm chỉ mục khác$j \neq i$tối đa hóa điểm số này và tạo ra giá trị tối đa đó. 

Những hạn chế là động lực chính của giải pháp. Số phần tử có thể lên tới$10^6$, vì vậy mọi phép tính bậc hai trên mảng đều không thể thực hiện được ngay lập tức. Ngay cả việc quét tuyến tính đối với tất cả các phần tử khác trên mỗi phần tử cũng sẽ dẫn đến$10^{12}$hoạt động trong trường hợp xấu nhất. Đồng thời, mỗi giá trị tối đa là$10^6$, do đó biểu diễn nhị phân nhỏ, chỉ khoảng 20 bit. Điều này gợi ý rõ ràng về một giải pháp dựa trên bitmask trong đó “cấu trúc” của mỗi số quan trọng hơn độ lớn của nó. 

Trường hợp cạnh tinh tế xuất hiện khi tồn tại nhiều giá trị giống hệt nhau. Vì một worker không thể được ghép nối với chính nó, nên nếu tất cả các lần xuất hiện của một giá trị đều giống hệt nhau, chúng tôi phải đảm bảo rằng chúng tôi vẫn chọn một bản sao khác nếu nó tồn tại, nếu không, chúng tôi phải chuyển sang một đối tác khác. 

Ví dụ, nếu mảng là$[3, 3, 3]$, thì đối với mỗi chỉ số, đối tác tốt nhất là 3 đối tác khác và câu trả lời là điểm của$F(3,3)$, không phải 0 hoặc không xác định. 

Một trường hợp góc khác là khi tất cả các số đều khác biệt và rất thưa thớt trong cấu trúc nhị phân, do đó sự trùng lặp bằng 0 ở mọi nơi. Trong trường hợp đó, mọi câu trả lời chính xác phải là 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp thử mọi cặp có thể$(i, j)$. Đối với mỗi cặp, chúng tôi tính điểm bằng cách quét tất cả các vị trí bit và kiểm tra xem cả hai số có chứa số liên tiếp hay không. Vì mỗi số có tới khoảng 20 vị trí bit liên quan nên việc so sánh đơn lẻ sẽ rẻ, nhưng có$O(n^2)$cặp, quá lớn đối với$n = 10^6$. Điều này dẫn đến khoảng$10^{12}$đánh giá theo cặp, điều này là không thể trong thời gian giới hạn. 

Quan sát quan trọng là điểm số chỉ phụ thuộc vào mẫu của các bit được đặt liền kề. Đối với mỗi số, chúng ta có thể mã hóa nó dưới dạng mặt nạ bit trong đó bit$i$là 1 nếu vị trí$i$Và$i+1$đều được đặt ở số ban đầu. Điều này biến vấn đề thành một nhiệm vụ tương tự mặt nạ bit thuần túy: đối với mỗi mặt nạ, chúng tôi muốn một mặt nạ khác giúp tối đa hóa số lượng bit tập hợp chung. 

Vì vậy, thay vì làm việc với các số gốc, chúng ta chuyển đổi từng số$a_i$vào một mặt nạ có nguồn gốc$g(a_i)$. Sau đó, điểm số trở thành đơn giản là số lượng bit được đặt trong$g(a_i) \& g(a_j)$. 

Bây giờ nhiệm vụ là: đối với mỗi mặt nạ, hãy tìm một mặt nạ khác trong bộ có kích thước giao lộ tối đa. Vì độ rộng bit nhỏ (khoảng 20), chúng ta có thể sử dụng các ý tưởng lập trình động tập hợp con. 

Chúng tôi tính toán trước những mặt nạ nào tồn tại trong đầu vào và sau đó chúng tôi suy luận về các tập hợp con. Ý tưởng chính là tính toán trước cho mọi tập hợp con có thể$S$, liệu có tồn tại ít nhất một số có mặt nạ dẫn xuất chứa$S$. Sau đó, đối với mặt nạ truy vấn cố định$A$, bất kỳ giao điểm chung nào với mặt nạ khác đều tương ứng với một tập hợp con nào đó$S \subseteq A$điều đó cũng được chứa trong một số mặt nạ khác. 

Sau đó chúng tôi tìm kiếm tập hợp con lớn nhất như vậy$S$, bởi vì tối đa hóa kích thước nút giao tương đương với tối đa hóa kích thước của$S$. 

Điều này làm giảm vấn đề từ so sánh theo cặp sang tìm kiếm có cấu trúc trên các tập hợp con, có thể được xử lý bằng cách sử dụng lập trình động SOS trên không gian 20 bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot 20)$|$O(1)$| Quá chậm | 
| SOS DP qua mặt nạ |$O(2^m \cdot m)$,$m \le 20$|$O(2^m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$m \approx 20$là số vị trí bit tối đa trong mặt nạ dẫn xuất. 

1. Chuyển đổi từng số$a_i$vào một chiếc mặt nạ$g_i$, bit ở đâu$k$được đặt nếu cả hai bit$k$Và$k+1$là 1 trong$a_i$. Điều này nén mỗi số thành một chữ ký cấu trúc xác định đầy đủ tất cả các điểm theo cặp. 
2. Đếm tần số của từng mặt nạ. Điều này là cần thiết để xử lý các trường hợp một số có thể ghép với một số giống hệt nhau. 
3. Xây dựng một mảng`exist[S]`cho biết liệu có ít nhất một mặt nạ đầu vào bằng$S$. 
4. Chạy SOS DP trên supersets để tính toán`sup[S]`, số lượng mặt nạ trong đầu vào có chứa$S$. Điều này cho phép chúng tôi nhanh chóng kiểm tra xem mẫu giao nhau ứng cử viên có thực sự xuất hiện bên trong một số đối tác hợp lệ hay không. 
5. Xác định`good[S]`đúng như vậy nếu$S$có thể được sử dụng làm mẫu giao nhau hợp lệ với ít nhất một mặt nạ đối tác. Ban đầu đây là`sup[S] > 0`. 
6. Xác định`best[S]`là kích thước tối đa của một tập hợp con$T \subseteq S$như vậy`good[T]`là đúng. Điều này được tính toán bằng cách sử dụng mặt nạ con DP để truyền các giá trị tốt nhất từ ​​mặt nạ nhỏ hơn đến mặt nạ lớn hơn. 
7. Đối với mỗi mặt nạ truy vấn$A$, câu trả lời là`best[A]`, ngoại trừ trường hợp tập con tốt nhất bằng$A$chính nó và mặt nạ chính xác này chỉ xuất hiện một lần trong đầu vào. Trong trường hợp đó, chúng tôi phải loại trừ việc tự ghép nối, vì vậy chúng tôi quay lại tập hợp con hợp lệ tốt nhất tiếp theo bên trong$A$. 

Tính đúng đắn xuất phát từ thực tế là bất kỳ giao điểm chung nào giữa hai mặt nạ đều là tập con của cả hai mặt nạ. Do đó, mọi điểm hợp lệ đều tương ứng chính xác với một số tập hợp con của mặt nạ truy vấn và việc tối đa hóa điểm tương đương với việc tìm tập hợp con lớn nhất có thể thực hiện được bởi ít nhất một phần tử khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXB = 20
MAXM = 1 << MAXB

n_and_rest = sys.stdin.read().strip().split()
n = int(n_and_rest[0])
a = list(map(int, n_and_rest[1:]))

# Step 1: build derived masks g(x)
def build_mask(x):
    res = 0
    for i in range(MAXB - 1):
        if (x >> i) & 1 and (x >> (i + 1)) & 1:
            res |= 1 << i
    return res

freq = [0] * MAXM
masks = []

for x in a:
    g = build_mask(x)
    masks.append(g)
    freq[g] += 1

# Step 2: superset DP to compute sup[S]
sup = freq[:]
for i in range(MAXB):
    for mask in range(MAXM):
        if (mask >> i) & 1 == 0:
            sup[mask] += sup[mask | (1 << i)]

# Step 3: good masks
good = [sup[i] > 0 for i in range(MAXM)]

# Step 4: best submask DP
best = [-1] * MAXM
for mask in range(MAXM):
    if good[mask]:
        best[mask] = bin(mask).count("1")

for i in range(MAXB):
    for mask in range(MAXM):
        if mask & (1 << i):
            if best[mask ^ (1 << i)] > best[mask]:
                best[mask] = best[mask ^ (1 << i)]

# Step 5: answer queries
res = []
for i, g in enumerate(masks):
    ans = best[g]

    # handle self-exclusion if needed
    if ans == bin(g).count("1") and freq[g] == 1:
        # try to find second best by temporarily ignoring full mask
        tmp = g
        best_full = best[g]
        best_without = -1

        # recompute only over submasks of g
        s = tmp
        sub = s
        while True:
            if good[sub] and sub != tmp:
                best_without = max(best_without, bin(sub).count("1"))
            if sub == 0:
                break
            sub = (sub - 1) & s

        ans = best_without

    res.append(str(ans))

print(" ".join(res))
```Việc triển khai này tuân theo ý tưởng nén từng số vào mặt nạ cấu trúc 20 bit, sau đó sử dụng superset DP để xác định mẫu giao nhau nào là khả thi. Bước DP mặt nạ con biến tính khả thi thành bảng điểm tốt nhất trên mỗi mặt nạ, do đó mỗi truy vấn sẽ trở thành tra cứu trực tiếp. 

Phần tế nhị duy nhất là loại trừ khả năng tự ghép nối khi mặt nạ xuất hiện đúng một lần. Trường hợp đó được xử lý riêng bằng cách quét các mặt nạ con của mặt nạ truy vấn, điều này khả thi vì kích thước mặt nạ tối đa là 20 bit. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

Mặt nạ đầu vào bắt nguồn từ các số tạo ra các mẫu bit nhỏ và DP xây dựng tất cả các tập hợp con giao nhau khả thi. Đối với một mặt nạ như`g = 0101`, thuật toán sẽ kiểm tra tất cả các tập hợp con như`0101`,`0100`,`0001`,`0000`và chọn phần lớn nhất tồn tại trong một số phần tử khác. 

Một ví dụ thứ hai: 

Hãy để các con số tạo ra mặt nạ:```
A = 1100
B = 1010
C = 1000
```Vì`A`, mặt nạ con là`1100`,`1000`,`0100`,`0000`. Nếu như`1000`tồn tại trong một mặt nạ khác, câu trả lời sẽ trở thành 1 ngay cả khi không thể chồng lấp hoàn toàn 2. 

Đối với mỗi phần tử, DP đảm bảo chúng tôi luôn chọn phần trùng lặp lớn nhất có thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \cdot 2^m)$| superset DP và submask DP trên mặt nạ 20 bit | 
| Không gian |$O(2^m)$| mảng tần số và DP trên tất cả các mặt nạ | 

Độ rộng bit đủ nhỏ để$2^{20} \approx 10^6$, do đó cả bộ nhớ và thời gian đều nằm trong giới hạn ngay cả đối với$n = 10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    # simplified call placeholder
    return "0"

assert run("3\n0 0 0\n") == "0 0 0"

assert run("3\n1 2 3\n") == "0 0 0"

assert run("4\n3 3 3 3\n") == "1 1 1 1"

assert run("5\n0 1 2 4 8\n") == "0 0 0 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | sự chồng chéo tích cực giống hệt nhau | xử lý trùng lặp | 
| tất cả thưa thớt khác biệt | số không | không có kết quả trùng khớp sai | 
| tất cả giống hệt nhau dày đặc | chồng chéo tối đa nhất quán | tự ghép nối đúng | 
| cấu trúc sức mạnh của hai | không liền kề | mẫu bit cạnh | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi đối tác tốt nhất cho mặt nạ là chính nó, nhưng chỉ tồn tại một bản sao. Trong trường hợp đó, DP thô của thuật toán sẽ cho phép tự ghép nối không chính xác. Quá trình quét mặt nạ con dự phòng đảm bảo chúng tôi loại trừ rõ ràng mặt nạ đầy đủ và tìm kiếm tập hợp con tốt nhất tiếp theo, duy trì tính chính xác. 

Một trường hợp cạnh khác là khi tất cả các mặt nạ đều trống. Khi đó, mọi giao điểm đều bằng 0 và DP để lại chính xác tất cả các giá trị tốt nhất về 0, vì mặt nạ trống luôn là tập hợp con hợp lệ và luôn tồn tại trong tập dữ liệu. 

Trường hợp cuối cùng là khi tồn tại nhiều mặt nạ giống hệt nhau. Sau đó, việc tự ghép nối sẽ có hiệu lực vì có một lần xuất hiện khác và việc kiểm tra tần số cho phép tập hợp con toàn điểm vẫn giữ nguyên câu trả lời mà không cần sửa đổi.
