---
title: "CF 1133C - Đội ngũ cân bằng"
description: "Chúng tôi có một danh sách sinh viên, mỗi sinh viên có điểm kỹ năng lập trình. Nhiệm vụ là thành lập nhóm lớn nhất có thể sao cho sự khác biệt giữa các thành viên có kỹ năng cao nhất và thấp nhất không vượt quá 5. Nói cách khác, đối với bất kỳ nhóm nào có quy mô $k$, nếu kỹ năng là $s1, s2, ."
date: "2026-06-12T04:03:53+07:00"
tags: ["codeforces", "competitive-programming", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1133
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 544 (Div. 3)"
rating: 1200
weight: 1133
solve_time_s: 69
verified: true
draft: false
---

[CF 1133C - Nhóm cân bằng](https://codeforces.com/problemset/problem/1133/C) 

**Đánh giá:** 1200 
**Thẻ:** sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một danh sách sinh viên, mỗi sinh viên có điểm kỹ năng lập trình. Nhiệm vụ là thành lập nhóm lớn nhất có thể sao cho sự khác biệt giữa các thành viên có kỹ năng cao nhất và kỹ năng thấp nhất không vượt quá 5. Nói cách khác, đối với bất kỳ nhóm quy mô nào$k$, nếu các kỹ năng$s_1, s_2, ..., s_k$, sau đó$\max(s_i) - \min(s_i) \le 5$. Mục tiêu của chúng tôi là xác định quy mô tối đa của một nhóm như vậy. 

Đầu vào bao gồm$n$, số lượng học sinh, tiếp theo là$n$số nguyên đại diện cho cấp độ kỹ năng. Đầu ra là một số nguyên duy nhất: kích thước nhóm cân bằng lớn nhất. Những ràng buộc cho chúng ta biết$n$có thể lên đến$2 \cdot 10^5$và mức độ kỹ năng có thể lớn như$10^9$. Điều này có nghĩa là một người ngây thơ$O(n^2)$phương pháp kiểm tra mọi nhóm sinh viên có thể sẽ yêu cầu tới$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất là quá chậm. Chúng ta cần một cái gì đó gần hơn với tuyến tính hoặc$O(n \log n)$để vừa với giới hạn 2 giây. 

Trường hợp đặc biệt xảy ra khi tất cả học sinh có trình độ kỹ năng như nhau. Bất kỳ nhóm nào đều hợp lệ, vì vậy câu trả lời phải là$n$. Một trường hợp đặc biệt khác là khi kỹ năng của mỗi học sinh khác nhau hơn 5 so với kỹ năng gần nhất tiếp theo; ở đây nhóm cân bằng lớn nhất chỉ có một thành viên. Cách tiếp cận bất cẩn có thể quên xử lý vấn đề này và giả định có ít nhất hai học sinh mỗi đội, dẫn đến kết quả sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp qua mọi tập hợp con có thể có của học sinh, tính toán kỹ năng tối đa và tối thiểu cho từng tập hợp con và kiểm tra xem chênh lệch của chúng có nhiều nhất là 5 hay không. Tập hợp con có kích thước lớn nhất thỏa mãn điều kiện này sẽ là câu trả lời. Điều này đúng vì nó kiểm tra toàn diện tất cả các nhóm hợp lệ, nhưng nó không thành công đối với nhiều nhóm.$n$do vụ nổ tổ hợp-có$2^n$tập hợp con. 

Thông tin chi tiết quan trọng cần tối ưu hóa là thứ tự học sinh không thành vấn đề khi chúng tôi sắp xếp chúng theo kỹ năng. Nếu mảng được sắp xếp, bất kỳ nhóm cân bằng nào cũng phải tương ứng với một mảng con liền kề trong đó chênh lệch giữa phần tử đầu tiên và phần tử cuối cùng nhiều nhất là 5. Điều này cho phép chúng tôi giảm vấn đề xuống một cửa sổ trượt hoặc kỹ thuật hai con trỏ. Chúng ta mở rộng con trỏ bên phải để bao gồm càng nhiều học sinh càng tốt cho đến khi chênh lệch vượt quá 5, sau đó di chuyển con trỏ bên trái về phía trước để khôi phục lại sự cân bằng. Quá trình quét tuyến tính sau khi sắp xếp này đảm bảo rằng chúng tôi kiểm tra mọi nhóm tiềm năng một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Hai con trỏ trên mảng được sắp xếp | O(n log n) | O(1) bổ sung | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số học sinh$n$và trình độ kỹ năng của họ thành một mảng. Việc sắp xếp chúng giúp dễ dàng kiểm tra các nhóm liền kề. 
2. Sắp xếp cấp độ kỹ năng theo thứ tự tăng dần. Sau khi sắp xếp, bất kỳ nhóm cân bằng nào cũng tương ứng với một mảng con trong đó phần tử đầu tiên và phần tử cuối cùng khác nhau tối đa 5. 
3. Khởi tạo hai con trỏ,`left`Và`right`, cả hai đều bắt đầu ở đầu mảng được sắp xếp. Đồng thời khởi tạo`max_team`về 0 để lưu trữ kích thước nhóm lớn nhất được tìm thấy. 
4. Mở rộng`right`con trỏ về phía trước. Đối với mỗi vị trí của`right`, kiểm tra sự khác biệt giữa`skills[right]`Và`skills[left]`. Nếu chênh lệch vượt quá 5 thì tăng`left`cho đến khi chênh lệch tối đa là 5 lần nữa. Điều này duy trì sự bất biến mà cửa sổ`[left, right]`luôn là một đội cân bằng. 
5. Cập nhật`max_team`với kích thước của cửa sổ hiện tại,`right - left + 1`, nếu nó lớn hơn mức tối đa trước đó. 
6. Tiếp tục quá trình này cho đến khi`right`đến cuối mảng. In`max_team`. 

Tại sao nó hoạt động: Bởi vì mảng được sắp xếp, bất kỳ mảng con nào chứa các phần tử có chênh lệch lớn hơn 5 đều không thể được mở rộng mà không vi phạm ràng buộc cân bằng. Trượt con trỏ bên trái đảm bảo chúng ta luôn duy trì cửa sổ lớn nhất có thể kết thúc tại`right`đó thỏa mãn điều kiện. Không có nhóm cân bằng nào bị bỏ sót vì mọi mảng con liền kề tiềm năng được coi là chính xác một lần`right`tiến bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
skills = list(map(int, input().split()))

skills.sort()
max_team = 0
left = 0

for right in range(n):
    while skills[right] - skills[left] > 5:
        left += 1
    max_team = max(max_team, right - left + 1)

print(max_team)
```Đầu tiên, mã sẽ đọc và sắp xếp các cấp độ kỹ năng. Vòng lặp hai con trỏ đảm bảo chúng tôi luôn duy trì một đội cân bằng. Vòng lặp while tiến lên`left`chỉ khi chênh lệch vượt quá 5, điều này rất khó thấy nhưng rất quan trọng để tránh các lỗi riêng lẻ. Đang cập nhật`max_team`sau khi điều chỉnh`left`đảm bảo cửa sổ lớn nhất được ghi lại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
6
1 10 17 12 15 2
```| đúng | trái | kỹ năng[phải] | kỹ năng[trái] | kích thước cửa sổ | max_team |
 | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 1 | 
| 1 | 0 | 2 | 1 | 2 | 2 | 
| 2 | 0 | 10 | 1 | 3 -> left moves to 2 | 2 | 
| 3 | 2 | 12 | 10 | 2 | 2 | 
| 4 | 2 | 15 | 10 | 3 | 3 | 
| 5 | 2 | 17 | 12 | 3 | 3 | 

Đội cân bằng lớn nhất có 3 học sinh`[12, 15, 17]`. 

Đầu vào mẫu 2:```
5
5 5 5 5 5
```| đúng | trái | kỹ năng[phải] | kỹ năng[trái] | kích thước cửa sổ | max_team | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | 5 | 1 | 1 | 
| 1 | 0 | 5 | 5 | 2 | 2 | 
| 2 | 0 | 5 | 5 | 3 | 3 | 
| 3 | 0 | 5 | 5 | 4 | 4 | 
| 4 | 0 | 5 | 5 | 5 | 5 | 

Tất cả học sinh tạo thành một đội cân bằng. Tính bất biến đó`skills[right] - skills[left] <= 5`được duy trì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế. Quá trình quét hai con trỏ là O(n) vì mỗi con trỏ di chuyển tối đa n lần. | 
| Không gian | O(n) | Mảng kỹ năng được lưu trữ. Việc sắp xếp có thể được thực hiện tại chỗ mà không cần thêm bộ nhớ. | 

Điều này rất phù hợp trong các ràng buộc: đối với$n = 2 \cdot 10^5$, việc sắp xếp mất khoảng$4 \cdot 10^6$hoạt động, và đường chuyền hai con trỏ sẽ thêm một đường dẫn khác$2 \cdot 10^5$, thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    skills = list(map(int, input().split()))
    skills.sort()
    max_team = 0
    left = 0
    for right in range(n):
        while skills[right] - skills[left] > 5:
            left += 1
        max_team = max(max_team, right - left + 1)
    return str(max_team)

# Provided samples
assert run("6\n1 10 17 12 15 2\n") == "3"
assert run("5\n5 5 5 5 5\n") == "5"
assert run("1\n100\n") == "1"

# Custom cases
assert run("3\n1 7 13\n") == "1"  # no pair within 5
assert run("4\n1 2 3 7\n") == "3"  # first three form a team
assert run("5\n1 2 3 4 6\n") == "4"  # largest balanced team [1,2,3,4]
assert run("2\n1 6\n") == "2"  # boundary case exactly 5 apart
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n1 7 13 | 1 | Xử lý trường hợp không có cặp hợp lệ | 
| 4\n1 2 3 7 | 3 | Lựa chọn đúng nhóm liền kề lớn nhất | 
| 5\n1 2 3 4 6 | 4 | Cửa sổ trượt thả phần tử ngoài cùng bên trái một cách chính xác | 
| 2\n1 6 | 2 | Trường hợp cạnh chênh lệch chính xác 5 | 

## Vỏ cạnh 

Đối với trường hợp học sinh độc thân`1\n100`, thuật toán khởi tạo`left`Và`right`ở 0, tính toán`window size = 1`và trả về 1. Không có trình kích hoạt vòng lặp while, do đó nó xử lý đầu vào có kích thước tối thiểu một cách chính xác. 

Đối với một mảng trong đó không có hai học sinh nào có thể thành lập một đội, như`3\n1 7 13`,`left`tăng lên mỗi lần `skills[right] - Skills[left] > 5
