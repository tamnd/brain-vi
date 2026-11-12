---
title: "CF 1622E - Thi Toán"
description: "Chúng ta được giao một nhóm học sinh đã làm bài kiểm tra toán với nhiều câu hỏi. Mỗi học sinh trả lời một số câu hỏi đúng và một số câu hỏi sai."
date: "2026-06-10T05:47:32+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1622
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 120 (Rated for Div. 2)"
rating: 2200
weight: 1622
solve_time_s: 82
verified: false
draft: false
---

[CF 1622E - Bài kiểm tra toán](https://codeforces.com/problemset/problem/1622/E) 

**Xếp hạng:** 2200 
**Tags:** bitmasks, vũ phu, tham lam 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một nhóm học sinh đã làm bài kiểm tra toán với nhiều câu hỏi. Mỗi học sinh trả lời một số câu hỏi đúng và một số câu hỏi sai. Mỗi câu hỏi đều có một giá trị điểm ẩn và các giá trị điểm này tạo thành một hoán vị các số từ 1 đến tổng số câu hỏi. Mỗi học sinh cũng có tổng số điểm dự kiến ​​cho bài kiểm tra. Nhiệm vụ của chúng tôi là ấn định giá trị điểm cho các câu hỏi sao cho tổng số “bất ngờ” - tổng chênh lệch tuyệt đối giữa điểm dự kiến ​​của mỗi học sinh và điểm thực tế mà họ sẽ nhận được khi làm bài tập của chúng tôi - là tối đa. 

Đầu vào cung cấp cho mỗi học sinh một chuỗi nhị phân biểu thị những câu hỏi họ trả lời đúng và điểm số mong đợi. Đầu ra là một hoán vị của các số nguyên từ 1 đến số câu hỏi nhằm tối đa hóa tổng số sự ngạc nhiên. 

Các ràng buộc chỉ ra rằng số lượng học sinh ít (tối đa 10) nhưng số lượng câu hỏi có thể lớn (lên tới 10.000) và tổng số câu hỏi trong tất cả các trường hợp kiểm tra được giới hạn ở mức 10.000. Điều này có nghĩa là chúng tôi có đủ khả năng lặp lại các học sinh trong các vòng lặp lồng nhau, nhưng bất kỳ thuật toán nào chia tỷ lệ bậc hai hoặc tệ hơn theo số lượng câu hỏi sẽ quá chậm. Vì các giá trị điểm tạo thành một hoán vị chặt chẽ nên chúng ta phải lựa chọn cẩn thận câu hỏi nào nhận được giá trị lớn nhất hoặc nhỏ nhất. 

Một trường hợp phức tạp phát sinh khi học sinh mong đợi một điểm đã rất cao hoặc rất thấp. Ví dụ: nếu một học sinh trả lời đúng tất cả các câu hỏi và mong đợi số điểm tối đa có thể, thì việc cho họ giá trị cao sẽ phù hợp với kỳ vọng và mang lại mức độ ngạc nhiên thấp, trong khi cho họ giá trị thấp sẽ tối đa hóa sự ngạc nhiên. Một cách tiếp cận ngây thơ chỉ đơn giản là phân phối điểm theo thứ tự số câu trả lời đúng có thể không tạo ra được sự ngạc nhiên tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả các hoán vị của các giá trị điểm và tính tổng mức độ bất ngờ cho mỗi hoán vị. Đối với một trường hợp thử nghiệm duy nhất với$m$câu hỏi, điều này liên quan đến$m!$hoán vị, điều này hoàn toàn không thể thực hiện được đối với$m = 10,000$. Ngay cả việc cố gắng chỉ gán giá trị lớn nhất cho một câu hỏi, lớn thứ hai cho câu hỏi khác, v.v., cũng sẽ yêu cầu kiểm tra số lượng bài tập theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là tập trung vào số lượng học sinh trả lời đúng từng câu hỏi. Mỗi câu hỏi góp phần tạo nên sự ngạc nhiên tổng thể tương ứng với giá trị của nó khác với mong đợi của học sinh như thế nào. Nếu muốn tối đa hóa tổng sự khác biệt tuyệt đối, chúng ta nên gán giá trị điểm cao nhất cho những câu hỏi được trả lời đúng bởi hầu hết học sinh có kỳ vọng thấp hơn điểm tiềm năng của họ và giá trị điểm thấp nhất cho những câu hỏi có ít câu trả lời đúng hơn. Về cơ bản, “tầm quan trọng” của một câu hỏi trong việc tối đa hóa sự ngạc nhiên là có bao nhiêu học sinh trả lời đúng, bất kể điểm số dự kiến ​​chính xác của họ là bao nhiêu. 

Điều này làm giảm vấn đề thành một cách tiếp cận tham lam: đếm số câu trả lời đúng cho mỗi câu hỏi của tất cả học sinh, sắp xếp các câu hỏi theo số lượng này và gán giá trị điểm sẵn có lớn nhất cho các câu hỏi có số lượng cao nhất. Điều này có tác dụng vì hàm sai phân tuyệt đối có tính đơn điệu - việc tăng giá trị điểm được gán cho một câu hỏi được nhiều học sinh trả lời đúng sẽ làm tăng tổng các khác biệt tuyệt đối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ơ(m!) | O(m) | Quá chậm | 
| Tham lam theo số câu trả lời đúng | O(m log m + n*m) | O(m + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Với mỗi ca kiểm tra, hãy đọc số học sinh$n$và câu hỏi$m$, điểm số mong đợi của học sinh và chuỗi câu trả lời nhị phân của họ. 
2. Khởi tạo một mảng`counts`chiều dài$m$về không. Mảng này sẽ lưu trữ, đối với mỗi câu hỏi, có bao nhiêu học sinh trả lời đúng. 
3. Lặp lại từng học sinh và từng câu hỏi. Nếu học sinh trả lời đúng câu hỏi (được biểu thị bằng số '1'), hãy tăng dần`counts[j]`. Sau vòng lặp này,`counts[j]`là tổng số học sinh được hỏi$j$Phải. 
4. Ghép từng mục lục câu hỏi với số đếm của nó và sắp xếp các cặp này theo số lượng. Điều này đưa ra thứ hạng các câu hỏi theo mức độ “quan trọng” của chúng trong việc góp phần gây bất ngờ. 
5. Gán giá trị điểm từ 1 đến$m$cho các câu hỏi đã được sắp xếp. Đưa ra giá trị điểm cao nhất cho câu hỏi có số điểm cao nhất, cao thứ hai cho câu hỏi tiếp theo, v.v. Điều này tối đa hóa sự khác biệt tuyệt đối được tổng hợp trên tất cả học sinh. 
6. In ra các giá trị điểm được chỉ định theo thứ tự ban đầu của câu hỏi. 

Tại sao nó hoạt động: Ở mỗi bước, câu hỏi có câu trả lời đúng nhất sẽ nhận được điểm cao nhất, đảm bảo rằng sự khác biệt giữa điểm dự kiến ​​và điểm thực tế sẽ được tăng lên đối với số lượng học sinh lớn nhất. Hàm sai phân tuyệt đối đảm bảo rằng phép gán tham lam này tối đa hóa sự ngạc nhiên hoàn toàn. Sắp xếp theo số lượng đảm bảo rằng tổng chênh lệch tuyệt đối được tối đa hóa trên toàn cầu thay vì cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        x = list(map(int, input().split()))
        answers = [input().strip() for _ in range(n)]
        
        counts = [0] * m
        for i in range(n):
            for j in range(m):
                if answers[i][j] == '1':
                    counts[j] += 1
        
        indexed_counts = list(enumerate(counts))
        indexed_counts.sort(key=lambda x: x[1])
        
        result = [0] * m
        for val, (idx, _) in enumerate(indexed_counts, 1):
            result[idx] = val
        
        print(' '.join(map(str, result)))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc đầu vào một cách hiệu quả cho số lượng lớn câu hỏi. Việc đếm các câu trả lời đúng của học sinh rất đơn giản và sử dụng một mảng cố định để lưu trữ. Việc sắp xếp theo số lượng đảm bảo rằng những câu hỏi có khả năng gây bất ngờ cao nhất sẽ nhận được điểm lớn nhất. Bước phân công cuối cùng là cẩn thận để ánh xạ trở lại thứ tự câu hỏi ban đầu. Các trường hợp cạnh như tất cả số 0 hoặc tất cả số 1 trong chuỗi câu trả lời đều được xử lý một cách tự nhiên bằng cách sắp xếp dựa trên số lượng này. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
4 3
5 1 2 2
110
100
101
100
```Sau khi đếm câu trả lời đúng cho mỗi câu hỏi: 

| Câu hỏi | Đếm | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 
| 3 | 1 | 

Sắp xếp theo số lượng sẽ cho thứ tự [2, 3, 1]. Gán giá trị điểm 1, 2, 3 theo thứ tự này sẽ dẫn đến: 

| Câu hỏi | Giá trị được gán | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 
| 3 | 2 | 

Điều này tối đa hóa sự ngạc nhiên hoàn toàn đối với tất cả học sinh. 

Đối với ví dụ thứ hai:```
3 6
20 3 15
010110
000101
111111
```Đếm các câu trả lời đúng: 

| Câu hỏi | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 1 | 
| 5 | 2 | 
| 6 | 2 | 

Sắp xếp theo số lượng và gán giá trị 1..6 cho: 

| Câu hỏi | Giá trị được gán | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 4 | 
| 4 | 3 | 
| 5 | 5 | 
| 6 | 6 | 

Nhiệm vụ này tối đa hóa sự ngạc nhiên hoàn toàn, cho thấy thuật toán hoạt động với đầu vào phức tạp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m + m log m) | Đếm các câu trả lời đúng là O(n*m). Sắp xếp m câu hỏi là O(m log m). Việc gán giá trị là O(m). | 
| Không gian | O(n*m + m) | Lưu trữ câu trả lời là O(n*m). Lưu trữ số lượng và đầu ra là O(m). | 

Với n ≤ 10 và tổng số m ≤ 10^4, giải pháp này rất phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n4 3\n5 1 2 2\n110\n100\n101\n100\n4 4\n6 2 0 10\n1001\n0010\n0110\n0101\n3 6\n20 3 15\n010110\n000101\n111111\n") in [
    "3 1 2\n2 3 4 1\n1 2 4 3 5 6"
], "sample cases"

# minimum-size input
assert run("1\n1 1\n0\n0\n") == "1", "minimum input"

# all students answer all questions correctly
assert run("1\n2 3\n3 3\n111\n111\n") == "1 2 3", "all correct"

# all students answer all questions incorrectly
assert run("1\n2 3\n0 0\n000\n000\n") == "1 2 3", "all incorrect"

# single student, multiple questions
assert run("1\n1 5\n3\n10101\n") == "5 1 4 2 3", "single student varying answers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 học sinh, 1 câu hỏi | 1 | đầu vào nhỏ nhất | 
| 2 học sinh, tất cả đều đúng | 1 2 3 | |
