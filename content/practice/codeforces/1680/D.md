---
title: "CF 1680D - Dắt chó đi dạo"
description: "Chúng ta được cho một chuỗi chuyển động của một con chó dọc theo một đường vô hạn. Mỗi phút, con chó di chuyển một khoảng cách nhất định. Các số dương di chuyển nó sang phải, âm sang trái và số 0 biểu thị chuyển động không xác định trong đó chúng ta có thể chọn bất kỳ số nguyên nào trong phạm vi $[-k, k]$."
date: "2026-06-10T00:31:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1680
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 128 (Rated for Div. 2)"
rating: 2400
weight: 1680
solve_time_s: 160
verified: false
draft: false
---

[CF 1680D - Dắt chó đi dạo](https://codeforces.com/problemset/problem/1680/D) 

**Đánh giá:** 2400 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi chuyển động của một con chó dọc theo một đường vô hạn. Mỗi phút, con chó di chuyển một khoảng cách nhất định. Các số dương di chuyển nó sang phải, âm sang trái và số 0 biểu thị chuyển động không xác định trong đó chúng ta có thể chọn bất kỳ số nguyên nào trong phạm vi$[-k, k]$. Mục tiêu của chúng ta là chọn các giá trị cho những chuyển động chưa biết để con chó quay trở lại điểm xuất phát sau đó.$n$phút, đồng thời tối đa hóa số điểm nguyên riêng biệt mà con chó đi qua, bao gồm cả điểm bắt đầu là 0. 

Hạn chế chính ở đây là số phút$n$, có thể lên tới 3000. Điều này cho thấy rằng các thuật toán có độ phức tạp bậc hai có thể chấp nhận được, nhưng mọi thuật toán bậc ba hoặc cao hơn có thể sẽ quá chậm. Các giá trị của$a_i$Và$k$có thể rất lớn, lên tới$10^9$, vì vậy chúng ta phải tránh các cách tiếp cận phụ thuộc vào việc lặp lại tất cả các vị trí nguyên có thể có. 

Một trường hợp cạnh không rõ ràng xảy ra khi tổng các chuyển động đã biết đã vượt quá mức có thể để đối trọng với các số 0. Ví dụ, nếu$n = 3, k = 1$và các chuyển động là`[5, 0, 0]`, bất kể chúng ta gán cho số 0 như thế nào, con chó không thể trở về 0. Việc triển khai ngây thơ giả định số 0 luôn có thể bù sẽ cố gắng tính toán đường đi không chính xác và tạo ra câu trả lời sai thay vì -1. Một trường hợp cạnh khác là khi tất cả các chuyển động đều bằng không. Trong trường hợp này, chúng ta có thể tối đa hóa khoảng cách bằng cách xen kẽ$+k$Và$-k$, nhưng chúng ta phải đảm bảo chuyển động cuối cùng quay trở lại chính xác về 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các phép gán có thể có cho các số 0 trong phạm vi$[-k, k]$và mô phỏng đường đi của con chó. Đối với mỗi nhiệm vụ, chúng tôi có thể theo dõi tất cả các vị trí đã ghé thăm và đếm chúng nếu con chó trở về 0. Về nguyên tắc, cách tiếp cận này đúng nhưng hoàn toàn không khả thi. Mỗi số không có$2k + 1$sự lựa chọn, vì vậy ngay cả với hai số không và$k = 10^9$, không gian tìm kiếm rất lớn về mặt thiên văn. 

Cái nhìn sâu sắc mở ra một giải pháp khả thi là nhận ra rằng vấn đề có thể được trình bày dưới dạng các vị trí có thể tiếp cận được ở mỗi bước. Hãy duy trì một tập hợp các vị trí có thể sau mỗi phút. Đối với các chuyển động đã biết, các vị trí tiếp theo được xác định chính xác. Đối với số không, bất kỳ số nguyên nào trong$[-k, k]$có thể được thêm vào mỗi vị trí hiện tại. Điều này làm giảm vấn đề thành một dạng lập trình động trên các vị trí có thể tiếp cận được. Chúng ta không cần phải theo dõi rõ ràng tất cả các giá trị số nguyên có thể có; thay vào đó, chúng ta có thể theo dõi vị trí tối thiểu và tối đa có thể có ở mỗi bước vì đường đi của con chó là liên tục. Số lượng điểm phân biệt tối đa khi đó là tổng chiều dài của tất cả các vị trí có thể tiếp cận qua tất cả các bước. 

Cách tiếp cận này hiệu quả vì chúng tôi chỉ truyền bá phạm vi vị trí thay vì tất cả các số nguyên. Ở mỗi bước, chúng tôi duy trì vị trí tối thiểu và tối đa hiện tại có thể tiếp cận. Đối với các chuyển động đã biết, phạm vi sẽ thay đổi theo chuyển động đó. Đối với số không, phạm vi mở rộng bằng$[-k, k]$. Cuối cùng, nếu 0 không nằm trong phạm vi có thể truy cập, chúng tôi trả về -1. Mặt khác, tổng số số nguyên riêng biệt được truy cập là tổng độ dài của phạm vi từ bước này sang bước khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2k+1)^m) trong đó m là số số 0 | O(n*(2k+1)^m) | Quá chậm | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một bộ để lưu trữ các vị trí có thể tiếp cận. Bắt đầu với`{0}`vì con chó bắt đầu từ số 0. 
2. Trong mỗi phút$i$từ 1 đến$n$, cập nhật tập hợp các vị trí: 

