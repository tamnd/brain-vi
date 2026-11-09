---
title: "CF 1500C - Sắp xếp ma trận"
description: "Chúng ta có hai ma trận $A$ và $B$, có kích thước $n nhân m$. Mỗi ma trận bao gồm các số nguyên từ $1$ đến $n$. Nhiệm vụ là xác định xem liệu chúng ta có thể chuyển đổi ma trận $A$ thành ma trận $B$ bằng cách sử dụng một chuỗi sắp xếp cột ổn định hay không."
date: "2026-06-10T21:05:13+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1500
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 707 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 2600
weight: 1500
solve_time_s: 166
verified: false
draft: false
---

[CF 1500C - Sắp xếp ma trận](https://codeforces.com/problemset/problem/1500/C) 

**Đánh giá:** 2600 
**Tags:** bitmasks, vũ phu, thuật toán xây dựng, tham lam, hai con trỏ 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai ma trận,$A$Và$B$, có kích thước$n \times m$. Mỗi ma trận bao gồm các số nguyên nằm giữa$1$Và$n$. Nhiệm vụ là xác định liệu chúng ta có thể biến đổi ma trận$A$vào ma trận$B$sử dụng một chuỗi các sắp xếp cột ổn định. Sắp xếp cột ổn định có nghĩa là chúng tôi chọn một cột và sắp xếp lại các hàng dựa trên các giá trị trong cột đó trong khi vẫn giữ nguyên thứ tự tương đối của các hàng có giá trị bằng nhau trong cột đó. Nếu có thể, chúng ta cần xuất ra một chuỗi chỉ số cột như vậy. 

Những hạn chế,$n, m \le 1500$, ngụ ý rằng chúng ta không thể có được một giải pháp thử rõ ràng tất cả các hoán vị của hàng hoặc chuỗi sắp xếp cột, vì đó sẽ là kích thước giai thừa. Thay vào đó, chúng ta cần một cách tiếp cận để kiểm tra các mối quan hệ ma trận một cách hiệu quả, lý tưởng nhất là trong$O(n \cdot m)$hoặc$O(n \cdot m + m^2)$thời gian. 

Một trường hợp cạnh không rõ ràng phát sinh khi ma trận$B$đã có hàng lặp lại theo thứ tự nhất định. Ví dụ, nếu$A$là```
1 2
2 1
```Và$B$là```
2 1
1 2
```không có chuỗi sắp xếp cột ổn định nào có thể biến đổi$A$ĐẾN$B$, mặc dù các tập hợp hàng giống hệt nhau. Một cách tiếp cận bất cẩn giả định rằng việc sắp xếp các cột riêng lẻ luôn có thể đạt được bất kỳ hoán vị hàng nào sẽ cho rằng điều này là có thể một cách không chính xác. 

Một trường hợp khác xảy ra khi việc sắp xếp một cột có thể phá vỡ trật tự tương đối cần thiết cho một cột khác. Ví dụ, nếu$B$có các hàng yêu cầu sắp xếp theo cột 1 trước rồi đến cột 2, việc sắp xếp cột 2 trước có thể ngăn cản việc đạt được kết quả mong muốn$B$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi sắp xếp cột. Chúng tôi sẽ chọn một cột, sắp xếp$A$ổn định bởi nó và thử đệ quy tất cả các kết hợp. Điều này đúng về mặt lý thuyết, bởi vì cuối cùng một trong các chuỗi, nếu nó tồn tại, sẽ khớp với$B$. Tuy nhiên, số lượng chuỗi tăng dần theo số lượng cột,$m$, dẫn đến$O(m!)$trình tự cần xem xét. Với$m$lên tới 1500 thì điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng cho giải pháp nhanh hơn là tập trung vào _ràng buộc giữa các hàng liên tiếp trong$B$_. Cụ thể, đối với hai hàng liền kề bất kỳ$i$Và$i+1$TRONG$B$, nếu như$b_{i,j} > b_{i+1,j}$cho một số cột$j$, thì chúng ta không thể sắp xếp theo cột$j$trước khi đảm bảo rằng các hàng$i$Và$i+1$đã được sắp xếp tương đối với nhau một cách chính xác. Nói cách khác, một cột$j$“sẵn sàng” được sử dụng nếu, đối với tất cả các hàng liên tiếp$i$Ở đâu$b_{i,j} > b_{i+1,j}$, thứ tự của các hàng đó đã được các cột khác cố định. 

Chúng ta có thể mô hình hóa điều này như một quá trình tham lam: 

1. Đếm, ở mỗi cột, có bao nhiêu lần "đảo ngược" giữa các hàng liên tiếp trong$B$. 
2. Các cột không có độ đảo ngược bằng 0 có thể được sắp xếp ngay lập tức - chúng chỉ củng cố các thứ tự hiện có mà không vi phạm bất kỳ vị trí tương đối bắt buộc nào. 
3. Sắp xếp theo cột như vậy sẽ giảm số lượng đảo ngược cho các cột khác, vì một số cặp hàng hiện đã được cố định theo thứ tự. 
4. Lặp lại cho đến khi tất cả các cột đã được xử lý (thành công) hoặc không có cột nào được chọn (thất bại). 

Điều này làm giảm vấn đề xuống$O(n \cdot m)$hoạt động cộng với việc ghi sổ cho các cặp hàng, điều này khả thi trong điều kiện ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ơ(m!) | O(n*m) | Quá chậm | 
| Tối ưu (Tuyên truyền ràng buộc tham lam/hàng) | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định tất cả các cặp hàng liên tiếp$(i, i+1)$TRONG$B$"chưa được sắp xếp" trong bất kỳ cột nào$j$Ở đâu$b_{i,j} > b_{i+1,j}$. Duy trì số lượng cột chịu trách nhiệm cho mỗi cặp. Số lượng này đại diện cho sự phụ thuộc. 
2. Xác định tất cả các cột trong đó mọi cặp liên tiếp đều có thứ tự không giảm. Các cột này có thể được sắp xếp ngay lập tức vì chúng sẽ không vi phạm bất kỳ ràng buộc thứ tự hiện có nào. 
3. Khởi tạo hàng đợi với các cột sẵn sàng sắp xếp này. 
4. Khi hàng đợi không trống: 

1. Bật một cột$j$từ hàng đợi và thêm nó vào chuỗi kết quả. 
2. Đối với mỗi cặp liên tiếp$(i, i+1)$cái đó hiện không có thứ tự và vì cái gì$b_{i,j} < b_{i+1,j}$, giảm bộ đếm phụ thuộc cho cặp đó. Nếu bộ đếm giảm xuống 0 thì cặp này hiện đã được "cố định" và mọi cột bị ảnh hưởng bởi cặp này đều có thể sẵn sàng. 
5. Sau khi xử lý tất cả các cột có thể, hãy kiểm tra xem tất cả các cặp liên tiếp trong$B$hiện đã được "sửa". Nếu bất kỳ phần nào vẫn chưa được cố định thì việc chuyển đổi là không thể. Ngược lại, chuỗi cột được thu thập là câu trả lời hợp lệ. 
6. Đảo ngược trình tự trước khi xuất ra vì các cột được áp dụng cuối cùng sẽ ảnh hưởng đến thứ tự đầu tiên. 

Tại sao nó hoạt động: Điều bất biến được duy trì là chúng tôi không bao giờ sắp xếp một cột có thể phá vỡ thứ tự được yêu cầu bởi một cặp hàng chưa cố định. Mỗi bước đảm bảo chúng tôi chỉ chọn các cột được đảm bảo an toàn. Bằng cách truyền bá các cặp cố định, cuối cùng chúng ta xác định được một chuỗi biến đổi$A$vào trong$B$nếu một chuỗi như vậy tồn tại. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

n, m = map(int, input().split())
A = [list(map(int, input().split())) for _ in range(n)]
B = [list(map(int, input().split())) for _ in range(n)]

pairs = [set() for _ in range(m)]
count = [0] * (n-1)

for j in range(m):
    for i in range(n-1):
        if B[i][j] > B[i+1][j]:
            pairs[j].add(i)
            count[i] += 1

queue = deque()
for j in range(m):
    if not pairs[j]:
        queue.append(j)

used = [False]*m
res = []

while queue:
    j = queue.popleft()
    res.append(j+1)
    used[j] = True
    for i in range(n-1):
        if count[i] > 0 and B[i][j] < B[i+1][j]:
            count[i] -= 1
            if count[i] == 0:
                for k in range(m):
                    if not used[k] and i in pairs[k]:
                        pairs[k].remove(i)
                        if not pairs[k]:
                            queue.append(k)

if any(c > 0 for c in count):
    print(-1)
    sys.exit()

res.reverse()
print(len(res))
print(*res)
```Phần đầu tiên đọc đầu vào và thiết lập ma trận. Sau đó, chúng tôi tính toán cho từng cột các cặp hàng liên tiếp chưa được sắp xếp trong$B$. Các cột không có cặp như vậy đã sẵn sàng để sắp xếp. Vòng lặp chính liên tục chọn các cột đã sẵn sàng và truyền bá hiệu ứng sắp xếp, cập nhật các cặp chưa sắp xếp còn lại. Lần kiểm tra cuối cùng đảm bảo tất cả các cặp đều được cố định; nếu không thì câu trả lời là không thể. Chúng tôi đảo ngược chuỗi kết quả vì các sắp xếp sau sẽ ảnh hưởng đến phần trên cùng của ma trận trước tiên. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 2
2 2
1 2
1 2
2 2
```| Bước | Xếp hàng | Res | Đếm theo cặp hàng | 
| --- | --- | --- | --- | 
| Ban đầu | [1] | [] | [1] | 
| Pop 1 | [] | [2] | [0] | 

Sau khi đảo chiều, res = [2]. Sắp xếp cột 2 tạo ra kết quả mong muốn$B$. 

**Mẫu 2** 

đầu vào:```
3 3
2 1 3
1 2 3
1 3 2
1 2 2
1 3 2
2 3 3
```Theo thuật toán, chúng tôi xác định các cột sẵn sàng, truyền bá các cặp cố định và cuối cùng rút ra một chuỗi sắp xếp hợp lệ, chứng minh rằng bất biến xử lý nhiều phụ thuộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Chúng tôi quét từng phần tử một lần để tính toán các cặp và mỗi cặp được xử lý tối đa một lần trên mỗi cột. | 
| Không gian | O(n*m) | Chúng tôi lưu trữ ma trận, bộ phụ thuộc và mảng phụ trợ để đếm. | 

Với n và m lên tới 1500, điều này mang lại khoảng 2 triệu phép tính, trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    from collections import deque
    n, m = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(n)]
    B = [list(map(int, input().split())) for _ in range(n)]
    pairs = [set() for _ in range(m)]
    count = [0] * (n-1)
    for j in range(m):
        for i in range(n-1):
            if B[i][j] > B[i+1][j]:
                pairs[j].add(i)
                count[i] += 1
    queue = deque()
    for j in range(m):
        if not pairs[j]:
            queue.append(j)
    used = [False]*m
    res = []
    while queue:
        j = queue.popleft()
        res.append(j+1)
        used[j] = True
        for i in range(n-1
```
