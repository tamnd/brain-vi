---
title: "CF 1178A - Thủ tướng"
description: "Có một số đảng phái chính trị và đảng đầu tiên thuộc về Alice. Cô ấy muốn thành lập một liên minh luôn có đảng của riêng mình và có thể có thêm một số đảng khác. Liên minh phải kiểm soát hơn một nửa số ghế trong quốc hội."
date: "2026-06-12T01:37:49+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1178
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 4"
rating: 800
weight: 1178
solve_time_s: 106
verified: true
draft: false
---

[CF 1178A - Thủ tướng](https://codeforces.com/problemset/problem/1178/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một số đảng phái chính trị và đảng đầu tiên thuộc về Alice. Cô ấy muốn thành lập một liên minh luôn có đảng của riêng mình và có thể có thêm một số đảng khác. Liên minh phải kiểm soát hơn một nửa số ghế trong quốc hội. Đồng thời, mỗi bữa tiệc được mời phải đủ nhỏ để bữa tiệc của Alice có ít nhất gấp đôi số ghế so với bữa tiệc đó. 

Dữ liệu đầu vào cung cấp số lượng đảng và số ghế mà mỗi đảng giành được. Nhiệm vụ là đưa ra bất kỳ tập hợp chỉ số đảng nào tạo thành một liên minh hợp lệ. Nếu không có liên minh như vậy tồn tại, chúng ta phải in`0`. 

Các giới hạn là rất nhỏ. Có nhiều nhất là 100 nhóm và mỗi số ghế tối đa là 100. Về mặt kỹ thuật, ngay cả việc tìm kiếm theo cấp số nhân trên tất cả các tập hợp con cũng có thể thực hiện được vì$2^{100}$lớn về mặt thiên văn và vượt xa những gì máy tính có thể liệt kê. Vì chỉ tồn tại 100 phần tử nên các thuật toán tuyến tính hoặc bậc hai là không đáng kể để vừa với giới hạn một giây. 

Một điểm tế nhị là việc thêm một đảng không bao giờ có thể làm tổn hại đến điều kiện đa số, nhưng việc thêm một đảng vi phạm quy tắc "gấp đôi số ghế" sẽ ngay lập tức khiến liên minh vô hiệu. 

Coi như```
3
10 6 5
```Không thể mời Bên 2 vì Alice chỉ có 10 ghế, tức là không ít nhất hai lần 6. Có thể mời Bên 3 vì 10 ≥ 2×5. Liên minh trở thành {1,3} với 15 ghế trong tổng số 21 ghế, chiếm đa số. Việc triển khai bất cẩn chỉ đơn giản là tiếp tục thêm các bên cho đến khi đạt được đa số sẽ bao gồm bên 2 một cách không chính xác. 

Một trường hợp khác xuất hiện khi Alice đã kiểm soát được đa số.```
3
10 1 1
```Câu trả lời chỉ là bên 1. Một giải pháp cho rằng cần có ít nhất một đồng minh sẽ dẫn đến kết quả sai. 

Trường hợp thứ ba xảy ra khi mọi bên khác đều quá lớn.```
3
5 4 4
```Không bên nào có thể tham gia vì 5 < 2×4. Riêng Alice chỉ có 5 trong số 13 ghế nên không có liên minh nào tồn tại. Đầu ra đúng là`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ lực là kiểm tra từng nhóm nhỏ các bên, buộc bên 1 phải có mặt và kiểm tra xem liên minh có đáp ứng cả hai điều kiện hay không. Điều này đúng vì mọi liên minh khả thi đều đã được thử nghiệm. Vấn đề là số lượng tập hợp con. Với 100 bên có khoảng$2^{99}$những lựa chọn khả thi, hoàn toàn không khả thi. 

Quan sát quan trọng là hạn chế đối với các đối tác liên minh chỉ phụ thuộc vào số ghế của Alice. Việc một bên khác có thể tham gia hay không không liên quan gì đến phần còn lại của liên minh. Nếu tiệc tùng$i$thỏa mãn$$a_1 \ge 2a_i,$$thì việc đưa nó vào chỉ làm tăng tổng số ghế liên minh và không bao giờ vi phạm bất kỳ điều kiện nào. Vì Alice không cần giảm thiểu số lượng các bên nên không có lý do gì để từ chối bất kỳ bên nào đủ điều kiện. 

Bởi vì điều này, vấn đề trở nên rất đơn giản. Tập hợp mọi nhóm có quy mô tối đa bằng một nửa quy mô của Alice, cộng số ghế của họ lại với nhau và kiểm tra xem liên minh kết quả có kiểm soát hơn một nửa số ghế hay không. Nếu có, hãy xuất các chỉ số đó. Nếu không thì không có giải pháp nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ghế và tính tổng số ghế trong quốc hội. 
2. Bắt đầu liên minh với bên 1 vì phải luôn có Alice. 
3. Lặp qua các bên từ 2 đến n. 
4. Nếu Alice có ít nhất gấp đôi số ghế so với đảng hiện tại, hãy đưa đảng đó vào liên minh và thêm số ghế của đảng đó vào tổng số ghế của liên minh. 

Điều kiện chỉ phụ thuộc vào số ghế của Alice nên mọi bên đủ điều kiện đều có thể được chấp nhận một cách an toàn. 
5. Sau khi xử lý tất cả các đảng, so sánh số ghế của liên minh với một nửa tổng số ghế. 
6. Nếu liên minh có hơn một nửa số ghế, hãy ghi số lượng đảng được chọn và chỉ số của họ. 
7. Nếu không thì in`0`. 

### Tại sao nó hoạt động 

Một bên thỏa mãn giới hạn về quy mô luôn có thể được thêm vào mà không vi phạm bất kỳ yêu cầu nào. Vì việc thêm ghế chỉ giúp đạt được đa số, nên liên minh tốt nhất có thể theo hạn chế sẽ đạt được bằng cách chọn mọi đảng đủ điều kiện. Nếu ngay cả liên minh hợp lệ lớn nhất này cũng không giành được đa số thì không liên minh hợp lệ nhỏ hơn nào có thể thành công. Do đó thuật toán luôn đưa ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution
n = int(input())
a = list(map(int, input().split()))

total = sum(a)

coalition = [1]
coalition_sum = a[0]

for i in range(1, n):
    if a[0] >= 2 * a[i]:
        coalition.append(i + 1)
        coalition_sum += a[i]

if coalition_sum * 2 > total:
    print(len(coalition))
    print(*coalition)
else:
    print(0)
```Biến`total`lưu trữ tổng số ghế trong quốc hội. Danh sách`coalition`ban đầu chỉ chứa bên 1 và`coalition_sum`bắt đầu với số chỗ ngồi của Alice. 

Vòng lặp kiểm tra mọi bên khác. Khi điều kiện`a[0] >= 2 * a[i]`giữ, nhóm có thể tham gia một cách an toàn và chỉ mục của nó được lưu trữ bằng cách sử dụng một cách đánh số dựa trên. 

Bài kiểm tra đa số sử dụng```
coalition_sum * 2 > total
```thay vì chia dấu phẩy động. Điều này tránh được các vấn đề về độ chính xác và thực hiện đúng yêu cầu của đa số nghiêm ngặt. 

Các chỉ số được lưu trữ trong`coalition`đã dựa trên một nền tảng nên không có vấn đề gì xảy ra khi in. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu```
3
100 50 50
```| Đảng | Ghế | Đạt chuẩn? | Chỉ số liên minh | Ghế liên minh | 
| --- | --- | --- | --- | --- | 
| 1 | 100 | Luôn luôn | [1] | 100 | 
| 2 | 50 | Có | [1,2] | 150 | 
| 3 | 50 | Có | [1,2,3] | 200 | 

Tổng số chỗ = 200. 

Vì 200 × 2 > 200, liên minh có đa số tuyệt đối. 

Ví dụ này cho thấy rằng việc tham gia mọi bên đủ điều kiện là vô hại. Bất kỳ tập hợp con nào chứa bên 1 và ít nhất một bên khác cũng sẽ hoạt động. 

Bây giờ hãy xem xét```
3
5 4 4
```| Đảng | Ghế | Đạt chuẩn? | Chỉ số liên minh | Ghế liên minh | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | Luôn luôn | [1] | 5 | 
| 2 | 4 | Không | [1] | 5 | 
| 3 | 4 | Không | [1] | 5 | 

Tổng số ghế = 13. 

Vì 5 × 2 = 10 ≤ 13 nên liên minh không chiếm đa số nên đáp án là`0`. 

Ví dụ này chứng minh rằng nếu liên minh hợp lệ lớn nhất thất bại thì sẽ không có giải pháp nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bên được xử lý một lần | 
| Không gian | O(n) | Danh sách câu trả lời có thể chứa tất cả các bên | 

Với tối đa 100 bên, thời gian tuyến tính là rất nhỏ. Thuật toán dễ dàng đáp ứng giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)
    coalition = [1]
    coalition_sum = a[0]

    for i in range(1, n):
        if a[0] >= 2 * a[i]:
            coalition.append(i + 1)
            coalition_sum += a[i]

    out = []
    if coalition_sum * 2 > total:
        out.append(str(len(coalition)))
        out.append(" ".join(map(str, coalition)))
    else:
        out.append("0")

    return "\n".join(out)

