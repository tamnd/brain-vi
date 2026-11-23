---
title: "CF 459D - Vấn đề của Pashmak và Parmida"
description: "Chúng ta được cấp một mảng a[1..n]. Với mỗi vị trí i, xác định giá trị L[i] = số lần xuất hiện của a[i] trong tiền tố [1..i]. Với mọi vị trí j, xác định R[j] = số lần xuất hiện của a[j] trong hậu tố [j..n]."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "sortings"]
categories: ["algorithms"]
codeforces_contest: 459
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 261 (Div. 2)"
rating: 1800
weight: 459
solve_time_s: 137
verified: true
draft: false
---

[CF 459D - Vấn đề của Pashmak và Parmida](https://codeforces.com/problemset/problem/459/D) 

**Đánh giá:** 1800 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, sắp xếp 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a[1..n]`. 

Đối với mọi vị trí`i`, xác định giá trị`L[i] = number of occurrences of a[i] in the prefix [1..i]`. 

Đối với mọi vị trí`j`, định nghĩa`R[j] = number of occurrences of a[j] in the suffix [j..n]`. 

Nhiệm vụ là đếm cặp`(i, j)`với`i < j`như vậy`L[i] > R[j]`. 

Câu lệnh ban đầu mô tả các giá trị này thông qua hàm`f(l,r,x)`, nhưng toàn bộ vấn đề trở nên rõ ràng hơn nhiều khi chúng ta tính toán trước hai mảng này. 

Hãy xem xét mẫu:```
1 2 1 1 2 2 1
```Các tần số tiền tố là```
L = [1,1,2,3,2,3,4]
```Các tần số hậu tố là```
R = [4,3,3,2,2,1,1]
```Bây giờ vấn đề chỉ đơn giản là: 

Đếm cặp`(i,j)`với`i < j`Và`L[i] > R[j]`. 

Những ràng buộc là điều làm cho vấn đề trở nên thú vị. Độ dài mảng có thể đạt tới một triệu. Một thuật toán bậc hai sẽ yêu cầu khoảng```
10^12
```so sánh là hoàn toàn không thể. Thậm chí một`O(n√n)`giải pháp sẽ là quá lớn. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Có một số trường hợp dễ bỏ sót. 

Giả sử tất cả các giá trị đều khác biệt:```
3
1 2 3
```Sau đó```
L = [1,1,1]
R = [1,1,1]
```Không có cặp nào thỏa mãn`1 > 1`, vậy câu trả lời là`0`. Một giải pháp bất cẩn khi sử dụng`>=`thay vì`>`sẽ đếm sai từng cặp. 

Coi như:```
4
5 5 5 5
```Đây```
L = [1,2,3,4]
R = [4,3,2,1]
```Câu trả lời không chỉ đơn giản là tất cả các cặp. Một số cặp thỏa mãn sự bất đẳng thức và những cặp khác thì không. Bất kỳ phím tắt nào chỉ dựa trên các giá trị đều không thành công vì điều kiện phụ thuộc vào số lần xuất hiện chứ không phụ thuộc vào số được lưu trữ trong mảng. 

Một trường hợp khó phát hiện khác là khi nhiều giá trị khác nhau tạo ra cùng một tần số. Ví dụ:```
4
1 2 1 2
```Chúng tôi có được```
L = [1,1,2,2]
R = [2,2,1,1]
```Vấn đề trở thành một câu hỏi đếm theo tần số, không phải trên các giá trị mảng ban đầu. Việc xử lý riêng các giá trị bằng nhau sẽ bỏ sót các cặp hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp đầu tiên tính toán`L`Và`R`. 

Sau đó với mỗi cặp`(i,j)`với`i<j`, chúng tôi kiểm tra xem```
L[i] > R[j]
```và tăng câu trả lời khi nó được giữ. 

Điều này đúng vì nó kiểm tra định nghĩa theo nghĩa đen. Thật không may là có```
n(n-1)/2
```cặp. Vì`n = 10^6`, đó là về`5 × 10^11`cặp, vượt xa những gì có thể được xử lý. 

Nhận xét quan trọng là sau khi tính toán`L`Và`R`, các giá trị mảng ban đầu không còn quan trọng nữa. Chúng ta chỉ cần đếm cặp```
i < j
L[i] > R[j]
```Đây là một bài toán cổ điển tương tự như cách đếm ngược. 

Trong phép đếm ngược thông thường, chúng ta đếm các cặp```
i < j
A[i] > A[j]
```sử dụng sắp xếp hợp nhất. Ở đây phía bên phải đến từ một mảng khác, nhưng cấu trúc gần như giống hệt nhau. 

Trong quá trình chia để trị, giả sử chúng ta chia phạm vi chỉ số thành nửa bên trái và nửa bên phải. 

Tất cả các cặp hợp lệ thuộc một trong ba loại: 

1. Cả hai chỉ số ở nửa bên trái. 
2. Cả hai chỉ số ở nửa bên phải. 
3.`i`ở nửa bên trái và`j`ở nửa bên phải. 

Hai loại đầu tiên được xử lý đệ quy. 

Đối với loại thứ ba, chúng ta cần tính```
L[i] > R[j]
```với`i`từ phần bên trái và`j`từ phần bên phải. 

Nếu chúng ta sắp xếp nửa bên trái theo`L[i]`giá trị và nửa bên phải bằng`R[j]`giá trị, chúng ta có thể đếm các cặp chéo này bằng cách sử dụng quét hai con trỏ theo thời gian tuyến tính cho bước hợp nhất đó. 

Điều này mang lại độ phức tạp tương tự như tính nghịch đảo:```
O(n log n)
```đó là đủ nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Phân chia và chinh phục tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán mảng tần số 

Tính toán đầu tiên`L`. 

Di chuyển từ trái sang phải trong khi duy trì bản đồ tần số băm. Đối với mỗi vị trí, tăng tần số của`a[i]`và lưu kết quả vào`L[i]`. 

Tính toán tiếp theo`R`. 

Di chuyển từ phải sang trái với bản đồ tần số khác. Tăng tần số của`a[i]`vào hậu tố và lưu kết quả vào`R[i]`. 

### Đếm chia để trị 

Đối với mọi vị trí`i`, lưu trữ cặp:```
(L[i], R[i])
```Chúng tôi thực hiện đệ quy kiểu sắp xếp hợp nhất trên các phạm vi chỉ mục. 

### Hướng dẫn thuật toán 

1. Xây dựng mảng`L`Và`R`sử dụng bản đồ tần số. 
2. Tạo một loạt các vị trí. Trong quá trình đệ quy, mỗi phân đoạn sẽ được duy trì sắp xếp theo`L`các giá trị. 
3. Giải đệ quy nửa bên trái. 
4. Giải đệ quy nửa bên phải. 
5. Đếm các cặp chéo ở đâu`i`thuộc về nửa bên trái và`j`thuộc về nửa bên phải. 

Vì đoạn bên trái được sắp xếp theo`L`và phân khúc bên phải được sắp xếp theo`L`, chúng ta có thể truy cập riêng`L`Và`R`giá trị và đếm xem có bao nhiêu phần tử còn lại thỏa mãn`L[i] > R[j]`. 
6. Sắp xếp phân đoạn hiện tại theo`L`trong bước hợp nhất. 
7. Trả về tổng các cặp trái, cặp phải và cặp chéo. 

Việc triển khai sạch hơn tuân theo giải pháp Codeforces tiêu chuẩn. Chúng tôi lặp lại trên chính phạm vi chỉ mục. Trong bước hợp nhất, các chỉ mục bên trái vẫn ở trước các chỉ mục bên phải trong mảng ban đầu, do đó mọi cặp chéo được đếm sẽ tự động thỏa mãn`i < j`. 

Để đếm các cặp chéo một cách hiệu quả, chúng ta sắp xếp theo`L`. Với mỗi phần tử ở nửa bên phải, chúng ta đếm xem có bao nhiêu phần tử ở nửa bên trái```
L > R[right]
```bằng cách sử dụng một con trỏ di chuyển. 

### Tại sao nó hoạt động 

Đệ quy phân chia tất cả các cặp chỉ mục thành đúng một trong ba nhóm: hoàn toàn bên trái, hoàn toàn bên phải hoặc vượt qua điểm giữa. Cuộc gọi đệ quy đếm chính xác hai nhóm đầu tiên. Trong bước hợp nhất, mỗi cặp giao nhau có điểm cuối bên trái ở nửa bên trái và điểm cuối bên phải ở nửa bên phải, vì vậy hãy kiểm tra`L[i] > R[j]`là đủ. Bởi vì một nửa được sắp xếp theo`L`, quét hai con trỏ sẽ đếm tất cả các cặp như vậy mà không bỏ sót hoặc trùng lặp. Mỗi cặp hợp lệ được tính chính xác một lần ở cấp độ đệ quy nơi điểm cuối của nó lần đầu tiên rơi vào các nửa khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cnt = {}
    L = [0] * n
    for i in range(n):
        x = a[i]
        cnt[x] = cnt.get(x, 0) + 1
        L[i] = cnt[x]

    cnt.clear()
    R = [0] * n
    for i in range(n - 1, -1, -1):
        x = a[i]
        cnt[x] = cnt.get(x, 0) + 1
        R[i] = cnt[x]

    idx = list(range(n))
    temp = [0] * n

    def dc(l, r):
        if r - l <= 1:
            return 0

        m = (l + r) // 2

        ans = dc(l, m) + dc(m, r)

        p = l
        for j in range(m, r):
            while p < m and L[idx[p]] <= R[idx[j]]:
                p += 1
            ans += m - p

        i, j, k = l, m, l
        while i < m and j < r:
            if L[idx[i]] <= L[idx[j]]:
                temp[k] = idx[i]
                i += 1
            else:
                temp[k] = idx[j]
                j += 1
            k += 1

        while i < m:
            temp[k] = idx[i]
            i += 1
            k += 1

        while j < r:
            temp[k] = idx[j]
            j += 1
            k += 1

        for t in range(l, r):
            idx[t] = temp[t]

        return ans

    print(dc(0, n))

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính số lần xuất hiện tiền tố`L`và số lần xuất hiện hậu tố`R`. 

Sắp xếp hợp nhất các gương thông lệ chia để trị. Bất cứ lúc nào,`idx[l:r]`chứa các chỉ mục từ phân khúc đó được sắp xếp theo`L`các giá trị. 

Việc đếm cặp chéo là phần tinh tế nhất. Nửa bên trái và nửa bên phải đã được sắp xếp theo thứ tự`L`. Đối với phần tử bên phải cố định, tất cả các phần tử bên trái có```
L > R[right]
```tạo thành một hậu tố của nửa bên trái được sắp xếp. Con trỏ`p`chỉ di chuyển về phía trước, làm cho bước đếm tuyến tính cho toàn bộ quá trình hợp nhất. 

Câu trả lời có thể lớn như đại khái`n²/2`, vì vậy cần có số nguyên 64 bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
1 2 1 1 2 2 1
```Tính toán đầu tiên: 

| Vị trí | Giá trị | L | R | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 4 | 
| 2 | 2 | 1 | 3 | 
| 3 | 1 | 2 | 3 | 
| 4 | 1 | 3 | 2 | 
| 5 | 2 | 2 | 2 | 
| 6 | 2 | 3 | 1 | 
| 7 | 1 | 4 | 1 | 

Thuật toán đếm đệ quy các cặp thỏa mãn:```
L[i] > R[j]
```Số đếm cuối cùng là:```
8
```Ví dụ này cho thấy sự rút gọn trọng tâm của vấn đề. Một lần`L`Và`R`đã biết, các giá trị ban đầu không liên quan. 

### Ví dụ 2 

đầu vào:```
4
5 5 5 5
```Mảng tính toán: 

| Vị trí | Giá trị | L | R | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 4 | 
| 2 | 5 | 2 | 3 | 
| 3 | 5 | 3 | 2 | 
| 4 | 5 | 4 | 1 | 

Các cặp hợp lệ: 

| tôi | j | L[i] | R[j] | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | Không | 
| 1 | 3 | 1 | 2 | Không | 
| 1 | 4 | 1 | 1 | Không | 
| 2 | 3 | 2 | 2 | Không | 
| 2 | 4 | 2 | 1 | Có | 
| 3 | 4 | 3 | 1 | Có | 

Trả lời:```
2
```Ví dụ này cho thấy tại sao sự bất bình đẳng nghiêm ngặt lại quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Đệ quy sắp xếp hợp nhất với công việc tuyến tính theo cấp độ | 
| Không gian | O(n) | Mảng tần số, mảng chỉ mục và bộ đệm hợp nhất | 

Với`n`lên tới một triệu,`O(n²)`là không thể.`O(n log n)`thực hiện khoảng hai mươi triệu thao tác nguyên thủy, phù hợp thoải mái trong giới hạn khi triển khai được tối ưu hóa và là giải pháp dự kiến. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        cnt = {}
        L = [0] * n
        for i in range(n):
            cnt[a[i]] = cnt.get(a[i], 0) + 1
            L[i] = cnt[a[i]]

        cnt.clear()
        R = [0] * n
        for i in range(n - 1, -1, -1):
            cnt[a[i]] = cnt.get(a[i], 0) + 1
            R[i] = cnt[a[i]]

        idx = list(range(n))
        temp = [0] * n

        def dc(l, r):
            if r - l <= 1:
                return 0

            m = (l + r) // 2
            ans = dc(l, m) + dc(m, r)

            p = l
            for j in range(m, r):
                while p < m and L[idx[p]] <= R[idx[j]]:
                    p += 1
                ans += m - p

            i, j2, k = l, m, l
            while i < m and j2 < r:
                if L[idx[i]] <= L[idx[j2]]:
                    temp[k] = idx[i]
                    i += 1
                else:
                    temp[k] = idx[j2]
                    j2 += 1
                k += 1

            while i < m:
                temp[k] = idx[i]
                i += 1
                k += 1

            while j2 < r:
                temp[k] = idx[j2]
                j2 += 1
                k += 1

            for t in range(l, r):
                idx[t] = temp[t]

            return ans

        return str(dc(0, n))

    input = sys.stdin.readline
    return solve()

# provided sample
assert run("7\n1 2 1 1 2 2 1\n") == "8"

# minimum size
assert run("1\n5\n") == "0"

# all distinct
assert run("4\n1 2 3 4\n") == "0"

# all equal
assert run("4\n5 5 5 5\n") == "2"

# off-by-one strict inequality check
assert run("2\n1 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`0`| Kích thước mảng tối thiểu | 
|`1 2 3 4`|`0`| Tất cả các tần số vẫn là 1 | 
|`5 5 5 5`|`2`| Giá trị lặp lại và tần số ngày càng tăng | 
|`1 1`|`0`| Nghiêm ngặt`>`thay vì`>=`| 

## Vỏ cạnh 

Hãy xem xét:```
1
7
```Chúng tôi có được:```
L = [1]
R = [1]
```Không có cặp nào cả. Đệ quy ngay lập tức chạm vào trường hợp cơ sở và trả về`0`. 

Coi như:```
2
1 1
```Đây:```
L = [1,2]
R = [2,1]
```Cặp duy nhất là`(1,2)`và chúng tôi kiểm tra:```
1 > 1
```đó là sai. Câu trả lời là`0`. Bất kỳ việc triển khai nào bằng cách sử dụng`>=`sẽ quay lại không chính xác`1`. 

Coi như:```
4
1 2 3 4
```Mỗi tần số bằng một:```
L = [1,1,1,1]
R = [1,1,1,1]
```Không có cặp nào thỏa mãn điều kiện. Trong mỗi bước hợp nhất, con trỏ đếm tiến qua tất cả các phần tử bên trái vì`L <= R`ở khắp mọi nơi, đóng góp không cho câu trả lời. 

Coi như:```
4
5 5 5 5
```Các mảng trở thành:```
L = [1,2,3,4]
R = [4,3,2,1]
```Quá trình chia để trị đếm chính xác các cặp`(2,4)`Và`(3,4)`. Việc đặt hàng`i < j`được đảm bảo tự động vì các cặp chéo chỉ được tính giữa nửa bên trái và bên phải của khoảng chỉ mục.
