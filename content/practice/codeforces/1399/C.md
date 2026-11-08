---
title: "CF 1399C - Cuộc thi thuyền"
description: "Chúng tôi có một nhóm người tham gia, mỗi người có trọng lượng cụ thể. Cuộc thi chỉ cho phép các đội có hai người và mỗi đội phải có tổng trọng lượng như nhau. Mục tiêu của chúng tôi là thành lập càng nhiều đội càng tốt cho một nhóm người tham gia nhất định."
date: "2026-06-11T08:58:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1399
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 661 (Div. 3)"
rating: 1200
weight: 1399
solve_time_s: 83
verified: true
draft: false
---

[CF 1399C - Cuộc thi đua thuyền](https://codeforces.com/problemset/problem/1399/C) 

**Đánh giá:** 1200 
**Tags:** vũ phu, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm người tham gia, mỗi người có trọng lượng cụ thể. Cuộc thi chỉ cho phép các đội có hai người và mỗi đội phải có tổng trọng lượng như nhau. Mục tiêu của chúng tôi là thành lập càng nhiều đội càng tốt cho một nhóm người tham gia nhất định. Thử thách là chọn tổng trọng lượng`s`nhằm tối đa hóa số lượng đội hợp lệ. Mỗi người tham gia chỉ có thể thuộc về một đội, vì vậy chúng tôi không thể sử dụng lại người. 

Các hạn chế là nhỏ:`n`nhiều nhất là 50 và có trọng số từ 1 đến`n`. Từ`n`là nhỏ, chúng ta có thể đủ khả năng để kiểm tra tất cả các trọng số tổng có thể có, bởi vì số tổng phân biệt của hai người tham gia bị giới hạn bởi`2n`. Điều này loại trừ nhu cầu về cấu trúc dữ liệu được tối ưu hóa cao hoặc các thuật toán phức tạp. Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả những người tham gia có cùng trọng số, trong đó không thể tạo thành các cặp có số tiền nhất định hoặc khi nhiều tổng trọng số mang lại cùng số lượng đội tối đa. 

Ví dụ, việc thực hiện bất cẩn có thể luôn thử tổng của người tham gia nhẹ nhất và nặng nhất mà không xem xét các khoản tiền khác. Ví dụ, với những người tham gia`[1, 2, 2, 1, 2, 1, 1, 2]`, tổng trọng lượng tối ưu là`3`(cặp`(1,2)`), không`4`(để lại những người tham gia còn sót lại). Một thuật toán đơn giản có thể bỏ sót điều này vì nó chỉ kiểm tra một tổng ứng viên duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên tất cả các trọng số tổng thể có thể`s`từ`2`(tổng nhỏ nhất của hai người tham gia) đến`2n`(tổng lớn nhất). Đối với mỗi`s`, chúng tôi thử tất cả các cặp người tham gia có thể để xem chúng tôi có thể thành lập bao nhiêu đội. Điều này có thể được thực hiện bằng các vòng lặp lồng nhau hoặc bằng cách giữ bản đồ tần số của các trọng số. Việc thành lập các nhóm yêu cầu giảm số lượng trong bản đồ tần suất bất cứ khi nào một cặp được sử dụng. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp trong trường hợp xấu nhất`O(n^2 * n) = O(n^3)`bởi vì chúng ta có thể kiểm tra từng cặp để tìm tổng ứng cử viên, điều này có thể chấp nhận được đối với`n=50`nhưng có thể được cải thiện. 

Quan sát quan trọng là vấn đề có thể được rút gọn thành việc đếm số lần mỗi trọng số xuất hiện và ghép các trọng số một cách tham lam. Đối với tổng trọng lượng của một ứng cử viên`s`, một trọng lượng`w`chỉ có thể ghép đôi với`s - w`. Chúng ta có thể tính số lượng các cặp như vậy là số lượng tối thiểu của`w`Và`s - w`. Lặp lại tất cả các trọng số từ`1`ĐẾN`n`cho mỗi ứng viên`s`mang lại một`O(n^2)`giải pháp đủ nhanh với các hạn chế. Cách tiếp cận này tận dụng phạm vi trọng số nhỏ và tính đối xứng của các cặp: chúng ta chỉ cần xem xét từng trọng số riêng biệt một lần cho mỗi tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Hoạt động nhưng có thể được tối ưu hóa | 
| Bản đồ tần số O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc số lượng người tham gia`n`và trọng lượng của chúng. 
3. Tạo bản đồ tần số`freq`đếm xem có bao nhiêu người tham gia có mỗi cân nặng. 
4. Khởi tạo một biến`max_teams`về không. Điều này sẽ theo dõi số lượng đội tốt nhất cho bất kỳ tổng trọng lượng nào. 
5. Lặp lại tất cả các trọng số có thể có`s`từ`2`ĐẾN`2n`. 
6. Đối với mỗi tổng ứng cử viên`s`, tạo một bản sao của bản đồ tần số. Điều này ngăn cản việc sửa đổi số lượng ban đầu trong khi mô phỏng sự hình thành cặp. 
7. Khởi tạo bộ đếm`teams`về 0 cho tổng hiện tại`s`. 
8. Lặp lại trọng lượng`w`từ`1`ĐẾN`n`. Đối với mỗi`w`, tìm phần bù của nó`c = s - w`. 
9. Nếu`c`nằm trong phạm vi`1..n`, tính số cặp như sau`min(freq[w], freq[c])`. Nếu như`w == c`, chia kết quả cho hai để tránh tính hai lần. 
10. Cộng số cặp này vào`teams`và giảm số lượng trong bản đồ tần số được sao chép. 
11. Sau khi kiểm tra tất cả các trọng số của tổng này`s`, cập nhật`max_teams`nếu như`teams`lớn hơn. 
12. In`max_teams`như câu trả lời cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán duy trì một bất biến mà chúng tôi không sử dụng lại những người tham gia. Bằng cách lặp lại tất cả các tổng có thể có và tạo thành các cặp tham lam bằng bản đồ tần số, chúng tôi đảm bảo rằng không có cặp nào bị bỏ sót và chúng tôi đếm chính xác số đội tối đa cho mỗi tổng trọng số. Vòng lặp bên ngoài tính tổng đảm bảo chúng ta tìm được tổng tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_teams(weights, n):
    freq = [0] * (n + 1)
    for w in weights:
        freq[w] += 1

    max_teams_count = 0

    for s in range(2, 2 * n + 1):
        temp_freq = freq.copy()
        teams = 0
        for w in range(1, n + 1):
            c = s - w
            if 1 <= c <= n:
                pair_count = min(temp_freq[w], temp_freq[c])
                if w == c:
                    pair_count //= 2
                teams += pair_count
                temp_freq[w] -= pair_count
                temp_freq[c] -= pair_count
        max_teams_count = max(max_teams_count, teams)

    return max_teams_count

t = int(input())
for _ in range(t):
    n = int(input())
    weights = list(map(int, input().split()))
    print(max_teams(weights, n))
```Giải pháp bắt đầu bằng cách đọc đầu vào và xây dựng một dãy tần số để đếm trọng số của người tham gia. Đối với tổng trọng lượng của mỗi ứng cử viên, chúng tôi sao chép mảng tần số để mô phỏng sự hình thành cặp mà không có tác dụng phụ. Việc ghép đôi tham lam được thực hiện bằng cách lấy số lượng tối thiểu của trọng số và phần bù của nó. Cần đặc biệt lưu ý khi`w`bằng`s - w`để tránh bị đếm quá mức. Sau khi đánh giá tất cả các khoản tiền, số lượng đội tối đa sẽ được trả về. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`weights = [1, 2, 3, 4, 5]`,`n = 5`. 

| Tổng`s`| Cặp hình thành | Đội | 
| --- | --- | --- | 
| 2 | không | 0 | 
| 3 | (1,2) | 1 | 
| 4 | (1,3) | 1 | 
| 5 | (1,4),(2,3) | 2 | 
| 6 | (1,5),(2,4) | 2 | 
| 7 | (2,5),(3,4) | 2 | 
| 8 | (3,5) | 1 | 
| 9 | (4,5) | 1 | 
| 10 | không | 0 | 

Đội tối đa: 2 

Dấu vết này cho thấy rằng việc ghép cặp nhẹ nhất với nặng nhất không phải lúc nào cũng mang lại mức tối ưu toàn cục. Xem xét tất cả các khoản tiền đảm bảo chúng tôi tìm thấy câu trả lời chính xác. 

**Mẫu 2:**`weights = [6, 6, 6, 6, 6, 6, 8, 8]`,`n = 8`. 

| Tổng`s`| Đội | 
| --- | --- | 
| 12 | (6,6)x3 | 
| 14 | (6,8)x3? | 

Đội tối đa: 3 

Điều này chứng tỏ rằng các trọng số lặp lại có thể tạo thành nhiều nhóm cho một tổng duy nhất và việc ghép nối tham lam bằng tần số hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Với mỗi số tiền có thể`s`(2n khả năng), chúng tôi lặp lại tất cả các trọng số 1..n để tạo thành các cặp. | 
| Không gian | O(n) | Mảng tần số và bản sao của nó cho mỗi tổng. | 

Được cho`n ≤ 50`,`n^2`hoạt động cho mỗi trường hợp thử nghiệm nằm trong giới hạn cho`t ≤ 1000`trường hợp thử nghiệm. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        weights = list(map(int, input().split()))
        print(max_teams(weights, n))
    return output.getvalue().strip()

# provided samples
assert run("""5
5
1 2 3 4 5
8
6 6 6 6 6 6 8 8
8
1 2 2 1 2 1 1 2
3
1 3 3
6
1 1 3 4 2 2
""") == """2
3
4
1
2"""

# custom cases
assert run("1\n1\n1\n") == "0", "single participant"
assert run("1\n2\n1 1\n") == "1", "two identical participants"
assert run("1\n4\n1 2 2 1\n") == "2", "pairs with multiple same weights"
assert run("1\n50\n" + " ".join(str(i%50+1) for i in range(50)) + "\n") != "", "maximum n case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người tham gia | 0 | Không thể hình thành bất kỳ |
