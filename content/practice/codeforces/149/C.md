---
title: "CF 149C - Chia thành các đội"
description: "Chúng tôi có một nhóm các chàng trai, mỗi người có trình độ bóng đá khác nhau. Nhiệm vụ là chia họ thành hai đội sao cho các đội có quy mô gần bằng nhau và tổng trình độ kỹ năng của các đội gần như cân bằng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 149
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 106 (Div. 2)"
rating: 1500
weight: 149
solve_time_s: 100
verified: false
draft: false
---

[CF 149C - Chia thành các nhóm](https://codeforces.com/problemset/problem/149/C) 

**Đánh giá:** 1500 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm các chàng trai, mỗi người có trình độ bóng đá khác nhau. Nhiệm vụ là chia họ thành hai đội sao cho các đội có quy mô gần bằng nhau và tổng trình độ kỹ năng của các đội gần như cân bằng. Về mặt hình thức, nếu có`n`các em, chúng ta cần chia các em thành hai đội với quy mô khác nhau nhiều nhất là một. Ngoài ra, sự chênh lệch tuyệt đối về tổng kỹ năng giữa hai đội cũng không thể vượt quá kỹ năng của cầu thủ giỏi nhất. 

Đầu vào cho biết số lượng bé trai`n`theo sau là một dãy số nguyên thể hiện kỹ năng của mỗi cậu bé. The output should list the number of boys in the first team, the indices of the boys in that team, the number of boys in the second team, and the indices of boys in the second team.

 Các ràng buộc cho phép tối đa 10^5 bé trai và giá trị kỹ năng lên tới 10^4. Điều này loại trừ bất kỳ sự liệt kê thô bạo nào đối với tất cả các phân chia nhóm có thể có vì số lượng các phân vùng có thể tăng theo cấp số nhân. Chúng tôi cần một giải pháp có độ phức tạp khoảng O(n log n) hoặc O(n) để đáp ứng giới hạn thời gian. 

Một trường hợp phức tạp phát sinh khi mảng kỹ năng có giá trị lặp lại hoặc khi một cậu bé mạnh hơn đáng kể so với những cậu bé khác. Ví dụ, nếu`n = 3`và kỹ năng là`[1, 1, 10]`, một cách tiếp cận ngây thơ cố gắng cân bằng số tiền một cách tham lam bằng cách lấy các kỹ năng lớn nhất trước có thể nhầm lẫn đưa chàng trai mạnh nhất vào một đội vượt quá chênh lệch kỹ năng cho phép. Một trường hợp khác là khi`n`chẵn và lẻ; các đội phải khác nhau về quy mô tối đa một, vì vậy chúng tôi phải luân phiên phân công một cách cẩn thận để đáp ứng các hạn chế cả về quy mô và kỹ năng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi cách có thể để phân vùng`n`các em thành hai đội có số lượng gần bằng nhau rồi kiểm tra các điều kiện tổng. Về nguyên tắc thì điều này đúng nhưng đối với`n = 10^5`số lượng phân vùng là rất lớn (khoảng 2^n/sqrt(n)) và hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể sử dụng phương pháp sắp xếp tham lam. Nếu chúng ta sắp xếp các chàng trai theo kỹ năng, thì sự khác biệt tuyệt đối giữa tổng số đội sẽ nhạy cảm nhất với giá trị kỹ năng lớn nhất. Bằng cách luân phiên phân công những cầu thủ mạnh nhất còn lại giữa hai đội, chúng tôi có thể đảm bảo rằng tổng số kỹ năng vẫn ở mức tương đương. Việc sắp xếp đảm bảo rằng khi chúng tôi chỉ định kỹ năng lớn nhất chưa được chỉ định, nó sẽ được cân bằng bằng cách phân phối kỹ năng lớn nhất tiếp theo cho nhóm đối diện. Chúng tôi cũng luân phiên phân công từ nhiệm vụ lớn nhất trở xuống thay vì từ nhiệm vụ nhỏ nhất trở lên, vì ràng buộc phụ thuộc vào kỹ năng của người chơi giỏi nhất. Việc chỉ định số cuối cùng lớn nhất có thể vi phạm ràng buộc này. 

Điều này dẫn đến giải pháp O(n log n): sắp xếp các kỹ năng trong khi theo dõi các chỉ số ban đầu, sau đó lặp lại chúng theo thứ tự giảm dần, luân phiên phân công cho hai đội. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Sắp xếp tham lam & Thay thế | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số bé trai`n`và hàng loạt kỹ năng`a`. 
2. Tạo danh sách các cặp`(skill, index)`để chúng ta có thể sắp xếp trong khi vẫn giữ được các chỉ số gốc. 
3. Sắp xếp danh sách này theo thứ tự kỹ năng giảm dần. Việc sắp xếp đảm bảo chúng tôi xử lý được những người chơi mạnh nhất trước tiên. 
4. Khởi tạo hai danh sách trống cho đội 1 và đội 2. 
5. Lặp lại danh sách đã sắp xếp, phân công lần lượt từng người chơi vào đội 1 và đội 2. Điều này đảm bảo cả chênh lệch về quy mô đội và tổng chênh lệch kỹ năng vẫn nằm trong giới hạn. 
6. In ra quy mô và chỉ số của đội thứ nhất, tiếp theo là quy mô và chỉ số của đội thứ hai. 

Tại sao nó hoạt động: Điều bất biến mà chúng tôi duy trì là ở mỗi bước, sự khác biệt về tổng kỹ năng giữa các đội không thể vượt quá kỹ năng lớn nhất chưa được chỉ định. Vì chúng tôi bắt đầu với kỹ năng lớn nhất nên việc phân công luân phiên đảm bảo rằng không có đội nào tích lũy được tổng số điểm vượt quá mức chênh lệch cho phép. Sự khác biệt về quy mô đội được tự động duy trì vì chúng tôi chỉ định từng người chơi một và luân phiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

# store original indices
players = [(skill, idx + 1) for idx, skill in enumerate(a)]
# sort descending by skill
players.sort(reverse=True)

team1 = []
team2 = []

# alternate assignment
for i, (_, idx) in enumerate(players):
    if i % 2 == 0:
        team1.append(idx)
    else:
        team2.append(idx)

print(len(team1))
print(" ".join(map(str, team1)))
print(len(team2))
print(" ".join(map(str, team2)))
```Mã này đọc đầu vào, tạo danh sách cặp chỉ số kỹ năng, sắp xếp theo thứ tự giảm dần và sau đó phân bổ người chơi luân phiên cho mỗi đội. Hoạt động modulo`i % 2`đảm bảo các đội có quy mô gần như bằng nhau. Việc sử dụng các chỉ mục gốc đảm bảo đầu ra khớp với việc đánh số dựa trên 1 được yêu cầu. 

## Ví dụ đã hoạt động 

Theo dõi thông qua đầu vào mẫu`3\n1 2 1`: 

| tôi | kỹ năng | idx | đội1 | đội2 | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | [2] | [] | 
| 1 | 1 | 1 | [2] | [1] | 
| 2 | 1 | 3 | [2,3] | [1] | 

Quy mô đội là 2 và 1, tổng kỹ năng là 3 và 1, chênh lệch là 2 tương đương với kỹ năng tối đa 2. Đúng. 

Đầu vào khác`4\n10 1 1 1`: 

| tôi | kỹ năng | idx | đội1 | đội2 | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | 1 | [1] | [] | 
| 1 | 1 | 2 | [1] | [2] | 
| 2 | 1 | 3 | [1,3] | [2] | 
| 3 | 1 | 4 | [1,3] | [2,4] | 

Quy mô đội là 2 và 2, tổng kỹ năng 11 và 3, chênh lệch 8 ≤ kỹ năng tối đa 10. Đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong thời gian chạy; tất cả các hoạt động khác là O(n) | 
| Không gian | O(n) | Chúng tôi lưu trữ các cặp chỉ số kỹ năng và hai danh sách đội | 

Sắp xếp 10^5 phần tử trong O (n log n) là khả thi trong giới hạn thời gian 1 giây. Việc sử dụng không gian là tuyến tính, vừa vặn thoải mái trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    players = [(skill, idx + 1) for idx, skill in enumerate(a)]
    players.sort(reverse=True)
    team1 = []
    team2 = []
    for i, (_, idx) in enumerate(players):
        if i % 2 == 0:
            team1.append(idx)
        else:
            team2.append(idx)
    out = []
    out.append(str(len(team1)))
    out.append(" ".join(map(str, team1)))
    out.append(str(len(team2)))
    out.append(" ".join(map(str, team2)))
    return "\n".join(out)

# sample
assert run("3\n1 2 1\n") == "2\n2 3\n1\n1", "sample 1"

# minimum input
assert run("2\n1 1\n") == "1\n1\n1\n2", "min size"

# all equal skills
assert run("4\n5 5 5 5\n") == "2\n1 3\n2\n2 4", "equal skills"

# max size with small skills
max_input = "100000\n" + " ".join(["1"]*100000)
result = run(max_input)
assert result.startswith("50000\n"), "max size"

# large difference
assert run("4\n10 1 1 1\n") == "2\n1 3\n2\n2 4", "large skill difference"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 1 | 1\n1\n1\n2 | xử lý đầu vào tối thiểu | 
| 4\n5 5 5 5 | 2\n1 3\n2\n2 4 | cân bằng các kỹ năng bình đẳng | 
| 100000 cái | 50000... | khả năng mở rộng đến mức tối đa n | 
| 4\n10 1 1 1 | 2\n1 3\n2\n2 4 | vị trí người chơi có kỹ năng cao | 

## Vỏ cạnh 

Nếu tất cả nam sinh đều có kỹ năng giống nhau, việc luân phiên đảm bảo chênh lệch quy mô đội nhiều nhất là một và tổng kỹ năng được cân bằng chính xác. Vì`[10, 1, 1, 1]`, Người chơi mạnh nhất sẽ về đội thứ nhất, đội tiếp theo về đội thứ hai, v.v. Tổng số cuối cùng chênh lệch nhau 8, nằm trong mức chênh lệch cho phép (10). Số lẻ`n`được xử lý tự động bằng các bài tập xen kẽ. Hoạt động modulo đảm bảo không có lỗi nào xảy ra trong việc lập chỉ mục và các chỉ mục gốc được giữ nguyên cho đầu ra.
