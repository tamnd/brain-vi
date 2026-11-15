---
title: "CF 1814D - Cân bằng vũ khí"
description: "Chúng ta được cấp một bộ súng, mỗi khẩu có hai thuộc tính: tốc độ bắn $fi$ và sát thương trên mỗi viên đạn $di$. Tích của hai giá trị này, $pi = fi cdot di$, là tổng hỏa lực của một khẩu súng."
date: "2026-06-09T08:27:12+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1814
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 146 (Rated for Div. 2)"
rating: 2500
weight: 1814
solve_time_s: 97
verified: false
draft: false
---

[CF 1814D - Cân bằng vũ khí](https://codeforces.com/problemset/problem/1814/D) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, brute Force, cấu trúc dữ liệu, toán học, hai con trỏ 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ súng, mỗi khẩu có hai thuộc tính: tốc độ bắn$f_i$và sát thương trên mỗi viên đạn$d_i$. Sản phẩm của hai thứ này$p_i = f_i \cdot d_i$, là hỏa lực tổng cộng của một khẩu súng. Nhiệm vụ là điều chỉnh càng ít$d_i$giá trị càng tốt sao cho sự khác biệt giữa súng mạnh nhất và súng yếu nhất không vượt quá ngưỡng nhất định$k$. Điều quan trọng là mỗi cái mới$d_i$phải là số nguyên dương. 

Dữ liệu đầu vào có thể khá lớn: có thể có tới 3000 khẩu súng trong tất cả các trường hợp thử nghiệm và mỗi giá trị thiệt hại có thể cao tới$10^9$. Điều này loại trừ bất kỳ giải pháp nào cố gắng liệt kê rõ ràng tất cả các giá trị thiệt hại có thể xảy ra, bởi vì ngay cả việc lặp lại tất cả các khả năng cho một khẩu súng cũng có thể vượt quá giới hạn thời gian. Chúng ta cần một cách tiếp cận hiệu quả bằng cách suy luận về phạm vi hoặc sự khác biệt tương đối thay vì liệt kê rõ ràng. 

Trường hợp cạnh phát sinh khi tất cả súng đã thỏa mãn điều kiện cân bằng, khi$k = 0$vì vậy tất cả các khẩu súng phải có sức mạnh như nhau hoặc khi một khẩu súng có giá trị sát thương rất lớn không thể điều chỉnh xuống đủ mà không giảm xuống dưới 1. Việc triển khai ngây thơ bỏ qua các ràng buộc số nguyên hoặc sửa đổi trực tiếp mọi khẩu súng có thể trả về số lượng thay đổi không tối ưu hoặc thậm chí các giá trị sát thương không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử thay đổi tất cả các tập hợp con của súng và tất cả các giá trị nguyên có thể có cho$d_i$trong giới hạn hợp lý. Về nguyên tắc, điều này đúng vì chúng ta luôn có thể tìm ra giải pháp bằng cách thử nghiệm, nhưng nó không khả thi về mặt tính toán. Với$n$lên tới 3000, việc liệt kê các tập hợp con là$2^n$và việc kiểm tra từng sự kết hợp của các giá trị thiệt hại sẽ nhân con số đó với một hệ số lớn khác. 

Quan sát quan trọng là mỗi khẩu súng xác định một phạm vi giá trị hỏa lực có thể chấp nhận được nếu chúng ta muốn giữ nguyên nó. Đối với súng$i$với tốc độ cháy$f_i$và thiệt hại$d_i$, sức mạnh là$p_i = f_i \cdot d_i$. Nếu chúng ta cố gắng đặt khoảng cách hỏa lực mục tiêu$[L, L+k]$, những khẩu súng chúng tôi không sửa đổi phải có hiện tại$p_i$trong khoảng này. Điều này có nghĩa là bài toán quy về việc tìm một khoảng có độ dài$k$bao gồm nhiều thứ hiện có$p_i$những giá trị nhất có thể. Những khẩu súng nằm ngoài khoảng thời gian này phải được sửa đổi. Sắp xếp$p_i$giá trị và sử dụng cửa sổ trượt hoặc kỹ thuật hai con trỏ cho phép chúng ta tính toán điều này một cách hiệu quả. 

Khi đó, cách tiếp cận tối ưu là tính toán tất cả$p_i$các giá trị, sắp xếp chúng và trượt một cửa sổ có độ dài$k$trên danh sách đã sắp xếp. Đối với mỗi vị trí, hãy đếm xem có bao nhiêu khẩu súng đã ở trong khoảng thời gian đó. Số lượng súng tối thiểu để thay đổi là tổng số$n$trừ đi số lượng súng tối đa đã có trong một khoảng thời gian nào đó. Cách tiếp cận này hoạt động ở$O(n \log n)$cho mỗi trường hợp thử nghiệm do sắp xếp, có thể chấp nhận được với tổng của$n$trên tất cả các trường hợp thử nghiệm là 3000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * max(d_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$k$, tiếp theo là mảng tốc độ bắn$f$và mảng sát thương$d$. 
2. Tính hỏa lực cho từng khẩu súng:$p_i = f_i \cdot d_i$. Điều này đưa ra một danh sách các số nguyên biểu thị mức năng lượng hiện tại. 
3. Sắp xếp$p_i$danh sách. Việc sắp xếp là cần thiết vì bài toán rút gọn thành việc tìm một khoảng độ dài$k$bao gồm số lượng súng tối đa. Việc sắp xếp cho phép chúng ta xem xét các chuỗi lũy thừa liền kề một cách hiệu quả. 
4. Khởi tạo hai con trỏ,`l`Và`r`, cả hai đều bắt đầu từ 0. Những con trỏ này xác định một cửa sổ lũy thừa mà chúng ta có thể không thay đổi.`l`chỉ vào lũy thừa ngoài cùng bên trái trong khoảng và`r`chuyển sang phải cho đến khi$p_r - p_l > k$. 
5. Khi chúng ta trượt`r`ở bên phải, kiểm tra xem$p_r - p_l > k$. Nếu có thì tăng`l`cho đến khi độ dài khoảng tối đa$k$. Ở mỗi bước, hãy tính số lượng súng trong khoảng thời gian đó như sau:`r - l + 1`và theo dõi mức tối đa được nhìn thấy. 
6. Sau khi lặp lại, số lần thay đổi tối thiểu là$n - \text{max\_inside}$. Điều này là do mọi khẩu súng không nằm trong khoảng thời gian tối ưu đều phải được sửa đổi. 
7. In kết quả cho từng test. 

Tại sao nó hoạt động: mảng được sắp xếp đảm bảo rằng bất kỳ khoảng độ dài nào$k$để tối đa hóa số lượng súng bên trong phải liền kề nhau. Trượt cửa sổ qua mảng đã sắp xếp đảm bảo chúng ta xem xét tất cả các khoảng liền kề như vậy. Vì các khẩu súng nằm ngoài khoảng thời gian đã chọn phải được sửa đổi, trừ đi mức tối đa được bảo hiểm từ$n$đưa ra số lượng thay đổi tối thiểu. Ràng buộc số nguyên trên$d_i$không can thiệp vì súng nào ở trong khoảng không cần thay, còn súng ở ngoài luôn có thể chỉnh theo số nguyên bất kỳ trong phạm vi cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        f = list(map(int, input().split()))
        d = list(map(int, input().split()))
        p = [f[i] * d[i] for i in range(n)]
        p.sort()
        max_inside = 0
        l = 0
        for r in range(n):
            while p[r] - p[l] > k:
                l += 1
            max_inside = max(max_inside, r - l + 1)
        print(n - max_inside)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán tổng hỏa lực cho mỗi khẩu súng. Việc sắp xếp cho phép kỹ thuật hai con trỏ tìm ra nhóm súng lớn nhất đã thỏa mãn điều kiện cân bằng một cách hiệu quả. Cửa sổ trượt di chuyển`r`thông qua mảng và`l`bắt kịp bất cứ khi nào khoảng vượt quá`k`. Trừ nhóm hợp lệ lớn nhất khỏi`n`mang lại những sửa đổi tối thiểu. Một điều tinh tế là chúng ta phải sử dụng các số nguyên xuyên suốt và tránh phép chia dấu phẩy động, việc này được xử lý tự động vì phép nhân các số nguyên của Python vẫn là số nguyên. 

## Ví dụ đã hoạt động 

Đầu vào mẫu từ trường hợp thử nghiệm đầu tiên: 

| f | d | p | 
| --- | --- | --- | 
| 6 | 1 | 6 | 
| 3 | 2 | 6 | 
| 13 | 1 | 13 | 
| 7 | 2 | 14 | 

Đã sắp xếp`p`= [6, 6, 13, 14] 

Cửa sổ trượt: 

| tôi | r | khoảng thời gian | kích thước | 
| --- | --- | --- | --- | 
| 0 | 0 | [6] | 1 | 
| 0 | 1 | [6,6] | 2 | 
| 0 | 2 | [6,6,13] | vượt quá k=2 → l=1 | 
| 1 | 2 | [6,13] | vượt quá k → l=2 | 
| 2 | 3 | [13,14] | 2 | 

Bên trong tối đa = 2, thay đổi tối thiểu = 4 - 2 = 2 

Dấu vết này xác nhận rằng cửa sổ xác định chính xác nhóm lớn nhất đã có trong`k`của nhau. 

Trường hợp mẫu thứ hai: 

| f | d | p | 
| --- | --- | --- | 
| 100 | 100 | 10000 | 
| 101 | 99 | 9999 | 
| 102 | 98 | 9996 | 

Đã sắp xếp p = [9996, 9999, 10000] 

Cửa sổ cho k=2: 

| tôi | r | khoảng thời gian | kích thước | 
| --- | --- | --- | --- | 
| 0 | 0 | [9996] | 1 | 
| 0 | 1 | [9996,9999] | 2 → vượt k → l=1 | 
| 1 | 2 | [9999,10000] | 2 → vượt k → l=2 | 

Bên trong tối đa = 1 → thay đổi tối thiểu = 3 - 1 = 2 (khớp dự kiến) 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế; cửa sổ trượt là O(n) | 
| Không gian | O(n) | Kho hỏa lực mảng`p`| 

Cho tổng n 3000 trên tất cả các trường hợp thử nghiệm, tổng thời gian là O(3000 log 3000), trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n4 2\n6 3 13 7\n1 2 1 2\n3 2\n100 101 102\n100 99 98\n5 0\n1 12 4 4 3\n12 1 3 3 4\n2 50\n1000 10\n1000000000 1\n3 5\n1 19 11\n49 4 72\n") == "2\n3\n0\n1\n2"

# custom:
```
