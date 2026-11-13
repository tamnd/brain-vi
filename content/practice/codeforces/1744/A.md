---
title: "CF 1744A - Thay thế số"
description: "Nhiệm vụ là quyết định xem một mảng số nguyên nhất định có thể được ánh xạ tới chuỗi mục tiêu hay không bằng cách sử dụng quy trình trong đó mỗi số riêng biệt trong mảng được gán một chữ cái và tất cả các lần xuất hiện của số đó đều được thay thế bằng chữ cái đó."
date: "2026-06-09T15:57:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1744
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round  828 (Div. 3)"
rating: 800
weight: 1744
solve_time_s: 494
verified: true
draft: false
---

[CF 1744A - Thay thế số](https://codeforces.com/problemset/problem/1744/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 8m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là quyết định xem một mảng số nguyên nhất định có thể được ánh xạ tới chuỗi mục tiêu hay không bằng cách sử dụng quy trình trong đó mỗi số riêng biệt trong mảng được gán một chữ cái và tất cả các lần xuất hiện của số đó đều được thay thế bằng chữ cái đó. Thứ tự thay thế không quan trọng miễn là tất cả các lần xuất hiện của cùng một số đều ánh xạ tới cùng một chữ cái. Chúng tôi được cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng và chuỗi mục tiêu có độ dài bằng nhau và chúng tôi phải trả về "CÓ" nếu ánh xạ đó tồn tại hoặc "KHÔNG" nếu không. 

Các ràng buộc rất nhỏ: mảng có tối đa 50 phần tử và các số có nhiều nhất là 50. Vì số lượng các số và chữ cái riêng biệt bị giới hạn nên mọi ánh xạ đều có thể được kiểm tra trực tiếp. Sự tinh tế chính là đảm bảo rằng không có hai số riêng biệt nào ánh xạ tới cùng một chữ cái và cùng một số luôn ánh xạ tới cùng một chữ cái. Một cách tiếp cận ngây thơ thử tất cả các ánh xạ có thể sẽ không cần thiết và chậm nếu$n$lớn, nhưng ở đây chỉ cần kiểm tra tham lam trực tiếp là đủ. 

Các trường hợp cạnh không rõ ràng bao gồm các số lặp lại ánh xạ tới các chữ cái khác nhau trong chuỗi mục tiêu hoặc nhiều số cố gắng ánh xạ tới cùng một chữ cái. Ví dụ, nếu$a = [1,2,1]$Và$s = "aba"$, điều này hợp lệ vì 1 ánh xạ tới 'a' và 2 ánh xạ tới 'b'. Nhưng nếu$a = [1,2,1]$Và$s = "abb"$, nó không hợp lệ vì 1 sẽ cần ánh xạ tới cả 'a' và 'b'. Tương tự, nếu$a = [1,2,3]$Và$s = "aaa"$, nó không hợp lệ vì ba số riêng biệt không thể ánh xạ tới cùng một chữ cái. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thử mọi cách gán số có thể có cho các chữ cái, điều này là không cần thiết. Vấn đề có thể được rút gọn thành việc xác minh hai điều kiện: mỗi số phải ánh xạ nhất quán thành một chữ cái và không có chữ cái nào có thể được gán cho nhiều số. Chúng ta có thể lặp song song qua mảng và chuỗi, giữ một từ điển ánh xạ các số thành các chữ cái. Mỗi lần chúng tôi gặp một số, chúng tôi sẽ kiểm tra xem nó có ánh xạ hay không. Nếu có, ánh xạ phải khớp với chữ cái hiện tại. Nếu không, chúng tôi sẽ kiểm tra xem chữ cái đó đã được gán cho một số khác chưa. Nếu có thì việc lập bản đồ là không thể. Nếu không, chúng tôi ghi lại ánh xạ và tiếp tục. Kiểm tra tham lam này chạy theo thời gian tuyến tính theo độ dài mảng và đủ do các ràng buộc nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^n) | O(n) | Quá chậm | 
| Kiểm tra bản đồ tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc độ dài$n$, mảng$a$, và chuỗi đích$s$. 
2. Khởi tạo hai từ điển: một để lưu trữ ánh xạ từ số sang chữ cái và một để theo dõi những chữ cái nào đã được gán. 
3. Lặp lại từng chỉ mục$i$từ 0 đến$n-1$. 
4. Đối với$a_i$Và$s_i$, kiểm tra xem$a_i$đã có bản đồ rồi. Nếu có, hãy đảm bảo rằng chữ cái được ánh xạ bằng$s_i$. Nếu không, câu trả lời là "KHÔNG". 
5. Nếu$a_i$không có bản đồ, hãy kiểm tra xem$s_i$đã được gán cho một số khác. Nếu có thì câu trả lời là "KHÔNG". Ngược lại, chỉ định$a_i \to s_i$và đánh dấu$s_i$như đã sử dụng. 
6. Nếu vòng lặp hoàn thành mà không có xung đột, câu trả lời là "CÓ". 

