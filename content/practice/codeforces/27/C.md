---
title: "CF 27C - Dãy số không có thứ tự"
description: "Chúng ta được cho một dãy số nguyên và chúng ta được yêu cầu trích xuất dãy con ngắn nhất _không có thứ tự_. Một dãy được coi là có thứ tự nếu nó hoàn toàn không giảm hoặc hoàn toàn không tăng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 27
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 27 (Codeforces format, Div. 2)"
rating: 1900
weight: 27
solve_time_s: 78
verified: false
draft: false
---
[CF 27C - Dãy số không có thứ tự](https://codeforces.com/problemset/problem/27/C) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta được yêu cầu trích xuất dãy con ngắn nhất _không có thứ tự_. Một dãy được coi là có thứ tự nếu nó hoàn toàn không giảm hoặc hoàn toàn không tăng. Nói cách khác, nếu mọi cặp liên tiếp không bao giờ giảm hoặc không bao giờ tăng thì dãy được sắp xếp. Nhiệm vụ của chúng ta là tìm một dãy con tối thiểu phá vỡ mẫu này. 

Đầu vào bao gồm một số nguyên duy nhất$n$chỉ định độ dài chuỗi, theo sau là$n$số nguyên. Đầu ra phải báo cáo độ dài của chuỗi con không có thứ tự ngắn nhất (nếu có) và chỉ số dựa trên 1 của các phần tử của nó. 

Với$n$lên tới$10^5$và giới hạn thời gian 2 giây, bất kỳ giải pháp nào có độ phức tạp tuyến tính kém hơn đều có thể là quá chậm. MỘT$O(n^2)$việc tìm kiếm tàn bạo tất cả các bộ ba đã được thực hiện$10^{10}$hoạt động trong trường hợp xấu nhất, rõ ràng là không thể thực hiện được. Điều này cho thấy sự cần thiết của phương pháp quét một lần hoặc gần tuyến tính. 

Các trường hợp cạnh rất tinh tế. Nếu tất cả các phần tử đều bằng nhau thì không có dãy con không có thứ tự, do đó kết quả đầu ra phải bằng 0. Nếu dãy chỉ có một hoặc hai phần tử thì dãy con nào cũng được sắp xếp tầm thường. Chúng ta cũng cần xử lý nghiêm ngặt các chuỗi tăng hoặc giảm dường như có “biến thể” nhưng thực tế không phá vỡ tính đơn điệu. 

## Phương pháp tiếp cận 

Giải pháp đơn giản sẽ kiểm tra tất cả các dãy con có độ dài 3, kiểm tra xem chúng có được sắp thứ tự hay không. Điều này có tác dụng vì bất kỳ dãy con không có thứ tự nào cũng phải có ít nhất ba phần tử: hai phần tử luôn được sắp xếp. Kiểm tra từng bộ ba trong một mảng có độ dài$n$yêu cầu đại khái$n^3$những hoạt động hoàn toàn không thực tế đối với$n = 10^5$. 

Điểm mấu chốt là dãy con không có thứ tự ngắn nhất luôn có độ dài bằng 3. Giả sử chúng ta tìm thấy ba phần tử liên tiếp$a_i, a_j, a_k$Ở đâu$i < j < k$như vậy$a_i < a_j > a_k$hoặc$a_i > a_j < a_k$. Bất kỳ chuỗi không có thứ tự nào có nhiều hơn ba phần tử đều chứa một bộ ba như vậy. Vì vậy, chúng ta chỉ cần quét mảng một lần, kiểm tra từng bộ ba phần tử liên tiếp. Ngay khi một bộ ba vi phạm tính đơn điệu, chúng ta có thể xuất nó ngay lập tức. 

Quan sát này làm giảm độ phức tạp từ$O(n^3)$ĐẾN$O(n)$với không gian cố định. Chúng ta chỉ cần phần tử hiện tại và các phần tử lân cận của nó để phát hiện bộ ba không có thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dãy số nguyên thành mảng. 
2. Nếu độ dài chuỗi nhỏ hơn 3, xuất ngay 0. Hai yếu tố luôn được sắp xếp. 
3. Lặp lại mảng bằng chỉ mục$i$từ 1 đến$n-2$. Đối với mỗi bộ ba$(a[i-1], a[i], a[i+1])$, kiểm tra điều kiện đặt hàng. 
4. Nếu phần tử ở giữa lớn hơn cả hai phần tử lân cận ($a[i-1] < a[i] > a[i+1]$) hoặc hoàn toàn nhỏ hơn cả hai hàng xóm ($a[i-1] > a[i] < a[i+1]$), xuất ra độ dài bộ ba 3 và các chỉ số dựa trên 1 của chúng$i, i+1, i+2$. Chấm dứt ngay lập tức. 
5. Nếu vòng lặp kết thúc mà không tìm thấy bộ ba không có thứ tự nào, xuất ra 0. 

Tại sao nó hoạt động: Thuật toán được đảm bảo tìm ra chuỗi con không có thứ tự ngắn nhất vì bất kỳ chuỗi nào gồm ba phần tử không đơn điệu đều là tối thiểu. Việc quét các bộ ba liên tiếp đảm bảo chúng ta không bao giờ bỏ lỡ dãy con như vậy. Không có chuỗi nào có độ dài hai có thể vi phạm trật tự, vì vậy 3 là độ dài tối thiểu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

if n < 3:
    print(0)
else:
    found = False
    for i in range(1, n-1):
        if (a[i-1] < a[i] > a[i+1]) or (a[i-1] > a[i] < a[i+1]):
            print(3)
            print(i, i+1, i+2)
            found = True
            break
    if not found:
        print(0)
```Mã bắt đầu bằng cách xử lý các chuỗi có độ dài nhỏ hơn ba. Sau đó, nó kiểm tra từng bộ ba liên tiếp đúng một lần. Điều kiện này kiểm tra rõ ràng các thành tạo “đỉnh” hoặc “thung lũng”. Việc sử dụng lập chỉ mục dựa trên 1 phù hợp với yêu cầu của vấn đề. Một điểm tinh tế là sự bình đẳng không được tính là vi phạm trật tự: các điều kiện sử dụng các bất đẳng thức nghiêm ngặt. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
5
67 499 600 42 23
```| tôi | một[i-1] | một [tôi] | a[i+1] | Kiểm tra | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 67 | 499 | 600 | 67<499>600? không | tiếp tục | 
| 2 | 499 | 600 | 42 | 499<600>42 có | in 3, chỉ số 2,3,4 | 

Thuật toán xác định chính xác đỉnh ở mức 600 và đưa ra chuỗi con không có thứ tự tối thiểu. 

**Ví dụ 2** 

đầu vào:```
4
1 2 3 4
```| tôi | một[i-1] | một [tôi] | a[i+1] | Kiểm tra | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 1<2>3? không | tiếp tục | 
| 2 | 2 | 3 | 4 | 2<3>4? không | tiếp tục | 

Không có bộ ba nào vi phạm thứ tự nên kết quả là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bộ ba được kiểm tra một lần trong một lần. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng gồm n số nguyên; không cần cấu trúc bổ sung. | 

Với$n \le 10^5$Và$O(n)$hoạt động, giải pháp sẽ chạy thoải mái trong vòng 2 giây. Việc sử dụng bộ nhớ cũng nằm trong khoảng 256 MB, vì các số nguyên dễ dàng khớp với các mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())  # assume solution code above is saved in solution.py
    return sys.stdout.getvalue().strip()

