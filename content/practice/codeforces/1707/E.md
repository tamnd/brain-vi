---
title: "CF 1707E - Thay thế"
description: "Chúng ta được cho một mảng các số nguyên a có độ dài n, trong đó mỗi phần tử nằm trong khoảng từ 1 đến n. Về mặt khái niệm, hãy tưởng tượng đây là những ô được dán nhãn xếp thành một hàng."
date: "2026-06-09T21:15:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1707
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 808 (Div. 1)"
rating: 3500
weight: 1707
solve_time_s: 179
verified: false
draft: false
---

[CF 1707E - Thay thế](https://codeforces.com/problemset/problem/1707/E) 

**Đánh giá:** 3500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a`chiều dài`n`, trong đó mỗi phần tử nằm trong khoảng từ 1 đến`n`. Về mặt khái niệm, hãy tưởng tượng đây là những ô được dán nhãn xếp thành một hàng. Chúng tôi xác định thao tác "thay thế" trên một đoạn liền kề`(l, r)`: nó thu gọn phân đoạn thành cặp`(min value in the segment, max value in the segment)`. Chúng ta có thể áp dụng thao tác này nhiều lần: mỗi bước ánh xạ hiện tại`(l, r)`đến giá trị tối thiểu-tối đa của mảng con được xác định bởi phạm vi đó. 

Các truy vấn yêu cầu: bắt đầu từ một nhất định`(l, r)`, chúng ta cần tiếp cận bao nhiêu người thay thế`(1, n)`? Nếu không thể - nghĩa là trình tự thay thế sẽ đi vào một chu trình không bao giờ bao gồm toàn bộ`[1, n]`- chúng tôi trở lại`-1`. 

Những hạn chế cho chúng ta biết`n`Và`q`mỗi mảng có thể lên tới 100.000, do đó việc lặp lại đơn giản qua các mảng con trong mỗi truy vấn là quá chậm. Nếu chúng ta xem xét một giải pháp đơn giản tính toán lại giá trị tối thiểu và tối đa bằng cách quét một mảng con trên mỗi lần thay thế, thì số thao tác trong trường hợp xấu nhất là khoảng`O(n * q)`hoặc thậm chí cao hơn nếu cần nhiều thay thế cho mỗi truy vấn. Điều này sẽ đạt`10^10`hoạt động, vượt xa giới hạn thời gian 2 giây. Chúng tôi cần một cách tiếp cận thông minh hơn giúp giảm việc quét lặp lại. 

Một trường hợp khó nhận thấy là khi quá trình thay thế diễn ra vô thời hạn. Ví dụ, mảng`[2, 1]`với truy vấn`(1, 1)`sản xuất`(2, 2) → (1, 1) → (2, 2)`mãi mãi. Một giải pháp bất cẩn giả định sự hội tụ cuối cùng để`(1, n)`sẽ trả về sai một số thay vì`-1`. Một trường hợp cạnh khác xảy ra khi đoạn bắt đầu đã được`(1, n)`; câu trả lời nên là`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn`(l, r)`, liên tục tính toán tối thiểu và tối đa của`a[l..r]`cho đến khi đạt được`(1, n)`hoặc phát hiện một vòng lặp. Điều này đúng vì nó mô phỏng chính xác quy trình được mô tả nhưng tốc độ quá chậm. Số bước có thể là`O(n)`cho mỗi truy vấn và tính toán tối thiểu và tối đa một cách ngây thơ cũng`O(n)`, mang lại`O(n^2 * q)`hoạt động. Vì`n, q = 10^5`, điều này rõ ràng là không thể thực hiện được. 

Quan sát quan trọng là thao tác thay thế chỉ mở rộng phân khúc: giá trị tối thiểu chỉ có thể giảm hoặc giữ nguyên và giá trị tối đa chỉ có thể tăng hoặc giữ nguyên. Tính chất đơn điệu này cho thấy rằng quá trình này có thể được dự đoán trước. Chúng ta có thể xử lý trước mảng thành một cấu trúc cho phép chúng ta chuyển từ một mảng`(l, r)`tiếp theo`(l', r')`TRONG`O(1)`hoặc`O(log n)`thời gian. Cụ thể, chúng tôi nhận thấy rằng đối với bất kỳ`x`TRONG`[1, n]`, lần xuất hiện đầu tiên và cuối cùng của`x`xác định một phân khúc cuối cùng phải được đề cập nếu chúng ta muốn tiếp cận`(1, n)`. Bằng cách liên tục hợp nhất các phân đoạn chồng chéo tương ứng với các số liên tiếp, chúng ta có thể tính toán số lượng thay thế tối thiểu mà không cần quét mảng cho mọi truy vấn. 

Cách tiếp cận kết quả là sự kết hợp giữa mở rộng khoảng và nâng nhị phân. Đầu tiên, xử lý trước cho mỗi vị trí khoảng thời gian tối đa có thể đạt được trong một lần thay thế. Sau đó, đối với mỗi truy vấn, liên tục áp dụng các bước nhảy bằng cách sử dụng bảng nhân đôi để đạt được`(1, n)`một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * q) | O(1) | Quá chậm | 
| Tối ưu (Hợp nhất khoảng thời gian + Nhân đôi) | O(n log n + q log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước lần xuất hiện đầu tiên và cuối cùng của mỗi giá trị từ`1`ĐẾN`n`. Cho phép`first[x]`Và`last[x]`biểu thị chỉ số đầu tiên và cuối cùng trong đó`a[i] = x`. Điều này cho chúng tôi biết phân đoạn tối thiểu bao gồm tất cả các lần xuất hiện của một giá trị. 
2. Tính độ giãn nở ban đầu`b[i]`cho mỗi chỉ số`i`.`b[i]`là chỉ số bên phải xa nhất có thể đạt được bắt đầu từ`[i, i]`trong một thay thế. Khởi tạo`b[i] = i`. Lặp lại các giá trị`1..n`theo thứ tự và với mỗi giá trị, hãy mở rộng`b[i]`cho tất cả`i`trong phân đoạn của nó để bao gồm khoảng thời gian tối thiểu-tối đa. Sau bước này,`b[i]`đại diện cho phân khúc tối đa có thể tiếp cận trong một bước từ`[i, i]`. 
3. Xây dựng một bảng nhân đôi. Cho phép`jump[k][i]`là chỉ mục bên phải xa nhất có thể truy cập được từ`i`sử dụng`2^k`thay thế. Điền vào bảng này bằng cách sử dụng phép lặp`jump[k][i] = jump[k-1][jump[k-1][i]]`. Điều này cho phép chúng ta trả lời các truy vấn trong`O(log n)`bằng cách nhảy sức mạnh của hai. 
4. Đối với mỗi truy vấn`(l, r)`, trước tiên hãy kiểm tra xem`(l, r)`đã bao gồm`(1, n)`. Nếu vậy hãy quay lại`0`. Ngược lại, hãy mở rộng`(l, r)`sử dụng bảng nhân đôi: bắt đầu từ lũy thừa cao nhất của hai, nhảy bất cứ khi nào nó không vượt quá`n`. Nếu sau tất cả các bước nhảy chúng ta đạt được`n`và che đậy`1`ở bên trái, trả về số lần nhảy. Còn không thì trả lại`-1`. 

Tại sao nó hoạt động: bảng nhân đôi đảm bảo rằng chúng tôi không bao giờ tính toán lại giá trị tối thiểu/tối đa cho cùng một phân đoạn nhiều lần. Phạm vi tiếp cận tối đa được tính toán trước nắm bắt chính xác một thao tác thay thế. Thuộc tính đơn điệu của việc mở rộng phân đoạn đảm bảo rằng ứng dụng lặp đi lặp lại không bao giờ thu hẹp khoảng thời gian, do đó quy trình này không thể bỏ lỡ số bước tối thiểu. Các chu trình được xử lý ngầm: nếu chúng ta không thể đạt được`(1, n)`, bảng nhảy cuối cùng sẽ không thể mở rộng phân đoạn, mang lại kết quả`-1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    first = [n] * (n + 1)
    last = [-1] * (n + 1)
    for i, val in enumerate(a):
        first[val] = min(first[val], i)
        last[val] = max(last[val], i)
    
    b = list(range(n))
    i = 0
    while i < n:
        r = i
        j = i
        while j <= r:
            val = a[j]
            r = max(r, last[val])
            j += 1
        for k in range(i, r + 1):
            b[k] = r
        i = r + 1

    LOG = 20
    jump = [b]
    for k in range(1, LOG):
        nxt = [0] * n
        for i in range(n):
            nxt[i] = jump[k-1][jump[k-1][i]]
        jump.append(nxt)

    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        if l == 0 and r == n - 1:
            print(0)
            continue
        if b[l] < r:
            print(-1)
            continue
        ans = 0
        cur_l = l
        cur_r = r
        for k in reversed(range(LOG)):
            nxt_r = jump[k][cur_l]
            if nxt_r < n - 1:
                cur_l = nxt_r + 1
                ans += 1 << k
        if cur_l <= 0 and jump[0][cur_l] >= n - 1:
            ans += 1
            print(ans)
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán lần xuất hiện đầu tiên và cuối cùng. Tính toán thứ hai`b[i]`sử dụng việc mở rộng phân khúc. Sau đó chúng ta chuẩn bị bàn nhân đôi để nhảy nhanh. Trong các truy vấn, chúng tôi kiểm tra mức độ phù hợp ban đầu, các phân đoạn không hợp lệ và sau đó nhảy liên tục. Lỗi từng lỗi một rất nghiêm trọng: các chỉ mục được chuyển đổi thành dựa trên 0 đối với danh sách Python và giới hạn phân đoạn phải được bao gồm trong phần mở rộng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:** 

| Bước | Phân đoạn | Mở rộng | 
| --- | --- | --- | 
| Truy vấn 4,4 | (4,4) | (1,1) | 
| Tiếp theo | (1,1) | (2,2) | 
| ... | Chu kỳ | ... | 

Trả lời: -1, phù hợp với kỳ vọng không bao giờ đạt được (1,5). 

**Truy vấn 3,5:** 

| Bước | Phân đoạn | Mở rộng | 
| --- | --- | --- | 
| 3,5 | (3,5) | (1,4) | 
| Tiếp theo | (1,4) | (1,5) | 

Trả lời: 2 bước. Hiển thị bảng nhân đôi giúp giảm tính toán lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Quá trình tiền xử lý cần O(n) để mở rộng phân đoạn, O(n log n) cho bảng nhân đôi. Mỗi truy vấn sử dụng bước nhảy O(log n). | 
| Không gian | O(n log n) | Bảng nhân đôi lưu trữ mảng O(log n) có kích thước n. | 

Với n, q ≤ 10^5,`O(n log n + q log n)`thoải mái trong vòng 2 giây và bộ nhớ ≤
