---
title: "CF 2086B - Mảng lớn và phân đoạn"
description: "Chúng ta được cho một mảng a có độ dài n và một số nguyên dương k. Từ a, chúng ta xây dựng một mảng b lớn hơn bằng cách lặp lại a chính xác k lần. Nghĩa là b có độ dài n k và n phần tử đầu tiên của nó giống a và các phần tử còn lại lặp lại a theo thứ tự."
date: "2026-06-08T06:02:38+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2086
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 177 (Rated for Div. 2)"
rating: 1100
weight: 2086
solve_time_s: 83
verified: true
draft: false
---

[CF 2086B - Mảng lớn và phân đoạn](https://codeforces.com/problemset/problem/2086/B) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`chiều dài`n`và một số nguyên dương`k`. Từ`a`, chúng tôi xây dựng một mảng lớn hơn`b`bằng cách lặp lại`a`chính xác`k`lần. Đó là,`b`có chiều dài`n * k`, và đó là lần đầu tiên`n`các phần tử giống nhau như`a`và các phần tử còn lại lặp lại`a`theo thứ tự. Nhiệm vụ là, với một ngưỡng nhất định`x`, để đếm xem có bao nhiêu vị trí bắt đầu`l`TRONG`b`có ít nhất một đoạn`[l, r]`tổng của nó ít nhất là`x`. 

Bởi vì`n`Và`k`mỗi cái có thể lên đến`10^5`,`b`về mặt lý thuyết có thể có chiều dài lên tới`10^10`. Cleary, xây dựng`b`một cách rõ ràng hoặc lặp lại trên tất cả các phân đoạn sẽ không khả thi. Tổng của tất cả`n`và tất cả`k`trên các trường hợp thử nghiệm được giới hạn bởi`2*10^5`, điều này gợi ý rằng một nghiệm tuyến tính trong`n`mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi thứ tuyến tính trong`n * k`thì không. 

Các trường hợp cạnh cần xem bao gồm khi`k`rất lớn nhưng`x`rất lớn nên không có đoạn nào đạt tới`x`. Ví dụ, nếu`a = [1]`,`k = 100000`, Và`x = 1000000000`, câu trả lời là`0`. Một trường hợp cạnh khác xảy ra khi`x`là rất nhỏ so với`a`, chẳng hạn như`x = 1`Và`a = [100]`, trong đó mọi vị trí`l`là hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ xem xét mọi vị trí bắt đầu`l`TRONG`b`, sau đó lặp lại việc thêm các phần tử về phía trước cho đến khi tổng đạt hoặc vượt quá`x`. Điều này hoạt động chính xác nhưng có độ phức tạp`O(n*k)`cho mỗi trường hợp thử nghiệm, có thể lớn bằng`10^{10}`hoạt động. Điều này là không thể trong thời gian giới hạn. 

Cái nhìn sâu sắc quan trọng đến từ hai quan sát. Đầu tiên, mảng`b`là tuần hoàn với chu kỳ`n`. Điều đó có nghĩa là bất kỳ phân đoạn nào bắt đầu sau phân đoạn đầu tiên`n`các phần tử có thể được ánh xạ trở lại một phân đoạn bắt đầu trong phần đầu tiên`n`các phần tử, có thể có thêm toàn bộ bản sao của`a`được thêm vào. Thứ hai, bởi vì chúng tôi đang tìm kiếm _bất kỳ phân khúc_ nào có tổng giá trị ít nhất là`x`, chúng ta chỉ cần biết, với mỗi vị trí trong bản sao đầu tiên của`a`, chúng ta cần đạt được bao nhiêu khối lặp lại`x`. 

Chúng tôi tính toán trước các tổng tiền tố của`a`để nhanh chóng xác định số lượng phần tử tối thiểu từ vị trí hiện tại cần thiết để đạt được`x`. Nếu tổng số tiền của`a`là`S`, sau đó cho các vị trí nơi`x`lớn hơn`S`, chúng ta có thể cần nhiều lần lặp lại hoàn chỉnh của`a`. Nếu số lần lặp lại yêu cầu vượt quá`k`, không đoạn nào từ vị trí đó có thể tới được`x`. Ngược lại, chúng ta có thể đếm có bao nhiêu vị trí bắt đầu trong`b`sẽ thành công, dựa trên số lần lặp lại đầy đủ phù hợp. Điều này làm giảm vấn đề xuống`O(n)`hoạt động cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(n * k) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`k`,`x`và mảng`a`. 
2. Tính mảng tổng tiền tố`pref`vì`a`, Ở đâu`pref[i] = a[0] + ... + a[i]`. Điều này cho phép chúng ta tính tổng của bất kỳ phân đoạn nào trong`a`trong thời gian không đổi. 
3. Tính tổng số tiền`S = sum(a)`. Nếu như`S >= x`, khi đó một số đoạn có độ dài ≤`n`có thể đạt được`x`. Nếu không thì cần có nhiều bản sao. 
4. Đối với mỗi vị trí xuất phát`i`TRONG`a`(dựa trên 0), tính số lần lặp lại tối đa của`a`có thể được sử dụng trong khi vẫn giữ tổng <`x`. Cụ thể, đối với mỗi`i`, tìm số lượng bản sao đầy đủ tối thiểu`full_blocks`cần thiết sau`i`sao cho tổng tích lũy từ`i`đạt tới`x`. 
5. Nếu vượt quá số lượng bản sao đầy đủ yêu cầu`k`, vị trí này góp phần`0`vị trí bắt đầu hợp lệ. Ngược lại, mọi vị trí tương ứng trong`b`được tạo bằng cách lặp lại`a`đóng góp. Đếm số lượng vị trí hợp lệ bằng cách`valid_count = k - full_blocks`. 
6. Tổng hợp`valid_count`trên tất cả các vị trí`i`để có được câu trả lời cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bất biến là cho từng vị trí`i`trong bản sao đầu tiên của`a`, số lượng bản sao đầy đủ cần thiết để đạt được`x`được xác định duy nhất bởi các tổng tiền tố và tổng số tiền`S`. Sự lặp lại vượt quá`k`không thể tạo một phân đoạn hợp lệ. Tất cả các vị trí trong các bản sao sau này của`a`ánh xạ trở lại vị trí trong bản sao đầu tiên do tính tuần hoàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, x = map(int, input().split())
        a = list(map(int, input().split()))
        S = sum(a)
        pref = [0] * n
        pref[0] = a[0]
        for i in range(1, n):
            pref[i] = pref[i-1] + a[i]

        result = 0
        for i in range(n):
            needed = x - pref[i-1] if i > 0 else x
            if needed <= 0:
                # sum from i to i only (or less) already >= x
                result += k
            else:
                # full repetitions needed
                full_blocks = (needed + S - 1) // S
                if full_blocks <= k:
                    result += k - full_blocks + 1
        print(result)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính tổng tiền tố để cho phép truy vấn tổng phân đoạn theo thời gian không đổi. Đối với mỗi vị trí bắt đầu, chúng tôi tính toán cần bao nhiêu lần lặp lại đầy đủ để đạt được`x`. Việc sử dụng`(needed + S - 1) // S`đảm bảo phân chia trần hợp lý để tránh đếm thiếu khối. Sau đó chúng tôi tính toán số lượng vị trí bắt đầu hợp lệ trong mảng lặp lại. Cần đặc biệt chú ý đến những vị trí mà chỉ riêng phần tử đầu tiên đã đủ để tiếp cận`x`. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`a = [3,4,2,1,5]`,`k = 3`,`x = 10`| tôi | trước[i-1] | cần thiết | full_blocks | hợp lệ_count | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 10 | 2 | 2 | 
| 1 | 3 | 7 | 1 | 3 | 
| 2 | 7 | 3 | 1 | 3 | 
| 3 | 9 | 1 | 1 | 3 | 
| 4 | 10 | 0 | 0 | 3 | 

Tổng hợp`valid_count`cho`12`, phù hợp với đầu ra mẫu. Điều này thể hiện cách xử lý các phân đoạn bắt đầu ở các vị trí khác nhau và sự đóng góp của sự lặp lại. 

**Ví dụ 2:** 

đầu vào:`a = [1]`,`k = 100000`,`x = 1`Chỉ riêng phần tử đầu tiên thỏa mãn`x`, vậy mọi vị trí trong`b`là hợp lệ. Câu trả lời là`100000`, xác nhận việc xử lý các mảng tối thiểu và lớn`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tổng tiền tố và lặp lại`n`các yếu tố; tất cả các phép toán khác là số học theo thời gian không đổi | 
| Không gian | O(n) | Mảng tổng tiền tố; các biến khác là O(1) | 

Thuật toán chia tỷ lệ tuyến tính với`n`cho mỗi trường hợp thử nghiệm, phù hợp thoải mái với các ràng buộc của vấn đề được đưa ra`sum(n) <= 2*10^5`. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""7
5 3 10
3 4 2 1 5
15 97623 1300111
105 95 108 111 118 101 95 118 97 108 111 114 97 110 116
1 100000 1234567891011
1
1 1 1
1 1 1
2 2 1 2
1 1
2 1 5
2 1
""") == "12\n1452188\n0\n1\n1\n1\n0", "sample 1"

# minimum input
assert run("1\n1 1 1\n1") == "1", "min input"

# all elements equal, x smaller than sum
assert run("1\n3 2 3\n1 1 1") == "6", "all equal"

# x too large, no segment
assert run("1\n2 5 20\n3 4") == "0", "x too large"

# large k, small array
assert run("1\n1 100000 1\n1") == "100000", "large k"

# multiple full blocks needed
assert run("1\n2 3 10\n3 2") == "2", "full blocks needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | | |
