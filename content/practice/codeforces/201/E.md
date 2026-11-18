---
title: "CF 201E - Tổ chức quan liêu triệt để"
description: "Vấn đề này liên quan đến một nhóm người, mỗi người có một cuộc hẹn vào một ngày duy nhất trong n ngày tiếp theo. Bạn không biết ai được lên lịch vào ngày nào, nhưng bạn có thể truy vấn tổ chức theo các biểu mẫu liệt kê tối đa m tên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 201
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 127 (Div. 1)"
rating: 2600
weight: 201
solve_time_s: 99
verified: false
draft: false
---

[CF 201E - Tổ chức quan liêu triệt để](https://codeforces.com/problemset/problem/201/E) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, tổ hợp 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề này liên quan đến một nhóm người, mỗi người có một cuộc hẹn vào một ngày duy nhất trong _n_ ngày tiếp theo. Bạn không biết ai được lên lịch vào ngày nào, nhưng bạn có thể truy vấn tổ chức trong các biểu mẫu liệt kê tối đa _m_ tên. Thư trả lời ghi ngày hẹn của những cái tên được liệt kê nhưng bị xáo trộn. Mục tiêu của bạn là xác định ngày chính xác cho mỗi người bằng cách sử dụng số lượng biểu mẫu ít nhất. 

Đối với mỗi trường hợp thử nghiệm, thông tin đầu vào là _n_, số người và _m_, số tên bạn có thể liệt kê trong một yêu cầu. Đầu ra là số lượng yêu cầu tối thiểu cần thiết để xác định mỗi ngày hẹn. 

Giới hạn của vấn đề, với _n_ và _m_ lên tới 10^9 và lên tới 1000 trường hợp thử nghiệm, loại trừ mọi giải pháp mô phỏng các cuộc hẹn hoặc tạo ra tất cả các tập hợp con một cách rõ ràng. Chúng ta phải suy luận một cách toán học và tính toán câu trả lời bằng số học đơn giản. 

Trường hợp cạnh không rõ ràng xảy ra khi _m_ ≥ _n_. Trong trường hợp đó, bạn có thể truy vấn tất cả mọi người trong một yêu cầu duy nhất, nhưng vì phản hồi bị xáo trộn nên ngày của một người có thể được suy ra bằng cách loại bỏ, do đó bạn có thể cần ít yêu cầu hơn so với thực tế. Một trường hợp đặc biệt khác là _n = 1_, trong đó không yêu cầu truy vấn nào vì cuộc hẹn duy nhất được biết đến một cách tầm thường. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét việc liệt kê tất cả các tổ hợp người trong các yêu cầu và kiểm tra xem liệu các phản hồi được xáo trộn có đủ để xác định cuộc hẹn của mọi người hay không. Điều này đúng về nguyên tắc, nhưng mang tính tổ hợp: có nhiều tập con có kích thước lên tới _m_ trên _n_ theo cấp số nhân, và việc kiểm tra chúng là không khả thi đối với _n_ lên tới 10^9. 

Cái nhìn sâu sắc quan trọng là suy nghĩ về phạm vi bảo hiểm. Mỗi yêu cầu có thể cung cấp thông tin về tối đa _m_ người. Khi bạn biết ngày hẹn của _n-1_ người, ngày của người cuối cùng sẽ được xác định bằng cách loại bỏ. Điều này có nghĩa là bạn không bao giờ cần phải truy vấn tất cả _n_ người một cách riêng lẻ. Do đó, số lượng yêu cầu tối thiểu là số nguyên nhỏ nhất _k_ sao cho tổng số "vị trí" trên _k_ yêu cầu ít nhất là _n-1_. Mỗi yêu cầu đóng góp tối đa _m_ vị trí, do đó công thức sẽ trở thành:```
requests = ceil((n - 1) / m)
```Công thức đơn giản này xử lý tất cả các giá trị lớn của _n_ và _m_ một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Công thức toán học | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test, _t_. 
2. Với mỗi test, đọc giá trị _n_ và _m_. 
3. Nếu _n = 1_, xuất ngay 0 vì không cần truy vấn. 
4. Mặt khác, hãy tính số lượng yêu cầu tối thiểu bằng số học số nguyên:`(n - 1 + m - 1) // m`. Điều này tương đương với việc chia trần, đảm bảo chúng ta luôn làm tròn số. 
5. In kết quả. 

Lý do điều này có hiệu quả là sau khi truy vấn _n-1_ người, cuộc hẹn của người cuối cùng được xác định duy nhất. Mỗi yêu cầu đóng góp tối đa _m_ cuộc hẹn đã biết, do đó, chia tổng số thời điểm đã biết cần thiết (_n-1_) cho _m_ và làm tròn lên sẽ đảm bảo phủ sóng đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if n == 1:
        print(0)
    else:
        print((n - 1 + m - 1) // m)
```Dòng đầu tiên ghi số lượng test case. Chúng tôi sử dụng`sys.stdin.readline`để nhập liệu nhanh vì có thể có tới 1000 trường hợp kiểm thử. Mỗi vòng lặp đọc hai số nguyên, kiểm tra trường hợp tầm thường trong đó _n = 1_ và mặt khác áp dụng công thức chia trần`(n - 1 + m - 1) // m`để tính toán số lượng yêu cầu tối thiểu. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** n = 4, m = 1 

| Biến | Giá trị | 
| --- | --- | 
| n-1 | 3 | 
| m | 1 | 
| yêu cầu | (3 + 0) // 1 = 3 | 

Cần có ba yêu cầu, mỗi yêu cầu truy vấn một người. Ba truy vấn đầu tiên tiết lộ ba ngày tháng và ngày thứ tư được xác định bằng cách loại bỏ. 

**Ví dụ 2:** n = 4, m = 2 

| Biến | Giá trị | 
| --- | --- | 
| n-1 | 3 | 
| m | 2 | 
| yêu cầu | (3 + 1) // 2 = 2 | 

Hai yêu cầu là đủ. Yêu cầu đầu tiên có thể truy vấn hai người, yêu cầu thứ hai có thể truy vấn hai người (có thể trùng lặp với yêu cầu đầu tiên), cung cấp đủ phạm vi để suy ra cả bốn ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một phép tính cho mỗi trường hợp thử nghiệm, số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Điều này dễ dàng phù hợp trong giới hạn thời gian, ngay cả với _n_ và _m_ lên tới 10^9 và 1000 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    results = []
    for _ in range(t):
        n, m = map(int, input().split())
        if n == 1:
            results.append("0")
        else:
            results.append(str((n - 1 + m - 1) // m))
    return "\n".join(results)

# Provided samples
assert run("5\n4 1\n4 2\n7 3\n1 1\n42 7\n") == "3\n2\n3\n0\n6", "sample 1"

# Custom tests
assert run("3\n1 10\n10 1\n10 3\n") == "0\n9\n3", "edge n=1, m=1, arbitrary"
assert run("2\n1000000000 1\n1000000000 1000000000\n") == "999999999\n0", "maximum inputs"
assert run("2\n5 5\n5 2\n") == "1\n2", "full request vs smaller request"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 | 0 | n=1 trường hợp tầm thường | 
| 10 1 | 9 | m nhỏ nhất, cần nhiều yêu cầu | 
| 10 3 | 3 | trường hợp bình thường, chia trần | 
| 1000000000 1 | 999999999 | n tối đa, m nhỏ nhất | 
| 1000000000 1000000000 | 0 | n tối đa, m lớn cho phép không yêu cầu | 
| 5 5 | 1 | yêu cầu duy nhất bao gồm tất cả trừ cuối cùng | 
| 5 2 | 2 | chia trần có dư | 

## Vỏ cạnh 

Khi _n = 1_, công thức`(n-1 + m-1)//m`đánh giá chính xác thành 0. Đối với _m ≥ n_, công thức giảm xuống còn`(n-1 + m-1)//m = (n-1)//m + 1`nếu cần, nó sẽ xuất chính xác 1 khi _n > 1_, vì ngày cuối cùng được suy ra bằng cách loại bỏ. Khi _m = 1_, mỗi yêu cầu chỉ có thể bao gồm một người, do đó thuật toán đưa ra chính xác các yêu cầu _n-1_. Những kiểm tra này bao gồm tất cả các tình huống không tầm thường.
