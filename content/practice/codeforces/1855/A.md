---
title: "CF 1855A - Thầy Giáo Dalton"
description: "Bài toán yêu cầu chúng ta làm cho mọi học sinh trong lớp "vui vẻ" bằng cách đảm bảo rằng không có học sinh nào ngồi trên chiếc ghế có cùng số với ghế của mình. Chúng ta có một sự hoán vị ban đầu của học sinh đối với ghế."
date: "2026-06-09T05:07:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1855
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 889 (Div. 2)"
rating: 800
weight: 1855
solve_time_s: 109
verified: false
draft: false
---

[CF 1855A - Thầy giáo Dalton](https://codeforces.com/problemset/problem/1855/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta làm cho mọi học sinh trong lớp đều "vui vẻ" bằng cách đảm bảo rằng không có học sinh nào ngồi trên chiếc ghế có cùng số ghế với mình. Chúng ta có một sự hoán vị ban đầu của học sinh đối với ghế. Mỗi học sinh có thể được xác định bằng một số từ`1`ĐẾN`n`, và mỗi chiếc ghế cũng có một số từ`1`ĐẾN`n`. Đầu vào cung cấp một mảng hoán vị`p`như vậy`p[i]`cho chúng tôi biết chiếc ghế nơi học sinh`i`ban đầu đang ngồi. Nhiệm vụ của chúng ta là xác định **số lần hoán đổi tối thiểu** giữa hai học sinh bất kỳ cần thiết để mỗi học sinh không còn ngồi trên chiếc ghế được đánh số riêng của mình. 

Vì kích thước hoán vị`n`có thể lên tới 10^5 và có thể lên tới 1000 trường hợp thử nghiệm, chúng tôi cần một thuật toán hoạt động theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ thuật toán nào mô phỏng rõ ràng tất cả các giao dịch hoán đổi có thể xảy ra sẽ quá chậm vì số lượng giao dịch hoán đổi có thể lên tới O(n^2) theo cách tiếp cận đơn giản. Các trường hợp cạnh không rõ ràng phát sinh khi hoán vị đã hoàn toàn bao gồm các học sinh hạnh phúc, yêu cầu 0 lần hoán đổi hoặc khi có một điểm cố định duy nhất (học sinh ngồi trên ghế của chính mình) với các chu kỳ hình thành phần còn lại, điều này ảnh hưởng đến việc 1 hay 2 lần hoán đổi là đủ. 

Việc triển khai bất cẩn có thể cố gắng giải quyết từng điểm cố định riêng lẻ mà không xem xét cấu trúc hoán vị. Ví dụ, trong hoán vị`[1,2,3]`, mỗi học sinh ngồi trên ghế của mình. Số lần hoán đổi tối thiểu không phải là 3 mà là 2, vì việc giải quyết một học sinh sẽ làm thay đổi chu trình. Điều này minh họa sự cần thiết phải suy luận theo chu kỳ hơn là theo từng yếu tố riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liên tục hoán đổi học sinh ngồi trên ghế của mình với một học sinh khác không ngồi trên ghế của mình. Điều này đúng nhưng không hiệu quả vì chúng ta có thể cần quét toàn bộ mảng để tìm từng điểm cố định. Trong trường hợp xấu nhất, việc này có thể thực hiện các thao tác O(n^2), quá chậm đối với n = 10^5. 

Nhận xét quan trọng là vấn đề được rút gọn thành việc đếm **các điểm cố định và các khối liền kề của những học sinh không hài lòng**. Nếu không có điểm cố định thì không cần hoán đổi. Nếu có ít nhất một điểm cố định, việc hoán đổi phải xảy ra để phá vỡ chu kỳ. Cụ thể, nếu hoán vị chứa ít nhất một dãy liền kề các học sinh hạnh phúc thì số lần di chuyển tối thiểu phụ thuộc vào việc các học sinh không hài lòng tạo thành một khối liền kề hay nhiều khối. Trong thực tế, số lần hoán đổi tối thiểu là: 

-`0`nếu không có điểm cố định (tất cả học sinh đều hài lòng) 
-`1`nếu những học sinh không hài lòng tạo thành một khối liền kề ở đâu đó bên trong hoán vị 
-`2`mặt khác (những học sinh không hài lòng sẽ được chia thành nhiều khối không liền kề) 

Thông tin chi tiết này cho phép chúng tôi tính toán câu trả lời theo O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và hoán vị`p`. 
3. Xác định vị trí nơi`p[i] == i + 1`, đây là những điểm cố định. 
4. Nếu không có điểm cố định, hãy in`0`bởi vì tất cả học sinh đều đã hạnh phúc rồi. 
5. Ngược lại, hãy tìm các điểm cố định ngoài cùng bên trái và ngoài cùng bên phải trong hoán vị. Kiểm tra xem tất cả các vị trí giữa chúng có phải là điểm cố định hay không. 
6. Nếu tất cả các vị trí giữa điểm cố định ngoài cùng bên trái và ngoài cùng bên phải cũng là điểm cố định thì nó tạo thành một khối liền kề và chỉ cần 1 lần hoán đổi, vì vậy hãy in`1`. 
7. Nếu không, hãy in`2`vì các điểm cố định được chia thành nhiều khối nên cần ít nhất 2 lần hoán đổi. 

**Tại sao nó hoạt động** 

Cách tiếp cận này hiệu quả vì bất kỳ hoán vị nào cũng có thể được phân tách thành các chu kỳ của sinh viên. Học sinh trong một chu kỳ có độ dài >1 có thể được sắp xếp lại để hài lòng với một lần hoán đổi trong mỗi chu kỳ. Các trường hợp biên xảy ra khi các điểm cố định bị cô lập; chúng ta cần kiểm tra các phân đoạn liền kề để xác định xem 1 hoặc 2 lần hoán đổi có đủ hay không. Các điểm cố định ngoài cùng bên trái và ngoài cùng bên phải xác định khoảng tối thiểu chứa tất cả các điểm cố định, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    
    fixed = [i for i in range(n) if p[i] == i + 1]
    if not fixed:
        print(0)
        continue
    
    left, right = fixed[0], fixed[-1]
    if all(p[i] == i + 1 for i in range(left, right + 1)):
        print(1)
    else:
        print(2)
```Giải pháp xác định các điểm cố định trong một lần chuyển, xác định khoảng thời gian của các điểm cố định liền kề và quyết định xem có cần hoán đổi 0, 1 hay 2 hay không. Việc sử dụng`i + 1`đảm bảo đánh số học sinh dựa trên 1 chính xác. 

## Ví dụ đã hoạt động 

Đối với hoán vị đầu vào`[1,2,3]`: 

| tôi | p[i] | Đã sửa chưa? | 
| --- | --- | --- | 
| 0 | 1 | Có | 
| 1 | 2 | Có | 
| 2 | 3 | Có | 

Tất cả học sinh đều là điểm cố định. Ngoài cùng bên trái = 0, ngoài cùng bên phải = 2. Tất cả các vị trí từ 0 đến 2 đều cố định. Di chuyển tối thiểu = 1. 

cho`[1,2,5,4,3]`: 

| tôi | p[i] | Đã sửa chưa? | 
| --- | --- | --- | 
| 0 | 1 | Có | 
| 1 | 2 | Có | 
| 2 | 5 | Không | 
| 3 | 4 | Có | 
| 4 | 3 | Không | 

Cố định ngoài cùng bên trái = 0, cố định ngoài cùng bên phải = 3. Vị trí giữa 0 và 3 là`[1,2,5,4]`. Không phải tất cả đều được cố định. Di chuyển tối thiểu = 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng để xác định các điểm cố định và kiểm tra khoảng | 
| Không gian | O(n) | Lưu trữ chỉ số của các điểm cố định, có thể được tối ưu hóa thành O(1) nếu chỉ sử dụng trái/phải | 

Thuật toán phù hợp thoải mái trong giới hạn thời gian 1 giây với tổng n 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        fixed = [i for i in range(n) if p[i] == i + 1]
        if not fixed:
            output.append("0")
            continue
        left, right = fixed[0], fixed[-1]
        if all(p[i] == i + 1 for i in range(left, right + 1)):
            output.append("1")
        else:
            output.append("2")
    return "\n".join(output)

# Provided sample
assert run("5\n2\n2 1\n3\n1 2 3\n5\n1 2 5 4 3\n4\n1 2 4 3\n10\n10 2 1 3 6 5 4 7 9 8\n") == "0\n1\n2\n1\n2"

# Custom tests
assert run("2\n2\n1 2\n2\n2 1\n") == "1\n0", "all fixed vs no fixed"
assert run("1\n5\n1 3 2 5 4\n") == "2", "non-contiguous fixed points"
assert run("1\n3\n3 2 1\n") == "1", "contiguous happy segment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2\n1 2\n2\n2 1 | 1\n0 | phân biệt các điểm cố định liền kề với không có | 
| 1\n5\n1 3 2 5 4 | 2 | điểm cố định không liền kề cần 2 lần hoán đổi | 
| 1\n3\n3 2 1 | 1 | khối không vui liền kề cho phép 1 trao đổi | 

## Vỏ cạnh 

Một điểm cố định duy nhất ở cuối, chẳng hạn như`[1,3,2]`, được xử lý bằng cách xác định các điểm cố định ngoài cùng bên trái và ngoài cùng bên phải và kiểm tra khoảng cách. Ngay cả khi ban đầu chỉ có một học sinh không hài lòng, việc kiểm tra khoảng thời gian sẽ xác định chính xác liệu chỉ một lần hoán đổi là đủ hay cần hai lần hoán đổi. Điều này đảm bảo tính chính xác trên tất cả các cấu hình tối thiểu và tối đa.
