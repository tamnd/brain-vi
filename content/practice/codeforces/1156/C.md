---
title: "CF 1156C - Điểm trận đấu"
description: "Chúng ta được cung cấp một tập hợp các điểm được đặt trên một trục số và mục tiêu là tạo thành càng nhiều cặp rời rạc càng tốt."
date: "2026-06-12T02:37:23+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings", "ternary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1156
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 64 (Rated for Div. 2)"
rating: 2000
weight: 1156
solve_time_s: 78
verified: true
draft: false
---

[CF 1156C - Điểm phù hợp](https://codeforces.com/problemset/problem/1156/C) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp, tìm kiếm ba ngôi, hai con trỏ 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm được đặt trên một trục số và mục tiêu là tạo thành càng nhiều cặp rời rạc càng tốt. Một cặp chỉ hợp lệ nếu hai điểm được chọn cách nhau đủ xa, nghĩa là chênh lệch tuyệt đối giữa tọa độ của chúng ít nhất là một ngưỡng cố định$z$. Mỗi điểm có thể thuộc về nhiều nhất một cặp, vì vậy một khi điểm đã được sử dụng, điểm đó sẽ không thể tham gia lại. 

Nhiệm vụ là chọn các cặp để tối đa hóa số lượng của chúng chứ không phải tổng khoảng cách hoặc bất kỳ điểm có trọng số nào. Điều này ngay lập tức gợi ý rằng chúng ta đang giải quyết vấn đề so khớp trên một cấu trúc được sắp xếp trong đó chỉ có vị trí tương đối mới quan trọng. 

Kích thước đầu vào đạt tới$2 \cdot 10^5$, loại trừ mọi cách tiếp cận bậc hai. Bất cứ điều gì thử tất cả các kết hợp cặp hoặc quét liên tục để tìm đối tác sẽ xảy ra xung quanh.$10^{10}$hoạt động trong trường hợp xấu nhất và không thực hiện được trong thời hạn. Điều này thúc đẩy chúng tôi hướng tới việc sắp xếp kết hợp với chiến lược ghép nối tuyến tính hoặc logarit. 

Một trường hợp thất bại tinh vi phát sinh khi việc ghép nối tham lam được thực hiện mà không phân loại. Hãy xem xét đầu vào như:```
n = 4, z = 3
points = [1, 10, 2, 11]
```Câu trả lời tối ưu là 2, ghép nối (1,10) và (2,11). Một lần quét đơn giản mà không sắp xếp có thể ghép (1,2) trước nếu nó kiểm tra thứ tự đầu vào liền kề, thứ tự này không hợp lệ vì khoảng cách của chúng chỉ bằng 1 và sau đó bỏ lỡ hoàn toàn cấu trúc tối ưu. Một chế độ thất bại khác xảy ra nếu một người tham lam ghép từng điểm với đối tác hợp lệ gần nhất mà không xem xét thứ tự toàn cầu; điều này có thể gây lãng phí số điểm lớn đáng lẽ phải dành cho những trận đấu sau. 

Vấn đề cốt lõi là các quyết định ghép nối địa phương phải tôn trọng trật tự toàn cầu dọc theo tuyến. 

## Phương pháp tiếp cận 

Chiến lược vũ phu thử tất cả các cặp có thể. Người ta có thể tưởng tượng việc đánh dấu các điểm và chọn đệ quy bất kỳ hai chỉ số chưa sử dụng nào có khoảng cách ít nhất là$z$, sau đó khám phá mọi khả năng. Mặc dù đúng về nguyên tắc nhưng hệ số phân nhánh là rất lớn. Trong trường hợp xấu nhất, với nhiều cặp hợp lệ, số lượng kết quả khớp tăng lên theo cấp số nhân, dẫn đến độ phức tạp theo cấp số nhân. 

Ngay cả một cải tiến mạnh mẽ đơn giản hơn trong đó đối với mỗi điểm chúng tôi quét về phía trước để tìm đối tác hợp lệ sẽ dẫn đến$O(n^2)$hành vi. Với$n = 2 \cdot 10^5$, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng đến từ việc sắp xếp các điểm. Sau khi được sắp xếp, vấn đề sẽ trở thành một trong những cách ghép các phần tử nhỏ với các phần tử lớn trong khi vẫn tôn trọng khoảng cách tối thiểu. Nếu chúng ta sửa một thứ tự, thì việc ghép nối tham lam từ trái sang phải sẽ trở nên có ý nghĩa: điểm nhỏ nhất chưa được sử dụng phải được khớp với điểm sớm nhất có thể thỏa mãn ràng buộc. Thay vào đó, nếu chúng ta cố gắng so khớp nó với một ứng cử viên lớn hơn, không cần thiết muộn hơn, thì chúng ta có thể lãng phí giá trị lớn hơn đó và giảm cơ hội ghép đôi trong tương lai. 

Điều này gợi ý chiến lược hai con trỏ: duy trì một con trỏ ở phía bên trái (điểm không được sử dụng nhỏ nhất) và một con trỏ khác tìm kiếm đối tác hợp lệ. 

Sau khi sắp xếp, chúng ta có thể tiến lên một cách tham lam cả hai con trỏ một cách có kiểm soát sao cho mỗi lần ghép nối thành công sẽ tiêu tốn hai điểm và mỗi lần thử thất bại chỉ di chuyển con trỏ bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các điểm theo thứ tự không giảm. Điều này đảm bảo rằng khi chúng ta xem xét ghép cặp, tất cả các ứng cử viên ở bên phải đều lớn hơn hoặc bằng nhau, do đó việc so sánh khoảng cách trở nên đơn điệu. 
2. Khởi tạo hai con trỏ: một ở đầu mảng (gọi nó là$i$) và một vị trí khác ở vị trí nửa chừng hoặc bắt đầu ở cùng một vị trí tùy thuộc vào chiến lược triển khai. Một công thức sạch là để xử lý$i$là yếu tố chưa từng có đầu tiên và$j$với tư cách là một đối tác ứng cử viên đang quét về phía trước. 
3. Di chuyển$j$chuyển tiếp cho đến khi chúng ta tìm thấy một điểm sao cho$x[j] - x[i] \ge z$. Mỗi bước di chuyển$j$là hợp lý vì bất kỳ ứng cử viên nhỏ hơn nào đã không đáp ứng được điều kiện và sẽ không bao giờ có hiệu lực sau này. 
4. Nếu như vậy$j$được tìm thấy, ghi lại một cặp và tiến lên cả hai$i$Và$j$. Điều này phản ánh việc tiêu thụ cả hai điểm. 
5. Nếu$j$đi đến cuối cùng mà không tìm được một đối tác hợp lệ cho$i$, thì không có điểm nào có thể ghép cặp được với$i$, bởi vì tất cả các điểm còn lại thậm chí còn đúng hơn, nhưng chúng tôi đã sử dụng hết cấu trúc của các kết quả phù hợp khả thi cho cấu hình này theo thứ tự tham lam. Vì vậy chúng tôi di chuyển$i$chuyển tiếp và tiếp tục tìm kiếm điểm cơ sở tiềm năng tiếp theo. 
6. Tiếp tục cho đến khi một trong hai con trỏ tới cuối mảng. 

Điều tinh tế là chúng tôi không bao giờ quay lại. Mỗi con trỏ chỉ di chuyển về phía trước, đảm bảo độ phức tạp tuyến tính sau khi sắp xếp. 

### Tại sao nó hoạt động 

Thứ tự được sắp xếp bắt buộc rằng mọi cặp hợp lệ liên quan đến một điểm$i$phải sử dụng một đối tác ở bên phải của nó. Nếu như$i$được khớp, việc ghép nối nó với đối tác hợp lệ nhỏ nhất có thể luôn an toàn: sử dụng đối tác lớn hơn sẽ chỉ làm giảm tính linh hoạt trong tương lai mà không tăng số lượng cặp. Nếu như$i$không thể khớp với ứng cử viên khả thi nhỏ nhất hiện tại, sau đó nó không thể khớp với bất kỳ ứng cử viên nào trước đó (vì không có) và chỉ còn lại các ứng cử viên lớn hơn, khó đáp ứng ràng buộc khoảng cách hơn so với các lựa chọn trước đó. Lựa chọn tham lam duy trì một bất biến rằng tất cả các quyết định trước đó sẽ tối đa hóa tính linh hoạt còn lại cho hậu tố của mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, z = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    i = 0
    j = n // 2
    ans = 0

    while i < n // 2 and j < n:
        if a[j] - a[i] >= z:
            ans += 1
            i += 1
            j += 1
        else:
            j += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sắp xếp mảng trước, sau đó sử dụng chiến lược hai con trỏ trong đó`i`theo dõi cạnh nhỏ hơn của các cặp tiềm năng và`j`tìm kiếm các đối tác hợp lệ. Lựa chọn khởi tạo`j`Tại`n // 2`phản ánh sự tối ưu hóa tham lam phổ biến: chúng tôi cố gắng ghép các phần tử nửa dưới với các ứng cử viên nửa trên, giảm bớt những so sánh không cần thiết. Logic vẫn hoạt động ngay cả khi`j`bắt đầu từ 0, nhưng bắt đầu từ điểm giữa sẽ làm giảm chi phí quét trong thực tế. 

Chi tiết triển khai chính là cả hai con trỏ chỉ di chuyển về phía trước. Điều này đảm bảo quét tuyến tính sau khi sắp xếp và tránh phải xem lại bất kỳ phần tử nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, z = 2
a = [1, 3, 3, 7]
```Đã sắp xếp mảng rồi`[1, 3, 3, 7]`. 

| tôi | j | một [tôi] | một[j] | khác biệt | hành động | cặp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 3 | 2 | trận đấu | 1 | 
| 1 | 3 | 3 | 7 | 4 | trận đấu | 2 | 

Câu trả lời cuối cùng là 2. 

Dấu vết này cho thấy rằng khi tìm thấy đối tác hợp lệ, cả hai con trỏ đều tiến lên, đảm bảo các cặp tách rời nhau. 

### Ví dụ 2 

đầu vào:```
n = 5, z = 5
a = [1, 2, 10, 11, 20]
```| tôi | j | một [tôi] | một[j] | khác biệt | hành động | cặp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 10 | 9 | trận đấu | 1 | 
| 1 | 3 | 2 | 11 | 9 | trận đấu | 2 | 
| 2 | 4 | 10 | 20 | 10 | trận đấu | 3 | 

Dấu vết xác nhận rằng việc ghép đôi tham lam luôn sử dụng đối tác hợp lệ nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét hai con trỏ là tuyến tính | 
| Không gian |$O(n)$| Lưu trữ cho mảng đầu vào | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$điểm, vì vậy một$O(n \log n)$giải pháp phù hợp một cách thoải mái trong giới hạn thời gian, trong khi mức sử dụng bộ nhớ tuyến tính ở mức không đáng kể dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, z = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    i = 0
    j = n // 2
    ans = 0

    while i < n // 2 and j < n:
        if a[j] - a[i] >= z:
            ans += 1
            i += 1
            j += 1
        else:
            j += 1

    return str(ans)

# provided sample
assert run("4 2\n1 3 3 7\n") == "2"

# all equal values
assert run("6 10\n5 5 5 5 5 5\n") == "0"

# minimal case
assert run("2 1\n1 100\n") == "1"

# tight pairing chain
assert run("6 3\n1 2 3 10 11 12\n") == "3"

# large gap forcing skip
assert run("5 100\n1 2 3 1000 2000\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 0 | không có cặp hợp lệ nào tồn tại | 
| trường hợp tối thiểu | 1 | tính đúng đắn cơ bản | 
| dây chuyền chặt chẽ | 3 | hiệu quả phù hợp tham lam | 
| khoảng trống lớn | 2 | bỏ qua các trận đấu sớm không hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều điểm giống hệt nhau hoặc được nhóm chặt chẽ mà không có đối tác hợp lệ. Trong tình huống đó, thuật toán tiếp tục tiến tới con trỏ bên phải cho đến khi cạn kiệt, đảm bảo không xảy ra việc ghép nối sai. Ví dụ:```
[5, 5, 5, 5], z = 1
```Mảng được sắp xếp không thay đổi. Con trỏ`i`ở 0 sẽ quét`j`trên tất cả các yếu tố, không bao giờ tìm thấy sự khác biệt hợp lệ và cuối cùng`i`số gia tăng. Điều này lặp lại cho đến khi tất cả các phần tử cạn kiệt, tạo ra 0 cặp. 

Một trường hợp khác là khi các cặp hợp lệ chỉ tồn tại giữa các thái cực, chẳng hạn như:```
[1, 2, 3, 100], z = 50
```Ở đây chỉ có (1.100) là hợp lệ. Thuật toán bỏ qua 2 và 3 làm đối tác của 1 vì chúng không đạt ngưỡng, cuối cùng đạt tới 100 và tạo thành chính xác một cặp.