# provided sample
assert run("3\n100 50 50\n") == "3\n1 2 3"

# minimum size, no solution
assert run("2\n1 1\n") == "0"

# Alice already has majority
assert run("3\n10 1 1\n") == "3\n1 2 3"

# all equal values
assert run("4\n5 5 5 5\n") == "0"

# boundary case with equality
assert run("3\n10 5 5\n") == "3\n1 2 3"

# all parties eligible
assert run("5\n20 1 2 3 4\n") == "5\n1 2 3 4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 / 1 1 | 0 | Kích thước tối thiểu không có đa số | 
| 3 / 10 1 1 | 1 2 3 | Alice đã kiểm soát quốc hội | 
| 4 / 5 5 5 5 | 0 | Giá trị bằng nhau vi phạm điều kiện tỷ lệ | 
| 3 / 10 5 5 | 1 2 3 | Bình đẳng trong điều kiện được phép | 
| 5/20 1 2 3 4 | 1 2 3 4 5 | Mọi bên đều có thể tham gia | 

## Vỏ cạnh 

Hãy xem xét```
3
10 6 5
```Thuật toán bắt đầu với liên minh`{1}`. Bên 2 bị từ chối vì`10 < 12`. Bên 3 được chấp nhận vì`10 ≥ 10`. Liên minh có 15 ghế trong tổng số 21 ghế, chiếm đa số nên đầu ra gồm có đảng 1 và 3. Điều này giải quyết tình trạng một số đảng quá lớn nhưng những đảng khác có thể chấp nhận được. 

Coi như```
3
10 1 1
```Liên minh bắt đầu với 10 ghế. Cả hai bên còn lại đều thỏa mãn điều kiện và được cộng vào. Liên minh kết thúc với tất cả 12 ghế. Ngay cả khi không có đảng phái nào được thêm vào, chỉ riêng Alice đã chiếm đa số. Bắt đầu với bên 1 đảm bảo trường hợp này hoạt động chính xác. 

Coi như```
3
5 4 4
```Không bên nào thỏa mãn điều kiện vì`5 < 8`. Liên minh vẫn còn`{1}`với 5 chỗ ngồi. Vì 5 không quá một nửa của 13 nên thuật toán sẽ in`0`. Bởi vì mọi liên minh hợp lệ là một tập hợp con của`{1}`, không có giải pháp tồn tại.
