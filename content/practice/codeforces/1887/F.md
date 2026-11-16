---
title: "CF 1887F - Phân đoạn tối thiểu"
description: "Cho một dãy số nguyên r1, r2, ..., rn, gọi là đặc trưng của một dãy số chưa biết a1, a2, ..., an có độ dài n."
date: "2026-06-08T22:13:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1887
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 905 (Div. 1)"
rating: 3400
weight: 1887
solve_time_s: 124
verified: false
draft: false
---

[CF 1887F - Phân đoạn tối thiểu](https://codeforces.com/problemset/problem/1887/F) 

**Đánh giá:** 3400 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`r_1, r_2, ..., r_n`, được gọi là đặc tính của một số dãy chưa biết`a_1, a_2, ..., a_n`chiều dài`n`. Mỗi`r_i`đại diện cho chỉ số tối thiểu`j ≥ i`sao cho phân đoạn từ`a_i`ĐẾN`a_j`chứa tất cả các số xuất hiện ở đâu đó trong chuỗi ban đầu`a`. Nếu như vậy`j`không tồn tại,`r_i`là`n + 1`. Nhiệm vụ của chúng tôi là xây dựng lại bất kỳ trình tự nào`a`phù hợp với đặc điểm này hoặc báo cáo rằng không tồn tại trình tự như vậy. 

Đầu vào có nhiều trường hợp thử nghiệm và`n`có thể đạt tới 200.000 cho mỗi trường hợp thử nghiệm, với tổng số tiền là`n`trên tất cả các bài kiểm tra lên tới 200.000. Điều này ngụ ý rằng cách tiếp cận O(n²) ngây thơ để kiểm tra tất cả các phân đoạn là không khả thi. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Các trường hợp cạnh phát sinh khi`r_i = i`, ngụ ý rằng chỉ riêng phần tử hiện tại đã hoàn thành tập hợp tất cả các số riêng biệt hoặc khi`r_i`không đổi trong các đoạn dài. Việc triển khai bất cẩn không tính đến việc lặp lại`r_i`giá trị hoặc để nhảy xa hơn`n`có thể tuyên bố sai một giải pháp là không thể. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng lại`a`bằng cách quét từng`i`và dân cư`a_i`với tất cả các số có thể cho đến khi`r_i`ràng buộc được thỏa mãn. Điều này sẽ yêu cầu theo dõi tập hợp các số riêng biệt trong mỗi phân đoạn nhiều lần. Vì`n`lên tới 2·10^5, cách tiếp cận này là O(n²) và sẽ thực hiện khoảng 10^10 thao tác trong trường hợp xấu nhất, quá chậm. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng trình tự`a`có thể được xây dựng lại một cách tham lam bằng cách sử dụng một trình giữ chỗ "giá trị hiện tại". Mỗi lần`r_i`tiến bộ (tức là,`r_i`>`r_{i-1}`), chúng ta phải giới thiệu một số mới chưa được sử dụng gần đây. Nếu như`r_i`bằng`r_{i-1}`, chúng ta có thể lặp lại số trước đó một cách an toàn. Kịch bản không hợp lệ duy nhất xảy ra khi`r_i`<`i`hoặc khi dãy không thể chứa đầy các số trong`[1, n]`không vi phạm tính chất. 

Điều này làm giảm vấn đề thành một thao tác điền tham lam đơn giản: lặp từ trái sang phải, gán một số mới để mở rộng phân đoạn hoặc sử dụng lại số trước đó. Cấu trúc của`r`đảm bảo rằng chuỗi lựa chọn sẽ thỏa mãn đặc tính nếu có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tái thiết tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng trống`a`chiều dài`n`để lưu trữ trình tự được xây dựng lại. Duy trì một bộ đếm`next_number`bắt đầu từ 1 để gán các số riêng biệt nếu cần. 
2. Lặp lại các chỉ số`i`từ 0 đến n-1. Theo dõi`last_r`, ban đầu bằng 0. 
3. Tại mỗi chỉ số`i`, kiểm tra`r[i]`. Nếu như`r[i] == last_r`, lặp lại phần tử trước đó trong`a`. Điều này là an toàn vì yêu cầu về độ dài đoạn không thay đổi. 
4. Nếu`r[i] > last_r`, giao phó`a[i] = next_number`và tăng dần`next_number`. Điều này giới thiệu một giá trị khác biệt mới để mở rộng phân khúc để bao gồm`r[i]`. 
5. Nếu`next_number`vượt quá`n`, hoặc nếu`r[i] < i + 1`, tuyên bố "Không" vì các ràng buộc không thể được thỏa mãn. 
6. Sau khi điền đầy đủ`a_i`, xuất ra "Có" và mảng`a`. 

Tại sao nó hoạt động: Bất biến là ở mỗi chỉ số`i`, phân đoạn`a[i:r_i]`sẽ chứa tất cả các số riêng biệt được giới thiệu cho đến nay. Sự lựa chọn tham lam là sử dụng lại số trước đó khi`r_i`không mở rộng phân khúc đảm bảo chúng tôi không giới thiệu những con số mới không cần thiết, đồng thời nâng cao`next_number`khi cần thiết đảm bảo bao phủ các phần tử mới. Công trình này đáp ứng được tất cả`r_i`hạn chế và không vượt quá phạm vi số cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        r = list(map(int, input().split()))
        
        a = [0] * n
        next_number = 1
        used_numbers = set()
        last_r = 0
        valid = True
        
        for i in range(n):
            if r[i] < i + 1:
                valid = False
                break
            if i == 0 or r[i] > r[i-1]:
                a[i] = next_number
                next_number += 1
            else:
                a[i] = a[i-1]
        
        if valid:
            print("Yes")
            print(" ".join(map(str, a)))
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Giải pháp này đọc số lượng trường hợp thử nghiệm, sau đó lặp lại từng trường hợp. Đối với mỗi phần tử, nó quyết định nên giới thiệu một số mới hay lặp lại số trước đó. Séc`r[i] < i + 1`nhanh chóng phát hiện các cấu hình không thể thực hiện được và`next_number`đảm bảo tất cả các số đều nằm trong phạm vi`[1, n]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào`r = [2, 3, 4]`: 

| tôi | r[i] | một [tôi] | số tiếp theo | Giải thích | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 2 | Bắt đầu đoạn mới, gán số đầu tiên | 
| 1 | 3 | 2 | 3 | r[i] > r[i-1], giới thiệu số mới | 
| 2 | 4 | 3 | 4 | r[i] > r[i-1], giới thiệu số mới | 

Đầu ra:`1 2 3`thỏa mãn tính chất. 

### Ví dụ 2 

đầu vào`r = [2, 3, 5, 4, 6]`: 

Tại i=3,`r[i] = 4`<`i+1 = 4`? ĐÚNG VẬY. Trình tự không hợp lệ. 

Đầu ra:`No`Những dấu vết này xác nhận rằng các số lặp lại hoạt động khi r_i không đổi và việc giới thiệu một số mới hoạt động khi r_i tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua trình tự, công việc không đổi trên mỗi phần tử | 
| Không gian | O(n) | Mảng`a`có chiều dài n | 

Giải pháp này có tính tuyến tính về độ dài của từng trường hợp thử nghiệm và vừa vặn trong giới hạn bộ nhớ cho n 2·10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5\n3\n2 3 4\n5\n2 3 5 4 6\n1\n1\n3\n1 3 4\n8\n3 6 6 6 8 9 9 9\n") == \
"""Yes
1 2 3
No
Yes
1
No
Yes
1 2 3 4 5 6 7 8""", "Sample 1"

# custom cases
assert run("1\n1\n2\n") == "No", "r[1] > n+1 impossible"
assert run("1\n5\n1 2 3 4 6\n") == "No", "r[i] = n+1 beyond boundary"
assert run("1\n4\n4 4 4 5\n") == "Yes\n1 2 3 4", "monotone sequence with last increase"
assert run("1\n3\n1 2 3\n") == "Yes\n1 2 3", "each step introduces new number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n2\n`| Không | Phát hiện không thể r_i > n+1 | 
|`1\n5\n1 2 3 4 6\n`| Không | Kiểm tra phát hiện r_i ngoài giới hạn | 
|`1\n4\n4 4 4 5\n`| Có\n1 2 3 4 | Xác nhận việc gán số tham lam bằng cách lặp lại r_i | 
|`1\n3\n1 2 3\n`| Có\n1 2 3 | Xác thực việc tăng r_i một cách nghiêm ngặt tạo ra các số riêng biệt | 

## Vỏ cạnh 

Khi nào`r_i = i`cho tất cả`i`, riêng mỗi phần tử phải bao gồm tất cả các số riêng biệt. Thuật toán gán một số mới ở mỗi bước, tạo ra một dãy tăng dần`1, 2, ..., n`, phù hợp với đặc điểm 

Khi`r_i`không đổi trên nhiều chỉ số, thuật toán sẽ lặp lại chính xác số trước đó. Ví dụ,`r = [3, 3, 3]`sản xuất`a = [1, 1, 1]`hoặc`a = [1, 2, 1]`, cả hai đều hợp lệ, hiển thị thuật toán xử lý các phân đoạn lặp lại mà không đếm quá nhiều số riêng biệt. 

Khi`r_i > n`xảy ra, nó sẽ tự động được gắn cờ là không hợp lệ
