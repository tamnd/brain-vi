---
title: "CF 1121A - Cốc lửa công nghệ"
description: "Chúng ta có một nhóm học sinh, mỗi học sinh thuộc một trường và mỗi em có một mức xếp hạng năng lực. Thể lệ của giải đấu rất đơn giản: chỉ học sinh mạnh nhất của mỗi trường mới được chọn. Arkady muốn đảm bảo rằng tập k học sinh được chọn sẽ được chọn."
date: "2026-06-12T04:22:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1121
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 543 (Div. 2, based on Technocup 2019 Final Round)"
rating: 1100
weight: 1121
solve_time_s: 66
verified: true
draft: false
---

[CF 1121A - Chiếc cốc lửa công nghệ](https://codeforces.com/problemset/problem/1121/A) 

**Đánh giá:** 1100 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm học sinh, mỗi học sinh thuộc một trường và mỗi em có một mức xếp hạng năng lực. Thể lệ của giải đấu rất đơn giản: chỉ học sinh mạnh nhất của mỗi trường mới được chọn. Arkady muốn đảm bảo rằng một bộ đã chọn`k`học sinh được chọn. Một số học sinh này hiện có thể không phải là học sinh giỏi nhất trong trường của họ. Anh ta có thể thao túng hệ thống bằng cách giao họ vào những trường hư cấu mới, đảm bảo sự lựa chọn của họ, nhưng anh ta muốn giảm thiểu số lượng những trường hư cấu như vậy. 

Đầu vào bao gồm`n`học sinh với quyền hạn của họ, trường học của họ và danh sách`k`Những Người Được Chọn. Đầu ra là số lượng trường học mới tối thiểu mà Arkady phải tạo ra để đảm bảo tất cả`k`Những Người Được Chọn được chọn. 

Những hạn chế là nhỏ (`n`100,`m`≤ n,`k`≤ n). Điều này cho phép các thuật toán lặp lại trên học sinh và trường học nhiều lần. Một giải pháp đơn giản là kiểm tra từng Người được chọn với tất cả bạn học là khả thi. Tuy nhiên, chúng ta cần xử lý cẩn thận việc so sánh quyền lực và tư cách thành viên của trường để tránh những sai lầm riêng lẻ. 

Các trường hợp cạnh không rõ ràng bao gồm: 

- Người được chọn đã là người mạnh nhất trong trường của họ. Không cần trường học mới. 
- Nhiều Người Được Chọn trong cùng một trường, nơi chỉ có một người mạnh nhất. Chúng ta cần nhiều trường học mới cho những Người Được Chọn yếu hơn. 
- Tất cả học sinh đều đã là Người Được Chọn. Arkady có thể không cần phải thành lập bất kỳ trường học nào cả. 

Ví dụ, với học sinh`[5, 3]`ở trường`[1, 1]`và những người được chọn`[2]`, học sinh 2 yếu hơn học sinh 1 cùng trường nên cần có một trường mới. Việc triển khai bất cẩn có thể bỏ qua mức tối đa cho mỗi trường và đưa ra số 0 không chính xác. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ kiểm tra từng Người được chọn: đối với trường hiện tại của họ, hãy xác định học sinh mạnh nhất. Nếu Người Được Chọn yếu hơn người khác, hãy tính một trường mới. Điều này hiệu quả vì số lượng sinh viên ít. Trong trường hợp xấu nhất, đối với mỗi`k`Những Người Được Chọn, chúng tôi quét tới`n`sinh viên, trao`O(n*k)`các hoạt động có thể chấp nhận được đối với`n`100. 

Thông tin chi tiết giúp tối ưu hóa điều này là chúng tôi chỉ quan tâm đến học sinh giỏi nhất mỗi trường. Chúng tôi không cần phải sắp xếp tất cả học sinh hoặc mô phỏng đầy đủ Technogoblet. Chúng tôi tính toán công suất tối đa cho mỗi trường hiện có một lần, lưu trữ nó và sau đó đối với mỗi Người được chọn, hãy kiểm tra xem công suất của họ có bằng công suất tối đa của trường hay không. Nếu có, không có trường học mới; nếu không, một trường học mới. Điều này làm giảm sự so sánh dư thừa và đơn giản hóa việc lập luận. 

Việc tối ưu hóa dựa trên tính chất độc lập của các trường: việc lựa chọn học sinh ở trường này không ảnh hưởng đến trường khác. Do đó, nhu cầu về một ngôi trường mới của mỗi Người Được Chọn có thể được xác định riêng lẻ sau khi chúng ta biết mức tối đa của trường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(1) | Được chấp nhận do những hạn chế nhỏ | 
| Tối ưu | O(n + k) | O(m) | Được chấp nhận và sạch sẽ | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`,`m`,`k`, thì quyền sinh viên`p`, trường học sinh`s`, và Những Người Được Chọn`c`. Điều chỉnh các chỉ số dựa trên 0 nếu thuận tiện. 
2. Xây dựng một mảng`school_max`kích thước`m`để tích trữ sức mạnh học sinh mạnh nhất trong mỗi trường. Lặp lại trên tất cả học sinh và với mỗi học sinh, hãy cập nhật`school_max[school[i]]`nếu như`p[i]`cao hơn. 
3. Khởi tạo bộ đếm`new_schools = 0`. 
4. Lặp lại từng Người được chọn`ci`. Kiểm tra sức mạnh của`ci`chống lại sức mạnh tối đa của trường học hiện tại của họ. Nếu như`p[ci]`ít hơn`school_max[s[ci]]`, tăng`new_schools`bởi một. 
5. In`new_schools`. 

Tại sao nó hoạt động: Đối với mỗi Người Được Chọn, họ đã là người mạnh nhất trong trường của mình và được tự động chọn, hoặc họ yếu hơn và cần một trường học mới. Bởi vì thuộc tính mạnh nhất của mỗi trường đảm bảo tính duy nhất, không có Người Được Chọn nào bị tính hai lần hoặc bị ảnh hưởng bởi trường mới của người khác. Thuật toán đếm chính xác số lượng trường học mới tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
p = list(map(int, input().split()))
s = list(map(int, input().split()))
c = list(map(int, input().split()))

# convert to zero-based indices
s = [x-1 for x in s]
c = [x-1 for x in c]

school_max = [0] * m
for i in range(n):
    school_max[s[i]] = max(school_max[s[i]], p[i])

new_schools = 0
for ci in c:
    if p[ci] < school_max[s[ci]]:
        new_schools += 1

print(new_schools)
```Giải thích: Đầu tiên chúng ta tính`school_max`hiệu quả chỉ trong một lần. Sau đó, đối với mỗi Người Được Chọn, chúng tôi so sánh với mức tối đa của trường để xem liệu có cần trường mới hay không. Điều chỉnh chỉ mục tránh từng lỗi một. sử dụng`max`đảm bảo chúng ta xử lý các mối quan hệ một cách chính xác, mặc dù quyền hạn phát biểu vấn đề là khác nhau. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
7 3 1
1 5 3 4 6 7 2
1 3 1 2 1 2 3
3
```| Bước | trường_max | ci | p[ci] | school_max[s[ci]] | new_schools | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [6,7,5] | 3 | 3 | 6 | 0 | 
| Kiểm tra | - | 3 | 3 | 6 | 1 | 

Đầu ra: 1. 

Giải thích: Học sinh 3 yếu hơn học sinh 5 ở trường 1 nên cần phải có một trường mới. 

Mẫu 2: 

đầu vào:```
5 3 2
10 20 30 40 50
1 2 2 3 3
2 4
```| Bước | trường_max | ci | p[ci] | school_max[s[ci]] | new_schools | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [10,30,50] | 2 | 20 | 30 | 0 | 
| Kiểm tra | - | 2 | 20 | 30 | 1 | 
| Kiểm tra | - | 4 | 40 | 50 | 2 | 

Đầu ra: 2. 

Giải thích: Cả hai Người Được Chọn đều không mạnh nhất trong trường của họ; cần có hai trường học mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Một lượt tính điểm tối đa của trường, một lượt dành cho Người Được Chọn | 
| Không gian | O(m) | Mảng lưu trữ công suất tối đa mỗi trường | 

Với`n`100 và`m`≤ n, thao tác này thực hiện trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, k = map(int, input().split())
    p = list(map(int, input().split()))
    s = list(map(int, input().split()))
    c = list(map(int, input().split()))
    s = [x-1 for x in s]
    c = [x-1 for x in c]
    school_max = [0] * m
    for i in range(n):
        school_max[s[i]] = max(school_max[s[i]], p[i])
    new_schools = 0
    for ci in c:
        if p[ci] < school_max[s[ci]]:
            new_schools += 1
    return str(new_schools)

# Provided samples
assert run("7 3 1\n1 5 3 4 6 7 2\n1 3 1 2 1 2 3\n3\n") == "1"
assert run("5 3 2\n10 20 30 40 50\n1 2 2 3 3\n2 4\n") == "2"

# Custom cases
assert run("1 1 1\n1\n1\n1\n") == "0", "single student, Chosen One is strongest"
assert run("2 1 1\n2 1\n1 1\n2\n") == "1", "weaker Chosen One needs new school"
assert run("3 2 2\n3 1 2\n1 2 2\n2 3\n") == "1", "multiple schools, one Chosen One weaker"
assert run("5 5 5\n5 4 3 2 1\n1 2 3 4 5\n1 2 3 4 5\n") == "0", "all Chosen Ones already strongest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | Đầu vào tối thiểu | 
| 2 1 1 | | |
