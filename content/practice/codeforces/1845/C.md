---
title: "CF 1845C - Mật khẩu mạnh"
description: "Chúng tôi được yêu cầu xác định xem liệu Monocarp có thể tạo mật khẩu có độ dài chính xác m thỏa mãn ba điều kiện hay không: mỗi chữ số phải nằm giữa các chữ số tương ứng của hai chuỗi l và r, mật khẩu chỉ được sử dụng các chữ số từ 0 đến 9 và không được xuất hiện dưới dạng…"
date: "2026-06-09T05:55:02+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1845
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 151 (Rated for Div. 2)"
rating: 1400
weight: 1845
solve_time_s: 79
verified: false
draft: false
---

[CF 1845C - Mật khẩu mạnh](https://codeforces.com/problemset/problem/1845/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, dp, tham lam, chuỗi 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem Monocarp có thể tạo mật khẩu có độ dài chính xác hay không`m`thỏa mãn ba điều kiện: mỗi chữ số phải nằm giữa các chữ số tương ứng của hai chuỗi`l`Và`r`, mật khẩu chỉ được sử dụng chữ số`0`bởi vì`9`và nó không được xuất hiện dưới dạng một chuỗi con trong một chuỗi nhất định`s`đại diện cho cơ sở dữ liệu của mật khẩu hiện có. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định`s`,`m`,`l`, Và`r`. Đối với mỗi trường hợp, đầu ra là "CÓ" nếu tồn tại ít nhất một mật khẩu hợp lệ và "KHÔNG" nếu ngược lại. 

Thách thức chính đó là`s`có thể rất dài, lên tới 300.000 ký tự và có thể có tới 10.000 trường hợp thử nghiệm. Điều này loại trừ bất kỳ thuật toán nào kiểm tra tất cả các mật khẩu có thể có một cách riêng lẻ, vì ngay cả đối với những mật khẩu nhỏ`m`giới hạn là 10, có thể lên tới$10^{10}$ứng viên trong trường hợp xấu nhất. Khó khăn không rõ ràng nằm ở việc đảm bảo rằng mật khẩu đã chọn không xuất hiện dưới dạng một chuỗi trong`s`, đòi hỏi phải suy luận về tất cả các vị trí trong`s`mà không liệt kê chúng một cách rõ ràng. 

Một cách tiếp cận đơn giản có thể thử tạo ra tất cả các chuỗi trong phạm vi`[l_i, r_i]`cho mỗi vị trí và kiểm tra xem mỗi vị trí có phải là một dãy con của`s`. Ví dụ, nếu`s = "123412341234"`,`m = 3`,`l = "111"`, Và`r = "444"`, cách tiếp cận ngây thơ có thể lặp qua "111", "112", ..., "444". Điều này là không khả thi vì có$4^3 = 64$trình tự trong ví dụ nhỏ này, nhưng cách tiếp cận có quy mô kém đối với phạm vi lớn hơn hoặc`m = 10`. Hơn nữa, việc triển khai bất cẩn có thể xử lý sai việc kiểm tra trình tự con, đặc biệt khi các chữ số lặp lại hoặc khi`l`Và`r`bằng nhau ở một số vị trí. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra mọi ứng cử viên mật khẩu phù hợp với các ràng buộc về chữ số`[l_i, r_i]`cho từng vị trí và kiểm tra xem nó có xuất hiện dưới dạng một dãy con trong`s`. Kiểm tra mật khẩu theo trình tự sau yêu cầu quét`s`tuyến tính, vì vậy đối với một ứng cử viên có độ dài`m`và một chuỗi`s`chiều dài`n`, điều này cần$O(n)$thời gian. Vì số lượng ứng viên có thể lên tới$10^{10}$vì`m = 10`, cách tiếp cận này rõ ràng là không thực tế. 

Quan sát quan trọng là`m`là nhỏ, nhiều nhất là 10. Điều này cho phép chúng ta xử lý vấn đề như một tìm kiếm giới hạn trên các vị trí mật khẩu. Thay vì lặp đi lặp lại một cách mù quáng, chúng ta có thể sử dụng quy hoạch động để ghi lại vị trí nào trong`s`có thể khớp với một phần mật khẩu. Cụ thể, chúng tôi xử lý trước`s`để biết mọi vị trí và mọi chữ số`d`nơi xuất hiện tiếp theo của`d`là. Điều này cho phép chúng tôi tiến lên thông qua`s`nhanh chóng khi kiểm tra các dãy con, giảm bớt sự ngây thơ$O(n)$quét tới$O(1)$cho mỗi vị trí sau khi quá trình tiền xử lý được thực hiện. Bằng cách sử dụng tính năng quay lui đệ quy với tính năng ghi nhớ hoặc DFS trên các vị trí mật khẩu, chúng tôi khám phá phạm vi`[l_i, r_i]`cho mỗi ký tự và dừng sớm khi tìm thấy mật khẩu hợp lệ. 

Sự quan sát đó`m <= 10`và chữ số chỉ`0-9`là rất quan trọng. Nó cho phép tìm kiếm đệ quy trên`m`vị trí, cố gắng tối đa 10 chữ số cho mỗi vị trí, đưa ra phân nhánh trong trường hợp xấu nhất$10^{10}$, nhưng thực tế là quá trình tiền xử lý`s`loại bỏ hầu hết các con đường không thể một cách nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^m \cdot | s | )) | 
| Tối ưu (DP + DFS lần xuất hiện tiếp theo) | (O( | s | \cdot 10 + 10^m)) khấu hao | 

