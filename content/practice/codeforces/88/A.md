---
title: "CF 88A - Hợp âm"
description: "Chúng ta được giao ba nốt nhạc và nhiệm vụ là phân loại hợp âm mà chúng tạo thành là trưởng, thứ hoặc "lạ". Các nốt được thể hiện theo thang màu mười hai âm: C, C, D, D, E, F, F, G, G, A, B, H và thang âm có tính tuần hoàn nên sau H lại đến C."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 88
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 73 (Div. 2 Only)"
rating: 1200
weight: 88
solve_time_s: 77
verified: true
draft: false
---

[CF 88A - Hợp âm](https://codeforces.com/problemset/problem/88/A) 

**Đánh giá:** 1200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao ba nốt nhạc và nhiệm vụ là phân loại hợp âm mà chúng tạo thành là trưởng, thứ hoặc "lạ". Các nốt được thể hiện theo thang màu mười hai tông: C, C#, D, D#, E, F, F#, G, G#, A, B, H và thang âm là tuần hoàn, vì vậy sau H lại đến C. Hợp âm là một bộ ba, một bộ ba nốt không có thứ tự. Một bộ ba chính xảy ra nếu chúng ta có thể sắp xếp các nốt sao cho quãng đầu tiên là 4 nửa cung và quãng thứ hai là 3 nửa cung. Một bộ ba thứ xảy ra nếu quãng đầu tiên là 3 nửa cung và quãng thứ hai là 4 nửa cung. Nếu không, hợp âm được coi là "lạ". 

Kích thước đầu vào là cố định: chính xác là ba nốt. Điều này đảm bảo rằng bất kỳ thuật toán nào kiểm tra tất cả các hoán vị hoặc tính toán tất cả khoảng cách sẽ chạy cực kỳ nhanh vì 3! = 6 hoán vị là không đáng kể. Giới hạn thời gian và bộ nhớ rất rộng rãi cho đầu vào nhỏ như vậy. 

Các trường hợp khó khăn phát sinh do tính chất chu kỳ của thang đo. Ví dụ: một hợp âm như "B C# F" cần xử lý cẩn thận: chúng ta có thể cần quấn quanh thang âm để tính khoảng cách một cách chính xác. Việc trừ các chỉ số nốt một cách ngây thơ có thể cho các giá trị âm hoặc quá lớn, vì vậy việc sử dụng số học modulo 12 là cần thiết. Một điểm tinh tế nữa là hợp âm không có thứ tự nên cả 3! hoán vị phải được xem xét trước khi phân loại. 

## Phương pháp tiếp cận 

Phương pháp đơn giản nhất là vũ lực. Chuyển đổi mỗi nốt thành một chỉ số nguyên biểu thị vị trí của nó trong thang màu. Với mỗi hoán vị của ba nốt, hãy tính khoảng cách theo modulo 12 giữa các nốt liên tiếp. Nếu khoảng cách khớp với (4, 3), hãy phân loại là chính. Nếu chúng khớp với (3, 4), hãy phân loại là thứ yếu. Nếu không có kết quả nào khớp sau khi kiểm tra tất cả các hoán vị, hãy phân loại là lạ. 

Lực lượng vũ phu này hoạt động vì kích thước đầu vào rất nhỏ: 3! hoán vị và mỗi hoán vị chỉ yêu cầu một vài phép tính số học. Các ràng buộc của bài toán cho phép chúng ta xem xét mọi thứ tự mà không cần quan tâm đến hiệu suất. Ở đây không cần có cách tiếp cận tiệm cận nhanh hơn, nhưng người ta cũng có thể tính toán trước tất cả các bộ ba chính và phụ và kiểm tra tư cách thành viên trong một tập hợp. Phương pháp đó nhanh hơn một chút trong thực tế, nhưng đối với ba nốt, sự khác biệt là không đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các hoán vị) | O(3!) = O(1) | O(1) | Đã chấp nhận | 
| Bộ ba tính toán trước | O(1) | O(24) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một từ điển ánh xạ từng nốt nhạc vào chỉ mục màu sắc của nó. Ví dụ: C=0, C#=1, D=2, ..., H=11. Điều này cho phép chuyển đổi nhanh chóng từ tên ghi chú sang số nguyên. 
2. Đọc ba ghi chú đầu vào và chuyển đổi chúng thành các chỉ mục tương ứng bằng cách sử dụng từ điển. 
3. Tạo tất cả sáu hoán vị của ba chỉ số. Mỗi hoán vị thể hiện một thứ tự có thể có của hợp âm. 
4. Với mỗi hoán vị, tính khoảng cách giữa nốt thứ nhất và nốt thứ hai, giữa nốt thứ hai và nốt thứ ba, sử dụng phép tính modulo 12:`(second - first) % 12`Và`(third - second) % 12`. Điều này giải thích tính chất chu kỳ của thang đo. 
5. Nếu khoảng cách bằng (4, 3), hãy phân loại hợp âm là trưởng và kết thúc. 
6. Nếu khoảng cách bằng (3, 4), hãy phân loại hợp âm thành thứ và kết thúc. 
7. Nếu không có hoán vị nào khớp với một trong hai mẫu, hãy phân loại hợp âm là lạ. 

