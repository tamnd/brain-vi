---
title: "CF 1850A - Gửi những người phê bình của tôi"
description: "Chúng ta được yêu cầu làm việc với ba chữ số, gọi chúng là a, b và c, mỗi chữ số nằm trong khoảng từ 0 đến 9. Nhiệm vụ là xác định xem liệu chúng ta có thể chọn bất kỳ hai chữ số riêng biệt nào trong số ba chữ số này sao cho tổng của chúng ít nhất là 10. Chúng ta lặp lại điều này cho t trường hợp kiểm thử độc lập."
date: "2026-06-09T05:28:54+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1850
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 886 (Div. 4)"
rating: 800
weight: 1850
solve_time_s: 62
verified: true
draft: false
---

[CF 1850A - Gửi những người phê bình của tôi](https://codeforces.com/problemset/problem/1850/A) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu làm việc với ba chữ số, gọi chúng là`a`,`b`, Và`c`, mỗi số nằm trong khoảng từ 0 đến 9. Nhiệm vụ là xác định xem liệu chúng ta có thể chọn bất kỳ hai chữ số phân biệt nào trong số ba chữ số này sao cho tổng của chúng ít nhất là 10. Chúng ta lặp lại điều này cho`t`trường hợp thử nghiệm độc lập. Đầu ra phải là "CÓ" đơn giản nếu một cặp như vậy tồn tại và "KHÔNG" nếu không có cặp nào trong ba cặp có thể đạt tổng bằng 10. 

Các ràng buộc rất nhỏ: mỗi chữ số nhiều nhất là 9 và chúng tôi có tới 1000 trường hợp thử nghiệm. Điều đó có nghĩa là ngay cả giải pháp kiểm tra tất cả các cặp có thể có cho từng trường hợp thử nghiệm cũng sẽ nhanh chóng, vì đối với mỗi trường hợp thử nghiệm chỉ có ba cặp:`(a, b)`,`(a, c)`,`(b, c)`. Nhân 3 cặp với 1000 trường hợp thử nghiệm chỉ cho 3000 thao tác, thấp hơn nhiều so với bất kỳ giới hạn thời gian nào. 

Ví dụ: các trường hợp cạnh đáng chú ý bao gồm các trường hợp tất cả các chữ số đều nhỏ`0 0 0`. Kết quả đầu ra đúng là "KHÔNG" vì không có cặp nào có tổng bằng 10. Tương tự, chẳng hạn, nếu tất cả các chữ số đều lớn`9 9 9`, bất kỳ cặp nào cũng có tổng bằng 18, vì vậy câu trả lời là "CÓ". Một cạnh tinh tế là khi hai chữ số nằm ở đường viền, như`1 9 0`-ở đây chỉ có một cặp`(1, 9)`đáp ứng điều kiện. Mã bất cẩn có thể đếm sai các cặp hoặc cố gắng tính tổng cùng một phần tử hai lần, đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi trường hợp kiểm thử, hãy xem xét tất cả ba cặp chữ số có thể có và kiểm tra xem tổng của chúng có ít nhất là 10 hay không. Nếu bất kỳ cặp nào đáp ứng tiêu chí, hãy in "CÓ"; nếu không thì in "KHÔNG". Điều này đúng vì nó kiểm tra tất cả các kết hợp, nhưng nói chung, việc kiểm tra tất cả các cặp có thể không hiệu quả nếu tập đầu vào lớn hơn. Đối với ba yếu tố, lực lượng vũ phu rất nhanh. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tinh tế hơn một chút là chúng ta chỉ cần kiểm tra tổng của ba cặp`(a + b)`,`(a + c)`,`(b + c)`. Điều này hiệu quả vì điều kiện là đối xứng và chỉ phụ thuộc vào các cặp và vì chỉ có ba phần tử nên ba tổng này bao hàm mọi khả năng. Không cần sắp xếp hoặc cấu trúc dữ liệu nâng cao hơn. Việc kiểm tra có thể được thực hiện nội tuyến, dẫn đến việc triển khai rất đơn giản và dễ đọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra tổng cặp) | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, đọc ba số nguyên`a`,`b`, Và`c`. 
3. Tính tổng của ba cặp có thể có:`a + b`,`a + c`, Và`b + c`. 
4. Kiểm tra xem có tổng nào lớn hơn hoặc bằng 10 không. 
5. Nếu có ít nhất một tổng đáp ứng tiêu chí, ghi "CÓ". Nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Thuật toán kiểm tra từng cặp chữ số riêng biệt. Bởi vì bài toán chỉ hỏi **bất kỳ cặp nào** có đạt tổng bằng 10 hay không, nên bao gồm cả ba cặp là đủ. Không có trường hợp ẩn nào ngoài ba trường hợp này nên kết quả được đảm bảo chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    if a + b >= 10 or a + c >= 10 or b + c >= 10:
        print("YES")
    else:
        print("NO")
