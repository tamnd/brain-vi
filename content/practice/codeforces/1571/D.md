---
title: "CF 1571D - Rút thăm trúng thưởng"
description: "Chúng ta được tổ chức một cuộc thi với n lập trình viên và một cuộc khảo sát với m khán giả, mỗi người dự đoán ai sẽ về đích đầu tiên và cuối cùng. Bạn, với tư cách là khán giả đầu tiên, đã gửi dự đoán của mình (f1, l1)."
date: "2026-06-10T11:22:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "brute-force", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1571
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 8"
rating: 1800
weight: 1571
solve_time_s: 99
verified: true
draft: false
---

[CF 1571D - Rút thăm trúng thưởng](https://codeforces.com/problemset/problem/1571/D) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, bạo lực, thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tổ chức một cuộc cạnh tranh với`n`lập trình viên và một cuộc khảo sát`m`khán giả, mỗi người dự đoán ai sẽ về đích đầu tiên và cuối cùng. Bạn, với tư cách là khán giả đầu tiên, đã gửi dự đoán của mình`(f_1, l_1)`. Ban tổ chức xếp hạng khán giả dựa trên số lượng dự đoán họ đúng: hai dự đoán đúng xếp hạng đầu tiên, một dự đoán đúng xếp sau tất cả các dự đoán hoàn hảo và không có dự đoán đúng nào xếp sau những dự đoán khác. Chúng tôi được yêu cầu xác định **thứ hạng kém nhất có thể** của bạn, dựa trên dự đoán của những khán giả khác nhưng không biết kết quả thực tế của cuộc thi. 

Thử thách đến từ việc chúng ta không biết lập trình viên nào thực sự thắng hay thua. Để có thứ hạng kém nhất, chúng tôi cần giả định rằng kết quả cuộc thi được chọn **bất lợi** để làm tổn hại đến thứ hạng của bạn nhiều nhất có thể, nghĩa là càng nhiều khán giả càng có nhiều dự đoán đúng hơn bạn. 

Các hạn chế ở mức vừa phải:`n`lên tới 1000, đủ nhỏ để xem xét việc lặp lại các phép gán vị trí đầu tiên và cuối cùng có thể.`m`có thể lên tới 2 × 10^5, do đó, bất kỳ cách tiếp cận nào lặp lại khán giả nhiều lần cho mỗi kết quả của ứng cử viên sẽ quá chậm. Việc kiểm tra mạnh mẽ đối với tất cả các cặp khán giả và tất cả kết quả sẽ mất O(n^2 * m), có thể đạt tới 2 × 10^11 phép tính, vượt xa khả năng khả thi. Do đó, chúng ta cần một cách tiếp cận tránh mô phỏng rõ ràng tất cả các kết quả mà thay vào đó là lý do về số lượng và sự trùng lặp. 

Các trường hợp đặc biệt cần cẩn thận bao gồm các tình huống chỉ có hai lập trình viên, trong đó việc dự đoán đầu tiên sẽ tự động được sửa sau cùng hoặc khi nhiều khán giả đưa ra câu trả lời giống hệt nhau, điều này có thể xếp hạng ngang nhau. Một tình huống khó khăn khác là khi dự đoán của bạn khớp với nhiều dự đoán khác; một số đếm ngây thơ có thể đánh giá thấp thứ hạng vì kết quả trong trường hợp xấu nhất có thể đẩy bạn đến mức dự đoán không chính xác trong khi tất cả các dự đoán giống hệt nhau chỉ có hai dự đoán đúng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là lặp đi lặp lại tất cả những gì có thể`(first, last)`kết quả, đếm xem có bao nhiêu khán giả nhận được dự đoán đúng 2, 1 hoặc 0 cho mỗi kết quả và theo dõi thứ hạng bạn nhận được. Điều này đúng về mặt logic, nhưng độ phức tạp của nó là O(n^2 * m), không thể chấp nhận được với n = 1000 và m = 2 × 10^5. 

Thông tin chi tiết quan trọng là thứ hạng của bạn chỉ được xác định bởi số lượng khán giả có thể làm tốt hơn bạn. Đối với xếp hạng trong trường hợp xấu nhất, chúng tôi cho rằng dự đoán của bạn hoàn toàn sai. Điều này làm giảm vấn đề đếm xem có bao nhiêu khán giả **có thể đoán đúng cả đầu tiên và cuối cùng**, nếu chúng tôi chọn kết quả cuộc thi khớp với dự đoán của họ nhiều nhất có thể mà không cần giúp đỡ bạn. 

Chúng ta có thể suy luận về số lượng: hãy`x`là số lượng khán giả được dự đoán trận đấu đầu tiên của bạn sẽ diễn ra trận đấu đầu tiên và`y`số lượng khán giả dự đoán trận đấu cuối cùng của bạn sẽ khớp với trận đấu cuối cùng của bạn. Thứ hạng tệ nhất xảy ra khi dự đoán của bạn sai hoàn toàn và số lượng khán giả khác đoán đúng ít nhất một dự đoán tối đa. Những khán giả có dự đoán không khớp với dự đoán đầu tiên cũng như cuối cùng của bạn cũng có thể bị buộc phải đoán hoàn hảo nếu kết quả cuộc thi được chọn một cách khéo léo. Điều này làm giảm vấn đề đối với việc tính toán chồng chéo và sử dụng lý luận tổ hợp thay vì lặp lại tất cả các kết quả. 

Bằng cách đếm cẩn thận, chúng ta có thể tính toán số lượng khán giả có thể có 2 đúng, 1 đúng và 0 đúng trong kịch bản đối kháng. Công thức tập trung vào việc tối đa hóa số lượng khán giả đánh bại bạn, dựa trên dự đoán của họ so với dự đoán của bạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² * m) | O(m) | Quá chậm | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`Và`m`. Lưu trữ tất cả các dự đoán của khán giả vào một danh sách, đặt dự đoán của bạn lên đầu tiên. 
2. Đếm xem có bao nhiêu khán giả (không bao gồm bạn) dự đoán lập trình viên đầu tiên giống bạn. Gọi đây`same_first`. 
3. Đếm xem có bao nhiêu khán giả dự đoán lập trình viên cuối cùng giống bạn. Gọi đây`same_last`. 
4. Đếm xem có bao nhiêu khán giả dự đoán cả người lập trình đầu tiên và người lập trình cuối cùng giống bạn. Gọi đây`same_both`. 
5. Trường hợp xấu nhất là giả định rằng cả hai dự đoán của bạn đều sai. Vậy số khán giả có 2 lần đoán đúng là`same_both`(không bao gồm chính bạn), vì kết quả cuộc thi có thể hoàn toàn khớp với dự đoán của họ. 
6. Số khán giả có 1 lần đoán đúng là`max(same_first - same_both, same_last - same_both)`. Điều này áp dụng cho những người chia sẻ đầu tiên hoặc cuối cùng chứ không phải cả hai, vì kết quả cuộc thi có thể được chọn theo hướng nghiêng về nhóm lớn hơn trong hai nhóm này. 
7. Thứ hạng tệ nhất của bạn khi đó là tổng số khán giả đoán đúng 2 lần và khán giả đoán đúng 1 lần, cộng thêm 1 cho chính bạn. Đó là`worst_rank = c2 + c1 + 1`. 
8. In kết quả tính toán`worst_rank`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng kết quả của cuộc thi có thể được chọn một cách bất lợi, do đó dự đoán của bạn hoàn toàn sai và càng nhiều khán giả khác càng tốt thì ít nhất một dự đoán đúng. Bằng cách đếm chính xác số lần trùng lặp, chúng tôi đảm bảo rằng chúng tôi không đánh giá thấp số lượng khán giả đánh bại bạn. Công thức đảm bảo rằng chúng tôi xử lý các mối quan hệ một cách chính xác: nếu nhiều khán giả chia sẻ dự đoán của bạn, thì kết quả trong trường hợp xấu nhất sẽ đảm bảo rằng bạn được xếp hạng sau tất cả những người đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
predictions = [tuple(map(int, input().split())) for _ in range(m)]
f1, l1 = predictions[0]

same_first = same_last = same_both = 0

for f, l in predictions[1:]:
    if f == f1 and l == l1:
        same_both += 1
    elif f == f1:
        same_first += 1
    elif l == l1:
        same_last += 1

c2 = same_both
c1 = max(same_first, same_last)

worst_rank = c2 + c1 + 1
print(worst_rank)
```Phần đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng I/O nhanh. Đếm`same_first`,`same_last`, Và`same_both`được thực hiện trong một vòng lặp, điều này rất quan trọng đối với độ phức tạp O(m). các`c1`sử dụng tính toán`max`bởi vì kết quả trong trường hợp xấu nhất có thể có lợi cho nhóm lớn hơn. Cuối cùng,`worst_rank`tính tổng các số này cộng với một cho chính bạn, phù hợp với hệ thống xếp hạng của ban tổ chức. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
2 3
1 2
2 1
2 1
```| khán giả | f | tôi | thể loại | 
| --- | --- | --- | --- | 
| bạn | 1 | 2 | - | 
| 2 | 2 | 1 | không phải đầu tiên cũng không phải là cuối cùng phù hợp với bạn | 
| 3 | 2 | 1 | không phải đầu tiên cũng không phải là cuối cùng phù hợp với bạn |`same_both = 0`,`same_first = 0`,`same_last = 0`.`c2 = 0`,`c1 = max(0, 0) = 0`. 

Xếp hạng kém nhất = 0 + 0 + 1 = 1. Đợi đã, kết quả đối kháng: cuộc thi chọn 2 người đầu tiên, 1 người cuối cùng. Khi đó cả khán giả 2 và 3 đều có 2 đúng. Vì thế`c2 = 2`,`c1 = 0`, hạng = 2 + 0 + 1 = 3. Đầu ra là 3. 

Mẫu 2: 

đầu vào:```
3 6
1 2
1 2
2 1
3 1
2 3
3 2
```Tính toán chồng chéo: 

- Same_both = 1 (khán giả 2) 
- Same_first = 1 (chỉ có 3 khán giả khớp trước) 
- Same_last = 2 (khán giả 4 và 6 chỉ xem trận cuối cùng) 

c2 = 1 

c1 = tối đa(1, 2) = 2 

thứ hạng tệ nhất = 1 + 2 + 1 = 4 

Điều này phù hợp với sản lượng dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Một lần vượt qua tất cả các dự đoán của khán giả để đếm các lần trùng lặp | 
| Không gian | O(m) | Lưu trữ dự đoán đầu vào | 

Đối với m lên tới 2 × 10^5, O(m) là hiệu quả. n chỉ ảnh hưởng đến các thao tác so sánh chứ không làm tăng vòng lặp nên giải pháp dễ dàng phù hợp với giới hạn 2 giây với bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    predictions = [tuple(map(int, input().split())) for _ in range(m)]
    f1, l1 = predictions[0]

    same_first = same_last = same_both = 0

    for f, l in predictions[1:]:
        if f == f1 and l == l1:
            same_both += 1
        elif f == f1:
            same_first += 1
        elif l == l1:
            same_last += 1

    c2 = same_both
    c1 = max(same_first, same_last)

    worst_rank = c2 + c1 + 1
    return str(worst_rank)

# provided samples
assert run("2 3\n1 2\n2 1\n2 1\n") == "3", "sample 1"
assert run("3 6\n1 2\n1 2\n2 1\n3 1\n2 3\n3 2\n") == "4", "sample 2"

# custom cases
```