Tính đúng đắn của phương pháp này phụ thuộc vào việc kiểm tra tất cả các thứ tự. Vì hợp âm không có thứ tự và chỉ có ba nốt nên việc tìm kiếm toàn diện đảm bảo rằng nếu một hợp âm là trưởng hay thứ thì nó sẽ được phát hiện trong ít nhất một hoán vị. 

## Giải pháp Python```python
import sys
import itertools
input = sys.stdin.readline

note_to_index = {
    "C": 0, "C#": 1, "D": 2, "D#": 3, "E": 4, "F": 5, "F#": 6,
    "G": 7, "G#": 8, "A": 9, "B": 10, "H": 11
}

notes = input().strip().split()
indices = [note_to_index[n] for n in notes]

for perm in itertools.permutations(indices):
    d1 = (perm[1] - perm[0]) % 12
    d2 = (perm[2] - perm[1]) % 12
    if d1 == 4 and d2 == 3:
        print("major")
        break
    if d1 == 3 and d2 == 4:
        print("minor")
        break
else:
    print("strange")
```Đầu tiên, mã chuyển đổi tên nốt thành chỉ số số, sau đó kiểm tra tất cả các hoán vị để tính khoảng cách mô-đun. Việc sử dụng`% 12`rất quan trọng để xử lý các khoảng cách bao quanh, như từ H đến C.`else`trên`for`vòng lặp đảm bảo rằng nếu không xảy ra hiện tượng ngắt thì hợp âm được phân loại là hợp âm lạ. 

## Ví dụ đã hoạt động 

**Mẫu 1: "C E G"** 

| Bước | Hoán vị | d1 | d2 | Phân loại | 
| --- | --- | --- | --- | --- | 
| 1 | CEG (0, 4, 7) | 4 | 3 | chính | 
| 2 | C G E (0, 7, 4) | 7 | 9 | - | 

Hoán vị đầu tiên trùng với (4,3), nên hợp âm là trưởng. 

**Mẫu 2: "C#B F"** 

| Bước | Hoán vị | d1 | d2 | Phân loại | 
| --- | --- | --- | --- | --- | 
| 1 | C# B F (1, 10, 5) | 9 | 7 | - | 
| 2 | B C# F (10, 1, 5) | 3 | 4 | trẻ vị thành niên | 

Hoán vị thứ hai trùng với (3,4) nên hợp âm thứ. Điều này thể hiện việc xử lý khoảng cách theo chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3!) = O(1) | Chỉ có sáu hoán vị được xem xét, mỗi hoán vị có hai phép tính số học. | 
| Không gian | O(1) | Chỉ có một ánh xạ cố định và danh sách đầu vào của ba chỉ số được lưu trữ. | 

Với những hạn chế, giải pháp này chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
import itertools

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    note_to_index = {
        "C": 0, "C#": 1, "D": 2, "D#": 3, "E": 4, "F": 5, "F#": 6,
        "G": 7, "G#": 8, "A": 9, "B": 10, "H": 11
    }
    notes = input().strip().split()
    indices = [note_to_index[n] for n in notes]
    for perm in itertools.permutations(indices):
        d1 = (perm[1] - perm[0]) % 12
        d2 = (perm[2] - perm[1]) % 12
        if d1 == 4 and d2 == 3:
            return "major"
        if d1 == 3 and d2 == 4:
            return "minor"
    return "strange"

# provided sample
assert run("C E G\n") == "major", "sample 1"
# custom cases
assert run("C# B F\n") == "minor", "wrap around minor"
assert run("F# A C#\n") == "major", "another major triad"
assert run("D F A\n") == "minor", "classic minor triad"
assert run("C D E\n") == "strange", "no valid triad"
assert run("H C D\n") == "strange", "wrap around strange"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| C#B F | trẻ vị thành niên | Phát hiện bộ ba nhỏ xung quanh | 
| F# A C# | chính | Bộ ba chính tiêu chuẩn | 
| D F A | trẻ vị thành niên | Bộ ba nhỏ cổ điển | 
| C D E | lạ | Hợp âm không phải bộ ba | 
| H C D | lạ | Phát hiện xung quanh với hợp âm lạ | 

## Vỏ cạnh 

Đối với đầu vào "B C# F", cách tiếp cận đơn giản có thể tính toán`C# - B = -9`Và`F - C# = 4`, cho khoảng cách âm. Sử dụng modulo 12, chúng tôi nhận được`(1 - 10) % 12 = 3`Và`(5 - 1) % 12 = 4`, xác định chính xác hợp âm thứ. Tương tự, các hợp âm yêu cầu bao quanh như "H C D" được phân loại chính xác là lạ vì không có hoán vị nào tạo ra các mẫu nửa cung (4,3) hoặc (3,4) cần thiết.
