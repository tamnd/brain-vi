---
title: "CF 279C - Thang"
description: "Chúng ta được cung cấp một mảng các số nguyên một chiều và một danh sách các truy vấn, mỗi truy vấn chỉ định một phân đoạn liền kề của mảng. Đối với mỗi truy vấn, chúng ta cần xác định xem phân đoạn con có tạo thành \"thang hay không."
date: "2026-06-05T05:50:51+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 279
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 171 (Div. 2)"
rating: 1700
weight: 279
solve_time_s: 87
verified: true
draft: false
---

[CF 279C - Thang](https://codeforces.com/problemset/problem/279/C) 

**Đánh giá:** 1700 
**Tags:** dp, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên một chiều và một danh sách các truy vấn, mỗi truy vấn chỉ định một phân đoạn liền kề của mảng. Đối với mỗi truy vấn, chúng ta cần xác định xem phân đoạn con có tạo thành một "bậc thang" hay không. Thang là một dãy tăng đơn điệu (không giảm) cho đến một điểm nào đó và sau đó giảm đơn điệu (không tăng) sau điểm đó. Điều này có nghĩa là dãy có thể hoàn toàn không giảm, hoàn toàn không tăng hoặc có một đỉnh duy nhất nơi nó chuyển từ không giảm sang không tăng. Đầu ra là "Có" nếu phân đoạn con là bậc thang và "Không" nếu ngược lại. 

Các ràng buộc cho chúng ta biết rằng cả kích thước mảng và số lượng truy vấn đều có thể lên tới 100.000. Một cách tiếp cận đơn giản quét từng phân đoạn riêng lẻ cho mỗi truy vấn sẽ yêu cầu kiểm tra tối đa 10^10 phần tử trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong 2 giây. Điều này ngụ ý rằng chúng ta cần một cách tiếp cận xử lý trước mảng để trả lời các truy vấn theo thời gian không đổi hoặc logarit. 

Các trường hợp cạnh phát sinh khi các chuỗi phẳng, một phần tử hoặc tăng hoặc giảm nghiêm ngặt. Ví dụ, một mảng`[5, 5, 5]`sẽ trả về "Có" cho bất kỳ phân đoạn nào, vì nó không giảm và không tăng một cách tầm thường. Phân đoạn một phần tử`[7]`cũng là một cái thang. Các vấn đề xuất hiện khi việc thực hiện bất cẩn giả định tăng hoặc giảm nghiêm ngặt thay vì so sánh không nghiêm ngặt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp lại từng truy vấn và kiểm tra trực tiếp phân đoạn tương ứng. Đối với mỗi phân đoạn, nó quét từ trái sang phải để tìm tiền tố không giảm dài nhất rồi quét từ cuối để tìm hậu tố không tăng dài nhất. Nếu hai vùng này bao phủ toàn bộ phân đoạn thì đó là một thang. Cách tiếp cận này hoạt động chính xác nhưng có độ phức tạp về thời gian là O(n * m) trong trường hợp xấu nhất, quá chậm so với các ràng buộc đã cho. 

Thông tin chi tiết quan trọng cần tối ưu hóa là chúng ta có thể xử lý trước mảng một lần để biết, đối với mỗi chỉ mục, các phân đoạn không giảm và không tăng sẽ mở rộng bao xa. Cụ thể, chúng ta có thể tính toán cho từng vị trí chỉ số xa nhất có thể tiếp cận bằng cách di chuyển sang phải trong khi không giảm (`up[i]`) và chỉ số xa nhất có thể truy cập bằng cách di chuyển sang phải trong khi không tăng (`down[i]`). Sau đó, đối với một truy vấn`[l, r]`, nếu như`down[up[l]] >= r`, đoạn này tạo thành một cái thang. Điều này hoạt động vì`up[l]`tìm điểm cuối của phần không giảm ban đầu bắt đầu từ`l`, Và`down[up[l]]`mở rộng phần không tăng từ đó. Quá trình tiền xử lý chạy theo thời gian tuyến tính và mỗi truy vấn được trả lời theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`up`chiều dài`n`để lưu trữ chỉ số xa nhất có thể truy cập được từ mỗi vị trí theo trình tự không giảm. Bắt đầu từ phần tử thứ hai đến cuối cùng và di chuyển sang trái, cập nhật`up[i]`ĐẾN`i+1`nếu như`a[i] <= a[i+1]`, nếu không thì`up[i] = i`. 
2. Khởi tạo một mảng`down`chiều dài`n`để lưu trữ chỉ số xa nhất có thể truy cập được từ mỗi vị trí theo trình tự không tăng. Bắt đầu từ phần tử thứ hai đến cuối cùng và di chuyển sang trái, cập nhật`down[i]`ĐẾN`i+1`nếu như`a[i] >= a[i+1]`, nếu không thì`down[i] = i`. 
3. Đối với mỗi truy vấn`[l, r]`, kiểm tra xem`down[up[l-1]] >= r-1`. Chúng tôi trừ đi một vì chỉ số mảng trong Python dựa trên 0. Nếu đúng thì in ra "Có"; nếu không thì in "Không". Điều này hoạt động vì`up[l-1]`đưa ra điểm cuối của đoạn không giảm bắt đầu từ`l-1`, Và`down[up[l-1]]`kéo dài phần không tăng từ đỉnh đó. Nếu nó đạt tới hoặc vượt qua`r-1`, phân đoạn này là một bậc thang. 

Tại sao nó hoạt động: Bất biến là vậy`up[i]`xác định chính xác mức độ tối đa của chuỗi không giảm bắt đầu từ`i`, Và`down[i]`thực hiện tương tự đối với dãy không tăng. Bằng cách kết hợp hai mảng được tính toán trước này, chúng tôi có thể xác minh theo thời gian không đổi xem phân đoạn con có thỏa mãn thuộc tính bậc thang hay không mà không cần quét từng phần tử nhiều lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

up = [0] * n
down = [0] * n

up[-1] = n - 1
for i in range(n - 2, -1, -1):
    if a[i] <= a[i + 1]:
        up[i] = up[i + 1]
    else:
        up[i] = i

down[-1] = n - 1
for i in range(n - 2, -1, -1):
    if a[i] >= a[i + 1]:
        down[i] = down[i + 1]
    else:
        down[i] = i

for _ in range(m):
    l, r = map(int, input().split())
    l -= 1
    r -= 1
    if down[up[l]] >= r:
        print("Yes")
    else:
        print("No")
```Cấu trúc vòng lặp đầu tiên`up`bằng cách quét từ phải sang trái, đảm bảo rằng mỗi chỉ mục biết phân khúc không giảm xa nhất mà nó có thể tiếp cận. Cấu trúc vòng lặp thứ hai`down`tương tự đối với dãy không tăng. Quá trình xử lý truy vấn sử dụng trực tiếp các mảng này để xác định tính hợp lệ của bậc thang trong thời gian không đổi cho mỗi truy vấn. Việc điều chỉnh lập chỉ mục cho mảng Python dựa trên 0 là nguồn lỗi phổ biến, nhưng ở đây`l-1`Và`r-1`xử lý nó đúng cách. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`1 2 1 3 3 5 2 1`, Truy vấn:`[1, 3]`| tôi | một [tôi] | lên[i] | xuống[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | 
| 1 | 2 | 1 | 2 | 
| 2 | 1 | 2 | 2 |`up[0] = 1`trỏ tới 2.`down[1] = 2`chỉ vào 1. Vì 2 >= 2 nên câu trả lời là "Có". 

Truy vấn`[2, 4]`:`up[1] = 4`(mở rộng đến 5),`down[4] = 4`(dừng ở số 3). Vì 4 < 3 là sai nên câu trả lời là "Không". Điều này chứng tỏ việc phát hiện chính xác một đoạn không có bậc thang. 

### Đoạn một phần tử 

đầu vào:`[7]`, Truy vấn:`[1,1]`.`up[0] = 0`,`down[0] = 0`. Từ`down[up[0]] = 0 >= 0`, đầu ra là "Có". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mảng tiền xử lý`up`Và`down`mất O (n). Mỗi truy vấn được trả lời trong O(1). | 
| Không gian | O(n) | Mảng`up`Và`down`lưu trữ một số nguyên cho mỗi phần tử. | 

Với các ràng buộc n, m ≤ 10^5, thuật toán này hiệu quả và phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    up = [0] * n
    down = [0] * n

    up[-1] = n - 1
    for i in range(n - 2, -1, -1):
        if a[i] <= a[i + 1]:
            up[i] = up[i + 1]
        else:
            up[i] = i

    down[-1] = n - 1
    for i in range(n - 2, -1, -1):
        if a[i] >= a[i + 1]:
            down[i] = down[i + 1]
        else:
            down[i] = i

    for _ in range(m):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        if down[up[l]] >= r:
            print("Yes")
        else:
            print("No")
    return output.getvalue().strip()

# provided sample
assert run("8 6\n1 2 1 3 3 5 2 1\n1 3\n2 3\n2 4\n8 8\n1 4\n5 8\n") == "Yes\nYes\nNo\nYes\nNo\nYes"

# custom cases
assert run("1 1\n5\n1 1\n") == "Yes", "single element"
assert run("3 3\n1 1 1\n1 3\n1 2\n2 3\n") == "Yes\nYes\nYes", "all equal
```