## Hướng dẫn thuật toán 

1. Tiền xử lý`s`vào bảng lần xuất hiện tiếp theo`next_pos[i][d]`Ở đâu`i`là một vị trí trong`s`Và`d`là một chữ số từ`0`ĐẾN`9`. Bảng này lưu trữ chỉ số sớm nhất ≥ i trong đó chữ số`d`xảy ra ở`s`. Chúng tôi xây dựng nó từ cuối`s`ngược lại, khởi tạo`next_pos[n][d] = n`(ngoài giới hạn) và cập nhật từng`i`với một trong hai`i`nếu như`s[i] == d`hoặc giá trị từ`i+1`. 
2. Định nghĩa hàm đệ quy`can_form(pos, s_index)`kiểm tra xem chúng tôi có thể hoàn thành mật khẩu bắt đầu từ vị trí mật khẩu hay không`pos`sử dụng`s`bắt đầu từ`s_index`. Nếu như`pos == m`, mật khẩu được tạo đầy đủ và không xuất hiện trong`s`, vì vậy hãy trả về True. Nếu không, lặp lại tất cả các chữ số`d`trong phạm vi`[l[pos], r[pos]]`. 
3. Với mỗi chữ số`d`, sử dụng`next_pos[s_index][d]`để xác định nơi sớm nhất`s_next`nơi chữ số này xuất hiện ở`s`. Nếu như`s_next`là vào cuối`s`, thì mật khẩu còn lại không thể xuất hiện dưới dạng một chuỗi con ngoài thời điểm này, nghĩa là chúng ta có thể đặt`d`một cách an toàn. 
4. Gọi đệ quy`can_form(pos+1, s_next+1)`. Nếu bất kỳ nhánh nào trả về True, câu trả lời là "CÓ". Ngược lại, sau khi khám phá tất cả các chữ số cho vị trí hiện tại, trả về Sai, nghĩa là không tồn tại mật khẩu hợp lệ. 
5. Chạy các bước trên cho từng trường hợp kiểm thử, in ra "CÓ" hoặc "KHÔNG". 

Tại sao nó hoạt động: Quá trình xử lý trước đảm bảo rằng đối với mọi vị trí mật khẩu, chúng tôi biết chính xác vị trí có thể khớp chữ số tương ứng trong`s`. Bằng cách khám phá đệ quy chỉ các chữ số khả thi và dừng khi mật khẩu được hoàn thành mà không khớp`s`, chúng tôi đảm bảo tính chính xác. Tính bất biến đó là`s_index`luôn trỏ đến tiền tố hiện tại của`s`có khả năng khớp với mật khẩu và bất kỳ việc hoàn thành thành công nào đều có nghĩa là mật khẩu không xuất hiện dưới dạng chuỗi tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        m = int(input())
        l = input().strip()
        r = input().strip()
        n = len(s)
        
        # Preprocess next occurrence
        next_pos = [[n]*10 for _ in range(n+1)]
        for i in range(n-1, -1, -1):
            for d in range(10):
                next_pos[i][d] = next_pos[i+1][d]
            next_pos[i][int(s[i])] = i
        
        memo = {}
        def can_form(pos, s_index):
            if pos == m:
                return True
            key = (pos, s_index)
            if key in memo:
                return memo[key]
            for d in range(int(l[pos]), int(r[pos])+1):
                next_i = next_pos[s_index][d]
                if next_i == n:
                    memo[key] = True
                    return True
                if can_form(pos+1, next_i+1):
                    memo[key] = True
                    return True
            memo[key] = False
            return False
        
        print("YES" if can_form(0, 0) else "NO")
```Giải pháp được cấu trúc để tiền xử lý đầu tiên`s`một cách hiệu quả, sau đó khám phá các khả năng đệ quy của mật khẩu đồng thời tận dụng khả năng ghi nhớ để tránh các tìm kiếm dư thừa. Các điều kiện biên, chẳng hạn như khi một chữ số không xuất hiện trong`s`, được xử lý chính xác bằng cách kiểm tra`next_i == n`. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| tư thế | s_index | chữ số đã thử | kết quả | 
| --- | --- | --- | --- | 
| 0 | 0 | 5,6 | tìm thấy mật khẩu "50" | 

Giải thích: Tại`pos=0`, chữ số`5`bản đồ tới`s_index=5`(lần xuất hiện đầu tiên của`5`). Tại`pos=1`, chữ số`0`bản đồ xa hơn`s`, vì vậy "50" là hợp lệ. 

**Mẫu 2** 

| tư thế | s_index | chữ số đã thử | kết quả | 
| --- | --- | --- | --- | 
| 0 | 0 | 1,2,3,4 | tất cả đều thất bại | 

Giải thích: Mỗi tổ hợp chữ số trong`[111,444]`xuất hiện dưới dạng một dãy con trong`s="123412341234"`, vì vậy không tồn tại mật khẩu hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O( | s | 

Giải pháp xử lý`|s| ≤ 3*10^5`Và`m ≤ 10`hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""5
88005553535123456
2
50
56
123412341234
3
111
444
1234
4
4321
4321
459
2
49
59
00010
2
10
11
""") == "YES\nNO\nYES\nNO\nYES"

# custom cases
assert run("""1
1111
3
```
