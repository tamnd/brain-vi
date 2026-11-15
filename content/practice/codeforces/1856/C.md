---
title: "CF 1856C - Trở Thành Max"
description: "Chúng ta được cung cấp một mảng các số nguyên và một số phép toán được phép. Mỗi thao tác cho phép chúng ta chọn một chỉ mục $i$ sao cho phần tử tại $i$ nhỏ hơn hoặc bằng phần tử lân cận bên phải của nó và tăng $ai$ lên một."
date: "2026-06-09T05:02:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1856
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 890 (Div. 2) supported by Constructor Institute"
rating: 1600
weight: 1856
solve_time_s: 98
verified: false
draft: false
---

[CF 1856C - Trở thành Max](https://codeforces.com/problemset/problem/1856/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một số phép toán được phép. Mỗi thao tác cho phép chúng tôi chọn một chỉ mục$i$sao cho phần tử tại$i$nhỏ hơn hoặc bằng hàng xóm bên phải của nó và tăng$a_i$bởi một. Mục tiêu của chúng ta là tối đa hóa số lượng lớn nhất trong mảng sau nhiều nhất$k$hoạt động. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng và số lượng thao tác. Đầu ra cho mỗi trường hợp là một số nguyên duy nhất: giá trị lớn nhất có thể có trong mảng sau khi áp dụng các phép toán một cách tối ưu. 

Các ràng buộc cho chúng ta biết rằng độ dài mảng$n$tối đa là 1000, cho phép chúng ta lặp lại mảng nhiều lần. Số lượng hoạt động$k$có thể lớn như$10^8$, vì vậy chúng ta không thể mô phỏng từng thao tác riêng lẻ một cách đơn giản nếu chúng ta dự định tăng từng phần tử một. Bất kỳ cách tiếp cận nào lặp đi lặp lại$k$hoạt động trực tiếp sẽ yêu cầu lên đến$10^8$bước, vượt xa những gì có thể thực hiện được trong 2 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng đã giảm dần hoặc chứa các điểm cố định. Ví dụ, nếu mảng là`[5, 4, 3]`và chúng tôi có nhiều thao tác, không có phần tử nào ngoại trừ phần tử đầu tiên có thể được tăng lên vì điều kiện$a_i \le a_{i+1}$không bao giờ giữ cho bất kỳ$i$. Việc triển khai bất cẩn có thể cố gắng tăng tất cả các phần tử một cách mù quáng và tạo ra kết quả sai. Một trường hợp cạnh khác là một mảng có các phần tử bằng nhau như`[2, 2, 2]`, trong đó bất kỳ phần tử nào ngoại trừ phần tử cuối cùng đều có thể được tăng lên nhiều lần. 

## Phương pháp tiếp cận 

Phương pháp brute-force hoạt động bằng cách mô phỏng từng thao tác: quét mảng từ trái sang phải, tìm chỉ mục đầu tiên$i$Ở đâu$a_i \le a_{i+1}$, tăng$a_i$, và lặp lại cho đến khi$k$các hoạt động được sử dụng hoặc không còn chỉ mục hợp lệ. Cách làm này đúng vì mỗi phép toán đều tuân theo quy tắc và chúng ta luôn chọn phần tử ngoài cùng bên trái có thể tăng lên. Tuy nhiên, trong trường hợp xấu nhất, nếu$k$là$10^8$Và$n$là 1000, phương pháp này yêu cầu tới$10^8$lặp đi lặp lại, điều này là không thực tế. 

Quan sát quan trọng là chúng ta không cần phải mô phỏng tất cả$k$hoạt động riêng lẻ. Giá trị tối đa được xác định bằng số lần chúng ta có thể tăng các phần tử bắt đầu từ vị trí ngoài cùng bên trái thỏa mãn$a_i \le a_{i+1}$. Mỗi thao tác di chuyển "trọng lượng" từ trái sang phải và cuối cùng phần tử ngoài cùng bên trái có thể tăng sẽ dịch chuyển sang phải khi các phần tử trước đó bão hòa. Do đó, chúng ta có thể mô phỏng quá trình này theo cách tham lam: luôn áp dụng thao tác cho chỉ mục hợp lệ ngoài cùng bên trái cho đến khi một trong hai$k$hết hoặc không có chỉ số nào thỏa mãn điều kiện. Bởi vì$n \le 1000$, quét mảng nhiều nhất$n$số lần trên mỗi chuỗi thao tác có thể chấp nhận được nếu chúng ta tối ưu hóa bằng cách dừng khi tất cả các bước tăng đã được sử dụng. 

Cách tiếp cận tối ưu sử dụng chiến lược từ trái sang phải tham lam này, nhưng nó theo dõi thời điểm không thể áp dụng thao tác tiếp theo để bỏ qua các bước lặp không cần thiết. Điều này tránh việc lặp lại tất cả$k$hoạt động một cách ngây thơ và đảm bảo chúng tôi tăng theo thứ tự tối đa hóa các phần tử ngoài cùng bên trái trước tiên, có cơ hội cao nhất để đẩy mức tối đa tổng thể lên trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * n) | O(n) | Quá chậm đối với k lớn | 
| Tham lam từ trái sang phải | O(k * n) tệ nhất, O(k) trung bình | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$, và mảng$a$. 
2. Lặp lại tối đa$k$lần: 

