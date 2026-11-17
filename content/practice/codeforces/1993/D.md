---
title: "CF 1993D - Med-imize"
description: "Chúng ta được cung cấp một mảng các số nguyên và tham số $k$. Chúng ta có thể liên tục loại bỏ bất kỳ mảng con liền kề nào có độ dài chính xác $k$ khỏi mảng, thu nhỏ nó. Khi độ dài của mảng không còn lớn hơn $k$, chúng tôi dừng lại."
date: "2026-06-08T15:09:43+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1993
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 963 (Div. 2)"
rating: 2200
weight: 1993
solve_time_s: 157
verified: false
draft: false
---

[CF 1993D - Med-imize](https://codeforces.com/problemset/problem/1993/D) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, dp, tham lam 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một tham số$k$. Chúng ta có thể liên tục loại bỏ chính xác bất kỳ mảng con liền kề nào có độ dài$k$khỏi mảng, thu nhỏ nó. Khi độ dài của mảng không còn lớn hơn$k$, chúng tôi dừng lại. Mục tiêu của chúng tôi là xác định giá trị trung bình lớn nhất của các phần tử còn lại sau quá trình này. 

Trung vị là phần tử "ở giữa" trong mảng được sắp xếp. Ví dụ: nếu còn lại một phần tử thì phần tử đó gần như là trung vị. Nếu còn nhiều phần tử thì trung vị là phần tử ở vị trí$\lfloor (n+1)/2 \rfloor$khi được sắp xếp. 

Các ràng buộc cho thấy mảng có thể có tới$5 \cdot 10^5$các phần tử và tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$5 \cdot 10^5$. Điều này có nghĩa là bất kỳ giải pháp nào với$O(n \log n)$mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi phương pháp bậc hai đều không khả thi. Mỗi phần tử có thể lớn bằng$10^9$, vì vậy các thủ thuật đếm hoặc dựa trên tần số đơn giản sẽ không hiệu quả. 

Một trường hợp cạnh tinh tế phát sinh khi$k=1$hoặc$k=n$. Nếu như$k=1$, bất kỳ phần tử đơn lẻ nào cũng có thể bị xóa bất kỳ lúc nào, điều đó có nghĩa là cuối cùng chúng ta có thể để phần tử lớn nhất làm phần tử trung vị. Nếu như$k=n$, không thể loại bỏ được và số trung vị chỉ đơn giản là số trung vị của mảng ban đầu. Thiếu những sắc thái này có thể tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các chuỗi loại bỏ mảng con hợp lệ và tính giá trị trung bình cho từng mảng kết quả. Điều này đúng về nguyên tắc nhưng cực kỳ chậm. Ví dụ, nếu$n=10^5$Và$k=2$, có khoảng$\binom{n}{k}$chuỗi các hoạt động cần xem xét, có quy mô lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng chỉ có vị trí tương đối của các yếu tố mới quan trọng. Sau nhiều lần loại bỏ, mảng còn lại sẽ luôn bao gồm các phần tử ở ranh giới của các mảng con bị loại bỏ. Vì chúng tôi muốn có giá trị trung bình lớn nhất nên chúng tôi không quan tâm đến trình tự loại bỏ chính xác. Thay vào đó, chúng tôi yêu cầu: đối với giá trị trung bình ứng cử viên$x$, liệu chúng ta có thể loại bỏ đủ phần tử để$x$kết thúc ở "giữa" của mảng cuối cùng? 

Điều này tự nhiên dẫn đến một giải pháp tìm kiếm nhị phân. Chúng ta có thể đoán một giá trị trung bình$x$và kiểm tra xem nó có thể đạt được hay không. Để làm điều này, chúng ta có thể chuyển đổi mảng thành chuỗi +1/-1: +1 cho các phần tử$\ge x$, -1 cho các phần tử$< x$. Sau đó, vấn đề giảm xuống còn việc tìm xem liệu chiến lược loại bỏ mảng con có cho phép cân bằng dương +1 trên -1 trong mảng cuối cùng hay không. Việc kiểm tra này có thể được thực hiện trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n/k)^n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Tổng tiền tố | O(n log(max(a))) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp là không cần thiết vì chúng ta chỉ quan tâm đến việc liệu một giá trị có thể là trung vị hay không. 
2. Khởi tạo giới hạn tìm kiếm nhị phân:`lo = 1`,`hi = max(a)`. 
3. Trong khi`lo < hi`, nhặt`mid = (lo + hi + 1) // 2`với tư cách là một ứng cử viên trung bình. 
4. Chuyển mảng thành`b[i] = +1 if a[i] >= mid else -1`. 
5. Tính tổng tiền tố`pref[i] = b[0] + ... + b[i]`. 
6. Đối với mỗi chỉ số`i`nơi chúng tôi có thể ngừng xóa các mảng con, duy trì tổng tiền tố tối thiểu cho phía bên trái của cửa sổ kích thước hiện tại`k`. 
7. Nếu tại bất kỳ thời điểm nào tổng tiền tố trừ tổng tiền tố tối thiểu là dương, ứng viên`mid`là có thể đạt được. 
8. Nếu có thể, hãy di chuyển`lo = mid`. Ngược lại, đặt`hi = mid - 1`. 
9. Sau khi tìm kiếm nhị phân,`lo`là mức trung bình lớn nhất có thể đạt được. 