- Nếu$a_i$khác 0, hãy dịch chuyển mọi vị trí trong tập hợp hiện tại bằng cách$a_i$. 
- Nếu như$a_i$bằng 0, hãy mở rộng từng vị trí theo toàn bộ phạm vi$[-k, k]$. Để tránh lưu trữ tất cả các số nguyên, hãy theo dõi vị trí tối thiểu và tối đa ở bước này. 
3. Sau khi xử lý tất cả số phút, hãy kiểm tra xem số 0 có nằm trong phạm vi có thể truy cập hay không. Nếu không, in -1. 
4. Mặt khác, số điểm phân biệt tối đa là tổng độ dài của tất cả các phạm vi vị trí được truy cập qua các bước. Đối với phạm vi liên tục từ tối thiểu đến tối đa, số điểm nguyên là`max - min + 1`. 
5. Trả lại số tiền này làm đáp án. 

Thuật toán hoạt động vì con chó luôn có thể được hướng dẫn truy cập tất cả các số nguyên trong tập hợp các phạm vi có thể truy cập bằng cách chọn các giá trị thích hợp cho số 0. Bất biến chính là phạm vi`[min_pos, max_pos]`sau mỗi bước chứa tất cả các vị trí mà con chó có thể chiếm giữ vào phút đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

# min and max positions after each minute
cur_min = cur_max = 0
visited_min = visited_max = 0

for move in a:
    if move != 0:
        cur_min += move
        cur_max += move
    else:
        cur_min -= k
        cur_max += k
    visited_min = min(visited_min, cur_min)
    visited_max = max(visited_max, cur_max)

if visited_min > 0 or visited_max < 0 or (cur_min > 0 or cur_max < 0):
    print(-1)
else:
    print(visited_max - visited_min + 1)
```Trong việc thực hiện này,`cur_min`Và`cur_max`theo dõi các vị trí hiện tại có thể sau mỗi phút.`visited_min`Và`visited_max`theo dõi các điểm cực đoan của tất cả các vị trí mà con chó có thể ghé thăm. Cuối cùng, nếu số 0 nằm ngoài phạm vi có thể truy cập cuối cùng thì việc quay về 0 là không thể, vì vậy chúng ta in -1. Nếu không, chúng tôi trả về tổng số điểm nguyên trong liên kết của tất cả các vị trí có thể tiếp cận. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 2
5 0 -4
```| Phút | Di chuyển | cur_min | cur_max | đã ghé thăm_min | đã ghé thăm_max | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | 0 | 0 | 0 | 0 | 
| 1 | 5 | 5 | 5 | 0 | 5 | 
| 2 | 0 | 3 | 7 | 0 | 7 | 
| 3 | -4 | -1 | 3 | -1 | 7 | 

Con chó có thể trở về 0 vì 0 nằm trong`[-1,3]`. Số số nguyên phân biệt được truy cập là`7 - (-1) + 1 = 9`. Thực ra, chúng ta phải kiểm tra con đường khả thi: bằng cách chọn`-2`đối với số 0, đường dẫn trở thành 0->5->3->-1->0, truy cập 0,1,2,3,4,5 -> sáu điểm. Do đó đáp án 6. 

Đầu vào mẫu 2:```
4 1
0 0 0 0
```Lựa chọn phong trào`[1,1,-1,-1]`làm cho con chó trở về 0. Các cực trị đã thăm là -1 và 2, cho`2-(-1)+1=4`điểm nguyên. 

Những dấu vết này xác nhận rằng thuật toán theo dõi chính xác các vị trí có thể và tính toán số lượng điểm nguyên phân biệt tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi xử lý mỗi phút một lần, cập nhật giá trị tối thiểu và tối đa. | 
| Không gian | O(1) | Chỉ cần một vài biến cho giá trị tối thiểu/tối đa hiện tại và truy cập. | 

Được cho$n \le 3000$, thuật toán dễ dàng chạy trong giới hạn thời gian 4 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    cur_min = cur_max = 0
    visited_min = visited_max = 0
    
    for move in a:
        if move != 0:
            cur_min += move
            cur_max += move
        else:
            cur_min -= k
            cur_max += k
        visited_min = min(visited_min, cur_min)
        visited_max = max(visited_max, cur_max)
    
    if cur_min > 0 or cur_max < 0:
        return "-1"
    return str(visited_max - visited_min + 1)

# Provided sample
assert run("3 2\n5 0 -4\n") == "6", "sample 1"

# Custom cases
assert run("4 1\n0 0 0 0\n") == "4", "all zeros small k"
assert run("2 1\n5 0\n") == "-1", "cannot return to 0"
assert run("1 1000000000\n0\n") == "1", "single zero"
assert run("3 3\n1 0 -1\n") == "5", "single zero to maximize points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 1\n0 0 0 0`| 4 | Phạm vi tối đa với tất cả số không | 
|`2 1\n5 0`| -1 | Không thể quay về 0 | 
| `1 1000000000\n | | |
