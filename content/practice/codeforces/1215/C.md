---
title: "CF 1215C - Hoán đổi chữ cái"
description: "Chúng ta có hai chuỗi, s và t, có độ dài bằng nhau chỉ gồm các chữ cái \"a\" và \"b\". Nhiệm vụ là làm cho s và t giống hệt nhau bằng cách sử dụng một chuỗi các thao tác hoán đổi được phép. Mỗi thao tác cho phép chúng ta chọn một ký tự từ s và một ký tự từ t và hoán đổi chúng."
date: "2026-06-11T22:56:25+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1215
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 585 (Div. 2)"
rating: 1500
weight: 1215
solve_time_s: 102
verified: true
draft: false
---

[CF 1215C - Hoán đổi chữ cái](https://codeforces.com/problemset/problem/1215/C) 

**Đánh giá:** 1500 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`s`Và`t`, có độ dài bằng nhau chỉ gồm các chữ cái "a" và "b". Nhiệm vụ là làm cho`s`Và`t`giống hệt nhau bằng cách sử dụng một chuỗi các thao tác hoán đổi được phép. Mỗi thao tác cho phép chúng ta chọn một ký tự từ`s`và một từ`t`và trao đổi chúng. Mục tiêu là đạt được sự bình đẳng với số lần hoán đổi ít nhất hoặc xác định rằng điều đó là không thể. 

Quan sát quan trọng là việc hoán đổi chỉ ảnh hưởng đến hai vị thế: một ở`s`và một trong`t`. Nếu tại một vị trí các ký tự đã bằng nhau thì chúng ta không cần hoán đổi chúng. Vì vậy, chúng ta chỉ cần xét đến những vị trí`s[i] != t[i]`. Điều này làm giảm vấn đề thành một tập hợp các cặp không khớp. Mỗi sự không phù hợp có thể được phân thành một trong hai loại:`s[i] = a, t[i] = b`(gọi loại này là AB) và`s[i] = b, t[i] = a`(loại BA). 

Cho rằng`n`có thể lên tới 200.000 và chúng tôi có giới hạn thời gian là 2 giây, một giải pháp kiểm tra tất cả các giao dịch hoán đổi có thể xảy ra một cách ngây thơ sẽ là quá chậm. Cách tiếp cận bạo lực khi thử tất cả các kết hợp sẽ là O(n²), điều này không thể chấp nhận được. Chúng ta cần một giải pháp thời gian tuyến tính. Các trường hợp đặc biệt bao gồm việc có một số lẻ thuộc một loại không khớp nhất định, điều này có thể khiến vấn đề không thể giải quyết được. Ví dụ: nếu chỉ có một AB không khớp và không có BA không khớp thì không có phép hoán đổi nào có thể giải quyết được, do đó đầu ra phải là -1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại qua tất cả các vị trí trong`s`Và`t`và hoán đổi bất cứ khi nào có sự không khớp, thử mọi cách kết hợp có thể cho đến khi`s`bằng`t`. Điều này đúng vì cuối cùng, mọi sự không phù hợp sẽ được giải quyết nếu có thể. Tuy nhiên, cách tiếp cận này quá chậm vì trong trường hợp xấu nhất, số lượng thao tác được xem xét có thể đạt tới n2, quá nhiều đối với n = 200.000. 

Cách tiếp cận tối ưu dựa trên hiểu biết sâu sắc rằng chúng ta có thể ghép các phần không khớp cùng loại để giảm số lượng giao dịch hoán đổi. Cụ thể, hai lỗi AB không khớp có thể được giải quyết bằng cách hoán đổi một ký tự từ`s`trong lần đầu tiên không khớp với lần thứ hai, điều này sẽ khắc phục cả hai. Tương tự, hai trường hợp BA không khớp có thể được giải quyết theo cùng một cách. Nếu có một AB còn sót lại và một BA còn sót lại không khớp, trước tiên chúng ta có thể thực hiện "tự hoán đổi" trong chuỗi`s`để biến nó thành một cặp có thể phân giải được, sau đó trao đổi qua lại`s`Và`t`. Cách tiếp cận này đảm bảo chúng tôi giảm thiểu số lần hoán đổi và chạy theo thời gian tuyến tính vì chúng tôi chỉ lặp lại các vị trí không khớp và xử lý chúng theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai danh sách,`ab`Và`ba`, để lưu trữ các chỉ số nơi xảy ra sự không khớp của loại AB và BA. Bước này đảm bảo chúng tôi phân loại các thông tin không khớp một cách hiệu quả. 
2. Lặp lại các chuỗi`s`Và`t`song song. Bất cứ khi nào`s[i] != t[i]`, nối thêm`i`vào danh sách thích hợp (`ab`nếu như`s[i]='a'`Và`t[i]='b'`,`ba`nếu như`s[i]='b'`Và`t[i]='a'`). Điều này tách vấn đề thành các vấn đề con độc lập cho từng loại không khớp. 
3. Nếu tổng số điểm không khớp là số lẻ thì không thể giải quyết được tất cả các điểm không khớp vì mỗi lần hoán đổi sẽ giải quyết được hai điểm không khớp. Trả về -1 trong trường hợp này. Cụ thể, nếu`(len(ab) + len(ba)) % 2 != 0`, chúng tôi xuất ra -1. 
4. Ghép nối các chỉ số trong`ab`danh sách theo thứ tự liên tiếp. Mỗi cặp`(i, j)`yêu cầu một trao đổi duy nhất giữa`s[i]`Và`t[j]`. Lặp lại quá trình tương tự cho`ba`danh sách. Điều này làm giảm số lượng thao tác bằng cách giải quyết hai điểm không khớp trong mỗi lần hoán đổi. 
5. Nếu cả hai`ab`Và`ba`có độ dài lẻ thì sẽ có một chỉ số còn sót lại trong mỗi đoạn. Đầu tiên thực hiện trao đổi trong`s`tại chỉ số AB còn lại và cùng chỉ số ở`t`để biến bài toán thành một cặp BA không khớp. Sau đó thực hiện hoán đổi giữa các vị trí được chuyển đổi này. Điều này đòi hỏi chính xác hai lần hoán đổi bổ sung. Thủ thuật này đảm bảo tất cả các điểm không khớp đều được giải quyết bằng các thao tác tối thiểu. 
6. Xuất ra tổng số lần hoán đổi và danh sách các thao tác hoán đổi theo thứ tự. Mỗi hoạt động được thể hiện bằng các chỉ số`(pos_s, pos_t)`. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi ghép nối các cặp không khớp cùng loại hoặc chuyển đổi một cặp không khớp thành cấu hình có thể phân giải được. Không có hoạt động nào tạo ra sự không phù hợp mới. Điều bất biến là sau khi xử lý tất cả các cặp, tất cả các điểm không khớp đều được giải quyết và các chuỗi đều bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()
t = input().strip()

ab = []
ba = []

for i in range(n):
    if s[i] != t[i]:
        if s[i] == 'a' and t[i] == 'b':
            ab.append(i + 1)  # using 1-based indexing
        else:
            ba.append(i + 1)

if (len(ab) + len(ba)) % 2 != 0:
    print(-1)
else:
    ops = []

    for i in range(0, len(ab) - len(ab) % 2, 2):
        ops.append((ab[i], ab[i + 1]))
    for i in range(0, len(ba) - len(ba) % 2, 2):
        ops.append((ba[i], ba[i + 1]))

    if len(ab) % 2 == 1:
        last_ab = ab[-1]
        last_ba = ba[-1]
        ops.append((last_ab, last_ab))
        ops.append((last_ab, last_ba))

    print(len(ops))
    for x, y in ops:
        print(x, y)
```Giải pháp trước tiên xác định các điểm không khớp, sau đó giải quyết chúng một cách hiệu quả bằng cách ghép nối và cuối cùng xử lý mọi cặp lẻ còn lại bằng hoán đổi hai bước. Điểm tinh tế là đảm bảo lập chỉ mục dựa trên 1 cho đầu ra và xử lý chính xác cặp còn lại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
abab
aabb
```| tôi | s[i] | t[i] | ab | ba | 
| --- | --- | --- | --- | --- | 
| 1 | một | một | | | 
| 2 | b | một | | 2 | 
| 3 | một | b | 3 | | 
| 4 | b | b | | | 

Xử lý: 

- Cặp chỉ số ab: [3] (lẻ) 
- Cặp ba chỉ số: [2] (lẻ) 
- Thực hiện tự hoán đổi ở mức 3, sau đó hoán đổi 3 với 2 
- Tổng số hoạt động: 2 

Đầu vào mẫu 2:```
2
ab
bb
```| tôi | s[i] | t[i] | ab | ba | 
| --- | --- | --- | --- | --- | 
| 1 | một | b | 1 | | 
| 2 | b | b | | | 

Tổng số không khớp = 1 (lẻ), không thể. 

Dấu vết này cho thấy cách thuật toán phát hiện những điều không thể xảy ra và xử lý chính xác các điểm không khớp lẻ còn sót lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để phân loại các hoạt động không khớp và ghép nối tuyến tính. | 
| Không gian | O(n) | Lưu trữ danh sách chỉ số AB và BA. | 

Thuật toán chia tỷ lệ tuyến tính với độ dài chuỗi, phù hợp với n lên tới 200.000 trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution code here
    n = int(input())
    s = input().strip()
    t = input().strip()

    ab = []
    ba = []

    for i in range(n):
        if s[i] != t[i]:
            if s[i] == 'a' and t[i] == 'b':
                ab.append(i + 1)
            else:
                ba.append(i + 1)

    if (len(ab) + len(ba)) % 2 != 0:
        print(-1)
    else:
        ops = []

        for i in range(0, len(ab) - len(ab) % 2, 2):
            ops.append((ab[i], ab[i + 1]))
        for i in range(0, len(ba) - len(ba) % 2, 2):
            ops.append((ba[i], ba[i + 1]))

        if len(ab) % 2 == 1:
            last_ab = ab[-1]
            last_ba = ba[-1]
            ops.append((last_ab, last_ab))
            ops.append((last_ab, last_ba))

        print(len(ops))
        for x, y in ops:
            print(x, y)
    return output.getvalue().strip()

# Provided samples
assert run("4\nabab\naabb\n") == "2\n3 3\n3 2", "sample 1"
assert run("2\nab\nbb\n") == "-1",
```