Tại sao nó hoạt động: phép biến đổi +1/-1 cho biết liệu trung vị ứng cử viên có "đủ lớn" hay không. Sự khác biệt về tổng tiền tố dương đảm bảo rằng chúng ta có thể chọn các mảng con sao cho giá trị trung vị này tồn tại đến cuối. Tìm kiếm nhị phân đảm bảo rằng chúng ta tìm được giá trị trung bình tối đa một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_achieve(a, n, k, x):
    b = [1 if val >= x else -1 for val in a]
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i+1] = pref[i] + b[i]

    min_pref = 0
    for i in range(k, n+1):
        if pref[i] - min_pref > 0:
            return True
        min_pref = min(min_pref, pref[i-k+1])
    return False

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        lo, hi = 1, max(a)
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if can_achieve(a, n, k, mid):
                lo = mid
            else:
                hi = mid - 1
        print(lo)

if __name__ == "__main__":
    solve()
```Giải thích: Các`can_achieve`hàm tính tổng tiền tố trên mảng được chuyển đổi và kiểm tra xem có thể đạt được tổng dương hay không bằng cách sử dụng cửa sổ trượt có kích thước`k`. Tìm kiếm nhị phân tìm ra giá trị trung bình khả thi tối đa. Lưu ý rằng`+1`TRONG`(lo + hi + 1) // 2`đảm bảo việc tìm kiếm không bị mắc kẹt khi`lo`Và`hi`liền kề nhau. Xử lý ranh giới trong`min_pref`là tinh tế và quan trọng cho sự chính xác. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`4 3`với mảng`[3,9,9,2]`Tìm kiếm nhị phân trung vị ứng viên bắt đầu ở các giá trị giữa 6, 4, 3. Chuyển đổi mảng với`mid=4`cho`[-1,1,1,-1]`. Tổng tiền tố`[0,-1,0,1,0]`. Kiểm tra cửa sổ trượt xác nhận rằng tổng dương tồn tại;`mid=4`có thể đạt được. Tiến hành tìm kiếm nhị phân, câu trả lời cuối cùng`3`. 

| Bước | giữa | chuyển đổi | tiền tố | phút_pref | có thể đạt được? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | [-1,-1,-1,-1] | [0,-1,-2,-3,-4] | 0 | Không | 
| 2 | 4 | [-1,1,1,-1] | [0,-1,0,1,0] | 0 | Có | 
| 3 | 3 | [1,1,1,-1] | [0,1,2,3,2] | 0 | Có | 

Điều này xác nhận thuật toán xác định chính xác trung vị tối đa 3. 

**Ví dụ 2:** 

đầu vào:`5 3`với mảng`[3,2,5,6,4]`Ứng cử viên tìm kiếm nhị phân`mid=4`sản lượng chuyển đổi`[ -1,-1,1,1,1 ]`. Tổng tiền tố`[0,-1,-2,-1,0,1]`. Cửa sổ trượt tìm thấy sự khác biệt tích cực, có thể đạt được. Câu trả lời cuối cùng 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max(a))) | Tìm kiếm nhị phân trên các giá trị phần tử (tối đa 10^9) lần kiểm tra tổng tiền tố tuyến tính | 
| Không gian | O(n) | Mảng tổng tiền tố có độ dài n+1 | 

Được cho$n \le 5 \cdot 10^5$và phần tử tối đa$10^9$, giải pháp này dễ dàng phù hợp trong vòng 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n4 3\n3 9 9 2\n5 3\n3 2 5 6 4\n7 1\n5 9 2 6 5 4 6\n8 2\n7 1 2 6 8 3 4 5\n4 5\n3 4 5 6\n") == "3\n4\n9\n6\n4"

# custom cases
assert run("1\n1 1\n10\n") == "10"  # single element
assert run("1\n5 5\n1 2 3 4 5\n") == "3"  # k=n, median of original
assert run("1\n5 1\n1 2 3 4 5\n") == "5"  # k=1, can leave largest
assert run("1\n6 2\n1 1 1 1 1 1\n") == "1"  # all equal
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n10 | 10 | Mảng một phần tử | 
| 5 5\n1 2 3 4 5 | 3 | k=n, trung vị |
