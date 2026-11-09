---
title: "CF 1473D - Chương trình"
description: "Chúng ta được cung cấp một chương trình thao tác một biến số nguyên x, bắt đầu từ 0. Mỗi lệnh tăng hoặc giảm x đi 1."
date: "2026-06-11T00:23:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1473
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 102 (Rated for Div. 2)"
rating: 1700
weight: 1473
solve_time_s: 106
verified: true
draft: false
---

[CF 1473D - Chương trình](https://codeforces.com/problemset/problem/1473/D) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, dp, triển khai, chuỗi 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chương trình thao tác một biến số nguyên`x`, bắt đầu từ`0`. Mỗi lệnh tăng hoặc giảm`x`qua`1`. Sau đó, chúng tôi được hỏi một loạt truy vấn: đối với mỗi truy vấn, bỏ qua một chuỗi hướng dẫn tiếp giáp và xác định có bao nhiêu giá trị riêng biệt`x`mất khi thực hiện các hướng dẫn còn lại theo thứ tự. Đầu ra của một truy vấn chỉ đơn giản là số lượng các giá trị riêng biệt`x`đạt được. 

Những hạn chế là đáng kể. Mỗi chương trình có thể có tới 200.000 hướng dẫn và có thể có tới 200.000 truy vấn. Trên tất cả các trường hợp thử nghiệm, tổng số hướng dẫn và truy vấn cũng được giới hạn ở mức 200.000. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng chương trình từ đầu cho mọi truy vấn, vì giải pháp đó có thể theo thứ tự$O(n \cdot m)$các hoạt động có thể được$4 \cdot 10^{10}$trong trường hợp xấu nhất. Chúng ta cần một giải pháp xử lý trước thông tin để mỗi truy vấn có thể được trả lời theo thời gian không đổi hoặc logarit. 

Một điều tinh tế quan trọng là việc bỏ qua một dãy con có thể dịch chuyển tất cả các giá trị tiếp theo bằng một độ lệch cố định. Ví dụ: hãy xem xét một chương trình "+-+-" và một truy vấn bỏ qua các hướng dẫn từ 2 đến 3. Chuỗi kết quả là "+-" từ hướng dẫn 1 và 4, nhưng giá trị tối thiểu và tối đa tương đối của`x`thay đổi dựa trên hiệu ứng tích lũy của các hướng dẫn bị bỏ qua. Các giải pháp bất cẩn chỉ tính toán lại chuỗi một cách ngây thơ sẽ đếm quá hoặc thiếu các giá trị riêng biệt. Tương tự, các truy vấn loại bỏ hướng dẫn đầu tiên hoặc cuối cùng phải xử lý chính xác các điều kiện biên trong đó chương trình có thể bị trống. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản rất đơn giản: với mỗi truy vấn, hãy xây dựng một chuỗi chương trình mới bằng cách bỏ qua các hướng dẫn từ`l`ĐẾN`r`, mô phỏng`x`từng bước một, ghi lại tất cả các giá trị và trả về số lượng các giá trị riêng biệt. Điều này hoạt động chính xác đối với đầu vào nhỏ, nhưng nó cực kỳ kém hiệu quả. Nếu như`n`Và`m`mỗi người là$2 \cdot 10^5$, điều này có thể yêu cầu$O(n \cdot m) = 4 \cdot 10^{10}$hoạt động vượt xa giới hạn 2 giây. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể xử lý trước thông tin tiền tố và hậu tố về chương trình. Nếu chúng ta biết, đối với mỗi vị trí, mức tối thiểu và tối đa`x`giá trị đạt tới điểm đó (tiền tố) và từ điểm đó đến điểm cuối (hậu tố), chúng ta có thể trả lời bất kỳ truy vấn nào bằng cách kết hợp tiền tố trước phân đoạn bị bỏ qua và hậu tố sau phân đoạn đó. Phân đoạn bị bỏ qua góp phần thay đổi ròng`delta`, là tổng của số tăng và số giảm trong phân đoạn đó. Các giá trị trong hậu tố thay đổi theo hướng này`delta`khi nối sau tiền tố. Chúng tôi không cần mô phỏng toàn bộ chương trình; chỉ cần theo dõi mức tối thiểu và tối đa là đủ`x`ở cả tiền tố và hậu tố và điều chỉnh chúng dựa trên sự thay đổi thực của phân đoạn bị bỏ qua. 

Điều này làm giảm việc tính toán trên mỗi truy vấn về thời gian không đổi sau khi xử lý trước tuyến tính. Quá trình tiền xử lý tính toán các mảng tiền tố min/max và hậu tố min/max trong$O(n)$. Mỗi truy vấn sử dụng các mảng này để tính toán mức tối thiểu và tối đa hiệu quả trên các hướng dẫn còn lại và đưa ra số lượng giá trị riêng biệt dưới dạng`max_val - min_val + 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển chuỗi chương trình thành mảng`a`Ở đâu`a[i] = 1`cho '+' và`a[i] = -1`vì '-'. Điều này cho phép thực hiện các phép toán số thay vì xử lý chuỗi. 
2. Xây dựng tổng tiền tố`pref[i]`như vậy`pref[i]`là giá trị của`x`sau khi thực hiện lần đầu tiên`i`hướng dẫn. Khởi tạo`pref[0] = 0`. 
3. Tính tiền tố tối thiểu`pref_min[i]`và tiền tố tối đa`pref_max[i]`mảng.`pref_min[i]`là giá trị nhỏ nhất`x`đạt từ đầu đến hướng dẫn`i`.`pref_max[i]`là lớn nhất. 
4. Tương tự, xây dựng thông tin hậu tố. Cho phép`suff[i]`đại diện cho tổng tích lũy từ hướng dẫn`i`đến cuối cùng. Sau đó tính toán`suff_min[i]`Và`suff_max[i]`, đại diện cho giá trị tối thiểu và tối đa của`x`trong hậu tố đó so với điểm bắt đầu của nó, ban đầu là`0`. 
5. Đối với mỗi truy vấn`[l, r]`, chia chương trình thành hai phần: tiền tố`1..l-1`và hậu tố`r+1..n`. Phân đoạn bị bỏ qua đóng góp một delta ròng của`pref[r] - pref[l-1]`. 
6. Điều chỉnh giá trị tối thiểu và tối đa của hậu tố theo sự thay đổi thực trong`x`do tiền tố (`pref[l-1]`). Giá trị tối thiểu của`x`qua các hướng dẫn còn lại là`min(pref_min[l-1], pref[l-1] + suff_min[r+1])`, và cực đại là`max(pref_max[l-1], pref[l-1] + suff_max[r+1])`. 
7. Câu trả lời cho truy vấn là`max_val - min_val + 1`. 

Tại sao nó hoạt động: Điều bất biến là bất kỳ việc loại bỏ chuỗi con liền kề nào cũng chỉ đơn giản là dịch chuyển tất cả các giá trị sau đó do hiệu ứng thực của phân đoạn bị loại bỏ. Bằng cách lưu trữ các giá trị tối thiểu và tối đa cho tất cả các tiền tố và hậu tố, chúng ta có thể xây dựng lại các giá trị cực trị mà không cần mô phỏng từng bước. Kết hợp hai giá trị cực trị sau khi dịch chuyển sẽ tạo ra tập hợp đầy đủ các giá trị riêng biệt vì mỗi lần tăng/giảm đều thay đổi`x`đúng bằng 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = input().strip()
        a = [1 if c == '+' else -1 for c in s]

        # prefix sums and min/max
        pref = [0] * (n + 1)
        pref_min = [0] * (n + 1)
        pref_max = [0] * (n + 1)
        for i in range(1, n + 1):
            pref[i] = pref[i - 1] + a[i - 1]
            pref_min[i] = min(pref_min[i - 1], pref[i])
            pref_max[i] = max(pref_max[i - 1], pref[i])

        # suffix min/max
        suff_min = [0] * (n + 2)
        suff_max = [0] * (n + 2)
        suff_sum = 0
        for i in range(n, 0, -1):
            suff_sum += a[i - 1]
            suff_min[i] = min(0, suff_min[i + 1] + a[i - 1])
            suff_max[i] = max(0, suff_max[i + 1] + a[i - 1])

        res = []
        for _ in range(m):
            l, r = map(int, input().split())
            # prefix before ignored segment
            pre_min = pref_min[l - 1]
            pre_max = pref_max[l - 1]
            # suffix after ignored segment, shifted by pref[l-1]
            suf_min = pref[l - 1] + suff_min[r + 1]
            suf_max = pref[l - 1] + suff_max[r + 1]
            total_min = min(pre_min, suf_min)
            total_max = max(pre_max, suf_max)
            res.append(str(total_max - total_min + 1))
        print('\n'.join(res))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi chuỗi thành các số tăng và giảm. Tổng tiền tố và giá trị tối thiểu/tối đa của chúng theo dõi các giá trị cực trị theo từng lệnh. Các mảng hậu tố tối thiểu/tối đa được tính ngược lại, tích lũy các giá trị tối thiểu/tối đa tương đối từ cuối. Đối với mỗi truy vấn, chúng tôi hợp nhất các giá trị tiền tố và hậu tố, điều chỉnh các giá trị hậu tố theo tổng tiền tố trước phân đoạn bị bỏ qua. Câu trả lời cuối cùng là phạm vi khác biệt`x`các giá trị. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
8 4
-+--+--+
1 8
2 8
2 5
1 1
```| Bước | trước | pref_min | pref_max | đủ_min | đủ_max | Truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 0 | 0 | - | - | 
| 1 | -1 | -1 | 0 | ... | ... | 1-8 | 1 | 
| 2 | -1+1=0 | -1 | 1 | ... | ... | 2-8 | 2 | 
| 3 | ... | ... | ... | ... | ... | 2-5 | 4 | 
| 4 | ... | ... | ... | ... | ... | 1-1 | 4 | 

Dấu vết này hiển thị giá trị tối thiểu và tối đa trước và sau các phân đoạn bị bỏ qua, thể hiện cách thuật toán tính toán phạm vi một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi mảng được xây dựng trong O(n) và mỗi truy vấn được trả lời trong O(1) | 
| Không gian | O(n) | Mảng tiền tố và hậu tố yêu cầu mỗi mảng có khoảng trống O(n) | 

Với n và m lên tới 2e5, các thao tác O(n + m) vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str)
```
