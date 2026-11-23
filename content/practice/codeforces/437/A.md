---
title: "CF 437A - Đứa trẻ và bài tập về nhà"
description: "Nhiệm vụ là mô phỏng cách trẻ chọn câu trả lời cho một câu hỏi trắc nghiệm với bốn lựa chọn, được gắn nhãn từ A đến D. Mỗi lựa chọn có một mô tả bằng văn bản."
date: "2026-06-07T03:07:03+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 437
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 250 (Div. 2)"
rating: 1300
weight: 437
solve_time_s: 67
verified: true
draft: false
---

[CF 437A - Đứa trẻ và bài tập về nhà](https://codeforces.com/problemset/problem/437/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là mô phỏng cách trẻ chọn câu trả lời cho một câu hỏi trắc nghiệm với bốn lựa chọn, được gắn nhãn từ A đến D. Mỗi lựa chọn có một mô tả bằng văn bản. Trẻ coi một lựa chọn là “tuyệt vời” nếu mô tả của nó ngắn hơn ít nhất hai lần so với mọi lựa chọn khác hoặc dài hơn ít nhất hai lần so với mọi lựa chọn khác. Nếu có đúng một lựa chọn tốt thì trẻ sẽ chọn lựa chọn đó. Ngược lại, đứa trẻ mặc định là C. 

Đầu vào bao gồm bốn dòng. Mỗi dòng bắt đầu bằng một nhãn như "A." hoặc "B." theo sau là mô tả. Phần mô tả là phần chúng tôi đo độ dài; bản thân nhãn không được tính. Đầu ra là một chữ cái thể hiện sự lựa chọn của trẻ. 

Độ dài tối đa của mô tả là 100 ký tự. Chỉ với bốn lựa chọn, không có rủi ro về vấn đề phức tạp về thời gian. Ngay cả sự so sánh về lực lượng vũ phu giữa tất cả các cặp cũng không đáng kể. Thử thách là xác định chính xác lựa chọn “tuyệt vời” theo quy luật. 

Trường hợp đặc biệt phát sinh khi có nhiều lựa chọn thỏa mãn điều kiện “tuyệt vời”. Ví dụ: nếu một tùy chọn ngắn hơn hai lần so với tất cả các tùy chọn khác và tùy chọn khác dài hơn tất cả các tùy chọn khác thì không có lựa chọn tuyệt vời duy nhất và trẻ nên chọn C. Một trường hợp khó phát hiện khác là khi các mô tả có độ dài rất gần nhau nhưng không hoàn toàn ngắn gấp đôi hoặc dài gấp đôi - phép so sánh phải sử dụng phép chia số nguyên hoặc so sánh dấu phẩy động chính xác một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ so sánh từng lựa chọn với tất cả những lựa chọn khác. Với mỗi lựa chọn, hãy tính độ dài của nó, sau đó kiểm tra xem nó có ngắn hơn tất cả các lựa chọn khác ít nhất hai lần hay dài hơn tất cả các lựa chọn khác ít nhất hai lần hay không. Với bốn tùy chọn, điều này đòi hỏi sáu phép so sánh cho mỗi tùy chọn, điều này không đáng kể. Cách tiếp cận này đúng và đủ nhanh vì kích thước đầu vào rất nhỏ. 

Thông tin chi tiết quan trọng để làm rõ là chúng ta chỉ cần tính độ dài một lần và sau đó kiểm tra từng tùy chọn riêng lẻ với các tùy chọn khác. Vì chỉ có bốn lựa chọn nên không cần sắp xếp hoặc cấu trúc dữ liệu nâng cao. Vấn đề chủ yếu nằm ở việc thực hiện cẩn thận: trích xuất độ dài một cách chính xác, áp dụng điều kiện "ngắn gấp đôi hoặc dài" và xử lý quy tắc ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

Trong bài toán này, lực lượng vũ phu và tối ưu về cơ bản là giống nhau do kích thước đầu vào nhỏ không đổi. 

## Hướng dẫn thuật toán 

1. Đọc bốn dòng đầu vào. Đối với mỗi dòng, loại bỏ hai ký tự đầu tiên (nhãn và dấu chấm) và tính độ dài của chuỗi còn lại. Lưu trữ các độ dài này trong một mảng tương ứng với các lựa chọn từ A đến D. 
2. Khởi tạo một danh sách trống để theo dõi những lựa chọn nào là “tuyệt vời”. 
3. Với mỗi lựa chọn, hãy so sánh độ dài của nó với ba lựa chọn còn lại. Một sự lựa chọn là tuyệt vời nếu chiều dài của nó ít nhất lớn gấp đôi chiều dài của ba chiều dài còn lại hoặc nhiều nhất là một nửa chiều dài của ba chiều dài còn lại. Sử dụng so sánh chặt chẽ với phép nhân với 2 để tránh lỗi dấu phẩy động. 
4. Nếu có chính xác một lựa chọn được đánh dấu là tuyệt vời, hãy in nhãn của lựa chọn đó. Ngược lại, in "C". 

Tại sao nó hoạt động: Bằng cách lặp lại từng lựa chọn và kiểm tra nó với những lựa chọn khác bằng số học chính xác, chúng tôi đảm bảo rằng chúng tôi phát hiện được tất cả và chỉ những lựa chọn “tuyệt vời”. Vì kích thước đầu vào được cố định ở mức 4 nên không thể bỏ qua lựa chọn nào và quy tắc ràng buộc được áp dụng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

choices = ['A', 'B', 'C', 'D']
lengths = []

for _ in range(4):
    line = input().strip()
    # ignore the first two characters (label and dot)
    desc_len = len(line[2:])
    lengths.append(desc_len)

great = []

for i in range(4):
    smaller = all(lengths[i] * 2 <= lengths[j] for j in range(4) if j != i)
    larger = all(lengths[i] >= 2 * lengths[j] for j in range(4) if j != i)
    if smaller or larger:
        great.append(choices[i])

if len(great) == 1:
    print(great[0])
else:
    print('C')
```Trước tiên, chúng tôi ánh xạ các lựa chọn theo độ dài, đảm bảo loại trừ nhãn. Sau đó, chúng tôi kiểm tra từng lựa chọn bằng cách sử dụng`all()`qua ba phần còn lại, làm cho việc so sánh trở nên chính xác và tránh các vấn đề chia số nguyên ngẫu nhiên. Logic lựa chọn cuối cùng rất đơn giản: nếu có chính xác một cái tốt, hãy chọn nó; nếu không thì mặc định là C. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

Độ dài đầu vào: A=39, B=35, C=37, D=15 

| Lựa chọn | Chiều dài | Nhỏ hơn hai lần người khác? | Lớn hơn gấp đôi những người khác? | Tuyệt vời? | 
| --- | --- | --- | --- | --- | 
| A | 39 | Sai | Sai | Sai | 
| B | 35 | Sai | Sai | Sai | 
| C | 37 | Sai | Sai | Sai | 
| D | 15 | Đúng | Sai | Đúng | 

Bảng khẳng định D là sự lựa chọn tuyệt vời duy nhất. Đầu ra: D 

**Mẫu 2** 

Độ dài đầu vào: A=3, B=5, C=4, D=6 

| Lựa chọn | Chiều dài | Nhỏ hơn hai lần người khác? | Lớn hơn gấp đôi những người khác? | Tuyệt vời? | 
| --- | --- | --- | --- | --- | 
| A | 3 | Sai | Sai | Sai | 
| B | 5 | Sai | Sai | Sai | 
| C | 4 | Sai | Sai | Sai | 
| D | 6 | Sai | Sai | Sai | 

Không có sự lựa chọn tuyệt vời duy nhất. Đầu ra: C 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Bốn lựa chọn và so sánh liên tục | 
| Không gian | O(1) | Lưu trữ bốn độ dài và một danh sách nhỏ các lựa chọn tuyệt vời | 

Giải pháp này cực kỳ nhẹ, nằm trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    choices = ['A', 'B', 'C', 'D']
    lengths = []
    for _ in range(4):
        line = input().strip()
        lengths.append(len(line[2:]))
    great = []
    for i in range(4):
        smaller = all(lengths[i] * 2 <= lengths[j] for j in range(4) if j != i)
        larger = all(lengths[i] >= 2 * lengths[j] for j in range(4) if j != i)
        if smaller or larger:
            great.append(choices[i])
    if len(great) == 1:
        return great[0]
    return 'C'

# provided samples
assert run("A.VFleaKing_is_the_author_of_this_problem\nB.Picks_is_the_author_of_this_problem\nC.Picking_is_the_author_of_this_problem\nD.Ftiasch_is_cute\n") == "D"
assert run("A.Ab\nB.Abc\nC.Abcd\nD.Abcde\n") == "C"

# custom cases
assert run("A.A\nB.AAA\nC.AAA\nD.AAA\n") == "A"  # A is twice shorter than all
assert run("A.AAA\nB.A\nC.A\nD.A\n") == "A"       # A is twice longer than all
assert run("A.AAA\nB.AA\nC.AA\nD.AA\n") == "C"    # No unique great
assert run("A.A\nB.A\nC.A\nD.A\n") == "C"        # All equal
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A.A\nB.AAA\nC.AAA\nD.AAA | A | Sự lựa chọn ngắn nhất duy nhất | 
| A.AAA\nB.A\nC.A\nD.A | A | Sự lựa chọn dài nhất độc đáo | 
| A.AAA\nB.AA\nC.AA\nD.AA | C | Nhiều lựa chọn gần như tuyệt vời, nên mặc định | 
| A.A\nB.A\nC.A\nD.A | C | Tất cả đều có độ dài bằng nhau, không có sự lựa chọn tuyệt vời | 

## Vỏ cạnh 

Khi tất cả các lựa chọn có độ dài bằng nhau, thuật toán sẽ đánh giá cả các kiểm tra nhỏ hơn và lớn hơn là sai đối với mọi lựa chọn. Không tìm thấy lựa chọn tuyệt vời nào nên nó in C, chính xác như yêu cầu. Ví dụ: với A=1, B=1, C=1, D=1, đầu ra là C. 

Nếu một tùy chọn ngắn chính xác gấp đôi các tùy chọn khác, chẳng hạn như độ dài [2,5,5,5], thì điều kiện nhỏ hơn được thỏa mãn cho A vì 2*2 <= 5 đối với tất cả các tùy chọn khác, đánh dấu A là tuyệt vời. Các lần kiểm tra còn lại đều thất bại nên A được chọn đúng. 

Nếu cả hai tùy chọn đều thỏa mãn điều kiện lớn, chẳng hạn như độ dài [2,4,1,2], thì danh sách các lựa chọn tốt có độ dài lớn hơn một. Thuật toán sau đó in ra C, khớp với quy tắc ràng buộc.
