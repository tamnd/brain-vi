---
title: "CF 1851A - Hội thoại thang cuốn"
description: "Chúng ta được cung cấp một cấu trúc thang cuốn cố định và một nhóm người có chiều cao đã biết. Vlad cũng có chiều cao. Thang cuốn có các bậc cách đều nhau và mỗi bậc tăng chiều cao một giá trị không đổi."
date: "2026-06-09T05:24:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1851
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 888 (Div. 3)"
rating: 800
weight: 1851
solve_time_s: 78
verified: true
draft: false
---

[CF 1851A - Cuộc trò chuyện trên thang cuốn](https://codeforces.com/problemset/problem/1851/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc thang cuốn cố định và một nhóm người có chiều cao đã biết. Vlad cũng có chiều cao. Thang cuốn có các bậc cách đều nhau và mỗi bậc tăng chiều cao một giá trị không đổi. Câu hỏi không phải là về chuyển động vật lý mà là liệu hai người có thể được đặt trên hai bậc thang khác nhau sao cho khoảng cách thẳng đứng giữa họ khớp chính xác với sự khác biệt về chiều cao của họ hay không. 

Đối với mỗi người trong đầu vào, chúng ta cần quyết định độc lập xem có tồn tại một cặp bước cho Vlad và người đó sao cho cả ràng buộc chênh lệch bước và ràng buộc chênh lệch độ cao đều căn chỉnh hoàn hảo hay không. Nếu có một vị trí như vậy, chúng tôi sẽ tính người đó. 

Quan sát quan trọng là độ chênh lệch của các bước luôn là bội số của một giá trị cố định, vì mỗi bước tăng chiều cao một cách chính xác.`k`. Điều này ngay lập tức biến vấn đề thành việc kiểm tra xem liệu chênh lệch độ cao nhất định có thể được biểu thị dưới dạng`d * k`đối với một số nguyên`d`phù hợp với giới hạn của thang cuốn. 

Các ràng buộc rất nhỏ: tối đa 1000 trường hợp thử nghiệm và nhiều nhất là 50 người cho mỗi trường hợp thử nghiệm. Điều này đảm bảo rằng O(n) cho mỗi giải pháp trường hợp thử nghiệm là đủ và thậm chí chỉ cần kiểm tra số học đơn giản cho mỗi người là đủ. 

Trường hợp cạnh chung phát sinh khi chênh lệch độ cao bằng 0 hoặc khi nó không chia hết cho`k`. Một trường hợp tinh tế khác là khi độ chênh lệch bước yêu cầu vượt quá số bước có sẵn, ngay cả khi tính chia hết được giữ nguyên. Cả hai điều kiện phải được thực thi đồng thời. 

Một sai lầm ngây thơ là cố gắng mô phỏng tất cả các cặp bước cho mỗi người, điều này là không cần thiết và có nguy cơ bỏ qua thực tế rằng chỉ có sự khác biệt mới quan trọng chứ không phải vị trí tuyệt đối. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử từng cặp bước cho Vlad và mọi người, kiểm tra xem liệu có sự liên kết hợp lệ giữa chênh lệch bước và chênh lệch chiều cao hay không. Đối với mỗi người, điều này sẽ yêu cầu lặp lại tất cả`m * m`cặp bước, dẫn đến O(n m^2) cho mỗi trường hợp thử nghiệm. Mặc dù vẫn còn hạn chế về mặt kỹ thuật nhưng nó không hiệu quả về mặt khái niệm và che khuất cấu trúc của vấn đề. 

Sự đơn giản hóa chính là nhận ra rằng điều duy nhất quan trọng là sự chênh lệch chiều cao tuyệt đối giữa Vlad và một người khác. Một khi sự khác biệt này được khắc phục, câu hỏi duy nhất là liệu chúng ta có thể nhận ra nó bằng cách sử dụng hình học thang cuốn hay không. Vì mỗi bước đóng góp chính xác`k`chênh lệch chiều cao, chúng tôi đang kiểm tra xem sự khác biệt có chia hết cho`k`và liệu khoảng cách bước ngụ ý có phù hợp với`[1, m-1]`. 

Điều này làm giảm bài toán thành điều kiện số học trực tiếp cho mỗi người, loại bỏ mọi lý luận tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n m^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chênh lệch chiều cao tuyệt đối giữa Vlad và mỗi người. Điều này thể hiện tổng khoảng cách theo chiều dọc phải được giải thích bằng các bước thang cuốn. 
2. Kiểm tra xem hiệu này có chia hết cho không`k`. Nếu không, thì không có chuỗi chuyển động bước nào có thể khớp chính xác với khoảng cách chiều cao, bởi vì mỗi bước đóng góp chính xác`k`đơn vị. 
3. Nếu chia hết, hãy tính khoảng cách bước cần thiết là`diff // k`. 
4. Xác minh rằng khoảng cách bước này nằm trong khoảng từ 1 đến`m - 1`. Chúng tôi yêu cầu hai bước khác nhau, vì vậy số 0 là không hợp lệ và chúng tôi không thể vượt quá phạm vi thang cuốn. 
5. Đếm tất cả những người thỏa mãn cả hai điều kiện. 

### Tại sao nó hoạt động 

Thang cuốn tạo ra một số liệu tuyến tính rời rạc trong đó chênh lệch độ cao hợp lệ chính xác là bội số nguyên của`k`. Bất kỳ cuộc hội thoại hợp lệ nào đều tương ứng với việc chọn hai bước riêng biệt có chênh lệch chỉ số khớp với thương số của chênh lệch chiều cao chia cho`k`. Vì các bước được giới hạn giữa 1 và`m`, chênh lệch chỉ số tối đa có thể đạt được là`m - 1`. Điều này mô tả đầy đủ tất cả các cấu hình hợp lệ, vì vậy việc kiểm tra tính phân chia và giới hạn là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, k, H = map(int, input().split())
    h = list(map(int, input().split()))
    
    ans = 0
    
    for x in h:
        diff = abs(x - H)
        
        if diff == 0:
            continue
        
        if diff % k != 0:
            continue
        
        d = diff // k
        
        if 1 <= d <= m - 1:
            ans += 1
    
    print(ans)
```Sau khi đọc dữ liệu đầu vào, chúng tôi duyệt qua từng người và tính toán chênh lệch chiều cao tuyệt đối của họ với Vlad. Chúng tôi loại bỏ ngay các trường hợp có chênh lệch bằng 0 vì cần có hai bước khác nhau. Sau đó, chúng tôi thực thi điều kiện chia hết bằng cách`k`, đảm bảo kích thước bậc thang cuốn có thể đại diện cho khoảng cách chiều cao. Cuối cùng, chúng tôi kiểm tra xem khoảng cách bước ngụ ý có phù hợp với phạm vi thang cuốn hay không. Mỗi người hợp lệ sẽ tăng câu trả lời. 

Việc triển khai được cố ý tối thiểu vì tất cả cấu trúc đều bị nắm bắt bởi các ràng buộc số học này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, m = 3, k = 3, H = 11
h = [5, 4, 14, 18, 2]
```| Người | Chênh lệch chiều cao | khác biệt %k | khoảng cách bước | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 5 | 6 | 0 | 2 | vâng | 
| 4 | 7 | - | - | không | 
| 14 | 3 | 0 | 1 | vâng | 
| 18 | 7 | - | - | không | 
| 2 | 9 | 0 | 3 | không | 

Chúng tôi đếm hai người hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 3, m = 1, k = 4, H = 10
h = [18, 6, 14]
```| Người | Chênh lệch chiều cao | khác biệt %k | khoảng cách bước | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 18 | 8 | 0 | 2 | không | 
| 6 | 4 | 0 | 1 | không | 
| 14 | 4 | 0 | 1 | không | 

Không có cặp hợp lệ nào tồn tại vì chỉ có một bước. 

Những ví dụ này xác nhận rằng cả ràng buộc về tính chia hết và phạm vi bước phải được thực thi đồng thời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi người được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và bộ lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 1000 trường hợp kiểm thử và mỗi trường hợp 50 phần tử, do đó việc quét tuyến tính trên mỗi trường hợp kiểm thử dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k, H = map(int, input().split())
        h = list(map(int, input().split()))
        ans = 0
        for x in h:
            diff = abs(x - H)
            if diff == 0:
                continue
            if diff % k == 0 and 1 <= diff // k <= m - 1:
                ans += 1
        out.append(str(ans))
    return "\n".join(out)

# provided sample
assert run("""7
5 3 3 11
5 4 14 18 2
2 9 5 6
11 9
10 50 3 11
43 44 74 98 62 60 99 4 11 73
4 8 8 49
68 58 82 73
7 1 4 66
18 66 39 83 48 99 79
9 1 1 13
26 23 84 6 60 87 40 41 25
6 13 3 28
30 70 85 13 1 55""") == """2
1
4
1
0
0
3"""

# custom cases
assert run("""1
1 10 2 5
7""") == "1", "single valid match"
assert run("""1
3 2 5 10
1 2 3""") == "0", "no divisibility case"
assert run("""1
4 3 1 10
11 9 8 10""") == "3", "k=1 dense reachability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn gần khớp | 1 | trường hợp chia hết cơ bản | 
| không có trận đấu | 0 | không thể thực hiện được do ràng buộc k | 
| k = 1 trường hợp | 3 | trường hợp cạnh khả năng tiếp cận đầy đủ | 

## Vỏ cạnh 

Khi tất cả các chiều cao bằng chiều cao của Vlad, mọi sự khác biệt đều bằng 0 và không có sự khác biệt nào được tính vì bài toán yêu cầu hai bước khác nhau, khiến việc tự khớp không hợp lệ. 

Khi`k`là lớn so với tất cả các khác biệt về chiều cao, hầu hết các khác biệt đều không vượt qua được việc kiểm tra khả năng chia hết, dẫn đến không có cuộc hội thoại hợp lệ nào ngay cả khi những khác biệt thô có vẻ gần giống nhau. 

Khi`m = 1`, không tồn tại cặp bước riêng biệt nào, vì vậy câu trả lời luôn bằng 0 bất kể giá trị độ cao là bao nhiêu, vì ràng buộc khoảng cách bước không thể được thỏa mãn.
