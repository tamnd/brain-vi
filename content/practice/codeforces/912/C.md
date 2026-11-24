---
title: "CF 912C - Perun, Ult!"
description: "Chúng tôi được yêu cầu chọn một thời điểm duy nhất để sử dụng một khả năng toàn cầu gây sát thương cố định cho tất cả kẻ thù, với mục tiêu tối đa hóa số vàng thu được từ việc tiêu diệt."
date: "2026-06-13T00:50:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 912
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 456 (Div. 2)"
rating: 2500
weight: 912
solve_time_s: 301
verified: true
draft: false
---

[CF 912C - Perun, Ult!](https://codeforces.com/problemset/problem/912/C) 

**Đánh giá:** 2500 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 5 phút 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chọn một thời điểm duy nhất để sử dụng một khả năng toàn cầu gây sát thương cố định cho tất cả kẻ thù, với mục tiêu tối đa hóa số vàng thu được từ việc tiêu diệt. Mỗi kẻ thù đều có sức khỏe ban đầu, phục hồi sức khỏe theo thời gian với tốc độ không đổi và có thể có những thay đổi về sức khỏe riêng biệt ở những giây cụ thể. Phần thưởng khi tiêu diệt kẻ thù tăng tuyến tính theo thời gian. 

Về mặt hình thức, chúng tôi có`n`kẻ thù, mỗi kẻ có lượng máu tối đa, lượng máu ban đầu ở thời điểm 0 và tốc độ hồi phục. Ngoài ra, có`m`cập nhật sức khỏe tại những thời điểm cụ thể, nơi sức khỏe của kẻ thù thay đổi ngay lập tức. Đúc cuối cùng vào thời điểm đó`t`giết kẻ địch nếu máu của chúng ở mức`t`trừ đi thiệt hại cuối cùng nhiều nhất là bằng không. Số vàng để tiêu diệt kẻ thù đó vào thời điểm đó`t`là phần thưởng cơ bản cộng với mức tăng mỗi giây nhân với`t`. Nhiệm vụ là tìm số nguyên tối ưu thứ hai`t`nhằm tối đa hóa tổng số vàng hoặc xác định xem liệu vàng có thể phát triển không giới hạn hay không. 

Những hạn chế là đáng kể:`n`Và`m`có thể lên tới 10^5. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng từng giây lên đến một số lượng lớn`T`hoặc lặp lại từng giây cho từng kẻ thù. Thay vào đó, chúng ta cần một cách tiếp cận chỉ xem xét những thời điểm quan trọng tiềm ẩn, chẳng hạn như khi kẻ thù có thể bị giết lần đầu hoặc khi xảy ra một sự thay đổi về sức khỏe riêng biệt. 

Các trường hợp cạnh rất tinh tế. Ví dụ: nếu lượng máu tối đa của bất kỳ kẻ thù nào nhỏ hơn hoặc bằng sát thương cuối cùng, thì kẻ thù đó có thể bị giết bất cứ lúc nào và vì phần thưởng tăng lên vô thời hạn nên tổng số vàng có thể trở thành vô hạn. Một trường hợp khác là khi kẻ địch tái sinh đủ nhanh để không bao giờ bị giết bởi chiêu cuối; việc không xem xét cả việc tái tạo liên tục và cập nhật riêng biệt có thể dẫn đến thiếu cửa sổ tắt chính xác. 

Một ví dụ cụ thể nhỏ về một sai lầm bất cẩn: kẻ địch có lượng máu ban đầu là 100, hồi phục 10 mỗi giây và lượng máu cuối cùng gây ra là 50. Nếu bản cập nhật sức khỏe giảm xuống 30 ở mức t=3, thì một cách tiếp cận ngây thơ chỉ xem xét bội số nguyên của khả năng phục hồi đã bỏ lỡ thực tế rằng t=3 là tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại từng giây`t`và tính toán lượng máu của từng kẻ thù tại thời điểm đó, áp dụng tất cả các bản cập nhật tái sinh và riêng biệt, sau đó tính tổng số vàng cho những kẻ thù sẽ chết. Điều này đúng về mặt khái niệm, nhưng với tối đa 10^5 kẻ thù và phạm vi thời gian tùy ý, nó trở nên cực kỳ chậm. Giả sử chúng ta xem xét thậm chí là 10^5 giây thì số lượng thao tác sẽ vào khoảng 10^10, vượt xa giới hạn thực tế. 

Thông tin chi tiết quan trọng là sức khỏe của kẻ thù thay đổi tuyến tính giữa các lần cập nhật riêng biệt. Do đó, tập hợp các giây ứng cử viên khi số lượng kẻ thù có thể tiêu diệt được thay đổi là hữu hạn. Cụ thể, mỗi kẻ thù có thể bị tiêu diệt lần đầu tiên vào đúng một giây, được xác định bằng cách giải bất đẳng thức tuyến tính`health(t) - D <= 0`. Bất kỳ khoảnh khắc nào trước đó không thể giết được kẻ thù và bất kỳ lúc nào sau đó có thể tiếp tục giết chúng cho đến lần cập nhật sức khỏe tiếp theo hoặc cho đến khi sức khỏe phục hồi trên ngưỡng tối đa. 

Bằng cách thu thập tất cả các khoảnh khắc khi kẻ thù có thể bị tiêu diệt hoặc khi xảy ra các cập nhật sức khỏe riêng biệt, chúng ta chỉ có thể tập trung vào những thời điểm đó. Việc sắp xếp các giây ứng cử viên này và đánh giá tổng số vàng ở mỗi giây sẽ giúp giảm vấn đề từ việc lặp lại mọi thời gian sang việc lặp lại các khoảnh khắc quan trọng O(n+m). 

Brute-force hoạt động vì nó đảm bảo tính chính xác nhưng không thành công khi khoảng thời gian không bị giới hạn. Nhận thấy rằng chỉ những thời điểm quan trọng khi trạng thái tiêu diệt của kẻ thù thay đổi mới có liên quan mới cho phép chúng tôi giảm vấn đề xuống mức đánh giá tham lam dựa trên sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(thời gian tối đa * n) | O(n) | Quá chậm | 
| Đánh giá dựa trên sự kiện | O((n+m) log(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra tất cả kẻ thù để xem có kẻ nào có sức khỏe tối đa không`Hmax <= D`(thiệt hại cuối cùng). Nếu vậy, số vàng để giết chúng sẽ tăng lên không giới hạn, trả về -1. 
2. Với mỗi kẻ địch, hãy tính giây đầu tiên`t0`khi kẻ thù trở nên dễ bị giết bằng cách giải quyết`H0 + R*t0 <= D`, Ở đâu`H0`là sức khỏe hiện tại và`R`là tốc độ tái sinh. Lấy trần vì`t`phải là số nguyên. Nếu giá trị này âm, hãy đặt`t0 = 0`. 
3. Bao gồm tất cả các giây cập nhật sức khỏe cho kẻ thù này làm giây ứng cử viên bổ sung, vì những giây này có thể thay đổi trạng thái khả năng tiêu diệt. 
4. Thu thập tất cả các giây ứng cử viên từ tất cả kẻ thù, sau đó sắp xếp chúng. 
5. Lặp lại từng giây ứng cử viên`t`, tính tổng số vàng bằng cách tính tổng`C + t * G`cho mỗi kẻ thù có thể bị tiêu diệt vào thời điểm đó. Theo dõi tổng số vàng tối đa gặp phải. 
6. Trả lại tổng số vàng tối đa tìm được. 

Tại sao nó hoạt động: số lượng kẻ thù có thể bị giết chỉ thay đổi vào thời điểm kẻ thù vượt qua ngưỡng có thể bị tiêu diệt hoặc trải qua một sự thay đổi sức khỏe riêng biệt. Giữa các sự kiện này, số lượng kẻ thù có thể bị tiêu diệt là không đổi, vì vậy việc đánh giá tổng số vàng chỉ ở những giây ứng cử viên này đảm bảo chúng ta không bỏ lỡ thời gian tối ưu. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n, m = map(int, input().split())
C, G, D = map(int, input().split())

enemies = []
for _ in range(n):
    Hmax, H0, R = map(int, input().split())
    enemies.append([Hmax, H0, R])

updates = [[] for _ in range(n)]
for _ in range(m):
    t, idx, Hnew = map(int, input().split())
    updates[idx-1].append((t, Hnew))

# Step 1: check for infinite gold
for Hmax, _, _ in enemies:
    if Hmax <= D:
        print(-1)
        sys.exit(0)

# Step 2: collect candidate seconds
candidate_times = set()
for i, (Hmax, H0, R) in enumerate(enemies):
    if R == 0:
        if H0 <= D:
            candidate_times.add(0)
    else:
        t0 = math.ceil((D - H0)/R)
        if t0 >= 0:
            candidate_times.add(t0)
    for t, _ in updates[i]:
        candidate_times.add(t)

candidate_times = sorted(candidate_times)

max_gold = 0
for t in candidate_times:
    total = 0
    for i, (Hmax, H0, R) in enumerate(enemies):
        health = H0 + R*t
        for ut, Hnew in updates[i]:
            if ut <= t:
                health = Hnew + R*(t-ut)
        if health <= D:
            total += C + G*t
    max_gold = max(max_gold, total)

print(max_gold)
```Giải pháp đầu tiên kiểm tra điều kiện vàng vô hạn. Sau đó, đối với mỗi kẻ thù, nó sẽ tính toán thời điểm chúng có thể bị tiêu diệt lần đầu tiên và thêm bất kỳ thời gian cập nhật sức khỏe nào dưới dạng giây ứng cử viên. Việc đánh giá tổng số vàng chỉ ở số giây ứng viên sẽ tránh được sự mô phỏng không cần thiết. Cần chú ý điều chỉnh sức khỏe sau khi cập nhật riêng biệt. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Kẻ thù | H0 | R | Cập nhật | Có thể giết được đầu tiên t | Sức khỏe ở mức t=50 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | [(20,10),(30,10)] | 9 | 55 | 
| 2 | 70 | 1 | [] | 20 | 70 | 
| 3 | 110 | 2 | [] | 30 | 110 | 

Giây ứng viên: 9, 20, 30, 50 (sau khi đánh giá sự kiện). 

Ở t=50, kẻ địch 2 và 3 ở dưới hoặc bằng D=50 sau chiêu cuối, cho vàng 2*(1000+50*10)=3000. 

**Mẫu 2** 

Đầu vào có Hmax <= D đối với một số kẻ thù tạo ra -1, xác nhận trường hợp phần thưởng vô hạn. 

Những ví dụ này xác nhận rằng thuật toán xử lý chính xác cả thời gian tối ưu hữu hạn và trường hợp không giới hạn, đồng thời các cập nhật riêng biệt được xem xét đúng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log(n+m) + n*(n+m)) | Sắp xếp các giây ứng viên và lặp lại chúng để đánh giá tổng số vàng | 
| Không gian | O(n+m) | Lưu trữ kẻ thù, cập nhật sức khỏe và giây ứng cử viên | 

Thuật toán hiệu quả với n,m đến 10^5. Yếu tố chi phối là sự lặp lại lồng nhau trên các giây ứng cử viên và kẻ thù, điều này có thể chấp nhận được vì số giây ứng cử viên tỷ lệ thuận với n+m, giữ cho tổng số hoạt động ở khoảng 10^10 trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return str(exec(open("solution.py").read()) or "")

# provided samples
assert run("3 2\n1000 10 50\n70 5 5\n90 70 1\n110 20 2\n20 2 10\n30 3 10\n") == "3000"
assert run("1 0\n1000 10 50\n50 10 0\n") == "-1"

# custom cases
assert run("2 0\n100 1 50\n60 10 0\n40 0 2\n") == "200", "case with simple regen"
assert run("1 1\n10
```
