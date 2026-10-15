---
title: "CF 105450A - Giảm thiểu sự khác biệt chua"
description: "Chúng ta được cung cấp một danh sách các giá trị độ chua nguyên đại diện cho kẹo. Alice được phép loại bỏ chính xác $k$ kẹo khỏi danh sách này, để lại $n-k$ kẹo."
date: "2026-06-23T03:05:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105450
codeforces_index: "A"
codeforces_contest_name: "UTPC x WiCS Contest 10-25-24 (UT Internal)"
rating: 0
weight: 105450
solve_time_s: 79
verified: false
draft: false
---

[CF 105450A - Giảm thiểu sự khác biệt về vị chua](https://codeforces.com/problemset/problem/105450/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các giá trị độ chua nguyên đại diện cho kẹo. Alice được phép loại bỏ chính xác$k$kẹo từ danh sách này, rời đi$n-k$kẹo phía sau. Sau khi loại bỏ, chúng tôi xem xét tập hợp còn lại và đo lường mức độ “trải rộng” của nó bằng cách lấy chênh lệch giữa giá trị còn lại lớn nhất và nhỏ nhất. Mục tiêu là chọn những viên kẹo cần loại bỏ sao cho mức chênh lệch cuối cùng này càng nhỏ càng tốt. 

Nói cách khác, chúng tôi không cố gắng kiểm soát trực tiếp sự khác biệt theo cặp bên trong một số tập hợp con đã chọn mà thay vào đó, chúng tôi đang kiểm soát phạm vi của những gì còn lại sau khi xóa. Mục tiêu giảm xuống còn làm cho các giá trị còn lại được phân cụm chặt chẽ nhất có thể trên trục số. 

Những hạn chế$n \le 1000$cho phép một giải pháp đại khái$O(n^2)$, mà còn có thể sử dụng tính năng sắp xếp và cửa sổ trượt mà không cần quan tâm đến hiệu suất. Bất cứ điều gì$O(n^3)$hoặc tệ hơn sẽ có rủi ro ở quy mô lớn, nhưng ngay cả$O(n^2)$sẽ trôi qua một cách thoải mái. 

Một trường hợp tế nhị nhưng quan trọng phát sinh khi tất cả các số đều giống hệt nhau. Trong trường hợp đó, mọi tập hợp con đều có phạm vi bằng 0 và câu trả lời phải bằng 0 bất kể có loại bỏ hay không. Một trường hợp khác là khi chỉ còn lại một viên kẹo sau khi loại bỏ, nghĩa là$n-k=1$. Phạm vi của một phần tử luôn bằng 0 và mọi cách tiếp cận đúng đều phải xử lý vấn đề này một cách rõ ràng hoặc ngầm định. 

Một cách tiếp cận không chính xác phổ biến là loại bỏ một cách tham lam các giá trị cực trị ở cả hai đầu mà không xem xét khối liền kề tốt nhất. Điều này có thể thất bại khi giải pháp tối ưu không được tập trung vào hoặc khi tồn tại nhiều cụm dày đặc. Ví dụ: với các giá trị như$[1, 2, 3, 100, 101]$, việc loại bỏ các cực đoan sai có thể để lại một khoảng cách rộng mặc dù tồn tại một khối chặt chẽ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi tập hợp con có kích thước có thể$n-k$, tính toán mức tối thiểu và tối đa của nó và theo dõi phạm vi nhỏ nhất có thể. Điều này đúng vì nó kiểm tra tất cả các cấu hình nhưng không khả thi về mặt tính toán. Số tập con là$\binom{n}{n-k}$, tăng theo cấp số nhân và trở nên không thể ngay cả đối với$n=30$. 

Nhận xét quan trọng là chỉ có thứ tự tương đối của các giá trị mới quan trọng chứ không phải vị trí ban đầu của chúng. Sau khi mảng được sắp xếp, mọi tập hợp kích thước tối ưu còn lại sẽ$n-k$phải bao gồm các phần tử liền kề theo thứ tự được sắp xếp này. Nếu nó không liền kề, sẽ có một khoảng trống bên trong vùng chọn và việc thay thế phần tử cực trị bằng phần tử bị thiếu gần hơn sẽ luôn giảm hoặc duy trì phạm vi. 

Điều này làm giảm vấn đề chọn cửa sổ có kích thước$n-k$trong mảng được sắp xếp và giảm thiểu sự khác biệt giữa các điểm cuối của nó. Mỗi cửa sổ tượng trưng cho việc giữ một nhóm kẹo được đóng gói chặt chẽ và loại bỏ mọi thứ bên ngoài nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{k} \cdot n)$|$O(n)$| Quá chậm | 
| Sắp xếp + Cửa sổ trượt |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp sắp xếp tất cả các ứng cử viên sao cho bất kỳ tập hợp con tối ưu nào cũng trở thành một phân đoạn liền kề theo thứ tự này. 
2. Tính số kẹo còn lại$m = n - k$. Đây là kích thước cố định của cửa sổ mà chúng ta phải đánh giá. 
3. Trượt một cửa sổ có kích thước$m$trên mảng đã được sắp xếp. Đối với mỗi cửa sổ bắt đầu từ chỉ mục$i$, cửa sổ bao gồm các phần tử từ$i$ĐẾN$i+m-1$. 
4. Đối với mỗi cửa sổ, hãy tính sự khác biệt giữa phần tử cuối cùng và phần tử đầu tiên của nó. Giá trị này đại diện cho phạm vi nếu chúng ta giữ chính xác những viên kẹo đó. 
5. Theo dõi sự khác biệt tối thiểu trên tất cả các cửa sổ và xuất nó ra. 