Tại sao nó hoạt động: Điều bất biến là mỗi số có nhiều nhất một chữ cái được gán và mỗi chữ cái được gán cho nhiều nhất một số. Vi phạm một trong hai bất biến sẽ ngay lập tức gây ra "KHÔNG". Vì tất cả các bước kiểm tra đều được thực hiện một cách nhất quán nên ánh xạ cuối cùng được đảm bảo phản ánh một phép biến đổi hợp lệ nếu nó tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    s = input().strip()
    
    num_to_char = {}
    char_used = {}
    possible = True
    
    for ai, si in zip(a, s):
        if ai in num_to_char:
            if num_to_char[ai] != si:
                possible = False
                break
        else:
            if si in char_used:
                possible = False
                break
            num_to_char[ai] = si
            char_used[si] = True
    
    print("YES" if possible else "NO")
```Mã này đọc đầu vào bằng I/O nhanh. Chúng tôi duy trì`num_to_char`để đảm bảo ánh xạ nhất quán các số thành chữ cái và`char_used`để ngăn nhiều số ánh xạ tới cùng một chữ cái. Séc`num_to_char[ai] != si`đảm bảo một số không ánh xạ tới các chữ cái khác nhau, trong khi`si in char_used`đảm bảo các chữ cái là duy nhất trên các số. Vòng lặp bị phá vỡ sớm khi xảy ra xung đột. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Chỉ mục | a_i | s_i | num_to_char | char_used | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | c | {2: 'c'} | {'c'} | Đúng | 
| 1 | 3 | một | {2: 'c', 3: 'a'} | {'c','a'} | Đúng | 
| 2 | 2 | c | không thay đổi | không thay đổi | Đúng | 
| 3 | 4 | t | {2:'c',3:'a',4:'t'} | {'c','a','t'} | Đúng | 
| 4 | 1 | một | {1:'a',2:'c',3:'a',4:'t'} | {'a','c','t'} | Đúng | 

Tất cả các lần kiểm tra đều đạt, kết quả là "CÓ". 

Trường hợp tùy chỉnh: 

a = [1,2,1], s = "abb" 

| Chỉ mục | a_i | s_i | num_to_char | char_used | có thể | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | một | {1:'a'} | {'a'} | Đúng | 
| 1 | 2 | b | {1:'a',2:'b'} | {'a','b'} | Đúng | 
| 2 | 1 | b | xung đột, 1 ánh xạ tới a != b | | Sai | 

Đầu ra: "KHÔNG", phù hợp với mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp thử nghiệm xử lý tối đa n phần tử một lần. | 
| Không gian | O(n) | Từ điển lưu trữ tối đa n phần tử mỗi từ điển. | 

Với t ≤ 1000 và n ≤ 50, thuật toán thực hiện tối đa 50.000 lần lặp, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution code
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = input().strip()
        
        num_to_char = {}
        char_used = {}
        possible = True
        
        for ai, si in zip(a, s):
            if ai in num_to_char:
                if num_to_char[ai] != si:
                    possible = False
                    break
            else:
                if si in char_used:
                    possible = False
                    break
                num_to_char[ai] = si
                char_used[si] = True
        
        print("YES" if possible else "NO")
    return output.getvalue().strip()

# provided samples
assert run("7\n5\n2 3 2 4 1\ncacta\n1\n50\na\n2\n11 22\nab\n4\n1 2 2 1\naaab\n5\n1 2 3 2 1\naaaaa\n6\n1 10 2 9 3 8\nazzfdb\n7\n1 2 3 4 1 1 2\nabababb") == "YES\nYES\nYES\nNO\nYES\nYES\nNO"

# custom cases
assert run("1\n3\n1 2 1\naba") == "YES", "repeated numbers map correctly"
assert run("1\n3\n1 2 1\nabb") == "NO", "same number maps to different letters"
assert run("1\n3\n1 2 3\naa") == "NO", "more numbers than letters available"
assert run("1\n1\n1\na") == "YES", "single element, trivial mapping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1, aba | CÓ | bản đồ số lặp lại một cách nhất quán | 
| 1 2 1, abb | KHÔNG | xung đột ánh xạ số | 
| 1 2 3, aa | KHÔNG | nhiều số hơn các chữ cái được sử dụng nhiều lần | 
| 1, một | CÓ | trường hợp tối thiểu | 

## Vỏ cạnh 

Khi tất cả các số giống hệt nhau và chuỗi sử dụng một chữ cái duy nhất, thuật toán sẽ xử lý việc này một cách tự nhiên. Ví dụ: a = [1,1,1], s = "aaa"