```Mã trực tiếp thực hiện thuật toán tối ưu. Chúng tôi sử dụng`sys.stdin.readline`để nhập liệu nhanh, điều này quan trọng hơn khi`t`là lớn. Mỗi trường hợp kiểm thử được xử lý độc lập và điều kiện được đánh giá bằng các câu lệnh OR logic đơn giản, đảm bảo tất cả các cặp đều được kiểm tra. sử dụng`>= 10`đảm bảo chúng tôi nắm bắt được ranh giới một cách chính xác. Không có vòng lặp nào ngoài việc đọc các trường hợp kiểm thử nên độ phức tạp là tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`8 1 2`| một | b | c | a+b | a+c | b+c | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 8 | 1 | 2 | 9 | 10 | 3 | CÓ | 

Cặp đôi`(a, c)`tổng bằng 10, vì vậy kết quả đầu ra là "CÓ". 

### Ví dụ 2 

đầu vào:`4 4 5`| một | b | c | a+b | a+c | b+c | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 4 | 4 | 5 | 8 | 9 | 9 | KHÔNG | 

Không có cặp nào đạt tổng bằng 10 nên kết quả là "KHÔNG". 

Những dấu vết này cho thấy thuật toán xác định chính xác các tổng cặp và tạo ra kết quả mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm 3 lần kiểm tra tổng, vì vậy đối với t trường hợp, đó là O(3t) → O(t) | 
| Không gian | O(1) | Chỉ một số lượng biến không đổi được sử dụng cho mỗi trường hợp thử nghiệm | 

Được cho`t <= 1000`và công việc liên tục cho mỗi trường hợp thử nghiệm, giải pháp cực kỳ nhanh chóng. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        if a + b >= 10 or a + c >= 10 or b + c >= 10:
            output.append("YES")
        else:
            output.append("NO")
    return "\n".join(output)

# Provided samples
assert run("5\n8 1 2\n4 4 5\n9 9 9\n0 0 0\n8 5 3\n") == "YES\nNO\nYES\nNO\nYES", "sample 1"

# Custom cases
assert run("2\n0 0 0\n9 9 1\n") == "NO\nYES", "edge min and max"
assert run("1\n5 5 5\n") == "YES", "all equal and sum >= 10"
assert run("1\n1 2 3\n") == "NO", "all small numbers"
assert run("1\n0 9 1\n") == "YES", "boundary pair"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | KHÔNG | chữ số nhỏ nhất, không có cặp ≥ 10 | 
| 9 9 1 | CÓ | chữ số lớn nhất, một cặp hợp lệ | 
| 5 5 5 | CÓ | tất cả các chữ số bằng nhau ≥ 5, tổng ≥ 10 | 
| 1 2 3 | KHÔNG | mọi số nhỏ, tổng < 10 | 
| 0 9 1 | CÓ | tổng cặp đường biên 10 | 

## Vỏ cạnh 

Thuật toán xử lý trường hợp cạnh trong đó tất cả các chữ số đều bằng 0. đầu vào`0 0 0`cho kết quả là tổng 0, 0, 0. Không có kết quả nào đáp ứng tiêu chí nên kết quả là "KHÔNG". Đối với chữ số tối đa`9 9 9`, tất cả các tổng là 18, tạo ra "CÓ". Đối với các điều kiện biên như`1 9 0`, nó xác định chính xác`(1, 9)`ghép nối và xuất ra "CÓ". Mọi kịch bản không rõ ràng đều được đề cập vì thuật toán kiểm tra rõ ràng cả ba cặp có thể xảy ra.
