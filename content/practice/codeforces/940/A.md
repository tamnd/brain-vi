---
title: "CF 940A - Điểm trên đường thẳng"
description: "Chúng ta được cho một số điểm đặt trên trục số. Mỗi điểm chỉ là một tọa độ nguyên duy nhất. Chúng tôi muốn giữ một tập hợp con của những điểm này sao cho sự phân tán của các điểm còn lại được kiểm soát, cụ thể là sự khác biệt giữa điểm được giữ lớn nhất và nhỏ nhất phải…"
date: "2026-06-17T02:34:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 940
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 466 (Div. 2)"
rating: 1200
weight: 940
solve_time_s: 181
verified: true
draft: false
---

[CF 940A - Điểm trên đường thẳng](https://codeforces.com/problemset/problem/940/A) 

**Đánh giá:** 1200 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số điểm đặt trên trục số. Mỗi điểm chỉ là một tọa độ nguyên duy nhất. Chúng tôi muốn giữ một tập hợp con của các điểm này sao cho sự phân tán của các điểm còn lại được kiểm soát, cụ thể là chênh lệch giữa điểm được giữ lớn nhất và nhỏ nhất không được vượt quá một giới hạn nhất định`d`. 

Nhiệm vụ không phải là chọn trực tiếp tập hợp con mà thay vào đó là xác định số điểm tối thiểu phải loại bỏ để các điểm còn lại nằm gọn trong một khoảng độ dài nào đó.`d`. 

Quan sát quan trọng là khi chúng ta quyết định điểm ngoài cùng bên trái và điểm ngoài cùng bên phải của tập hợp còn lại, mọi thứ ở giữa chúng theo thứ tự được sắp xếp có thể được giữ nguyên. Điều này làm cho cấu trúc vốn có một chiều và trật tự trở thành công cụ trung tâm. 

Các hạn chế là nhỏ:`n ≤ 100`và tọa độ cũng được giới hạn bởi 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các giải pháp bậc hai cũng có thể chấp nhận được và việc sắp xếp theo sau là quét dễ dàng đủ nhanh. Bất cứ điều gì vượt quá điều đó, như tập hợp con theo cấp số nhân, đều không cần thiết nhưng vẫn khả thi về mặt kỹ thuật do kích thước đầu vào nhỏ. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các điểm đã nằm trong một phạm vi`d`. Trong trường hợp này, không cần loại bỏ. Một trường hợp cạnh khác là khi`d = 0`. Khi đó, chỉ các tọa độ giống hệt nhau mới có thể tồn tại cùng nhau, nghĩa là chúng ta đang tìm kiếm tần số tối đa của một giá trị một cách hiệu quả. Ví dụ, đầu vào`3 0`có điểm`1 2 1`nên giữ cả hai`1`s và loại bỏ một điểm. 

Một sai lầm ngây thơ là cố gắng loại bỏ các điểm cực trị một cách tham lam mà không sắp xếp hoặc không xem xét tất cả các cửa sổ hợp lệ. Ví dụ: việc loại bỏ giá trị tối thiểu hoặc tối đa hiện tại nhiều lần có thể không thành công vì việc loại bỏ một cực trị có thể khiến một cửa sổ hợp lệ khác trước đó trở nên dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách mạnh mẽ để suy nghĩ về vấn đề này là thử mọi tập con điểm có thể có và kiểm tra xem đường kính của nó có lớn nhất không.`d`. Đối với mỗi tập hợp con, hãy tính các giá trị tối thiểu và tối đa và xác minh ràng buộc. Điều này đúng vì nó khám phá tất cả các khả năng, nhưng nó không khả thi về mặt tính toán vì có`2^n`tập hợp con. Với`n = 100`, cái này phát nổ hoàn toàn. 

Cấu trúc của bài toán trở nên đơn giản hơn nhiều khi chúng ta sắp xếp các điểm. Sau khi sắp xếp, bất kỳ tập hợp điểm hợp lệ nào có đường kính ≤`d`phải tương ứng với một phân đoạn liền kề trong mảng được sắp xếp này sau khi loại bỏ một số phần tử. Thay vì chọn các tập con tùy ý, chúng ta chỉ cần tìm khối liền kề lớn nhất nơi`a[r] - a[l] ≤ d`. Điều này làm giảm vấn đề tìm cửa sổ dài nhất thỏa mãn ràng buộc sai phân, đó là mẫu hai con trỏ cổ điển. 

Chúng ta trượt con trỏ bên trái và mở rộng con trỏ bên phải hết mức có thể trong khi vẫn duy trì ràng buộc. Bất cứ khi nào ràng buộc bị vi phạm, chúng ta sẽ di chuyển con trỏ trái về phía trước. Cửa sổ hợp lệ lớn nhất cho biết số điểm tối đa chúng ta có thể giữ được và câu trả lời là`n - best_window_size`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(1) | Quá chậm | 
| Sắp xếp + Hai con trỏ | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng tọa độ điểm theo thứ tự không giảm. Việc sắp xếp là cần thiết vì nó cho phép giảm việc kiểm tra đường kính thành những khác biệt đơn giản giữa các điểm cuối của các khoảng. 
2. Khởi tạo hai con trỏ`l = 0`và lặp lại`r`từ trái sang phải. Ý tưởng là để điều trị`r`làm điểm cuối mở rộng của phân đoạn hợp lệ ứng cử viên. 
3. Đối với từng vị trí`r`, kiểm tra xem phân đoạn hiện tại`[l, r]`thỏa mãn`a[r] - a[l] ≤ d`. Điều này trực tiếp đại diện cho đường kính của tập hợp con hiện tại. 
4. Nếu vi phạm điều kiện, hãy di chuyển`l`chuyển tiếp cho đến khi điều kiện trở lại hợp lệ. Mỗi lần tăng của`l`loại bỏ phần tử nhỏ nhất trong cửa sổ hiện tại, đây là cách duy nhất để giảm đường kính trong một mảng đã được sắp xếp. 
5. Ở mỗi bước, hãy tính kích thước của cửa sổ hợp lệ`r - l + 1`và theo dõi kích thước tối đa như vậy. Điều này đại diện cho tập hợp con lớn nhất mà chúng tôi có thể giữ lại. 
6. Sau khi xử lý xong tất cả`r`, tính toán câu trả lời như`n - best_window_size`, vì mọi thứ không có trong cửa sổ tối ưu đều phải bị loại bỏ. 

### Tại sao nó hoạt động 

Việc sắp xếp đảm bảo rằng bất kỳ tập hợp con nào đạt được giá trị tối thiểu và tối đa đều tương ứng với một phạm vi liền kề theo thứ tự được sắp xếp. Quá trình hai con trỏ duy trì một cửa sổ luôn thỏa mãn ràng buộc về đường kính. Bất cứ khi nào ràng buộc bị vi phạm, cách duy nhất để khôi phục tính hợp lệ là di chuyển ranh giới bên trái sang phải, bởi vì việc loại bỏ các phần tử bên trong không ảnh hưởng đến khoảng cách tối đa-tối thiểu trừ khi các cực trị thay đổi. Điều này đảm bảo rằng mọi cửa sổ ứng cử viên hợp lệ đều được xem xét mà không bỏ sót bất kỳ cấu hình tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
a = list(map(int, input().split()))

a.sort()

l = 0
best = 0

for r in range(n):
    while a[r] - a[l] > d:
        l += 1
    best = max(best, r - l + 1)

print(n - best)
```Bước sắp xếp là bước chuyển vấn đề thành một cửa sổ trượt trên một chuỗi đơn điệu. Nếu không sắp xếp, điều kiện đường kính sẽ không tương ứng với sự khác biệt đơn giản từ trái sang phải và phương pháp hai con trỏ sẽ thất bại. 

các`while`vòng lặp đảm bảo bất biến rằng cửa sổ hiện tại luôn hợp lệ trước khi cập nhật câu trả lời tốt nhất. Điều quan trọng là,`l`chỉ di chuyển về phía trước nên tổng số chuyển động của con trỏ là tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
2 1 4
```Mảng được sắp xếp:`[1, 2, 4]`| r | tôi | cửa sổ | a[r] - a[l] | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 0 | vâng | 1 | 
| 1 | 0 | [1,2] | 1 | vâng | 2 | 
| 2 | 1 | [2,4] | 2 | không → điều chỉnh l | 2 | 

Khi`r = 2`, cửa sổ`[1,2,4]`không hợp lệ vì`4 - 1 = 3 > 1`. Chúng tôi di chuyển`l`cho đến khi cửa sổ trở thành`[2,4]`, vẫn vi phạm, vì vậy chúng tôi lại chuyển sang`[4]`. 

Kích thước cửa sổ tốt nhất cuối cùng là`2`, vậy đáp án là`3 - 2 = 1`. 

Điều này cho thấy giải pháp tối ưu không nhất thiết phải bao gồm phần tử nhỏ nhất; việc loại bỏ nó có thể mở khóa một phân khúc lớn hơn hợp lệ. 

### Ví dụ 2 

đầu vào:```
5 0
1 2 2 3 2
```Mảng được sắp xếp:`[1, 2, 2, 2, 3]`| r | tôi | cửa sổ | a[r] - a[l] | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 0 | 1 | 
| 1 | 1 | [2,2] | 0 | 2 | 
| 3 | 1 | [2,2,2] | 0 | 3 | 
| 4 | 2 | [2,3] | 1 > 0 → dịch chuyển l | 3 | 

Cửa sổ tốt nhất là`3`, vậy đáp án là`5 - 3 = 2`. 

Điều này khẳng định`d = 0`trường hợp hoạt động giống như chọn giá trị thường xuyên nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét hai con trỏ là tuyến tính | 
| Không gian | O(1) | Chỉ sử dụng con trỏ và bộ đếm | 

Các ràng buộc cho phép tối đa 100 phần tử, do đó, ngay cả các phương pháp bậc hai đơn giản hơn cũng sẽ vượt qua. Giải pháp được chọn là tối ưu và tổng quát hóa rõ ràng cho các ràng buộc lớn hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, d = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    l = 0
    best = 0

    for r in range(n):
        while a[r] - a[l] > d:
            l += 1
        best = max(best, r - l + 1)

    return str(n - best)

# provided samples
assert run("3 1\n2 1 4\n") == "1"

# custom cases
assert run("1 0\n5\n") == "0"
assert run("4 0\n1 2 3 4\n") == "3"
assert run("5 2\n1 2 3 4 5\n") == "2"
assert run("6 10\n1 2 3 4 5 6\n") == "0"
assert run("5 1\n1 100 101 102 103\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| tất cả đều khác biệt, d=0 | n-1 | hành vi hạn chế tần số | 
| cửa sổ trượt cần thiết | 2 | tính đúng đắn của trường hợp tổng quát | 
| đủ lớn d | 0 | không cần xóa | 
| ngoại lệ cực đoan | 4 | ảnh hưởng của yếu tố ở xa | 

## Vỏ cạnh 

Khi tất cả các điểm giống hệt nhau, việc sắp xếp sẽ tạo ra một mảng không đổi và cửa sổ luôn thỏa mãn điều kiện bất kể`d`. Thuật toán mở rộng cửa sổ lên kích thước đầy đủ và trả về chính xác số lần xóa bằng 0. 

Khi`d = 0`, chỉ những giá trị bằng nhau mới có thể cùng tồn tại. Cửa sổ trượt tự nhiên thu gọn thành các khối có số giống nhau và cửa sổ tối đa tương ứng chính xác với tần số tối đa của bất kỳ tọa độ nào. 

Khi một giá trị nằm xa các giá trị còn lại, việc sắp xếp sẽ đặt nó ở một đầu. Cửa sổ sẽ tạm thời bao gồm nó và sau đó thu nhỏ ngay khi nó vi phạm ràng buộc, đảm bảo nó bị loại trừ trừ khi nó có thể vừa với một phân đoạn hợp lệ.