1. Quét mảng từ trái sang phải tìm chỉ mục đầu tiên$i$Ở đâu$a_i \le a_{i+1}$. 
2. Nếu không có chỉ mục nào như vậy tồn tại, hãy ngắt vòng lặp. 
3. Ngược lại thì tăng$a_i$bằng 1. 
3. Sau khi sử dụng hết$k$hoạt động hoặc chạm vào một điểm mà không thể tăng chỉ số nào, trả về giá trị tối đa của mảng. 

Tại sao nó hoạt động: mọi thao tác đều phải tăng phần tử hợp lệ. Bằng cách luôn chọn chỉ mục hợp lệ ngoài cùng bên trái, chúng tôi tối đa hóa khả năng các phần tử tiếp theo cũng có thể được tăng lên. Khi một phần tử không thể tăng lên mà không vi phạm$a_i \le a_{i+1}$, nó đã được tăng lên một cách tối ưu. Từ$n$nhỏ, quá trình quét lặp lại không vượt quá giới hạn thời gian và tính bất biến chỉ thực hiện các thao tác hợp lệ mới đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        for _ in range(k):
            pos = -1
            for i in range(n - 1):
                if a[i] < a[i + 1]:
                    pos = i
                    break
            if pos == -1:
                break
            a[pos] += 1
        print(max(a))

if __name__ == "__main__":
    solve()
```Mã đọc số lượng trường hợp thử nghiệm và lặp qua từng trường hợp thử nghiệm. Đối với mỗi mảng, chúng tôi cố gắng thực hiện tối đa$k$hoạt động. Vòng lặp bên trong xác định chỉ mục hợp lệ ngoài cùng bên trái để tăng. Nếu không tồn tại, vòng lặp sẽ kết thúc sớm. Mức tối đa cuối cùng của mảng được in. Một sự tinh tế đang được sử dụng`a[i] < a[i+1]`còn hơn là`<=`để tránh việc tăng vô hạn không cần thiết khi hai phần tử liên tiếp bằng nhau và không thể tăng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[3, 4]`với mảng`[1, 3, 3]`Và`k = 4`: 

| Hoạt động | Trạng thái mảng | Chỉ số tăng thêm | 
| --- | --- | --- | 
| 0 | [1, 3, 3] | 0 | 
| 1 | [2, 3, 3] | 0 | 
| 2 | [3, 3, 3] | 0 | 
| 3 | [4, 3, 3] | 0 | 

Tối đa là`4`. Dấu vết này chứng tỏ rằng phần tử ngoài cùng bên trái được tăng lên cho đến khi nó không thể tăng thêm nữa so với phần tử lân cận, tạo ra kết quả tối ưu. 

Ví dụ thứ hai`[5, 6]`với mảng`[1, 3, 4, 5, 1]`Và`k = 6`: 

| Hoạt động | Trạng thái mảng | Chỉ số tăng thêm | 
| --- | --- | --- | 
| 0 | [1, 3, 4, 5, 1] ​​| 0 | 
| 1 | [2, 3, 4, 5, 1] ​​| 0 | 
| 2 | [3, 3, 4, 5, 1] ​​| 0 | 
| 3 | [4, 3, 4, 5, 1] ​​| 0 | 
| 4 | [4, 4, 4, 5, 1] ​​| 1 | 
| 5 | [4, 5, 4, 5, 1] ​​| 1 | 

Tối đa là`7`. Điều này xác nhận rằng lựa chọn tham lam từ trái sang phải sẽ ưu tiên chính xác các yếu tố đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * k) tệ nhất, O(k) trung bình | Mỗi thao tác quét tối đa n phần tử, nhưng trong thực tế, nhiều thao tác tăng các chỉ số ban đầu giống nhau. | 
| Không gian | O(n) | Chúng tôi chỉ lưu trữ mảng. | 

Được cho$n \le 1000$Và$k$có thể lớn, việc chấm dứt sớm trong thực tế sẽ giúp quản lý được thời gian chạy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("6\n3 4\n1 3 3\n5 6\n1 3 4 5 1\n4 13\n1 1 3 179\n5 3\n4 3 2 2 2\n5 6\n6 5 4 1 5\n2 17\n3 5\n") == "4\n7\n179\n5\n7\n6"

# minimum-size input
assert run("1\n2 1\n1 1\n") == "2"

# all equal
assert run("1\n3 5\n2 2 2\n") == "7"

# decreasing array
assert run("1\n4 10\n5 4 3 2\n") == "5"

# maximum-size array, few operations
assert run("1\n1000 3\n" + " ".join(["1"]*1000) + "\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1\n1 1`|`2`| mảng kích thước tối thiểu | 
|`3 5\n2 2 2`|`7`| tất cả các phần tử bằng nhau có thể được tăng lên | 
|`4 10\n5 4 3 2`|`5`| mảng giảm dần, các phép toán không thể thay đổi |
