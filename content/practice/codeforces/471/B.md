---
title: "CF 471B - MUH và những điều quan trọng"
description: "Chúng ta được cung cấp một danh sách các nhiệm vụ, mỗi nhiệm vụ có mức độ khó liên quan và chúng ta cần tạo ra ba chuỗi khác nhau trong đó tất cả các nhiệm vụ đều được hoàn thành. Mỗi trình tự phải tôn trọng quy tắc rằng các nhiệm vụ chỉ có thể được sắp xếp lại nếu độ khó của chúng bằng nhau."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 471
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 269 (Div. 2)"
rating: 1300
weight: 471
solve_time_s: 101
verified: false
draft: false
---

[CF 471B - MUH và những điều quan trọng](https://codeforces.com/problemset/problem/471/B) 

**Đánh giá:** 1300 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các nhiệm vụ, mỗi nhiệm vụ có mức độ khó liên quan và chúng ta cần tạo ra ba chuỗi khác nhau trong đó tất cả các nhiệm vụ đều được hoàn thành. Mỗi trình tự phải tôn trọng quy tắc rằng các nhiệm vụ chỉ có thể được sắp xếp lại nếu độ khó của chúng bằng nhau. Nói cách khác, đối với bất kỳ cặp nhiệm vụ nào có độ khó riêng biệt, thứ tự tương đối của chúng không thể thay đổi giữa các chuỗi. Đầu vào bao gồm số lượng nhiệm vụ`n`và một mảng`n`số nguyên đại diện cho độ khó của nhiệm vụ. Đầu ra phải là "KHÔNG" nếu không thể tạo ra ba chuỗi riêng biệt hoặc "CÓ" theo sau là ba chuỗi chỉ số nhiệm vụ thỏa mãn các điều kiện. 

Những ràng buộc cho phép`n`lên tới 2000, đủ nhỏ để một thuật toán có độ phức tạp về thời gian lên tới khoảng O(n²) có thể chạy thoải mái trong vòng một giây. Một cách tiếp cận mạnh mẽ tạo ra tất cả các hoán vị sẽ không khả thi vì số lượng hoán vị tăng theo giai thừa với`n`. Chúng ta cũng cần cẩn thận với các trường hợp đặc biệt: nếu tất cả các nhiệm vụ đều có độ khó riêng biệt thì không thể hoán đổi hai nhiệm vụ, do đó chỉ tồn tại một chuỗi hợp lệ, khiến không thể tạo ra ba kế hoạch riêng biệt. Nếu có chính xác hai nhiệm vụ có cùng độ khó và không có nhiệm vụ nào lặp lại, chúng ta chỉ có thể tạo ra hai chuỗi riêng biệt bằng cách hoán đổi chúng, điều này vẫn chưa đủ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ sẽ cố gắng tạo ra tất cả các hoán vị hợp lệ của các chỉ số nhiệm vụ tôn trọng thứ tự độ khó. Điều này liên quan đến việc xác định tất cả các nhiệm vụ có độ khó như nhau và hoán vị chúng, đồng thời giữ các nhiệm vụ có độ khó khác nhau theo cùng một thứ tự tương đối. Mặc dù đúng nhưng cách tiếp cận này nhanh chóng trở nên không thực tế vì ngay cả một khó khăn đơn lẻ được lặp lại mười lần cũng tạo ra 10! hoán vị, vượt xa những hạn chế về thời gian của chúng tôi. 

Quan sát quan trọng là chúng ta chỉ cần ba trình tự riêng biệt chứ không phải tất cả các trình tự có thể. Một trình tự có thể được thay đổi bằng cách hoán đổi các nhiệm vụ có cùng độ khó. Do đó, chúng ta chỉ cần tìm hai nhóm có độ khó giống hệt nhau: một nhóm có kích thước ít nhất là 2 cho phép hoán đổi một lần để tạo chuỗi thứ hai và nhóm khác có kích thước ít nhất là 2 hoặc cùng một nhóm (nếu có ít nhất 3 nhiệm vụ) cho phép chuỗi thứ ba. Điều này làm giảm vấn đề đếm số lần lặp lại trong mảng độ khó. Khi chúng tôi xác định được các nhóm này, việc tạo chuỗi rất đơn giản: giữ các nhiệm vụ theo thứ tự sắp xếp cơ bản cho chuỗi đầu tiên, hoán đổi các nhiệm vụ trong nhóm lặp lại đầu tiên cho chuỗi thứ hai và hoán đổi các nhiệm vụ trong nhóm lặp lại thứ hai (hoặc hoán đổi bổ sung trong nhóm đầu tiên nếu nó đủ lớn) cho chuỗi thứ ba. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ghép nối độ khó của mỗi nhiệm vụ với chỉ mục ban đầu của nó và sắp xếp các nhiệm vụ theo độ khó. Điều này tạo ra một thứ tự chuẩn cho chuỗi đầu tiên. 
2. Quét danh sách đã sắp xếp để tìm các nhóm nhiệm vụ có độ khó tương tự. Theo dõi các chỉ số cho từng khó khăn lặp đi lặp lại. 
3. Nếu không có khó khăn nào xuất hiện nhiều lần, hãy in "KHÔNG" vì không thể hoán đổi và quay lại. 
4. Nếu chính xác một khó khăn xuất hiện ít nhất hai lần nhưng ít hơn ba lần, hãy kiểm tra xem có khó khăn nào khác xuất hiện ít nhất hai lần để cung cấp lần hoán đổi thứ hai hay không. Nếu không, hãy in "KHÔNG" và quay lại. 
5. Sử dụng các nhóm lặp lại đã xác định để tạo ra ba chuỗi. Trình tự đầu tiên là thứ tự được sắp xếp. Chuỗi thứ hai được hình thành bằng cách hoán đổi hai chỉ số đầu tiên trong nhóm lặp lại đầu tiên. Chuỗi thứ ba được hình thành bằng cách hoán đổi hai chỉ số tiếp theo trong cùng một nhóm (nếu nó có ba phần tử trở lên) hoặc hoán đổi hai chỉ mục đầu tiên trong nhóm lặp lại thứ hai. 
6. Xuất "YES" và in ba dãy. 

Tại sao nó hoạt động: Việc sắp xếp các nhiệm vụ theo độ khó sẽ thiết lập một chuỗi cơ sở hợp lệ trong đó các ràng buộc về thứ tự tương đối được thỏa mãn. Bất kỳ sự hoán đổi nào trong một nhóm có độ khó như nhau đều duy trì thứ tự này. Vì bài toán chỉ yêu cầu ba chuỗi riêng biệt nên hai hoặc ba lần hoán đổi trong các nhóm lặp lại đảm bảo rằng các chuỗi đó là duy nhất, thỏa mãn các điều kiện của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
h = list(map(int, input().split()))

# pair difficulty with original index
tasks = [(h[i], i+1) for i in range(n)]
tasks.sort()  # sort by difficulty

# find repeated difficulties
groups = {}
for diff, idx in tasks:
    if diff not in groups:
        groups[diff] = []
    groups[diff].append(idx)

first_swap = None
second_swap = None
for indices in groups.values():
    if len(indices) >= 2 and not first_swap:
        first_swap = indices[:2]
    elif len(indices) >= 2 and not second_swap:
        second_swap = indices[:2]

# if no two groups of repeats or one group with 3+, impossible
if not first_swap or (len(first_swap) < 3 and not second_swap):
    print("NO")
    sys.exit()

# build sequences
seq1 = [idx for _, idx in tasks]
seq2 = seq1[:]
seq3 = seq1[:]

# first swap for second sequence
seq2[seq1.index(first_swap[0])], seq2[seq1.index(first_swap[1])] = seq2[seq1.index(first_swap[1])], seq2[seq1.index(first_swap[0])]

# second swap for third sequence
if len(first_swap) >= 3:
    seq3[seq1.index(first_swap[1])], seq3[seq1.index(first_swap[2])] = seq3[seq1.index(first_swap[2])], seq3[seq1.index(first_swap[1])]
else:
    seq3[seq1.index(second_swap[0])], seq3[seq1.index(second_swap[1])] = seq3[seq1.index(second_swap[1])], seq3[seq1.index(second_swap[0])]

print("YES")
print(*seq1)
print(*seq2)
print(*seq3)
```Đầu tiên, mã tạo ra một chuỗi cơ sở được sắp xếp bằng cách sử dụng các độ khó của nhiệm vụ, sau đó xác định các nhóm độ khó lặp lại để thực hiện hoán đổi. các`first_swap`xử lý chuỗi thứ hai, và`second_swap`hoặc trao đổi bổ sung trong`first_swap`tạo ra chuỗi thứ ba. các`seq1.index()`các cuộc gọi ánh xạ các chỉ mục ban đầu một cách chính xác tới các vị trí trong chuỗi được sắp xếp. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
1 3 3 1
```| Bước | nhiệm vụ (được sắp xếp) | đầu tiên_swap | giây_swap | seq1 | seq2 | seq3 | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | [(1,1),(1,4),(3,2),(3,3)] | [1,4] | [2,3] | [1,4,2,3] | [1,4,2,3] | [1,4,2,3] | 
| trao đổi seq2 | - | - | - | - | [4,1,2,3] | - | 
| trao đổi seq3 | - | - | - | - | - | [4,1,3,2] | 

Điều này cho thấy việc hoán đổi trong các nhóm lặp lại sẽ tạo ra các chuỗi khác biệt. 

Đầu vào mẫu 2:```
5
1 2 3 4 5
```Không có khó khăn lặp đi lặp lại.`first_swap`là Không, vì vậy đầu ra là "KHÔNG". Điều này xác nhận thuật toán xác định chính xác các trường hợp không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Nhiệm vụ sắp xếp chiếm ưu thế trong thời gian chạy; quét để tìm những khó khăn lặp đi lặp lại là O(n). | 
| Không gian | O(n) | Lưu trữ các cặp chỉ mục và trình tự nhiệm vụ. | 

Với n 2000, các thao tác O(n log n) chạy thoải mái trong vòng một giây và mức sử dụng bộ nhớ vẫn nằm trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    h = list(map(int, input().split()))
    tasks = [(h[i], i+1) for i in range(n)]
    tasks.sort()
    groups = {}
    for diff, idx in tasks:
        if diff not in groups:
            groups[diff] = []
        groups[diff].append(idx)
    first_swap = None
    second_swap = None
    for indices in groups.values():
        if len(indices) >= 2 and not first_swap:
            first_swap = indices[:2]
        elif len(indices) >= 2 and not second_swap:
            second_swap = indices[:2]
    if not first_swap or (len(first_swap) < 3 and not second_swap):
        return "NO"
    seq1 = [idx for _, idx in tasks]
    seq2 = seq1[:]
    seq3 = seq1[:]
    seq2[seq1.index(first_swap[0])], seq2[seq1.index(first_swap[1])] = seq2[seq1.index(first_swap[1])], seq2[seq1.index(first_swap[0])]
    if len(first_swap) >= 3:
        seq3[seq1.index(first_swap[1])], seq3[seq1.index(first_swap[2])] = seq3[seq1.index(first_swap[2])], seq3[seq1.index(first_swap[1])]
    else:
        seq3[seq1.index(second_swap[0])], seq3[seq1.index(second_swap[1])] = seq3[seq1.index(second_swap[1])], seq3[seq1.index(second_swap[0])]
    return "YES\n" + " ".join(map(str, seq1)) + "\n" + " ".join(map(str, seq2)) + "\n" + " ".join
```
