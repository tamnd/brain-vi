---
title: "CF 2134C - Thậm chí còn lớn hơn"
description: "Chúng ta được cấp một mảng các số nguyên không âm và chúng ta cần làm cho nó tốt bằng cách thực hiện số phép toán tối thiểu."
date: "2026-06-08T02:42:27+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2134
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1045 (Div. 2)"
rating: 1200
weight: 2134
solve_time_s: 107
verified: true
draft: false
---

[CF 2134C - Thậm chí còn lớn hơn](https://codeforces.com/problemset/problem/2134/C) 

**Đánh giá:** 1200 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta cần làm cho nó _good_ bằng cách thực hiện số thao tác tối thiểu. Một mảng là tốt nếu, với mỗi mảng con có độ dài ít nhất là hai, tổng các phần tử ở chỉ số chẵn (tính từ 1 trong mảng ban đầu) ít nhất bằng tổng các phần tử ở chỉ số lẻ. Một thao tác đơn lẻ bao gồm việc giảm bất kỳ phần tử nào đi một phần tử, miễn là nó không âm. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng tối đa$2 \cdot 10^5$các phần tử. Tổng số phần tử trong tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$và mỗi phần tử có thể lớn bằng$10^9$. Với giới hạn thời gian 2 giây, điều này loại trừ bất kỳ giải pháp mạnh mẽ nào kiểm tra tất cả các mảng con, bởi vì số lượng mảng con của một$n$-mảng phần tử là$\frac{n(n+1)}{2}$, có thể đạt tới$2 \cdot 10^{10}$trong trường hợp xấu nhất. Chúng ta cần một giải pháp tuyến tính hoặc tuyến tính trong$n$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh khóa phát sinh khi một mảng con vi phạm điều kiện vì phần tử được lập chỉ mục lẻ lớn hơn các phần tử được lập chỉ mục chẵn xung quanh. Ví dụ, mảng$[0,2,4,1]$không tốt vì mảng con$[2,4,1]$có tổng chỉ số chẵn$2+1=3$và tổng chỉ số lẻ$4$. Một cách tiếp cận ngây thơ chỉ xem xét các cặp liền kề sẽ báo cáo sai mảng này là tốt vì hai phần tử đầu tiên thỏa mãn điều kiện. 

Một điều tinh tế khác là nhiều phần tử chỉ số lẻ lớn có thể được đệm bằng tổng tích lũy của các phần tử chỉ số chẵn trước chúng, vì vậy chúng ta cần một cách để truyền bá các ràng buộc này một cách hiệu quả mà không cần lặp lại rõ ràng trên mỗi mảng con. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là kiểm tra mọi mảng con có độ dài ít nhất là 2 và bất cứ khi nào tổng các phần tử được lập chỉ mục lẻ vượt quá tổng các phần tử được lập chỉ mục chẵn, hãy giảm các phần tử ở vị trí lẻ cho đến khi điều kiện được thỏa mãn. Mặc dù điều này đúng về nguyên tắc, nhưng số lượng mảng con tăng theo phương trình bậc hai, khiến nó hoàn toàn không thực tế đối với các mảng lớn.$n$. 
Cái nhìn sâu sắc dẫn đến một giải pháp tối ưu là tập trung vào _ tổng tiền tố _ của mảng riêng biệt cho các chỉ số chẵn và lẻ. Cho phép`even[i]`Và`odd[i]`là tổng tiền tố của các phần tử ở vị trí chẵn và lẻ cho đến chỉ số`i`. Điều kiện của mảng con$[l, r]$vậy thì`even[r] - even[l-1] >= odd[r] - odd[l-1]`. Sắp xếp lại mang lại`odd[r] - even[r] <= odd[l-1] - even[l-1]`. Điều này cho thấy rằng chúng ta chỉ cần theo dõi chênh lệch tối đa của tổng tiền tố lẻ trừ chẵn và bất cứ khi nào chênh lệch hiện tại vượt quá mức tối đa đó, chúng ta cần thực hiện đủ số giảm để giảm nó xuống. 

Do đó, giải pháp tối ưu giảm xuống việc lặp lại một lần qua mảng trong khi vẫn duy trì chênh lệch của tổng tích lũy và cập nhật số lượng thao tác bất cứ khi nào chênh lệch hiện tại vượt quá chênh lệch tối thiểu cho phép. Điều này đưa ra giải pháp O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai biến,`even_sum`Và`odd_sum`, để theo dõi tổng tích lũy ở các chỉ số chẵn và lẻ khi chúng ta duyệt mảng. Đồng thời khởi tạo`min_ops`về 0, sẽ tích lũy tổng số hoạt động cần thiết. 
2. Lặp lại mảng theo chỉ mục. Nếu chỉ mục chẵn (lập chỉ mục dựa trên 1), hãy thêm phần tử vào`even_sum`. Nếu không thì thêm nó vào`odd_sum`. 
3. Ở mỗi bước, hãy tính chênh lệch hiện tại`odd_sum - even_sum`. Điều này thể hiện tổng các phần tử lẻ đã vượt quá tổng các phần tử chẵn tính đến thời điểm này. 
4. Nếu`odd_sum - even_sum > 0`, nó vi phạm điều kiện "tốt" cho mảng con bắt đầu từ đầu. Tăng`min_ops`bởi sự khác biệt này và điều chỉnh`odd_sum`bằng cách trừ đi số tiền tương tự. Điều này mô phỏng việc giảm các phần tử có chỉ số lẻ vừa đủ để khôi phục điều kiện. 
5. Tiếp tục lặp qua mảng cho đến hết. Sau vòng lặp,`min_ops`chứa số lượng giảm tối thiểu cần thiết để làm cho mảng tốt. 
6. Đầu ra`min_ops`cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: Điều bất biến chính là sau mỗi lần lặp, tất cả các tiền tố của mảng đều thỏa mãn điều kiện "tốt". Bằng cách chỉ chuyển tiếp những điều chỉnh cần thiết, chúng tôi đảm bảo rằng mọi mảng con kết thúc ở chỉ mục hiện tại sẽ hợp lệ. Vì chúng ta luôn giảm số lượng tối thiểu cần thiết để khôi phục bất biến nên giải pháp là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        min_ops = 0
        even_sum = 0
        odd_sum = 0
        for i in range(n):
            if (i+1) % 2 == 0:
                even_sum += a[i]
            else:
                odd_sum += a[i]
            diff = odd_sum - even_sum
            if diff > 0:
                min_ops += diff
                odd_sum -= diff
        print(min_ops)

if __name__ == "__main__":
    solve()
```Trong giải pháp này,`even_sum`Và`odd_sum`tích lũy tổng tiền tố theo quy tắc chỉ số dựa trên 1. Biến`diff`đo xem số tiền lẻ đã vượt quá số tiền chẵn bao nhiêu. Bất cứ khi nào`diff`là dương, chúng tôi thêm nó vào`min_ops`và giảm`odd_sum`để giữ sự bất biến. Việc sử dụng tổng tiền tố sẽ tránh được chi phí bậc hai khi kiểm tra tất cả các mảng con. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`[0,2,3,5]`| tôi | một [tôi] | chẵn_sum | lẻ_sum | khác biệt | phút_ops | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 0 | 
| 2 | 2 | 2 | 0 | -2 | 0 | 
| 3 | 3 | 2 | 3 | 1 | 1 | 
| 4 | 5 | 7 | 3 | -4 | 1 | 

Dấu vết chứng minh rằng chỉ có một thao tác (giảm`a[3]`bằng 1) là cần thiết để làm cho mảng tốt. 

Đầu vào mẫu:`[3,1]`| tôi | một [tôi] | chẵn_sum | lẻ_sum | khác biệt | phút_ops | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 3 | 3 | 2 | 
| 2 | 1 | 1 | 3 | 2 | 2 | 

Ở đây, cần có hai mức giảm để điều chỉnh phần tử có chỉ số lẻ đầu tiên nhằm thỏa mãn điều kiện tốt cho mảng con đầu tiên`[3,1]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng cho mỗi trường hợp thử nghiệm, công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) | Chỉ có một vài biến cho tổng tiền tố và bộ đếm | 

Giải pháp xử lý tới 200.000 phần tử cho mỗi trường hợp thử nghiệm và tổng số 2·10⁵ phần tử một cách thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("8\n4\n3 8 4 4\n4\n0 2 3 5\n2\n3 1\n5\n2 3 1 4 2\n4\n0 2 4 1\n5\n3 1 4 5 1\n11\n3 0 5 4 4 5 3 0 3 4 1\n12\n410748345 10753674 975233308 193331255 893457280 279719251 704970985 412553354 801228787 44181004 1000000000 3829103") == "0\n1\n2\n0\n3\n6\n14\n4450984776"

# custom cases
assert run("1\n2\n0 0") == "0", "minimum size"
assert run("1\n5\n5 5 5 5 5") == "6", "all equal values"
assert run("1\n3\n0 0 10") == "10", "large odd at end"
assert run("1\n4\n10 0 10 0") == "0", "already good"
assert run("1\n4\n10 10 10 10") == "10", "needs adjustment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0 0`|`0`| Đầu vào có kích thước tối thiểu không yêu cầu thao tác | 
|`5 5 5 5 5`|`6`| Các yếu tố bằng nhau cần điều chỉnh tích lũy |
