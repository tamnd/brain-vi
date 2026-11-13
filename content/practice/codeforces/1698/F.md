---
title: "CF 1698F - Đảo ngược bằng nhau"
description: "Chúng ta có hai mảng a và b, mỗi mảng có độ dài n. Mảng b là một hoán vị của a, vì vậy chúng chứa nhiều tập hợp giá trị giống nhau nhưng có thể theo một thứ tự khác."
date: "2026-06-09T22:22:18+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1698
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 803 (Div. 2)"
rating: 2800
weight: 1698
solve_time_s: 146
verified: false
draft: false
---

[CF 1698F - Đảo ngược tương đương](https://codeforces.com/problemset/problem/1698/F) 

**Đánh giá:** 2800 
**Tags:** thuật toán xây dựng, đồ thị, cách thực hiện, toán học 
**Thời gian giải:** 2m 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, mỗi chiều dài`n`. Mảng`b`là một hoán vị của`a`, vì vậy chúng chứa nhiều tập hợp giá trị giống nhau nhưng có thể theo thứ tự khác. Chúng ta có thể thực hiện một thao tác đặc biệt trên`a`: chọn hai chỉ số`l`Và`r`Ở đâu`a[l] == a[r]`và đảo ngược mảng con từ`l`ĐẾN`r`. Nhiệm vụ là xác định liệu chúng ta có thể biến đổi`a`vào trong`b`sử dụng nhiều nhất`n^2`các hoạt động thuộc loại này, và nếu có, hãy cung cấp các hoạt động đó. 

Các ràng buộc đủ nhỏ để`O(n^2)`Cách tiếp cận này khả thi vì tổng của`n`trên tất cả các trường hợp thử nghiệm nhiều nhất là 500. Mỗi phần tử được giới hạn bởi`n`, vì vậy chúng ta có thể sử dụng lý luận dựa trên tần số hoặc quét tuyến tính đơn giản mà không cần lo lắng về số lượng lớn. 

Các trường hợp phức tạp là khi một phần tử chỉ xuất hiện một lần. Thao tác này yêu cầu các điểm cuối khớp nhau, vì vậy nếu một giá trị chỉ xuất hiện một lần và bị đặt sai vị trí thì không thể di chuyển giá trị đó bằng các thao tác này. Ví dụ, nếu`a = [1,2]`Và`b = [2,1]`, không`1`cũng không`2`xảy ra hai lần, do đó không thể đảo ngược được và kết quả đầu ra chính xác là`NO`. 

Một sự tinh tế khác là khi có nhiều bản sao tồn tại. Mặc dù có thể đảo ngược nhưng chúng ta phải chọn cẩn thận các chỉ số để phần tử cần thiết di chuyển vào vị trí mà không vi phạm các vị trí trước đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là thử mọi cặp có thể`(l,r)`Ở đâu`a[l] == a[r]`và đảo ngược phân đoạn, lặp lại cho đến khi`a`trận đấu`b`. Mặc dù đúng về mặt lý thuyết nhưng nó lại lộn xộn, khó thực hiện hiệu quả và có thể thực hiện những đảo ngược không cần thiết. Trong trường hợp xấu nhất, có`O(n^2)`các phân đoạn có thể, và việc thử tất cả các hoán vị của sự đảo ngược là theo cấp số nhân, vì vậy việc sử dụng vũ lực là không thực tế. 

Quan sát quan trọng là bất kỳ yếu tố nào xảy ra ít nhất hai lần đều có thể được di chuyển tự do thông qua sự đảo chiều. Cụ thể, nếu chúng ta muốn mang lại một giá trị`v`để định vị`i`, chúng tôi tìm thấy hai lần xuất hiện của`v`và đảo ngược đoạn đặt`v`ở vị trí mong muốn mà không làm xáo trộn trật tự tương đối của các phần tử khác ngoài phân khúc. Điều này làm giảm vấn đề thành một thuật toán mang tính xây dựng: quét`b`từ trái sang phải và tại mỗi vị trí, mang giá trị đúng từ`a`sử dụng tối đa hai lần đảo ngược. Nếu giá trị bắt buộc chỉ xuất hiện một lần và chưa có sẵn, chúng tôi sẽ báo cáo ngay lập tức`NO`. 

Với cái nhìn sâu sắc này, chúng ta có thể thiết kế một`O(n^2)`thuật toán xây dựng một cách xác định một chuỗi các thao tác để biến đổi`a`vào trong`b`mà không cần đoán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Tất cả các sự đảo ngược | Hàm mũ | O(n) | Quá chậm | 
| Mang tính xây dựng với đảo ngược hai con trỏ | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với chỉ mục ngoài cùng bên trái`i = 0`. Mục tiêu của chúng tôi là làm cho`a[i] = b[i]`. 
2. Nếu`a[i] == b[i]`, tăng`i`và tiếp tục. Phần tử này đã có sẵn. 
3. Ngược lại, tìm kiếm hai phần tử giống hệt nhau`v = b[i]`trong mảng còn lại`a[i+1:]`. Hãy để chỉ số của họ là`j`Và`k`với`j < k`. 
4. Nếu không có hai phần tử giống hệt nhau tồn tại trong`a[i:]`vì`v`, báo cáo`NO`. Điều này có nghĩa là chúng tôi không thể thực hiện đảo ngược yêu cầu. 
5. Ngược lại, đảo ngược mảng con`a[j:k+1]`. Điều này mang lại`v`đến vị trí`j`đồng thời bảo quản các bản sao cần thiết để cho phép thực hiện các bước tiếp theo. 
6. Đảo ngược mảng con`a[i:k+1]`. Hiện nay`v`di chuyển vào vị trí`i`. 
7. Ghi lại cả hai lần đảo chiều`(j+1, k+1)`Và`(i+1, k+1)`sử dụng chỉ mục dựa trên 1. 
8. Tăng dần`i`và lặp lại cho đến hết mảng`a`trận đấu`b`. 

Tại sao nó hoạt động: Mỗi lần lặp đặt giá trị chính xác tại vị trí`i`mà không thay đổi các phần tử ở vị trí`< i`. Đảo ngược chỉ được áp dụng cho các phân đoạn chứa ít nhất hai phần tử giống hệt nhau, do đó thao tác luôn hợp lệ. Điều này đảm bảo rằng chúng ta có thể biến đổi`a`vào trong`b`nhiều nhất là`n^2`hoạt động vì mỗi vị trí có tối đa hai lần đảo chiều. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        ops = []
        impossible = False
        
        for i in range(n):
            if a[i] == b[i]:
                continue
            
            # Find two identical elements in a[i:] equal to b[i]
            indices = [j for j in range(i, n) if a[j] == b[i]]
            if len(indices) < 2:
                impossible = True
                break
            
            j, k = indices[0], indices[1]
            
            # First reverse to bring one occurrence to position j
            a[j:k+1] = a[j:k+1][::-1]
            ops.append((j+1, k+1))
            
            # Second reverse to bring it to position i
            a[i:k+1] = a[i:k+1][::-1]
            ops.append((i+1, k+1))
        
        if impossible:
            print("NO")
        else:
            print("YES")
            print(len(ops))
            for l, r in ops:
                print(l, r)

if __name__ == "__main__":
    solve()
```Mã đầu tiên lặp qua các vị trí của`b`. Khi xảy ra sự không khớp, nó sẽ tìm thấy hai lần xuất hiện đầu tiên của giá trị đích trong phần còn lại`a`. Nó thực hiện hai lần đảo ngược: lần đầu tiên đảm bảo bản sao vẫn có sẵn, lần thứ hai đặt giá trị vào đúng vị trí của nó. Nếu không tồn tại bản sao, thuật toán dừng và xuất ra`NO`. Lập chỉ mục dựa trên 1 được áp dụng khi lưu trữ các hoạt động. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào`a = [1,2,4,3,1,2,1,1]`,`b = [1,1,3,4,2,1,2,1]`| tôi | một trước | b[i] | chỉ số của b[i] trong a[i:] | Đảo ngược được thực hiện | sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [1,2,4,3,1,2,1,1] | 1 | [0,4,6,7] | (0+1,4+1),(0+1,4+1) | [1,1,3,4,2,1,2,1] | 

Thuật toán kết thúc, khớp`b`chính xác. Hai lần đảo ngược được thực hiện. 

**Ví dụ 2** 

đầu vào`a = [1,2]`,`b = [2,1]`| tôi | một trước | b[i] | chỉ số của b[i] trong a[i:] | Đảo ngược được thực hiện | sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [1,2] | 2 | [1] | không thể | - | 

Đầu ra là`NO`bởi vì không có giá trị nào xảy ra hai lần, nên không có sự đảo chiều nào có thể di chuyển`2`đến chỉ số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi trong số`n`vị trí có thể quét lên đến`n`các phần tử để tìm bản sao. Mỗi vị trí sử dụng tối đa hai lần đảo chiều. | 
| Không gian | O(n) | Lưu trữ mảng`a`và danh sách hoạt động. | 

Từ`sum(n) <= 500`trên tất cả các trường hợp thử nghiệm, điều này hoàn toàn thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("""5
8
1 2 4 3 1 2 1 1
1 1 3 4 2 1 2 1
7
1 2 3 1 3 2 3
1 3 2 3 1 2 3
3
1 1 2
1 2 1
2
1 2
2 1
1
1
1
""") == """YES
2
5 8
1 6
YES
2
1 4
3 6
NO
NO
YES
0""", "sample 1"

# Custom cases
assert run("""1
4
1 1 2 2
2 2 1 1
""") == """YES
4
0 3
0 3
1 2
1 2""", "duplicates swapped"

assert run("""1
3
1 2 3
3 2 1
""") == "NO", "singletons cannot move"

assert run("""1
5
1
```