# provided sample
assert run("5\n67 499 600 42 23\n") == "3\n2 3 4"

# all increasing
assert run("4\n1 2 3 4\n") == "0"

# all decreasing
assert run("3\n5 3 1\n") == "0"

# single element
assert run("1\n42\n") == "0"

# peak in middle
assert run("5\n1 3 2 4 5\n") == "3\n2 3 4"

# valley in middle
assert run("5\n5 2 4 6 7\n") == "3\n1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 67 499 600 42 23 | 3 2 3 4 | phát hiện đỉnh | 
| 4 1 2 3 4 | 0 | trình tự tăng chặt chẽ | 
| 3 5 3 1 | 0 | dãy giảm dần | 
| 1 42 | 0 | trường hợp cạnh đơn phần tử | 
| 5 1 3 2 4 5 | 3 2 3 4 | dãy con không có thứ tự với đỉnh | 
| 5 5 2 4 6 7 | 3 1 2 3 | dãy con không có thứ tự với thung lũng | 

## Vỏ cạnh 

Đối với một chuỗi có các phần tử lặp lại, ví dụ:`2 2 2`, thuật toán kiểm tra các bộ ba liên tiếp. Không có bất đẳng thức nghiêm ngặt nào được thỏa mãn nên kết quả là 0. Đối với các chuỗi có độ dài 2, chẳng hạn như`1 2`, kiểm tra ban đầu ngay lập tức trả về 0. Đối với các chuỗi có chuỗi con không có thứ tự tối thiểu ở đầu, giữa hoặc cuối, thuật toán luôn xác định bộ ba đầu tiên gặp phải. 

Bài xã luận này sẽ cung cấp cho người đọc đủ ngữ cảnh để lý giải từ những nguyên tắc đầu tiên rằng dãy con không có thứ tự ngắn nhất luôn có độ dài ba và có thể được phát hiện bằng một lần quét tuyến tính.
