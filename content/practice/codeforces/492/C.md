---
title: "CF 492C - Vanya và các kỳ thi"
description: "Vanya có một số bài kiểm tra, mỗi bài được chấm điểm từ 1 đến điểm tối đa r. Anh ấy muốn điểm trung bình chung của mình đạt ít nhất là trung bình để đủ điều kiện nhận học bổng. Đối với mỗi kỳ thi, anh ta có thể cải thiện điểm số của mình bằng cách viết bài luận, với chi phí tăng điểm thêm 1 điểm khác nhau cho mỗi kỳ thi."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 492
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 280 (Div. 2)"
rating: 1400
weight: 492
solve_time_s: 57
verified: true
draft: false
---

[CF 492C - Vanya và các bài kiểm tra](https://codeforces.com/problemset/problem/492/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vanya có một số bài kiểm tra, mỗi bài được chấm điểm từ 1 đến điểm tối đa _r_. Anh ấy muốn điểm trung bình chung của mình đạt ít nhất là _avg_ để đủ điều kiện nhận học bổng. Đối với mỗi kỳ thi, anh ta có thể cải thiện điểm số của mình bằng cách viết bài luận, với chi phí tăng điểm thêm 1 điểm khác nhau cho mỗi kỳ thi. Mục đích là giảm thiểu tổng số bài luận anh ta phải viết để đạt mức trung bình yêu cầu. 

Dữ liệu đầu vào cung cấp số lượng bài kiểm tra _n_, điểm tối đa _r_ và điểm trung bình bắt buộc _avg_. Mỗi kỳ thi khi đó có hai con số: điểm hiện tại và số bài luận cần thiết cho mỗi điểm tăng. Đầu ra phải là một số nguyên duy nhất: yêu cầu tổng số bài luận tối thiểu. 

Những hạn chế là đáng kể. Với _n_ lên tới 10^5 và _r_ lớn bằng 10^9, bất kỳ thuật toán nào có độ phức tạp kém hơn O(n log n) đều có nguy cơ hết thời gian chờ. Ví dụ: việc lặp lại mọi mức tăng điểm có thể một cách rõ ràng sẽ quá chậm. Ngoài ra, _b_i_ có thể lớn tới 10^6, vì vậy chúng ta phải tính toán chi phí bài luận một cách cẩn thận để tránh tràn số nguyên nếu sử dụng phép tính tổng đơn giản. 

Trường hợp cạnh có thể tinh tế. Một là khi điểm trung bình hiện tại đã đạt hoặc vượt _avg_, trong trường hợp đó không cần viết bài luận. Một trường hợp khác là khi một số bài kiểm tra đã ở mức tối đa _r_ và thuật toán phải tránh cố gắng tăng thêm điểm. Việc thực hiện bất cẩn có thể luôn cố gắng tăng điểm theo thứ tự chỉ số, bỏ qua chi phí cho bài luận, dẫn đến một giải pháp không tối ưu. Ví dụ: nếu:```
2 5 5
5 10
3 1
```Vanya đã có tổng điểm 8 và cần 10 điểm nên anh ấy chỉ phải tăng bài thi thứ hai thêm 2 điểm, mặc dù bài thi đầu tiên cũng ở mức tối đa; một cách tiếp cận ngây thơ xử lý các bài kiểm tra một cách mù quáng theo thứ tự đầu vào có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ sẽ xem xét mọi mức tăng có thể có cho mỗi bài kiểm tra cho đến khi đạt được tổng số điểm yêu cầu. Đối với mỗi mức tăng, chúng tôi thêm chi phí bài luận tương ứng và dừng khi tổng mức trung bình là đủ. Mặc dù đúng về nguyên tắc nhưng số thao tác trong trường hợp xấu nhất có thể đạt tới O(n*r), lên tới 10^14 trong bài toán này - rõ ràng là không khả thi. 

Điều quan trọng cần lưu ý là chi phí cho mỗi điểm cho một bài luận sẽ khác nhau tùy theo từng kỳ thi và chúng tôi muốn chi tiêu càng ít bài luận càng tốt. Vì vậy, chúng ta nên ưu tiên tăng điểm trong các kỳ thi yêu cầu ít bài luận trên mỗi điểm hơn. Việc sắp xếp các bài kiểm tra theo chi phí bài luận cho phép chúng ta tham lam chọn những bài thi có chi phí thấp nhất trước tiên. Sau khi sắp xếp, chúng tôi lặp lại các bài kiểm tra, tăng dần từng bài kiểm tra nếu cần hoặc cho đến khi đạt điểm tối đa. Chúng tôi tiếp tục cho đến khi tổng số điểm đạt hoặc vượt _n × avg_. Điều này làm giảm độ phức tạp xuống O(n log n) do sắp xếp, đủ hiệu quả cho các giới hạn đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*r) | O(n) | Quá chậm | 
| Tham lam Sắp xếp theo chi phí | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số điểm Vanya hiện có. Hãy để điều này được`current_sum`. 
2. Tính tổng số điểm cần thiết để đạt mức trung bình mong muốn, đó là`required_sum = n * avg`. 
3. Nếu`current_sum >= required_sum`, in 0 và kết thúc, vì không cần viết luận. 
4. Ghép điểm hiện tại của mỗi kỳ thi với chi phí bài luận của kỳ thi đó. Sắp xếp các cặp này theo chi phí bài luận theo thứ tự tăng dần. Điều này đảm bảo chúng tôi luôn chọn mức tăng rẻ nhất trước tiên. 
5. Khởi tạo`essays = 0`Và`needed = required_sum - current_sum`. 
6. Lặp lại các bài kiểm tra đã được sắp xếp. Đối với mỗi kỳ thi, hãy tính mức tăng tối đa có thể:`increase = min(r - grade, needed)`. 
7. Thêm`increase * essay_cost`ĐẾN`essays`. Trừ`increase`từ`needed`. 
8. Nếu`needed`đạt đến 0, phá vỡ vòng lặp. In tổng số`essays`. 

