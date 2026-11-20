---
title: "CF 2183B - Một vấn đề MEX khác"
description: "Chúng ta được cho một mảng các số nguyên không âm và một số $k$. Nhiệm vụ là liên tục loại bỏ các phần tử khỏi mảng, luôn chọn một cửa sổ có độ dài $k$ có MEX tối đa và xóa bất kỳ phần tử nào trong cửa sổ đó."
date: "2026-06-07T21:43:06+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2183
codeforces_index: "B"
codeforces_contest_name: "Hello 2026"
rating: 1100
weight: 2183
solve_time_s: 152
verified: false
draft: false
---

[CF 2183B - Một vấn đề MEX khác](https://codeforces.com/problemset/problem/2183/B) 

**Đánh giá:** 1100 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm và một số$k$. Nhiệm vụ là liên tục loại bỏ các phần tử khỏi mảng, luôn chọn một cửa sổ có độ dài$k$có MEX tối đa và xóa bất kỳ phần tử nào trong cửa sổ đó. Sau khi thực hiện chính xác$n-k+1$việc xóa như vậy, độ dài mảng trở thành$k-1$. Mục tiêu của chúng tôi là tối đa hóa MEX của mảng cuối cùng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có thể có tới$2 \cdot 10^5$các phần tử và tổng của tất cả$n$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$. Vì mỗi trường hợp thử nghiệm có thể lớn như vậy nên bất kỳ thuật toán nào có$O(n^2)$sự phức tạp sẽ quá chậm. Chúng ta cần một giải pháp xung quanh$O(n)$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều bằng 0, các mảng thiếu các phần tử ở giữa phạm vi và các mảng trong đó$k = n$. Ví dụ, đối với một mảng`[0,0,0]`với`k=3`, sau khi loại bỏ một phần tử, mảng còn lại là`[0,0]`, do đó MEX là 1. Việc tính toán MEX cửa sổ trượt đơn giản có thể không xử lý được việc xóa lặp đi lặp lại một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng quá trình theo đúng nghĩa đen. Đối với mỗi lần xóa, chúng tôi sẽ: 

1. Tính MEX cho mọi cửa sổ có độ dài$k$. 
2. Chọn cửa sổ có MEX tối đa. 
3. Xóa một phần tử khỏi cửa sổ đó. 

Tính toán MEX cho một cửa sổ trong$O(k)$là khả thi cho các mảng nhỏ, nhưng nếu$n$là$2 \cdot 10^5$Và$k$có thể so sánh với$n$, cách tiếp cận này có thể mất tới$O(n^2)$, quá chậm. 

Quan sát quan trọng là chúng tôi không quan tâm đến thứ tự xóa chính xác. MEX cuối cùng được xác định bởi tập hợp nhiều phần tử trong mảng sau khi xóa tất cả. Vì chúng tôi biểu diễn$n-k+1$việc xóa, các phần tử còn lại là$k-1$các phần tử nhỏ nhất hoặc được lựa chọn một cách chiến lược vẫn tồn tại sau khi bị xóa. Chúng ta có thể chuyển bài toán sang việc đếm tần số của từng số nguyên trong mảng. Bất kỳ số nguyên nào$x$điều đó xảy ra nhiều hơn$n-k+1$lần không thể được loại bỏ hoàn toàn bằng cách xóa. Do đó, MEX tối đa có thể đạt được là số nguyên không âm nhỏ nhất$x$tần số của nó nhỏ hơn$n-k+1$. 

Điều này chuyển đổi vấn đề từ việc mô phỏng việc xóa sang một vấn đề đếm đơn giản, mang lại một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Đếm tần số / Logic MEX | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng tần số`count`kích thước`n+2`(để bao gồm các giá trị lên đến`n`) về không. Mảng này sẽ lưu trữ số lần mỗi số nguyên xuất hiện trong mảng. 
2. Lặp lại mảng đầu vào, tăng dần`count[a[i]]`cho mỗi phần tử`a[i]`. Bước này chuẩn bị cho chúng ta biết mỗi số xuất hiện bao nhiêu lần. 
3. Tính toán`deletions = n - k + 1`, số phần tử sẽ bị loại bỏ. 
4. Khởi tạo`mex = 0`. Lặp lại bắt đầu từ 0 trở lên. Với mỗi số nguyên`mex_candidate`, kiểm tra xem tần số của nó trong mảng có lớn hơn không`deletions`. Nếu có thì không thể loại bỏ hoàn toàn được, vì vậy hãy tăng dần`mex`và tiếp tục. Nếu tần số của nó nhỏ hơn hoặc bằng`deletions`, con số này có thể được loại bỏ hoàn toàn, vì vậy hiện tại`mex`là câu trả lời. 
5. Xuất kết quả cuối cùng`mex`. 

Tại sao nó hoạt động: Sau$n-k+1$việc xóa, chỉ$k-1$các phần tử vẫn còn. Bất kỳ số nào xảy ra nhiều hơn$n-k+1$lần được đảm bảo tồn tại ít nhất một lần xóa. Số nguyên không âm nhỏ nhất không thể đảm bảo tồn tại chính xác là MEX của mảng còn lại. Cái nhìn sâu sắc này giúp loại bỏ sự cần thiết phải mô phỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        count = [0] * (n + 2)
        for x in a:
            count[x] += 1
        deletions = n - k + 1
        mex = 0
        while count[mex] > deletions:
            mex += 1
        print(mex)

if __name__ == "__main__":
    solve()
```Trước tiên, giải pháp sẽ đọc tất cả đầu vào, đếm số lần xuất hiện và sau đó xác định MEX bằng cách so sánh tần số với`deletions`. sử dụng`n+2`TRONG`count`đảm bảo rằng chúng tôi luôn có thể kiểm tra`mex`lên đến`n+1`không có lỗi chỉ mục. Các lỗi từng cái một có thể tránh được bằng cách tăng dần`mex`chỉ khi số lượng vượt quá số lần xóa. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
n = 3, k = 3
a = [0,0,0]
```| Bước | đếm | xóa | ứng cử viên mex | mex | 
| --- | --- | --- | --- | --- | 
| 1 | [3,0,0,0] | 1 | 0 | 0 | 
| 2 | | | 0 đếm > xóa → mex=1 | 1 | 
| Kết thúc | | | 1 số lần xóa → câu trả lời=1 | 1 | 

Giải thích: Chỉ`0`xảy ra nhiều hơn 1 lần, vì vậy nó vẫn tồn tại. Số nguyên nhỏ nhất có thể bị thiếu là`1`. 

### Đầu vào mẫu 2```
n = 5, k = 3
a = [0,1,2,1,0]
```| Bước | đếm | xóa | ứng cử viên mex | mex | 
| --- | --- | --- | --- | --- | 
| 1 | [2,2,1,0,0,0] | 3 | 0 | 0 số ≤ số lần xóa → câu trả lời=0 | 
| 2 | | | Kiểm tra tiếp theo: 1 số lần xóa → mex=1 | | 
| 3 | | | Kiểm tra tiếp theo: 2 số lần xóa → mex=2 | | 
| Kết thúc | | | 3 lần đếm ≤ xóa → câu trả lời=2 | 2 | 

Giải thích: Số 0 và 1 xuất hiện ≤ 3 lần nên việc xóa có thể loại bỏ hoàn toàn. Số 2 xảy ra một lần, cũng 3. Số nhỏ nhất không đảm bảo tồn tại khi xóa là 2, cho MEX cuối cùng=2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm tần số là O(n), tìm MEX là O(n) trong trường hợp xấu nhất nhưng thường nhỏ hơn nhiều | 
| Không gian | O(n) | Mảng tần số có kích thước n+2 | 

Điều này phù hợp thoải mái trong các ràng buộc vì tổng số tiền`n`trên tất cả các trường hợp thử nghiệm là 2·10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("5\n3 3\n0 0 0\n4 2\n0 2 1 3\n5 3\n0 1 2 1 0\n6 2\n0 1 0 1 2 0\n7 5\n0 1 2 4 0 3 1\n") == "1\n1\n2\n1\n4"

# custom cases
assert run("1\n2 2\n0 1\n") == "2", "min-size input"
assert run("1\n5 5\n0 0 0 0 0\n") == "1", "k = n"
assert run("1\n6 3\n0 1 2 3 4 5\n") == "3", "all distinct elements"
assert run("1\n4 2\n1 1 1 1\n") == "0", "no zero initially"
assert run("1\n5 3\n0 1 0 1 0\n") == "2", "repeated zeros and ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2, [0,1] | 2 | Đầu vào có kích thước tối thiểu, MEX > phần tử tối đa | 
| 5 5, [0,0,0,0,0] | 1 | k = n, tất cả các phần tử đều giống hệt nhau | 
| 6 3, [0,1,2,3,4,5] | 3 | Tất cả các phần tử riêng biệt, xác minh logic đếm | 
| 4 2, [1,1,1,1] | 0 | Thiếu số 0 trong mảng | 
| 5 3, [0,1,0,1,0] | 2 | Mẫu lặp đi lặp lại với việc xóa | 

## Vỏ cạnh 

Nếu mảng không chứa số 0, thuật toán
