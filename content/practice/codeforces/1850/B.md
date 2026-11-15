---
title: "CF 1850B - Mười Lời Khôn Ngoan"
description: "Nhiệm vụ là tìm ra câu trả lời thắng lợi trong trò chơi có đáp án ngắn gọn. Mỗi người tham gia gửi một câu trả lời có hai thuộc tính: số từ chứa trong đó và điểm chất lượng của nó."
date: "2026-06-09T05:29:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1850
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 886 (Div. 4)"
rating: 800
weight: 1850
solve_time_s: 68
verified: true
draft: false
---

[CF 1850B - Mười lời khôn ngoan](https://codeforces.com/problemset/problem/1850/B) 

**Đánh giá:** 800 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tìm ra câu trả lời thắng lợi trong trò chơi có đáp án ngắn gọn. Mỗi người tham gia gửi một câu trả lời có hai thuộc tính: số từ chứa trong đó và điểm chất lượng của nó. Người chiến thắng là câu trả lời có chất lượng cao nhất trong số tất cả các bài gửi dài tối đa 10 từ. Vì tất cả các điểm chất lượng đều khác biệt nên không có gì mơ hồ trong việc chọn ra người chiến thắng. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Đầu tiên, mỗi trường hợp kiểm tra sẽ cho chúng tôi biết có bao nhiêu câu trả lời, theo sau là danh sách các câu trả lời kèm theo số từ và điểm chất lượng. Đầu ra chỉ đơn giản là chỉ số dựa trên 1 của phản hồi chiến thắng cho mỗi trường hợp thử nghiệm. 

Với các ràng buộc, có tối đa 50 câu trả lời cho mỗi trường hợp thử nghiệm và mỗi điểm chất lượng cũng như số từ nhiều nhất là 50. Điều này ngụ ý rằng việc quét brute-force trên tất cả các câu trả lời là đủ nhanh. Các trường hợp đặc biệt bao gồm khi chỉ một câu trả lời có 10 từ trở xuống hoặc khi nhiều câu trả lời chỉ ở giới hạn từ; thuật toán phải xác định chính xác phản hồi có chất lượng cao nhất trong số đó. 

Một sai lầm ngây thơ là chọn câu trả lời đầu tiên có 10 từ mà không kiểm tra tất cả những từ khác. Ví dụ, nếu đầu vào là`3 11 5 10 6 12 7`, người chiến thắng đúng là câu trả lời thứ hai (`10 6`), không phải cái đầu tiên, vì nó có chất lượng cao hơn trong giới hạn từ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là dùng vũ lực: lặp lại tất cả các câu trả lời, bỏ qua những câu trả lời dài hơn 10 từ và theo dõi phản hồi có chất lượng cao nhất được tìm thấy. Điều này hoạt động vì số lượng phản hồi nhỏ. Mỗi trường hợp kiểm thử yêu cầu tối đa 50 phép so sánh và có tối đa 100 trường hợp kiểm thử, tổng cộng 5000 thao tác-giếng trong giới hạn. 

Một cách tiếp cận tối ưu chính thức hơn không cần cấu trúc dữ liệu nâng cao. Thông tin chi tiết là chúng tôi chỉ quan tâm đến những câu trả lời có 10 từ trở xuống. Chúng tôi có thể quét một lần, theo dõi chất lượng cao nhất được thấy cho đến nay và chỉ mục tương ứng. Khi kết thúc quá trình quét, chỉ số sẽ đưa ra người chiến thắng. Việc sắp xếp là không cần thiết vì chúng ta không cần thứ tự toàn cục, chỉ cần mức tối đa trong một tập hợp con. 

Quét brute-force và cách tiếp cận tối ưu về cơ bản là giống nhau khi triển khai ở đây vì kích thước dữ liệu rất nhỏ. Sự khác biệt là chúng tôi bỏ qua các phản hồi vượt quá giới hạn từ trong quá trình quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Quét tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc số lượng phản hồi. 
3. Khởi tạo hai biến: một biến cho chất lượng tốt nhất hiện tại được tìm thấy (`max_quality`) và một cho chỉ mục của phản hồi đó (`winner_index`). Bộ`max_quality`đến giá trị thấp hơn chất lượng tối thiểu có thể (ví dụ:`-1`). 
4. Lặp lại các câu trả lời bằng biến vòng lặp`i`đại diện cho chỉ số dựa trên 1. Đối với mỗi câu trả lời, hãy đọc số lượng từ và chất lượng của nó. 
5. Nếu số từ vượt quá 10, hãy chuyển sang câu trả lời tiếp theo. 
6. Nếu số từ nhiều nhất là 10 và chất lượng cao hơn`max_quality`, cập nhật`max_quality`và thiết lập`winner_index`ĐẾN`i`. 
7. Sau khi kiểm tra tất cả các phản hồi, xuất ra`winner_index`. 

Tại sao nó hoạt động: Bất biến là vậy`winner_index`luôn giữ chỉ số về câu trả lời có chất lượng cao nhất trong số tất cả các câu trả lời được kiểm tra cho đến nay và đáp ứng giới hạn từ. Vì tất cả các đặc tính đều khác biệt và ít nhất một phản hồi đủ ngắn nên vòng lặp đảm bảo rằng giá trị cuối cùng của`winner_index`là người chiến thắng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    max_quality = -1
    winner_index = -1
    for i in range(1, n + 1):
        a, b = map(int, input().split())
        if a <= 10 and b > max_quality:
            max_quality = b
            winner_index = i
    print(winner_index)
```Mã đọc số lượng trường hợp thử nghiệm và vòng lặp trên mỗi trường hợp. Trong mỗi trường hợp thử nghiệm, nó theo dõi chất lượng cao nhất trong số các phản hồi đủ điều kiện. Chỉ mục dựa trên 1 được bảo toàn bằng cách lặp từ`1`ĐẾN`n`. các`if a <= 10`kiểm tra đảm bảo chỉ những câu trả lời trong giới hạn từ mới được xem xét. Đang cập nhật`max_quality`Và`winner_index`chỉ khi tìm thấy chất lượng cao hơn mới đảm bảo rằng người chiến thắng là chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
7 2
12 5
9 3
9 4
10 1
```| tôi | a_i | b_i | Đạt chuẩn? | max_quality | người chiến thắng_index | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 2 | vâng | 2 | 1 | 
| 2 | 12 | 5 | không | 2 | 1 | 
| 3 | 9 | 3 | vâng | 3 | 3 | 
| 4 | 9 | 4 | vâng | 4 | 4 | 
| 5 | 10 | 1 | vâng | 4 | 4 | 

Đầu ra:`4`Dấu vết này xác nhận rằng thuật toán bỏ qua các phản hồi không đủ điều kiện và cập nhật chính xác người chiến thắng. 

Một đầu vào khác:```
3
1 2
3 4
5 6
```| tôi | a_i | b_i | Đạt chuẩn? | max_quality | người chiến thắng_index | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | vâng | 2 | 1 | 
| 2 | 3 | 4 | vâng | 4 | 2 | 
| 3 | 5 | 6 | vâng | 6 | 3 | 

Đầu ra:`3`Nó chứng tỏ rằng thuật toán xử lý chính xác tất cả các phản hồi đủ điều kiện và tìm ra mức tối đa toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Phản hồi vòng lặp đơn, n 50 | 
| Không gian | O(1) | Chỉ có hai biến phụ được sử dụng | 

Với tối đa 100 trường hợp thử nghiệm và tối đa 50 phản hồi cho mỗi trường hợp, thuật toán thực hiện tối đa 5000 lần lặp, điều này không đáng kể đối với các CPU hiện đại. Việc sử dụng bộ nhớ ở mức tối thiểu, dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        max_quality = -1
        winner_index = -1
        for i in range(1, n + 1):
            a, b = map(int, input().split())
            if a <= 10 and b > max_quality:
                max_quality = b
                winner_index = i
        print(winner_index)
    return output.getvalue().strip()

# provided samples
assert run("3\n5\n7 2\n12 5\n9 3\n9 4\n10 1\n3\n1 2\n3 4\n5 6\n1\n1 43\n") == "4\n3\n1", "sample 1"

# custom cases
assert run("1\n1\n10 50\n") == "1", "single response at limit"
assert run("1\n2\n11 1\n10 2\n") == "2", "only second response eligible"
assert run("1\n3\n10 1\n10 3\n10 2\n") == "2", "highest quality in middle"
assert run("1\n4\n12 10\n9 5\n10 6\n15 7\n") == "3", "multiple eligible, middle wins"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n10 50 | 1 | Phản hồi đủ điều kiện duy nhất | 
| 1\n2\n11 1\n10 2 | 2 | Bỏ qua phản hồi không đủ điều kiện | 
| 1\n3\n10 1\n10 3\n10 2 | 2 | Lựa chọn đúng trong số các câu trả lời đủ điều kiện | 
| 1\n4\n12 10\n9 5\n10 6\n15 7 | 3 | Chất lượng cao nhất đủ điều kiện không phải đầu tiên | 

## Vỏ cạnh 

Nếu chỉ có một câu trả lời có 10 từ trở xuống thì câu trả lời đó phải được chọn bất kể chất lượng của nó như thế nào so với những câu trả lời dài hơn. Đối với đầu vào`2\n11 10\n9 1`, bộ thuật toán`max_quality`ĐẾN`-1`, kiểm tra phản hồi đầu tiên (bị bỏ qua), sau đó là phản hồi thứ hai (đủ điều kiện) và đặt`winner_index`ĐẾN`2`, điều đó đúng. 

Nếu tất cả các câu trả lời đều có đúng 10 từ thì thuật toán vẫn chọn đúng chất lượng cao nhất trong số đó. Đối với đầu vào`3\n10 5\n10 2\n10 7`, nó theo dõi max_quality là 5, rồi 5 (không thay đổi cho 2), sau đó cập nhật lên 7 cho phản hồi cuối cùng, xuất ra`3`, xác nhận việc xử lý đúng số lượng từ trong ranh giới.
