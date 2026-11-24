---
title: "CF 69B - Cược"
description: "Mỗi vận động viên chạy qua một khoảng thời gian liền kề nhau. Khi một vận động viên ở trong một phần thi, họ dành chính xác t[i] thời gian cho phần thi đó, vì vậy người chiến thắng trong phần thi đó chỉ đơn giản là vận động viên tích cực có t nhỏ nhất."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 69
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 63 (Div. 2)"
rating: 1200
weight: 69
solve_time_s: 121
verified: true
draft: false
---

[CF 69B - Cược](https://codeforces.com/problemset/problem/69/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi vận động viên chạy qua một khoảng thời gian liền kề nhau. Khi một vận động viên ở trong một khu vực thi đấu, họ dành chính xác`t[i]`thời gian trong phần thi đó, vì vậy người chiến thắng trong phần thi đó chỉ đơn giản là vận động viên tích cực có thành tích nhỏ nhất`t`. Nếu nhiều vận động viên có cùng thời gian thì chỉ số nhỏ hơn sẽ thắng. 

Chúng tôi được phép đặt tối đa một cược cho mỗi phần thi và chỉ đặt cược vào các vận động viên thực sự chạy trong phần thi đó. Nếu chúng ta đặt cược vào người chiến thắng phần thi, chúng ta sẽ nhận được phần thưởng của vận động viên đó`c[i]`. Vì các phần là độc lập nên nhiệm vụ sẽ trở thành: 

Đối với mỗi phần, hãy xác định vận động viên nào thắng ở đó, sau đó cộng lợi nhuận của vận động viên đó. 

Các hạn chế rất nhỏ, chỉ tối đa 100 phần và 100 vận động viên. Thậm chí một`O(n * m^2)`giải pháp sẽ trôi qua một cách thoải mái vì trường hợp xấu nhất chỉ khoảng một triệu thao tác. Điều này có nghĩa là chúng tôi không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa đường quét. Một mô phỏng trực tiếp là đủ. 

Phần khó khăn không phải là hiệu suất, mà là việc thực hiện chính xác các quy tắc chiến thắng. 

Một sai lầm dễ mắc phải là quên quy tắc ràng buộc theo chỉ số. 

Coi như:```
3 2
1 3 5 10
1 3 5 20
```Cả hai vận động viên đều có tốc độ ngang nhau ở mọi nơi. Vận động viên 1 thắng mọi phần thi vì chỉ số nhỏ hơn nên đáp án là:```
30
```Việc thực hiện bất cẩn chọn phần thưởng lớn hơn thay vì người chiến thắng chính thức sẽ đưa ra kết quả không chính xác`60`. 

Một trường hợp tế nhị khác là những đoạn đường không có vận động viên.```
5 1
2 4 3 10
```Phần 1 và 5 không có người tham gia nên không có cược nào có thể thắng ở đó. Chỉ có phần 2, 3 và 4 đóng góp lợi nhuận. Câu trả lời đúng là:```
30
```Việc triển khai giả định mọi phần luôn có người chiến thắng sẽ truy cập vào dữ liệu không hợp lệ hoặc tính thêm lợi nhuận. 

Lỗi phổ biến thứ ba là kiểm tra tổng thời gian đua thay vì thời gian mỗi phần.```
4 2
1 4 10 5
2 2 1 100
```Vận động viên 2 chỉ xuất hiện ở phần 2 nhưng ở đó họ nhanh hơn rất nhiều và giành chiến thắng ở phần đó. Tổng đúng là: 

Phần 1 -> vận động viên 1, đạt 5 

Phần 2 -> vận động viên 2, đạt 100 

Phần 3 và 4 -> vận động viên 1, mỗi phần tăng 5 

Tổng số:```
115
```Toàn bộ thời gian cuộc đua không thành vấn đề. Chỉ có thời gian cho mỗi phần`t[i]`vấn đề bên trong mỗi phần. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng phần một cách độc lập. Đối với một phần cố định, chúng tôi quét tất cả các vận động viên và chỉ giữ lại những người có khoảng thời gian bao gồm phần này. Trong số những vận động viên tích cực đó, chúng tôi chọn người có vóc dáng nhỏ nhất.`t`. Nếu một số vận động viên có chung`t`, chúng tôi chọn chỉ số nhỏ hơn. Sau đó, chúng tôi thêm phần thưởng của vận động viên đó vào câu trả lời. 

Ý tưởng vũ phu này đã đủ nhanh rồi. Có tối đa 100 phần và 100 vận động viên nên chúng tôi thực hiện tối đa 10.000 lần kiểm tra vận động viên. Ngay cả khi chúng tôi thêm các so sánh bổ sung để phá vỡ mối quan hệ, thời gian chạy vẫn không đáng kể. 

Lý do phương pháp đơn giản này hoạt động là vì vấn đề không có sự tương tác giữa các phần. Đặt cược vào một phần không ảnh hưởng đến phần khác. Khi chúng ta đã biết người chiến thắng trong một phần thi, chúng ta buộc phải đưa ra lựa chọn tối ưu: đặt cược vào người chiến thắng đó và nhận phần thưởng. Không bao giờ có lý do để đặt cược vào bất cứ ai khác. 

Một cách giải thích phức tạp hơn có thể gợi ý lập trình động hoặc xử lý theo khoảng thời gian vì các vận động viên chiếm giữ nhiều phần thi. Nhưng quan sát quan trọng là người chiến thắng được xác định theo từng khu vực, từng khu vực. Khoảng thời gian chỉ cho chúng tôi biết liệu một vận động viên có tham gia vào một phần thi hay không. 

Chúng ta có thể coi thuật toán như mô phỏng dòng thời gian của cuộc đua. Đối với mỗi phần: 

1. Tìm tất cả các vận động viên đang hoạt động. 
2. Chọn nhanh nhất. 
3. Thêm phần thưởng của họ. 

Đó là toàn bộ giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n * m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số phần thi và vận động viên. 
2. Lưu trữ tất cả vận động viên dưới dạng bộ dữ liệu`(l, r, t, c)`. 
3. Lặp lại từng phần từ`1`ĐẾN`n`. 
4. Đối với phần hiện tại, hãy quét tất cả các vận động viên và kiểm tra xem vận động viên đó có hoạt động ở đó hay không, nghĩa là`l <= section <= r`. 
5. Trong số tất cả các vận động viên đang hoạt động, hãy giữ một người có: 

1. nhỏ nhất`t`2. và nếu thời gian bằng nhau thì chỉ số vận động viên nhỏ nhất 

Điều này hoàn toàn phù hợp với các quy tắc cuộc đua. 
6. Nếu có ít nhất một vận động viên hoạt động trong phần này, hãy thêm phần thưởng của người chiến thắng đó`c`để trả lời. 
7. Sau khi xử lý tất cả các phần, hãy in tổng lợi nhuận. 

### Tại sao nó hoạt động 

Đối với mỗi phần, kết quả hoàn toàn độc lập với mọi phần khác. Người chiến thắng chính thức được xác định duy nhất dựa trên thời gian thi đấu tối thiểu và quy tắc hòa về chỉ số. 

Nếu chúng ta đặt cược vào bất kỳ ai ngoại trừ người chiến thắng, chúng ta sẽ không nhận được gì từ phần đó. Đặt cược vào người chiến thắng mang lại kết quả chính xác`c[i]`. Vì không có hạn chế khi kết nối các phần khác nhau nên việc chọn người chiến thắng một cách độc lập cho mỗi phần sẽ tối đa hóa tổng lợi nhuận. 

Thuật toán tính toán rõ ràng người chiến thắng chính thức cho mọi phần, do đó tổng được tạo ra là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

athletes = []
for _ in range(m):
    l, r, t, c = map(int, input().split())
    athletes.append((l, r, t, c))

answer = 0

for section in range(1, n + 1):
    best_time = float('inf')
    best_index = -1

    for idx, (l, r, t, c) in enumerate(athletes):
        if l <= section <= r:
            if t < best_time:
                best_time = t
                best_index = idx
            elif t == best_time and idx < best_index:
                best_index = idx

    if best_index != -1:
        answer += athletes[best_index][3]

print(answer)
```Chương trình lưu trữ từng vận động viên cùng với quãng đường, tốc độ và phần thưởng của họ. 

Vòng lặp bên ngoài xử lý từng phần một. Đối với mỗi phần, chúng tôi tìm kiếm vận động viên chiến thắng.`best_time`theo dõi thời gian phần tối thiểu được thấy cho đến nay, trong khi`best_index`lưu trữ vận động viên nào hiện đang thắng trong loạt tie-break. 

điều kiện`l <= section <= r`xác định liệu vận động viên có tham gia phần thi này hay không. Chỉ những vận động viên tích cực mới được xem xét. 

Logic ràng buộc đáng được chú ý. Giả sử hai vận động viên có bằng nhau`t`. Luật quy định chỉ số vận động viên nhỏ hơn sẽ thắng. Vì các chỉ số Python bắt đầu từ 0 và việc đánh số đầu vào tuân theo thứ tự chèn một cách hiệu quả, so sánh`idx`tái tạo chính xác quy tắc chính thức. 

Séc`best_index != -1`xử lý các phần trống một cách an toàn. Nếu không có vận động viên nào hoàn thành phần hiện tại thì phần thưởng sẽ không được thêm vào. 

Bởi vì các ràng buộc rất nhỏ nên không cần phải xử lý trước hoặc sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 4
1 4 20 5
1 3 21 10
3 3 4 30
3 4 4 20
```Xử lý dấu vết: 

| Phần | Vận động viên năng động | Người chiến thắng | Lợi nhuận được thêm vào | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1, 2 | 1 | 5 | 5 | 
| 2 | 1, 2 | 1 | 5 | 10 | 
| 3 | 1, 2, 3, 4 | 3 | 30 | 40 | 
| 4 | 1, 4 | 4 | 20 | 60 | 

Ở phần 1 và 2, vận động viên 1 đánh bại vận động viên 2 vì`20 < 21`. Ở phần 3, vận động viên 3 và 4 đều có thời gian`4`, vậy vận động viên thứ 3 thắng do chỉ số nhỏ hơn. Phần 4 thuộc về vận động viên 4 vì vận động viên 3 không còn hoạt động. 

Ví dụ này thể hiện cả lọc theo khoảng thời gian và phá vỡ ràng buộc. 

### Ví dụ bổ sung 

đầu vào:```
5 3
1 5 10 5
2 4 3 20
3 3 1 100
```Xử lý dấu vết: 

| Phần | Vận động viên năng động | Người chiến thắng | Lợi nhuận được thêm vào | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 5 | 5 | 
| 2 | 1, 2 | 2 | 20 | 25 | 
| 3 | 1, 2, 3 | 3 | 100 | 125 | 
| 4 | 1, 2 | 2 | 20 | 145 | 
| 5 | 1 | 1 | 5 | 150 | 

Ví dụ này cho thấy một vận động viên chạy cự ly ngắn vẫn có thể thống trị một phần thi nếu thời gian của họ ở đó là ít nhất. Người chiến thắng luôn được xác định cục bộ cho phần hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Đối với mỗi phần, chúng tôi quét tất cả các vận động viên | 
| Không gian | O(1) không bao gồm lưu trữ đầu vào | Chỉ có một số biến theo dõi được sử dụng | 

Với`n, m <= 100`, thời gian chạy trong trường hợp xấu nhất chỉ là 10.000 lượt kiểm tra, thấp hơn nhiều so với giới hạn. Việc sử dụng bộ nhớ rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    athletes = []
    for _ in range(m):
        l, r, t, c = map(int, input().split())
        athletes.append((l, r, t, c))

    ans = 0

    for section in range(1, n + 1):
        best_time = float('inf')
        best_idx = -1

        for idx, (l, r, t, c) in enumerate(athletes):
            if l <= section <= r:
                if t < best_time:
                    best_time = t
                    best_idx = idx
                elif t == best_time and idx < best_idx:
                    best_idx = idx

        if best_idx != -1:
            ans += athletes[best_idx][3]

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""4 4
1 4 20 5
1 3 21 10
3 3 4 30
3 4 4 20
"""
) == "60", "sample 1"

# minimum size
assert run(
"""1 1
1 1 5 10
"""
) == "10", "single section single athlete"

# tie-breaking by smaller index
assert run(
"""3 2
1 3 5 10
1 3 5 20
"""
) == "30", "smaller index must win ties"

# sections without participants
assert run(
"""5 1
2 4 3 10
"""
) == "30", "empty sections give no reward"

# short interval fast athlete
assert run(
"""4 2
1 4 10 5
2 2 1 100
"""
) == "115", "winner determined per section"

# all athletes equal
assert run(
"""2 3
1 2 7 10
1 2 7 20
1 2 7 30
"""
) == "20", "first athlete wins all sections"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vận động viên đơn trên một phần thi | 10 | Hạn chế tối thiểu | 
| Thời gian bình đẳng cho tất cả các vận động viên | 30 | Thắt dây đúng cách | 
| Phần ranh giới trống | 30 | Không xử lý người chiến thắng | 
| Vận động viên nhanh trên một phần | 115 | Người chiến thắng dựa trên khu vực địa phương | 
| Tất cả các vận động viên giống hệt nhau | 20 | Lựa chọn chỉ số nhỏ nhất ổn định | 

## Vỏ cạnh 

Xem xét tốc độ bằng nhau với các phần thưởng khác nhau:```
3 2
1 3 5 10
1 3 5 20
```Ở mỗi phần thi, cả hai vận động viên đều năng động và đều có thời gian`5`. Thuật toán so sánh các chỉ số và luôn chọn vận động viên 1. Tổng điểm trở thành`10 + 10 + 10 = 30`. 

Một giải pháp có lỗi tham lam chọn phần thưởng lớn hơn sẽ xuất kết quả không chính xác`60`. 

Bây giờ hãy xem xét các phần trống:```
5 1
2 4 3 10
```Thuật toán xử lý từng phần một cách độc lập. 

Phần 1: 

Không có vận động viên tích cực, không có gì thêm vào. 

Phần 2, 3, 4: 

Vận động viên 1 thắng, thêm`10`mỗi lần. 

Phần 5: 

Không có vận động viên hoạt động nữa. 

Câu trả lời cuối cùng:```
30
```các`best_index != -1`kiểm tra ngăn chặn việc thêm lợi nhuận cho những người chiến thắng không tồn tại. 

Cuối cùng, hãy xem xét một vận động viên chạy nhanh với khoảng cách rất ngắn:```
4 2
1 4 10 5
2 2 1 100
```Thuật toán kiểm tra hoạt động trên mỗi phần. 

| Phần | Người chiến thắng | Tổng cộng | 
| --- | --- | --- | 
| 1 | Vận động viên 1 | 5 | 
| 2 | Vận động viên 2 | 105 | 
| 3 | Vận động viên 1 | 110 | 
| 4 | Vận động viên 1 | 115 | 

Điều này xác nhận rằng người chiến thắng chỉ phụ thuộc vào phần thi hiện tại chứ không phụ thuộc vào tổng thời lượng cuộc đua hoặc độ dài quãng đường.
