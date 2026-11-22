---
title: "CF 330B - Xây dựng đường bộ"
description: "Chúng ta có một đồ thị có n thành phố và ban đầu không có con đường nào. Một số cặp thành phố bị cấm, có nghĩa là chúng ta không được phép xây đường trực tiếp giữa chúng. Chúng ta phải cộng số lượng đường nhỏ nhất có thể để thỏa mãn hai điều kiện."
date: "2026-06-06T09:26:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs"]
categories: ["algorithms"]
codeforces_contest: 330
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 192 (Div. 2)"
rating: 1300
weight: 330
solve_time_s: 116
verified: false
draft: false
---

[CF 330B - Xây dựng đường bộ](https://codeforces.com/problemset/problem/330/B) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, đồ thị 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị với`n`thành phố và ban đầu không có đường. Một số cặp thành phố bị cấm, có nghĩa là chúng ta không được phép xây đường trực tiếp giữa chúng. 

Chúng ta phải cộng số lượng đường nhỏ nhất có thể để thỏa mãn hai điều kiện. 

Đầu tiên, mọi thành phố phải có thể truy cập được từ mọi thành phố khác. 

Thứ hai, khoảng cách giữa hai thành phố bất kỳ tối đa là 2, nghĩa là chúng ta có thể đi từ thành phố này sang thành phố khác bằng một hoặc hai con đường. 

Đầu vào mô tả các cặp bị cấm. Mỗi cặp không được liệt kê đều có sẵn để xây dựng. Bài toán đảm bảo rằng có ít nhất một lời giải hợp lệ tồn tại. 

Quan sát chính về các yêu cầu là chúng cực kỳ mạnh mẽ. Một biểu đồ được kết nối trên`n`đỉnh cần ít nhất`n - 1`các cạnh, bởi vì mọi đồ thị liên thông đều chứa một cây bao trùm. Vì chúng ta được hỏi về số lượng đường tối thiểu nên mọi câu trả lời hợp lệ đều phải sử dụng chính xác`n - 1`những con đường. 

Một đồ thị với`n - 1`các cạnh là một cái cây. Trong số các cây, cây duy nhất có đường kính tối đa bằng 2 là cây sao. Bất kỳ cây nào khác đều chứa đường đi có độ dài ít nhất là 3. 

Điều này ngay lập tức gợi ý rằng câu trả lời phải là một ngôi sao tập trung ở một thành phố nào đó. 

Một điểm tinh tế là chọn trung tâm. Nếu chúng ta chọn một thành phố có cặp cấm với thành phố khác, chúng ta không thể kết nối rìa ngôi sao với thành phố đó. Sự đảm bảo về vấn đề ngụ ý rằng tồn tại ít nhất một thành phố không liên quan đến bất kỳ cặp bị cấm nào. Tìm thành phố đó là toàn bộ vấn đề. 

Xem xét đầu vào```
4 1
1 3
```Thành phố 2 không bao giờ xuất hiện trong cặp cấm đoán. Một ngôi sao có tâm ở thành phố 2 sử dụng các cạnh`(2,1)`,`(2,3)`,`(2,4)`. Mỗi cặp thành phố đều được kết nối trực tiếp với 2 hoặc có thể gặp nhau qua 2 theo đúng hai bước. 

Một cách tiếp cận bất cẩn có thể cố gắng kết nối các thành phố một cách tham lam trong khi tránh các khu vực bị cấm. Ví dụ,```
4 2
1 2
3 4
```Xây dựng các con đường hợp pháp tùy ý có thể tạo ra một biểu đồ được kết nối, nhưng nó có thể sử dụng nhiều hơn`n - 1`các cạnh và sẽ không còn là tối thiểu. 

Một sai lầm dễ mắc phải khác là tìm kiếm một thành phố không bị cấm với từng thành phố khác. Vấn đề đơn giản hơn. Chúng ta chỉ cần một thành phố không bao giờ xuất hiện trong bất kỳ cặp cấm nào cả. Một thành phố như vậy có thể kết nối với tất cả mọi người vì mọi ranh giới bị cấm đều có sự tham gia của nó, điều này không bao giờ xảy ra. 

Ví dụ,```
5 3
1 2
1 3
1 4
```Thành phố 5 không xuất hiện trong danh sách cấm. Mọi cạnh`(5,x)`là hợp pháp, vì vậy một ngôi sao ở giữa số 5 là hợp lệ. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là thử mọi đồ thị có thể thỏa mãn các ràng buộc cạnh cấm và kiểm tra xem tất cả các khoảng cách theo cặp có tối đa là 2 hay không. Ngay cả khi chúng ta giới hạn bản thân trong các đồ thị được kết nối, số lượng tập hợp con cạnh có thể là hàm mũ theo số cạnh có sẵn. Điều này là hoàn toàn không thể thực hiện được. 

Một ý tưởng mạnh mẽ hợp lý hơn là thử nghiệm mọi thành phố như một trung tâm khả thi. Đối với mỗi thành phố, hãy kiểm tra xem nó có thể kết nối với mọi thành phố khác hay không, sau đó xây dựng ngôi sao tương ứng. Điều này đã gợi ý về cấu trúc thực sự của giải pháp. 

Quan sát quan trọng đến từ việc kết hợp hai sự kiện. 

Bất kỳ biểu đồ được kết nối nào trên`n`đỉnh cần ít nhất`n - 1`các cạnh. 

Một đồ thị có chính xác`n - 1`các cạnh là một cái cây. 

Trong số tất cả các cây, cách duy nhất để giữ mọi cặp đỉnh nằm trong khoảng cách 2 là sử dụng một ngôi sao. 

Vì vậy, thay vì xây dựng một đồ thị tùy ý, chúng ta chỉ cần xây dựng một ngôi sao. Câu hỏi còn lại là liệu một trung tâm hợp lệ có tồn tại hay không. 

Chúng ta hãy đánh dấu mỗi thành phố xuất hiện trong ít nhất một cặp bị cấm. Vì mỗi cạnh cấm đánh dấu cả hai điểm cuối nên bất kỳ thành phố nào không được đánh dấu sẽ không bao giờ tham gia vào một cặp bị cấm. Mọi ranh giới từ thành phố này đến thành phố khác đều hợp pháp. 

Việc đảm bảo vấn đề đảm bảo có ít nhất một thành phố như vậy tồn tại. Sau khi tìm thấy, chúng tôi kết nối nó với tất cả các thành phố khác. Kết quả là một ngôi sao với chính xác`n - 1`các cạnh, đó là mức tối thiểu có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng sao tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng boolean cho biết liệu mỗi thành phố có xuất hiện trong bất kỳ cặp bị cấm nào hay không. 
2. Đọc từng cặp cấm`(u, v)`. 
3. Đánh dấu cả hai`u`Và`v`như xuất hiện trong một cặp bị cấm. 
4. Sau khi xử lý tất cả các cặp, hãy tìm thành phố`center`điều đó chưa bao giờ được đánh dấu. 

Một thành phố như vậy không thuộc cặp cấm nào nên mọi con đường từ`center`đến một thành phố khác được phép. 
5. Đầu ra`n - 1`, bởi vì một ngôi sao trên`n`đỉnh chứa chính xác`n - 1`những con đường. 
6. Đối với mỗi thành phố khác nhau`center`, xuất đường`(center, city)`. 

### Tại sao nó hoạt động 

hãy để`center`là thành phố không bao giờ xuất hiện trong bất kỳ cặp cấm nào. 

Từ`center`vắng mặt trong tất cả các cặp bị cấm, mọi cạnh`(center, v)`là hợp pháp. Như vậy mọi con đường của ngôi sao đã xây dựng đều có thể xây dựng được. 

Biểu đồ kết quả được kết nối vì mọi thành phố đều được kết nối trực tiếp với`center`. 

Bất kỳ hai thành phố không phải là trung tâm đều có thể đến được với nhau thông qua`center`ở đúng hai con đường. Bất kỳ thành phố và trung tâm được kết nối bằng một con đường. Do đó mỗi cặp thành phố có khoảng cách tối đa là 2. 

Đồ thị chứa chính xác`n - 1`các cạnh. Không có đồ thị liên thông nào có thể sử dụng ít cạnh hơn nên việc xây dựng là tối thiểu. 

Vì bài toán đảm bảo tồn tại lời giải nên luôn có ít nhất một thành phố không bao giờ xuất hiện trong cặp cấm và thuật toán luôn tìm thấy một thành phố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    used = [False] * (n + 1)

    for _ in range(m):
        u, v = map(int, input().split())
        used[u] = True
        used[v] = True

    center = 1
    for i in range(1, n + 1):
        if not used[i]:
            center = i
            break

    print(n - 1)
    for i in range(1, n + 1):
        if i != center:
            print(center, i)

solve()
```Phần đầu tiên ghi lại những thành phố nào tham gia vào ít nhất một cặp bị cấm. Chúng ta không cần phải lưu trữ các cạnh bị cấm. Thông tin duy nhất quan trọng là liệu một thành phố có xuất hiện ở bất kỳ trong số đó hay không. 

Việc tìm kiếm`center`quét các thành phố một lần. Vì bài toán đảm bảo một giải pháp hợp lý nên tồn tại ít nhất một thành phố không được đánh dấu. 

Sau khi tìm được trung tâm thì việc xây dựng rất đơn giản. Chúng tôi kết nối trung tâm với mọi thành phố khác, tạo ra chính xác`n - 1`những con đường. 

Một lỗi triển khai phổ biến là lưu trữ các cạnh bị cấm và sau đó liên tục kiểm tra xem liệu`(center, i)`bị cấm. Công việc đó là không cần thiết. Theo cách xây dựng, một tâm không được đánh dấu sẽ không bao giờ xuất hiện trong bất kỳ cặp cấm nào, do đó mọi cạnh như vậy đều tự động hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
1 3
```Xử lý các cặp bị cấm: 

| Bước | Cặp | Thành phố được đánh dấu | 
| --- | --- | --- | 
| Ban đầu | - | {} | 
| 1 | (1,3) | {1,3} | 

Tìm tâm: 

| Thành phố | Đánh dấu? | 
| --- | --- | 
| 1 | Có | 
| 2 | Không | 

Vì thế`center = 2`. 

Các cạnh đầu ra: 

| Cạnh | 
| --- | 
| (2,1) | 
| (2,3) | 
| (2,4) | 

Đồ thị là một ngôi sao. Mỗi cặp lá giao tiếp qua thành phố 2 theo hai bước. 

### Ví dụ 2 

đầu vào:```
5 3
1 2
1 3
1 4
```Xử lý: 

| Bước | Cặp | Thành phố được đánh dấu | 
| --- | --- | --- | 
| 1 | (1,2) | {1,2} | 
| 2 | (1,3) | {1,2,3} | 
| 3 | (1,4) | {1,2,3,4} | 

Tìm tâm: 

| Thành phố | Đánh dấu? | 
| --- | --- | 
| 1 | Có | 
| 2 | Có | 
| 3 | Có | 
| 4 | Có | 
| 5 | Không | 

Vì thế`center = 5`. 

Các cạnh được xây dựng: 

| Cạnh | 
| --- | 
| (5,1) | 
| (5,2) | 
| (5,3) | 
| (5,4) | 

Ví dụ này chứng minh rằng trung tâm hoàn toàn không liên quan đến cấu trúc bị cấm. Nó chỉ cần tránh xuất hiện trong đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần vượt qua các cặp cấm và một lần vượt qua các thành phố | 
| Không gian | O(n) | Mảng được đánh dấu lưu trữ một boolean cho mỗi thành phố | 

Thuật toán chỉ thực hiện công việc tuyến tính ở kích thước đầu vào. Ngay cả đối với các giá trị được phép lớn nhất của`n`Và`m`, điều này dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ cũng ở mức tối thiểu, chỉ cần một mảng kích thước duy nhất`n`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())

    used = [False] * (n + 1)

    for _ in range(m):
        u, v = map(int, input().split())
        used[u] = True
        used[v] = True

    center = next(i for i in range(1, n + 1) if not used[i])

    out = [str(n - 1)]
    for i in range(1, n + 1):
        if i != center:
            out.append(f"{center} {i}")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""4 1