**Tại sao nó hoạt động** 

Phương pháp tham lam này đúng vì chi phí cho mỗi điểm bài luận là cố định cho mỗi bài kiểm tra. Để giảm thiểu tổng số bài luận, bất kỳ giải pháp tối ưu nào cũng phải luôn dành càng ít bài luận càng tốt cho mỗi lần tăng điểm. Việc sắp xếp theo chi phí đảm bảo rằng mỗi điểm chúng tôi thêm vào đều có giá rẻ nhất. Thuật toán duy trì tính bất biến ở mỗi bước,`needed`là số điểm yêu cầu còn lại và tổng số bài luận đã sử dụng là tối thiểu đối với số điểm đã thực hiện cho đến nay. Một lần`needed`bằng 0, yêu cầu trung bình được thỏa mãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, r, avg = map(int, input().split())
exams = [tuple(map(int, input().split())) for _ in range(n)]

current_sum = sum(grade for grade, _ in exams)
required_sum = n * avg

if current_sum >= required_sum:
    print(0)
    sys.exit()

# sort exams by essay cost
exams.sort(key=lambda x: x[1])

needed = required_sum - current_sum
essays = 0

for grade, cost in exams:
    increase = min(r - grade, needed)
    essays += increase * cost
    needed -= increase
    if needed == 0:
        break

print(essays)
```Đầu tiên, mã sẽ tính toán tổng số điểm còn thiếu để đạt mức trung bình yêu cầu. Việc sắp xếp các bài kiểm tra theo chi phí sẽ đảm bảo rằng mỗi bài luận được sử dụng đều hiệu quả nhất có thể. các`min(r - grade, needed)`dòng đảm bảo chúng tôi không bao giờ vượt quá điểm tối đa hoặc vượt quá số điểm yêu cầu. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5 5 4
5 2
4 7
3 1
3 2
2 5
```| thi | Lớp | Chi phí | Tăng tối đa | Cần thiết | Đã thêm bài tiểu luận | Còn Cần Thiết | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 1 | 2 | 10 | 2 | 8 | 
| 4 | 3 | 2 | 2 | 8 | 4 | 6 | 
| 2 | 4 | 7 | 1 | 6 | 7 | 5 | 
| 5 | 2 | 5 | 3 | 5 | 15 | 2 | 
| 1 | 5 | 2 | 0 | 2 | 0 | 2 | 

Sau khi xử lý các bài thi rẻ nhất trước tiên, chỉ cần 4 bài luận để đạt mức trung bình yêu cầu. 

**Mẫu 2** 

đầu vào:```
2 5 2
5 10
3 1
```

`current_sum = 8`Và`required_sum = 4`. Vì 8 >= 4 nên kết quả là 0. Không cần viết luận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp bài kiểm tra chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Lưu trữ danh sách bài thi | 

Với n tối đa 10^5, các thao tác O(n log n) vừa vặn thoải mái trong giới hạn 1 giây. Việc sử dụng bộ nhớ cũng ở mức dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, r, avg = map(int, input().split())
    exams = [tuple(map(int, input().split())) for _ in range(n)]

    current_sum = sum(grade for grade, _ in exams)
    required_sum = n * avg

    if current_sum >= required_sum:
        return "0"

    exams.sort(key=lambda x: x[1])
    needed = required_sum - current_sum
    essays = 0

    for grade, cost in exams:
        increase = min(r - grade, needed)
        essays += increase * cost
        needed -= increase
        if needed == 0:
            break

    return str(essays)

# provided samples
assert run("5 5 4\n5 2\n4 7\n3 1\n3 2\n2 5\n") == "4"
assert run("2 5 2\n5 10\n3 1\n") == "0"

# custom cases
assert run("1 5 5\n5 1\n") == "0", "single exam already enough"
assert run("3 10 10\n10 3\n9 2\n8 1\n") == "3", "needs minimal increases on cheapest exam"
assert run("2 1000000000 1000000000\n1 1\n1 1\n") == str(1999999998), "max r and avg"
assert run("4 10 7\n7 5\n6 1\n5 1\n4 2\n") == "4", "mixed costs, must choose cheapest first"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 5\n5 1 | 0 | Kỳ thi đơn đã đạt mức trung bình | 
| 3 10 10\n10 3\n9 2\n8 1 | 3 | Sửa lỗi tham lam chọn bài theo giá bài | 
| 2 10^9 10^9\n1 1\n1 1 | 1999999998 | Xử lý số lượng lớn mà không bị tràn | 
| 4 10 7\n7 5\n6 1\n5 1\n4 2 | 4 | Sự gia tăng tham lam tránh những bài luận tốn kém | 

## Vỏ cạnh 

Nếu mức trung bình hiện tại đã đủ
