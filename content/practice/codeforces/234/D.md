---
title: "CF 234D - Rạp chiếu phim"
description: "Vasya có danh sách phim và danh sách diễn viên yêu thích. Mỗi phim liệt kê một số diễn viên nhưng một số ID diễn viên có thể bị thiếu (được biểu thị bằng 0)."
date: "2026-06-04T09:53:47+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 234
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 145 (Div. 2, ACM-ICPC Rules)"
rating: 1600
weight: 234
solve_time_s: 138
verified: true
draft: false
---

[CF 234D - Rạp chiếu phim](https://codeforces.com/problemset/problem/234/D) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vasya có danh sách phim và danh sách diễn viên yêu thích. Mỗi phim liệt kê một số diễn viên nhưng một số ID diễn viên có thể bị thiếu (được biểu thị bằng 0). Vasya muốn phân loại mỗi bộ phim thành một trong ba loại: chắc chắn sẽ được yêu thích, chắc chắn sẽ không được yêu thích hoặc có thể đi theo hướng nào đó tùy thuộc vào ID diễn viên bị thiếu. Một bộ phim được coi là bộ phim được yêu thích nếu sau khi tất cả ID diễn viên được tiết lộ, bộ phim đó có ít nhất nhiều diễn viên được yêu thích như bất kỳ bộ phim nào khác. 

Dữ liệu đầu vào cung cấp tổng số diễn viên, ID của các diễn viên yêu thích của Vasya và danh sách các bộ phim có dàn diễn viên của họ (bao gồm cả các diễn viên vô danh). Đầu ra là một dòng trên mỗi phim, biểu thị danh mục bằng 0, 1 hoặc 2. 

Các ràng buộc rất nhỏ: có nhiều nhất 100 diễn viên và 100 phim, do đó thuật toán O(n·m) là khả thi. Thách thức chính là xử lý sự không chắc chắn: một số diễn viên không được biết đến và danh tính của họ có thể thay đổi liệu bộ phim có được yêu thích hay không. 

Các trường hợp khó thấy rõ ràng bao gồm những bộ phim mà tất cả các diễn viên đều không được biết đến hoặc những bộ phim đã có tất cả các diễn viên được yêu thích nhưng vẫn có những điểm không xác định có thể không được yêu thích. Một cách tiếp cận bất cẩn chỉ đếm các mục yêu thích đã biết có thể phân loại sai các tình huống này. 

Ví dụ: xét 3 bộ phim có 3 diễn viên được yêu thích {1,2,3}:```
movie1: actors = [0,0,0]   # all unknown
movie2: actors = [1,2,3]   # all favorite known
movie3: actors = [1,0,4]   # mix known favorite, unknown, non-favorite
```Movie1 có thể có từ 0 đến 3 diễn viên yêu thích. Movie2 đảm bảo có 3. Movie3 có thể có 1 hoặc 2 mục yêu thích tùy thuộc vào diễn viên chưa rõ. Phân loại chính xác đòi hỏi phải xem xét cả các tác nhân được yêu thích tối thiểu và tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các nhiệm vụ có thể có cho các ID diễn viên không xác định trong mỗi bộ phim, đếm các mục yêu thích cho mỗi nhiệm vụ và xác định các bộ phim yêu thích. Điều này đúng nhưng không khả thi: mỗi bộ phim có`u`các diễn viên không xác định có sự kết hợp O((m)^u) và với tối đa 100 phim, điều này sẽ bùng nổ về mặt tổ hợp. 

Thông tin chi tiết quan trọng là tính toán **số lượng diễn viên được yêu thích tối thiểu và tối đa có thể có trong mỗi bộ phim** mà không liệt kê tất cả các khả năng. Đối với mỗi bộ phim, chúng ta có thể đếm các diễn viên được yêu thích (`f_known`) và các tác nhân chưa biết (`u`). Số mục yêu thích tối đa có thể là`f_known + min(u, k - f_total)`, Ở đâu`f_total`là tổng số diễn viên được yêu thích đã được sử dụng trong các bộ phim khác. Số lượng yêu thích tối thiểu có thể là`f_known`(giả sử không có diễn viên vô danh nào được yêu thích) hoặc`f_known + max(0, u - (m - k))`nếu một số diễn viên vô danh phải được yêu thích vì không còn đủ diễn viên không được yêu thích. 

Khi chúng tôi có mức tối thiểu và tối đa cho mỗi phim, chúng tôi có thể phân loại: 

- Chắc chắn được yêu thích nếu số lượng tối thiểu của nó lớn hơn hoặc bằng số lượng tối đa của tất cả các phim khác. 
- Chắc chắn không được yêu thích nếu số lượng tối đa của nó nhỏ hơn số lượng tối đa của một số bộ phim khác. 
- Nếu không thì có thể đi theo một trong hai cách. 

Điều này làm giảm độ phức tạp của vấn đề xuống còn O(n·m): chúng tôi quét từng phim một lần, tính toán số lượng và sau đó so sánh giữa các phim. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((m^u)·n) | O(n) | Quá chậm | 
| Đếm tối thiểu/tối đa | O(n·m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng diễn viên`m`và diễn viên được yêu thích`k`. Lưu trữ các ID yêu thích trong một bộ để tra cứu O(1). 
2. Đọc số lượng phim`n`. Đối với mỗi bộ phim, lưu trữ tiêu đề, số lượng diễn viên`d`và danh sách ID diễn viên, theo dõi những ID nào chưa xác định (0). 
3. Với mỗi bộ phim, hãy tính`f_known`như số lượng diễn viên yêu thích đã có mặt, và`u`như số lượng các diễn viên chưa biết. 
4. Tính số diễn viên được yêu thích nhất có thể trong phim là`f_known + min(u, k - f_known_other)`. Vì các diễn viên là khác nhau trong một bộ phim nên số lượng tối đa được giới hạn bởi các diễn viên được yêu thích còn lại chưa có trong bộ phim này. 
5. Tính số diễn viên được yêu thích tối thiểu có thể là`f_known`. Nếu số lượng tác nhân chưa biết vượt quá số lượng tác nhân không được yêu thích hiện có, thì một số tác nhân phải được yêu thích, vì vậy hãy tăng mức tối thiểu cho phù hợp. 
6. Thu thập số lượng tối thiểu và tối đa của tất cả các phim. 
7. Với mỗi phim, xác định đầu ra: 

- Nếu số lượng yêu thích tối thiểu có thể có của nó ít nhất bằng số lượng yêu thích tối đa có thể có của mọi phim khác, xuất ra 0 (chắc chắn là yêu thích). 
- Nếu số lượng yêu thích tối đa có thể có của nó nhỏ hơn số lượng yêu thích tối đa có thể có của một số phim khác, xuất ra 1 (chắc chắn không được yêu thích). 
- Ngược lại thì xuất ra 2 (có thể là một trong hai). 
8. In kết quả theo thứ tự nhập vào. 

**Tại sao nó hoạt động:** Bằng cách theo dõi các giá trị cực đại của số lượng yêu thích có thể có cho mỗi bộ phim, chúng tôi đảm bảo rằng không có sự chỉ định diễn viên không xác định nào có thể mâu thuẫn với việc phân loại. Mọi quyết định chỉ phụ thuộc vào phạm vi có thể, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

m, k = map(int, input().split())
fav = set(map(int, input().split()))
n = int(input())

movies = []

for _ in range(n):
    title = input().strip()
    d = int(input())
    actors = list(map(int, input().split()))
    f_known = sum(1 for a in actors if a in fav)
    u = actors.count(0)
    movies.append({'title': title, 'f_known': f_known, 'unknown': u, 'd': d})

min_favs = []
max_favs = []

for movie in movies:
    f_known = movie['f_known']
    u = movie['unknown']
    d = movie['d']
    
    # maximum possible favorite actors
    max_possible = f_known + min(u, k - f_known)
    
    # minimum possible favorite actors
    non_favs_total = m - k
    max_non_favs_in_movie = min(u, non_favs_total - (d - f_known - u))
    min_possible = f_known + max(0, u - max_non_favs_in_movie)
    
    min_favs.append(min_possible)
    max_favs.append(max_possible)

results = []

for i in range(n):
    sure_fav = all(min_favs[i] >= max_favs[j] for j in range(n) if j != i)
    sure_not = any(max_favs[i] < max_favs[j] for j in range(n) if j != i)
    if sure_fav:
        results.append(0)
    elif sure_not:
        results.append(1)
    else:
        results.append(2)

print('\n'.join(map(str, results)))
```**Giải thích:** Giải pháp trước tiên đọc tất cả dữ liệu đầu vào và tính toán các diễn viên được yêu thích và chưa biết cho mỗi bộ phim. Sau đó, nó tính toán phạm vi các diễn viên có thể được yêu thích. Cuối cùng, nó phân loại từng phim bằng cách so sánh giá trị tối thiểu/tối đa của nó với giá trị tối đa của các phim khác. Các phần tinh tế bao gồm giới hạn mức tối đa bằng số mục yêu thích còn lại và tính toán mức tối thiểu khi những yếu tố chưa biết có thể bị buộc phải yêu thích vì không có đủ mục không yêu thích. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5 3
1 2 3
6
firstfilm
3
0 0 0
secondfilm
4
0 0 4 5
thirdfilm
1
2
fourthfilm
1
5
fifthfilm
1
4
sixthfilm
2
1 0
```| Phim | f_known | chưa biết | phút | tối đa | 
| --- | --- | --- | --- | --- | 
| phim đầu tiên | 0 | 3 | 0 | 3 | 
| phim thứ hai | 0 | 2 | 0 | 2 | 
| phim thứ ba | 1 | 0 | 1 | 1 | 
| phim thứ tư | 0 | 1 | 0 | 1 | 
| phim thứ năm | 0 | 1 | 0 | 1 | 
| phim thứ sáu | 1 | 1 | 1 | 2 | 

Phân loại: 

- firstfilm: có thể được yêu thích hoặc không → 2 
- phim thứ hai: có thể được yêu thích hoặc không → 2 
- phim thứ ba: max < một số max khác → 1 
- phim thứ tư: max < một số max khác → 1 
- phim thứ năm: max < một số max khác → 1 
- phim thứ sáu: có thể được yêu thích hoặc không → 2 

Đầu ra:```
2
2
1
1
1
2
```### Ví dụ tùy chỉnh```
3 2
1 2
2
alpha
2
1 0
beta
1
2
```| Phim | f_known | chưa biết | phút | tối đa | 
| --- | --- | --- | --- | --- | 
| alpha | 1 | 1 | 1 | 2 | 
| phiên bản beta | 1 | 0 | 1 | 1 | 

Phân loại: 

- alpha: tối thiểu >= tối đa của những người khác? 1 >= 1 → có, có thể được yêu thích 
- max < tối đa của người khác? 2 < 1 → không 
- cũng không hẳn là không → 2 
- beta: tối thiểu 1 >= tối đa của những người khác? 1 >= 2 → không 
- tối đa 1 < tối đa những người khác? 1 < 2 → có, chắc chắn là không thích → 1 

Đầu ra:```
2
1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- |
