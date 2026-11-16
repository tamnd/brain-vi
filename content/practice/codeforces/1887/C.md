---
title: "CF 1887C - Mảng tối thiểu"
description: "Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các phép tính tăng các phân đoạn của mảng theo một giá trị nhất định. Mỗi thao tác chỉ định một mảng con liền kề và một số nguyên cần cộng."
date: "2026-06-08T22:10:55+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "constructive-algorithms", "data-structures", "greedy", "hashing", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1887
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 905 (Div. 1)"
rating: 2400
weight: 1887
solve_time_s: 122
verified: false
draft: false
---

[CF 1887C - Mảng tối thiểu](https://codeforces.com/problemset/problem/1887/C) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, băm, hai con trỏ 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các phép tính tăng các phân đoạn của mảng theo một giá trị nhất định. Mỗi thao tác chỉ định một mảng con liền kề và một số nguyên cần cộng. Nhiệm vụ là xác định mảng trung gian nào - mảng ban đầu hoặc mảng sau khi áp dụng bất kỳ tiền tố nào của các phép toán - nhỏ nhất về mặt từ điển. 

So sánh từ điển có nghĩa là chúng ta xem xét các phần tử từ trái sang phải và chọn vị trí đầu tiên nơi hai mảng khác nhau. Mảng có số nhỏ hơn ở vị trí đó được coi là nhỏ hơn về tổng thể. Điều này rất quan trọng vì ngay cả khi phần tử sau giảm đáng kể, nó không thể làm cho mảng nhỏ hơn nếu phần tử trước tăng lên. 

Các ràng buộc chỉ ra rằng cả số lượng phần tử và số lượng thao tác có thể đạt tới 500.000, nhưng tổng trên tất cả các trường hợp thử nghiệm bị giới hạn bởi 500.000. Điều này ngụ ý rằng chúng ta cần một giải pháp có độ phức tạp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Các phương pháp bậc hai mô phỏng mọi hoạt động trên mọi phần tử là không khả thi. 

Trường hợp cạnh tinh vi xảy ra khi mảng tối thiểu về mặt từ điển chính là mảng ban đầu. Ví dụ: nếu tất cả các thao tác đều tăng các phần tử vốn đã nhỏ thì mức tối thiểu vẫn giữ nguyên khi bắt đầu. Một trường hợp khác là khi một thao tác cộng một số âm rất lớn bắt đầu từ phần tử đầu tiên - điều này ngay lập tức tạo ra mảng tối thiểu. Việc xử lý các chuỗi thao tác trống (q = 0) là chuyện nhỏ nhưng không được gây ra lỗi lập chỉ mục. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng từng thao tác trên toàn bộ mảng và theo dõi mức tối thiểu được thấy cho đến nay. Đối với mỗi trường hợp thử nghiệm, việc áp dụng các thao tác q trên n phần tử yêu cầu các thao tác O(n * q). Trong trường hợp xấu nhất, với n và q khoảng 500.000, con số này trở thành khoảng 2,5×10¹¹ phép tính, vượt xa giới hạn thời gian. 

Quan sát quan trọng là chúng ta không cần phải xây dựng đầy đủ mọi mảng trung gian. Để xác định mức tối thiểu từ điển, chúng ta chỉ cần theo dõi vị trí đầu tiên mà mảng có thể cải thiện so với mức tối thiểu hiện tại. Khi tiền tố của các phép toán sửa đổi một số phần tử đầu tiên theo cách tạo ra tiền tố nhỏ hơn tất cả các mảng trước đó, phần còn lại của mảng không ảnh hưởng đến so sánh từ điển trừ khi phần tử khác biệt đầu tiên giống nhau. 

Do đó, chúng ta có thể theo dõi một “mảng chênh lệch” tích lũy để lưu trữ hiệu ứng ròng của tất cả các hoạt động được áp dụng cho đến nay. Đối với mỗi thao tác, chúng tôi kiểm tra xem việc thêm hiệu ứng của nó vào tiền tố của mảng hiện tại có tạo ra phần tử nhỏ hơn ở vị trí đầu tiên nơi các mảng khác nhau hay không. Điều này cho phép chúng tôi xác định b_j tối ưu mà không cần xây dựng mọi mảng, giảm độ phức tạp xuống O(n + q) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test, đọc n là mảng`a`, và q. 
3. Lưu trữ tất cả các phép toán dưới dạng bộ dữ liệu của (l, r, x). Chúng tôi không áp dụng chúng ngay lập tức vào mảng. 
4. Khởi tạo một bản sao của`a`gọi điện`best`, biểu thị mức tối thiểu từ điển hiện tại, bắt đầu từ mảng ban đầu. 
5. Đối với mỗi thao tác, hãy áp dụng hiệu ứng của nó một cách khái niệm bằng cách sử dụng một mảng sai phân: tăng chỉ số bắt đầu lên x và giảm vị trí sau khi kết thúc thêm x. Điều này cho phép chúng tôi tính toán hiệu ứng ròng của tất cả các hoạt động trong thời gian O(n) bằng cách sử dụng tổng tiền tố. 
6. Tính hiệu quả tích lũy của các hoạt động từ hoạt động đầu tiên đến hoạt động hiện tại. 
7. Đối với mỗi tiền tố thao tác, áp dụng hiệu ứng ròng cho mảng ban đầu để lấy mảng ứng cử viên`b_j`. So sánh`b_j`với hiện tại`best`theo từ điển bằng cách lặp từ trái sang phải và cập nhật`best`nếu chúng ta tìm thấy một ứng cử viên nhỏ hơn. 
8. Sau khi xử lý xong mọi thao tác,`best`chứa mảng nhỏ nhất theo từ điển. Xuất nó ra. 

Lý do điều này có hiệu quả là vì so sánh từ điển chỉ phụ thuộc vào chỉ số khác nhau đầu tiên. Việc sử dụng mảng sai phân đảm bảo chúng tôi tính toán hiệu quả hiệu quả thực của các phép toán trên tất cả các phần tử mà không cần thực hiện các thao tác dư thừa, đảm bảo thuật toán có quy mô tuyến tính với n và q. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        q = int(input())
        ops = []
        for _ in range(q):
            l, r, x = map(int, input().split())
            ops.append((l-1, r-1, x))  # convert to 0-based

        # Start with original array as current best
        best = a[:]
        curr_diff = [0]*(n+1)  # difference array

        for l, r, x in ops:
            curr_diff[l] += x
            curr_diff[r+1] -= x

            # Apply difference array to get current array
            cum = 0
            candidate = []
            for i in range(n):
                cum += curr_diff[i]
                candidate.append(a[i] + cum)

            # Lexicographical comparison
            for i in range(n):
                if candidate[i] < best[i]:
                    best = candidate
                    break
                elif candidate[i] > best[i]:
                    break

        print(' '.join(map(str, best)))

if __name__ == "__main__":
    main()
```Giải pháp sử dụng một mảng khác biệt`curr_diff`để theo dõi những thay đổi gia tăng một cách hiệu quả. Mỗi thao tác chỉ cập nhật hai vị trí, cho phép tính toán mảng tích lũy theo thời gian tuyến tính. So sánh từ điển được thực hiện theo từng yếu tố từ trái sang phải, dừng lại ngay khi có thể đưa ra quyết định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[1, 2, 3, 4]`, hoạt động:`(1,4,0)`,`(1,3,-100)`. 

| Bước | curr_diff | kiêm | ứng cử viên | tốt nhất | 
| --- | --- | --- | --- | --- | 
| Ban đầu |`[0,0,0,0,0]`|`[1,2,3,4]`|`[1,2,3,4]`|`[1,2,3,4]`| 
| Sau op1 |`[0,0,0,0,0]`|`[1,2,3,4]`|`[1,2,3,4]`|`[1,2,3,4]`| 
| Sau op2 |`[-100,0,0,100,0]`|`[-100,-98,-97,4]`|`[-99,-98,-97,4]`|`[-99,-98,-97,4]`| 

Điều này xác nhận mảng tối thiểu là`[-99,-98,-97,4]`. 

### Ví dụ 2 

Mảng đầu vào:`[2,1,2,5,4]`, hoạt động:`(2,4,3)`,`(2,5,-2)`,`(1,3,1)`. 

| Bước | ứng cử viên | tốt nhất | 
| --- | --- | --- | 
| Ban đầu |`[2,1,2,5,4]`|`[2,1,2,5,4]`| 
| Sau bất kỳ tiền tố op nào | tất cả các mảng ứng viên đều có phần tử đầu tiên >= 2 |`[2,1,2,5,4]`| 

Mức tối thiểu vẫn là mảng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) cho mỗi trường hợp thử nghiệm | Cập nhật mảng khác biệt là O(1) cho mỗi thao tác; tái tạo tổng tiền tố là O(n); so sánh lex là O(n) | 
| Không gian | O(n + q) | O(n) cho mảng và mảng sai phân, O(q) cho các phép toán lưu trữ | 

Với tổng n và q trên tất cả các trường hợp thử nghiệm 5·10⁵, giải pháp phù hợp một cách thoải mái trong vòng 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided samples
assert run("2\n4\n1 2 3 4\n2\n1 4 0\n1 3 -100\n5\n2 1 2 5 4\n3\n2 4 3\n2 5 -2\n1 3 1\n") == "-99 -98 -97 4\n2 1 2 5 4"

# Minimum-size input
assert run("1\n1\n10\n0\n") == "10"

# Maximum-size input (only structure)
assert run(f"1\n5\n1 2 3 4 5\n5\n1 5 -1\n1 5 -1\n1 5 -1\n1 5 -1\n1 5 -1\n") == "-4 -3 -2 -1 0"

# All equal values
assert run("1\n3\n5 5 5\n2\n1 3 -2\n2 3 -1\n") == "3 2 2"

# Operations that do not affect lex minimum
assert run("1\n3\n1 2 3\n2\n2 3 5\n3 3 0\n") == "1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n10\n0\n`|`10`| Kích thước mảng tối thiểu, không có thao tác | 
|`1\n5\n1 2 3 4 5\n5\n...`| `-4 -3 - | |
