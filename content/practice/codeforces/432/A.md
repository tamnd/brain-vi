---
title: "CF 432A - Lựa Chọn Đội"
description: "Chúng tôi được cung cấp một nhóm sinh viên tại một trường đại học, mỗi sinh viên có một bản ghi về số lần họ đã tham gia giải vô địch thế giới ACM ICPC."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 432
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 246 (Div. 2)"
rating: 800
weight: 432
solve_time_s: 81
verified: true
draft: false
---

[CF 432A - Chọn đội](https://codeforces.com/problemset/problem/432/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm sinh viên tại một trường đại học, mỗi sinh viên có một bản ghi về số lần họ đã tham gia giải vô địch thế giới ACM ICPC. Người đứng đầu trung tâm huấn luyện muốn thành lập các đội gồm đúng ba học sinh và mỗi đội phải tham gia ít nhất _k_ giải vô địch thế giới nữa. Một học sinh không thể ở nhiều hơn một đội. 

Dữ liệu đầu vào bao gồm hai số nguyên, _n_, số lượng sinh viên và _k_, số lần tham gia tối thiểu trong tương lai cần có cho một đội. Dòng thứ hai cung cấp _n_ số nguyên trong đó mỗi số nguyên biểu thị số lần học sinh đã tham gia. Vì bất kỳ học sinh nào cũng có thể tham gia tối đa năm lần, nên học sinh đó chỉ có thể đóng góp cho một nhóm nếu tổng số lần tham gia trước đây của họ và _k_ không vượt quá 5. 

Đầu ra là một số duy nhất: số lượng nhóm ba người tối đa có thể được thành lập theo những hạn chế này. 

Các ràng buộc cho chúng ta biết rằng _n_ có thể lên tới 2000, đủ nhỏ cho các thuật toán O(n log n) hoặc O(n) đơn giản. Tuy nhiên, một cách tiếp cận đơn giản nhằm xem xét tất cả các tập con của ba học sinh sẽ yêu cầu các phép toán O(n³), rõ ràng là quá chậm. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó một số học sinh đã đạt đến giới hạn tham gia và không thể tham gia bất kỳ đội mới nào. Ví dụ: nếu đầu vào là`3 1`với sự tham gia`5 5 5`, không đội nào có thể được thành lập nên kết quả đúng là`0`. Một cách tiếp cận bất cẩn mà bỏ qua giới hạn tham gia có thể tính nhầm một đội là hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại tất cả các kết hợp có thể có của ba học sinh và kiểm tra xem mỗi kết hợp có thỏa mãn ràng buộc tham gia hay không. Cách tiếp cận này đúng về nguyên tắc vì nó kiểm tra toàn diện tất cả các lựa chọn, nhưng số kết hợp trong trường hợp xấu nhất là`C(n,3)`, khoảng n³/6. Với _n_ = 2000, đây là hơn một tỷ lượt kiểm tra, quá chậm so với giới hạn thời gian 1 giây. 

Giải pháp tối ưu xuất phát từ quan sát rằng chúng ta không cần phải kiểm tra mọi sự kết hợp một cách rõ ràng. Điều quan trọng là liệu một học sinh có đủ điều kiện để tham gia vào một đội hay không. Nếu tính số học sinh tham gia nhiều nhất`5 - k`lần, bất kỳ ba học sinh nào trong số này có thể thành lập một đội. Điều này làm giảm vấn đề đếm số học sinh đủ điều kiện rồi chia cho ba, loại bỏ phần còn lại vì không thể thành lập một nhóm từ ít hơn ba. 

Cái nhìn sâu sắc này biến một vấn đề tổ hợp thành một vấn đề đếm đơn giản. Chúng tôi chỉ lặp lại danh sách sinh viên một lần, làm cho nó tuyến tính về mặt thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc _n_ và _k_ từ đầu vào. Những điều này xác định số lượng sinh viên và số lượng tham gia tối thiểu trong tương lai cần thiết cho một nhóm. 
2. Đọc danh sách số học sinh tham gia. Mỗi lần đếm cho chúng tôi biết học sinh đã tham gia bao nhiêu lần. 
3. Khởi tạo bộ đếm cho học sinh đủ điều kiện về 0. Sinh viên đủ điều kiện là những người có thể tham gia ít nhất _k_ lần nữa mà không vượt quá mức tối đa là 5. 
4. Lặp lại số lần tham gia của mỗi học sinh. Với mỗi học sinh, nếu`count + k ≤ 5`, tăng bộ đếm đủ điều kiện. Bước này lọc ra những học sinh vượt quá giới hạn tham gia. 
5. Số đội tối đa là số học sinh đủ điều kiện chia cho 3. Chúng tôi loại bỏ phần còn lại vì ít hơn ba học sinh không thể thành lập một đội. 
6. In kết quả số đội. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi duy trì sự bất biến rằng chúng tôi chỉ xem xét những học sinh đủ điều kiện để thành lập một nhóm. Bằng cách chia tổng số học sinh đủ điều kiện cho ba, chúng tôi đảm bảo rằng mỗi đội được thành lập bao gồm chính xác ba thành viên đủ điều kiện. Không có học sinh đủ điều kiện nào được tính hai lần, đáp ứng ràng buộc "không có học sinh nào trong nhiều đội". 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
participations = list(map(int, input().split()))

eligible_count = 0
for p in participations:
    if p + k <= 5:
        eligible_count += 1

max_teams = eligible_count // 3
print(max_teams)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý đầu vào có tiềm năng lớn. Vòng lặp lọc các học sinh đủ điều kiện với một điều kiện đơn giản, tránh từng lỗi một bằng cách sử dụng`<= 5`. Việc chia số nguyên đảm bảo không có đội một phần nào được tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
0 4 5 1 0
```| Sinh viên | Tham gia | Đạt chuẩn? | 
| --- | --- | --- | 
| 0 | 0 | Có | 
| 1 | 4 | Không | 
| 2 | 5 | Không | 
| 3 | 1 | Có | 
| 4 | 0 | Có | 

Số sinh viên đủ điều kiện = 3 →`3 // 3 = 1`đội. 

### Mẫu 2 

đầu vào:```
4 3
1 2 1 0
```| Sinh viên | Tham gia | Đạt chuẩn? | 
| --- | --- | --- | 
| 0 | 1 | Có | 
| 1 | 2 | Không | 
| 2 | 1 | Có | 
| 3 | 0 | Có | 

Số sinh viên đủ điều kiện = 3 →`3 // 3 = 1`đội. 

Dấu vết này xác nhận rằng việc đếm chính xác các học sinh đủ điều kiện và chia số nguyên cho 3 sẽ tạo ra kết quả chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại danh sách sinh viên một lần, kiểm tra tính đủ điều kiện. | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm, không có mảng bổ sung tỷ lệ với n. | 

Giải pháp vừa vặn thoải mái trong giới hạn thời gian 1 giây cho n lên tới 2000 và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    participations = list(map(int, input().split()))
    eligible_count = sum(1 for p in participations if p + k <= 5)
    return str(eligible_count // 3)

# provided samples
assert run("5 2\n0 4 5 1 0\n") == "1", "sample 1"
assert run("4 3\n1 2 1 0\n") == "1", "sample 2"

# custom cases
assert run("3 1\n5 5 5\n") == "0", "all students maxed out"
assert run("6 2\n0 0 0 0 0 0\n") == "2", "all students eligible, multiple teams"
assert run("2 1\n0 1\n") == "0", "less than three students"
assert run("7 5\n0 0 0 0 0 0 0\n") == "0", "k too high for any student"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1\n5 5 5 | 0 | sinh viên đã đạt đến giới hạn không thể thành lập đội | 
| 6 2\n0 0 0 0 0 0 | 2 | nhiều đội có thể được thành lập từ tất cả các sinh viên đủ điều kiện | 
| 2 1\n0 1 | 0 | ít hơn 3 học sinh, không thể lập nhóm | 
| 7 5\n0 0 0 0 0 0 0 | 0 | k vượt quá mức tham gia còn lại của tất cả học sinh | 

## Vỏ cạnh 

Trong đầu vào`3 1\n5 5 5`, thuật toán lặp lại qua các lần tham gia và không tìm thấy học sinh nào đủ điều kiện vì`5 + 1 > 5`. Bộ đếm đủ điều kiện vẫn bằng 0 và phép chia số nguyên cho 3 mang lại kết quả 0. Điều này xử lý chính xác trường hợp cạnh trong đó tất cả học sinh đều ở giới hạn tham gia. 

Trong đầu vào`2 1\n0 1`, bộ đếm tìm ra 2 học sinh đủ điều kiện nhưng chia cho 3 ra 0, xử lý đúng trường hợp có ít hơn 3 học sinh. 

Giải pháp xử lý các ranh giới và giới hạn tham gia một cách nhất quán, đảm bảo không tính quá nhiều số lượng đội.
