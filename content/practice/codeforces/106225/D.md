---
title: "CF 106225D - Cân bằng trong ngục tối"
description: "Chúng ta được cung cấp một mảng trong đó mỗi giá trị đại diện cho một loại quái vật. Một mức được coi là cân bằng khi mỗi loại còn lại xuất hiện chính xác gấp nhiều lần giá trị của chính nó. Ví dụ: nếu có loại 4 thì nó phải xảy ra chính xác bốn lần."
date: "2026-06-19T14:02:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106225
codeforces_index: "D"
codeforces_contest_name: "2025-2026 ICPC Southwestern European Regional Contest (SWERC 2025)"
rating: 0
weight: 106225
solve_time_s: 59
verified: true
draft: false
---

[CF 106225D - Cân bằng trong ngục tối](https://codeforces.com/problemset/problem/106225/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi giá trị đại diện cho một loại quái vật. Một mức được coi là cân bằng khi mỗi loại còn lại xuất hiện chính xác gấp nhiều lần giá trị của chính nó. Ví dụ, nếu gõ`4`có mặt thì nó phải xảy ra đúng bốn lần. Các loại được loại bỏ hoàn toàn không có yêu cầu. Nhiệm vụ là xóa càng ít phần tử càng tốt để phần tử còn lại thỏa mãn quy tắc này. 

Đầu vào bao gồm`n`các loại quái vật, mỗi loại ở giữa`0`Và`n`. Vì chỉ cho phép xóa nên chúng tôi không bao giờ tăng tần suất của bất kỳ giá trị nào. Đối với mỗi loại, chúng tôi giữ tần số chính xác bằng giá trị của nó hoặc xóa tất cả các bản sao của loại đó. 

Sự ràng buộc`n ≤ 1000`là nhỏ, nên ngay cả nghiệm bậc hai cũng có thể vừa vặn một cách thoải mái. Tuy nhiên, cấu trúc của bài toán cho phép giải tuyến tính sau khi đếm tần số. Thông tin duy nhất quan trọng là mỗi giá trị xuất hiện bao nhiêu lần chứ không phải thứ tự của mảng. 

Một số tình huống rất dễ xử lý sai. 

Giả sử một giá trị xuất hiện ít lần hơn giá trị của chính nó.```
3
2 2 1
```Kiểu`2`xuất hiện hai lần, vì vậy nó có thể ở lại. Kiểu`1`xuất hiện một lần thì cũng có thể ở lại. Không cần xóa và câu trả lời là`0`. Một cách tiếp cận bất cẩn loại bỏ bất kỳ giá trị nào có số lượng không lớn hơn giá trị sẽ xóa loại không chính xác`1`. 

Một trường hợp khác là khi một giá trị xuất hiện quá ít lần.```
2
3 3
```Kiểu`3`chỉ xuất hiện hai lần, nhưng nó sẽ cần ba bản sao để tồn tại. Vì không thể thêm các phần tử nên cả hai bản sao đều phải bị xóa. Câu trả lời là`2`. 

Một giá trị đặc biệt là`0`.```
4
0 0 1 1
```Nếu gõ`0`vẫn còn, nó sẽ cần xuất hiện chính xác bằng 0 lần, điều đó có nghĩa là nó hoàn toàn không thể tồn tại. Cả hai số 0 phải được loại bỏ. Kiểu`1`xuất hiện hai lần nên một bản sẽ bị xóa và một bản được giữ lại. Câu trả lời là`3`. Việc quên rằng số 0 không cần số lần xuất hiện dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là xử lý từng giá trị riêng biệt một cách độc lập. Đối với một giá trị`x`với tần số`f`, chúng ta có thể thử mọi khả năng và xác định xem việc giữ một số bản sao có hợp pháp hay không. Vì tần số còn lại hợp lệ duy nhất là chính xác`x`, chỉ có hai lựa chọn có ý nghĩa. Nếu như`f ≥ x`, chúng ta có thể giữ`x`sao chép và xóa`f - x`, nếu không chúng ta phải xóa tất cả`f`bản sao. 

Việc triển khai bạo lực ít có tổ chức hơn có thể liên tục quét mảng để tìm từng giá trị, dẫn đến khoảng`n²`hoạt động trong trường hợp xấu nhất. Với`n = 1000`, điều này vẫn hoạt động, nhưng nó thực hiện những công việc không cần thiết. 

Quan sát quan trọng là các giá trị khác nhau không tương tác. Quyết định về loại`5`không ảnh hưởng đến quyết định về loại`7`. Khi chúng tôi biết tần suất của mọi giá trị, số lần xóa tối thiểu cho từng loại sẽ được xác định độc lập. 

Nếu một giá trị`x`xuất hiện`f`lần, có hai trường hợp phát sinh. Khi`f < x`, việc giữ nguyên loại là không thể vì chúng tôi không thể tạo thêm bản sao, vì vậy tất cả`f`các lần xuất hiện sẽ bị xóa. Khi`f ≥ x`, chúng tôi giữ chính xác`x`sao chép và xóa phần bổ sung`f - x`. 

Bản đồ tần số cung cấp tất cả thông tin cần thiết và mỗi giá trị riêng biệt được xử lý một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và đếm số lần mỗi giá trị xuất hiện. 
2. Khởi tạo câu trả lời về 0. 
3. Với mọi giá trị phân biệt`x`với tần số`f`, xác định xem có đủ bản sao để giữ loại hay không. 
4. Nếu`f < x`, thêm vào`f`cho câu trả lời vì tất cả các bản sao của giá trị này phải được loại bỏ. 

Một giá trị không thể tồn tại với ít hơn`x`lần xuất hiện và không được phép thêm các phần tử. 
5. Nếu không, hãy thêm`f - x`cho câu trả lời bởi vì chính xác`x`bản sao nên vẫn còn. 

Mọi bản sao vượt quá số lượng yêu cầu đều không cần thiết và phải bị xóa. 
6. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Mỗi giá trị hoàn toàn độc lập với những giá trị khác. Đối với một loại`x`, tần số hợp pháp còn lại duy nhất là`x`. Nếu tần số hiện tại`f`nhỏ hơn`x`, không tồn tại cấu hình hợp lệ nào chứa loại đó, vì vậy việc xóa tất cả các bản sao là điều không thể tránh khỏi. Nếu như`f`ít nhất là`x`, giữ chính xác`x`các bản sao sẽ giảm thiểu việc xóa vì mọi tần số dương nhỏ hơn đều không hợp lệ và mọi tần số lớn hơn đều vi phạm điều kiện. Tổng hợp chi phí tối ưu cho mọi giá trị sẽ mang lại mức tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution
from collections import Counter

n = int(input())
a = list(map(int, input().split()))

cnt = Counter(a)

ans = 0

for x, f in cnt.items():
    if f < x:
        ans += f
    else:
        ans += f - x

print(ans)
```Phần đầu tiên đếm tần suất của mọi loại quái vật bằng cách sử dụng`Counter`. Vì thứ tự của mảng không liên quan nên tần số chứa tất cả thông tin cần thiết. 

Biến`ans`lưu trữ tổng số lần xóa. Với mỗi giá trị`x`, mã phân biệt hai trường hợp được mô tả trong thuật toán. 

Khi`f < x`, mọi lần xuất hiện đều bị loại bỏ vì đạt tần suất`x`là không thể. Nếu không thì chính xác`x`các bản sao được lưu giữ và phần dư thừa`f - x`các bản sao sẽ bị xóa. 

giá trị`0`được xử lý tự động. Từ`f < 0`không bao giờ đúng, nhánh thứ hai đóng góp`f - 0 = f`, nghĩa là tất cả các số 0 đều bị xóa, phù hợp với định nghĩa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
5
1 1 2 2 3
```| Giá trị x | Tần số f | Hành động | Đã thêm vào câu trả lời | Chạy Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | Giữ 1 bản | 1 | 1 | 
| 2 | 2 | Giữ 2 bản | 0 | 1 | 
| 3 | 1 | Xóa tất cả các bản sao | 1 | 2 | 

Câu trả lời cuối cùng là`2`. Một bản sao của loại`1`và bản sao duy nhất của loại`3`bị xóa. Mảng còn lại có thể là`[1, 2, 2]`. 

### Mẫu 2 

đầu vào```
10
1 2 3 2 4 4 4 4 5 2
```| Giá trị x | Tần số f | Hành động | Đã thêm vào câu trả lời | Chạy Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Giữ 1 bản | 0 | 0 | 
| 2 | 3 | Giữ 2 bản | 1 | 1 | 
| 3 | 1 | Xóa tất cả các bản sao | 1 | 2 | 
| 4 | 4 | Giữ 4 bản | 0 | 2 | 
| 5 | 1 | Xóa tất cả các bản sao | 1 | 3 | 

Câu trả lời là`3`. Kiểu`2`góp phần xóa một lần vì nó xuất hiện quá nhiều lần. Các loại`3`Và`5`không thể đạt được tần số yêu cầu nên tất cả các bản sao của loại đó sẽ bị xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm tần số và xử lý từng giá trị riêng biệt cùng nhau yêu cầu thời gian tuyến tính | 
| Không gian | O(n) | Bảng tần số có thể chứa tối đa n giá trị riêng biệt | 

Vì độ dài mảng tối đa là 1000 nên giải pháp này đủ nhanh. Ngay cả đối với các giới hạn lớn hơn nhiều, việc truyền tuyến tính qua các tần số vẫn có hiệu quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    cnt = Counter(a)

    ans = 0
    for x, f in cnt.items():
        if f < x:
            ans += f
        else:
            ans += f - x

    return str(ans)

# provided samples
assert run("5\n1 1 2 2 3\n") == "2", "sample 1"
assert run("10\n1 2 3 2 4 4 4 4 5 2\n") == "3", "sample 2"

# custom cases
assert run("1\n1\n") == "0", "minimum size"
assert run("4\n0 0 0 0\n") == "4", "all zeroes"
assert run("5\n5 5 5 5 5\n") == "0", "all equal and already balanced"
assert run("2\n3 3\n") == "2", "frequency smaller than value"
assert run("6\n2 2 2 2 1 1\n") == "3", "excess copies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Kích thước đầu vào tối thiểu | 
|`0 0 0 0`|`4`| Giá trị 0 phải biến mất | 
|`5 5 5 5 5`|`0`| Đã cân bằng mảng | 
|`3 3`|`2`| Không thể giữ giá trị với tần suất không đủ | 
|`2 2 2 2 1 1`|`3`| Loại bỏ các bản sao thừa một cách chính xác | 

## Vỏ cạnh 

Xem xét đầu vào```
2
3 3
```Bảng tần số chỉ chứa giá trị`3`với tần số`2`. Từ`2 < 3`, giữ kiểu`3`là không thể. Thuật toán bổ sung`2`đến câu trả lời và kết quả đầu ra`2`. Bất kỳ nỗ lực nào để giữ những con quái vật này sẽ để lại tần số`2`, vi phạm yêu cầu. 

Bây giờ hãy xem xét```
4
0 0 1 1
```Các tần số là`0 → 2`Và`1 → 2`. Đối với giá trị`0`, thuật toán đóng góp`2 - 0 = 2`, loại bỏ tất cả các số 0. Đối với giá trị`1`, nó góp phần`2 - 1 = 1`, giữ đúng một bản sao. Câu trả lời cuối cùng là`3`. 

Cuối cùng, hãy xem xét```
3
2 2 1
```Kiểu`2`xuất hiện hai lần và gõ`1`xuất hiện một lần. Cả hai tần số đều đã khớp với giá trị của chúng, vì vậy mỗi tần số đều không góp phần xóa. Câu trả lời là`0`, xác nhận rằng thuật toán không thay đổi cấu hình đã cân bằng.