1 3
"""
) == (
"""3
2 1
2 3
2 4
"""
)

# minimum size
assert run(
"""2 0
"""
) == (
"""1
1 2
"""
)

# one city involved in all forbidden pairs
assert run(
"""5 3
1 2
1 3
1 4
"""
) == (
"""4
5 1
5 2
5 3
5 4
"""
)

# no forbidden pairs
assert run(
"""4 0
"""
) == (
"""3
1 2
1 3
1 4
"""
)

# smallest nontrivial center at the end
assert run(
"""4 2
1 2
2 3
"""
) == (
"""3
4 1
4 2
4 3
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0`| Cạnh đơn | Kích thước biểu đồ tối thiểu | 
| Cấm cặp tất cả liên lạc thành phố 1 | Sao tập trung ở nơi khác | Lựa chọn trung tâm đúng | 
| Không có cặp bị cấm | Ngôi sao tập trung ở thành phố đầu tiên | Bộ ràng buộc trống | 
| Center xuất hiện ở chỉ số cao nhất | Sử dụng trung tâm cuối cùng có thể | Quét chính xác | 
| Trường hợp mẫu | Xây dựng tối thiểu hợp lệ | Chức năng cơ bản | 

## Vỏ cạnh 

Hãy xem xét:```
4 0
```Không có thành phố nào xuất hiện trong cặp cấm. Thuật toán chọn thành phố 1 làm trung tâm và đưa ra kết quả:```
3
1 2
1 3
1 4
```Mỗi cặp thành phố không phải là trung tâm đều có khoảng cách 2 đến thành phố 1. 

Bây giờ hãy xem xét:```
5 4
1 2
1 3
1 4
1 5
```Mọi thành phố ngoại trừ 1 đều xuất hiện trong một cặp bị cấm và thành phố 1 xuất hiện trong tất cả các thành phố đó. Đầu vào này sẽ không đáp ứng được vấn đề đảm bảo vì mọi thành phố đều được đánh dấu. Sự đảm bảo loại trừ những trường hợp như vậy, đó là lý do tại sao thuật toán có thể giả định một cách an toàn rằng một thành phố không được đánh dấu tồn tại. 

Cuối cùng, hãy xem xét:```
6 3
1 2
2 3
3 4
```Thành phố 5 và 6 không bao giờ xuất hiện trong bất kỳ cặp cấm nào. Thuật toán có thể chọn thành phố 5. Nó xuất ra:```
5 1
5 2
5 3
5 4
5 6
```Mọi con đường đều hợp pháp vì thành phố 5 không bao giờ tham gia vào một cặp cấm. Mỗi thành phố có khoảng cách tối đa là 2 từ mọi thành phố khác qua trung tâm. Điều này xác nhận bất biến chính được sử dụng bởi giải pháp.