Mỗi bước đều cần thiết vì việc sắp xếp sẽ tạo ra cấu trúc, cửa sổ thực thi giới hạn kích thước tập hợp con và việc đánh giá các điểm cuối sẽ trực tiếp nắm bắt phạm vi mà không cần quét bên trong cửa sổ. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ tập hợp con nào có kích thước$m$không liền kề có thể được cải thiện bằng cách thay thế một khoảng cách lớn bên trong bằng một phần tử gần hơn bên ngoài tập hợp con. Sự thay thế này không bao giờ tăng phạm vi và thường giảm nó. Việc lặp lại lập luận này buộc một giải pháp tối ưu trở thành một khối liền kề. Vì vậy, mọi lời giải tối ưu đều tương ứng chính xác với một cửa sổ trượt có độ dài$m$và việc kiểm tra tất cả các cửa sổ như vậy đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    m = n - k
    if m <= 1:
        print(0)
        return
    
    ans = float('inf')
    
    for i in range(n - m + 1):
        ans = min(ans, a[i + m - 1] - a[i])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho mọi lựa chọn tối ưu đều phải xuất hiện dưới dạng khối liền kề. Biến`m`thể hiện số lượng kẹo còn lại sau khi loại bỏ. Nếu chỉ còn lại một viên kẹo, câu trả lời ngay lập tức là 0 vì không tồn tại chênh lệch. 

Vòng lặp kiểm tra mọi cửa sổ có thể có kích thước`m`. Đối với mỗi chỉ số bắt đầu`i`, sự khác biệt`a[i + m - 1] - a[i]`tính toán phạm vi của phân khúc đó. Mức tối thiểu trên tất cả các cửa sổ là cấu hình tốt nhất có thể đạt được. 

Một lỗi triển khai phổ biến là sử dụng`n-k`không chính xác bằng số phần tử bị loại bỏ thay vì số phần tử còn lại. Một vấn đề tế nhị khác là quên rằng cửa sổ hợp lệ cuối cùng bắt đầu lúc`n-m`, không`n-m+1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 3
1 4 8 9 12 16
```Đây$m = 3$. Sau khi sắp xếp (đã được sắp xếp), chúng tôi đánh giá các cửa sổ có kích thước 3. 

| tôi | cửa sổ | phạm vi | 
| --- | --- | --- | 
| 0 | [1, 4, 8] | 7 | 
| 1 | [4, 8, 9] | 5 | 
| 2 | [8, 9, 12] | 4 | 
| 3 | [9, 12, 16] | 7 | 

Phạm vi tối thiểu là 4. 

Dấu vết này cho thấy giải pháp tốt nhất không nhất thiết phải ở điểm cực trị mà ở vùng dày đặc nhất của mảng. 

### Mẫu 2 

đầu vào:```
4 1
9 3 1 2
```Mảng được sắp xếp: [1, 2, 3, 9],$m = 3$| tôi | cửa sổ | phạm vi | 
| --- | --- | --- | 
| 0 | [1, 2, 3] | 2 | 
| 1 | [2, 3, 9] | 7 | 

Phạm vi tối thiểu là 2. 

Điều này chứng tỏ rằng việc loại bỏ một phần tử cực trị (9) sẽ tạo ra cụm chặt chẽ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, cửa sổ trượt là tuyến tính | 
| Không gian |$O(1)$thêm | Sắp xếp tại chỗ và các biến phụ trợ không đổi | 

Những hạn chế$n \le 1000$làm điều này nhanh chóng thoải mái. Ngay cả việc sắp xếp lặp đi lặp lại trên nhiều trường hợp thử nghiệm cũng sẽ vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("6 3\n1 4 8 9 12 16\n") == "4"

# sample 2
assert run("4 1\n9 3 1 2\n") == "2"

# sample 3
assert run("6 4\n10 8 2 6 7 10\n") == "0"

# custom cases
assert run("2 1\n5 100\n") == "0"
assert run("5 2\n1 100 101 102 103\n") == "2"
assert run("5 4\n1 2 3 4 5\n") == "0"
assert run("6 2\n1 10 20 30 40 50\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 / 5 100 | 0 | single remaining element |
 | 5 2 / 1 100 101 102 103 | 2 | optimal dense block in middle |
 | 5 4 / 1 2 3 4 5 | 0 | loại bỏ một phần tử khỏi trường hợp cạnh tiến triển thống nhất | 
| 6 2/1 10 20 30 40 50 | 20 | khoảng cách rộng với lựa chọn đoạn liền kề tốt nhất | 

## Vỏ cạnh 

Đối với trường hợp phần tử đơn còn lại, chẳng hạn như đầu vào:```
3 2
10 1 7
```Sau khi sắp xếp, chúng ta nhận được [1, 7, 10] và$m = 1$. Thuật toán ngay lập tức trả về 0 vì bất kỳ cửa sổ phần tử đơn lẻ nào cũng có phạm vi bằng 0. Điều này tránh việc truy cập các điểm cuối cửa sổ không hợp lệ và phản ánh chính xác định nghĩa về phạm vi. 

Đối với các giá trị bằng nhau:```
5 2
7 7 7 7 7
```Every window of any size has identical endpoints, so every computed range is 0. The minimum remains 0, and the algorithm naturally handles this without special casing.

 Đối với các cụm tách biệt rộng rãi:```
6 2
1 2 3 100 101 102
```Mảng được sắp xếp đã hiển thị hai nhóm chặt chẽ. Windows cỡ 4 sẽ bao gồm cả hai cụm hoặc chỉ một cụm. Cửa sổ tối ưu là [1, 2, 3, 100] cho phạm vi 99 hoặc [2, 3, 100, 101] cho phạm vi 99. Điều này xác nhận thuật toán đánh giá một cách có hệ thống tất cả các lựa chọn liền kề của ứng viên và không bỏ sót cấu hình cụm phân chia.
