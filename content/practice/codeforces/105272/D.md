---
title: "CF 105272D – Chia bánh pizza năng lượng mặt trời"
description: "Chúng ta có một số bạn bè chẵn, chẳng hạn là 2n, và mỗi người bạn có một giá trị nguyên duy nhất biểu thị sở thích về hương vị pizza. Nhóm muốn đặt chính xác n chiếc pizza và mỗi chiếc pizza phải được làm theo sở thích của hai người bạn khác nhau."
date: "2026-06-23T14:02:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "D"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 42
verified: true
draft: false
---

[CF 105272D - Chia bánh pizza năng lượng mặt trời](https://codeforces.com/problemset/problem/105272/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số bạn bè chẵn, chẳng hạn là 2n, và mỗi người bạn có một giá trị nguyên duy nhất biểu thị sở thích về hương vị pizza. Nhóm muốn đặt chính xác n chiếc pizza và mỗi chiếc pizza phải được làm theo sở thích của hai người bạn khác nhau. Mỗi giá trị ưu tiên phải được sử dụng chính xác một lần, nghĩa là chúng ta ghép tất cả 2n số thành n cặp. 

Mỗi cặp tạo thành một chiếc bánh pizza và giá của một chiếc bánh pizza được xác định là giá trị tối đa của hai giá trị trong cặp đó. Mục tiêu là chọn một chiến lược ghép nối để giảm thiểu tổng số tiền tối đa này. 

Vì vậy, vấn đề giảm xuống việc ghép 2n số thành n cặp, giảm thiểu tổng cực đại theo cặp. 

Các ràng buộc cho phép n lên tới 100.000, nghĩa là lên tới 200.000 số. Bất kỳ giải pháp nào tệ hơn O(n log n) đều có nguy cơ quá chậm. Chiến lược ghép đôi bậc hai thử tất cả các kết hợp cặp hoặc sử dụng lập trình động trên các tập hợp con ngay lập tức là không thể vì nó sẽ liên quan đến hành vi gần như n² hoặc 2^(2n), cả hai đều vượt xa giới hạn khả thi. 

Trường hợp cạnh tinh tế phát sinh khi các giá trị mất cân bằng cao. Ví dụ: nếu hầu hết các giá trị đều nhỏ và một giá trị cực kỳ lớn, thì việc ghép nối tham lam ngây thơ gắn giá trị lớn không chính xác có thể làm tăng tổng chi phí. Hãy xem xét đầu vào như 1 1 1 1 100. Nếu được ghép nối kém, 100 có thể sẽ làm tăng chi phí nhiều cặp trong sơ đồ tham lam gián tiếp, nhưng cấu trúc tối ưu sẽ tách nó thành chính xác một cặp. 

Một trường hợp đặc biệt khác là khi các giá trị đã được sắp xếp hoặc gần giống nhau. Trong những trường hợp như vậy, các chiến lược ghép đôi khác nhau có thể xuất hiện tương đương cục bộ nhưng chỉ có một cấu trúc đảm bảo tính tối ưu toàn cục. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi cách có thể để ghép 2n phần tử và tính tổng cực đại cho mỗi cặp. Điều này đúng vì nó khám phá mọi phân vùng hợp lệ của mảng thành từng cặp. Tuy nhiên, số cặp là (2n - 1)!!, tăng nhanh hơn hàm mũ trong n. Ngay cả với n = 20, điều này vẫn không thể thực hiện được và ở n = 100.000 thì điều đó hoàn toàn không thể xảy ra. 

Để cải thiện, chúng tôi tìm kiếm cấu trúc theo cách các cực đại của cặp đóng góp vào tổng số. Mỗi cặp đóng góp phần lớn hơn trong hai phần tử của nó. Nếu sắp xếp mảng, chúng ta sẽ có quyền kiểm soát những phần tử nào có khả năng đạt giá trị cực đại. Quan sát quan trọng là để giảm thiểu lãng phí các giá trị lớn, chúng tôi muốn “ghép nối các phần tử lớn một cách cẩn thận để mỗi phần tử lớn chỉ thanh toán một lần và không được ghép nối với thứ gì đó thậm chí còn lớn hơn một cách không cần thiết”. 

Việc sắp xếp cho thấy chiến lược tối ưu là ghép các phần tử liền kề theo thứ tự được sắp xếp. Điều này đảm bảo rằng trong mỗi cặp, phần tử lớn hơn sẽ càng nhỏ càng tốt trong số các lựa chọn còn lại. Bất kỳ nỗ lực nào để bỏ qua việc ghép nối liền kề đều có nguy cơ ghép một phần tử lớn với một phần tử lớn hơn nhiều sau đó, làm tăng tổng. 

Vì vậy, vấn đề giảm xuống còn việc sắp xếp và tính tổng từng phần tử thứ hai từ cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2n−1)!!) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + ghép nối tham lam) | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu bằng cách sắp xếp mảng giá trị 2n theo thứ tự không giảm. 

1. Sắp xếp tất cả các giá trị. Điều này tạo ra một cấu trúc toàn cầu trong đó sự lân cận cục bộ phản ánh sự gần gũi về giá trị, điều này rất quan trọng để giảm thiểu mức tối đa trong mỗi cặp. 
2. Lặp lại từ giá trị lớn nhất trở xuống, mỗi lần bước lên hai vị trí. Nói cách khác, lấy các chỉ số 2n−1, 2n−3, 2n−5, ​​v.v. 
3. Tính tổng các phần tử đã chọn này. Mỗi trong số này tương ứng với mức tối đa của một cặp được tạo thành với phần tử ngay trước nó theo thứ tự được sắp xếp. 
4. Xuất ra tổng. 

Việc ghép đôi ngầm định là: (a[0], a[1]), (a[2], a[3]), ..., (a[2n−2], a[2n−1]) sau khi sắp xếp. Chúng tôi không xây dựng các cặp một cách rõ ràng vì chỉ có cực đại mới quan trọng. 

### Tại sao nó hoạt động

Sau khi sắp xếp, hãy xem xét mọi cặp đôi tối ưu. Tập trung vào yếu tố lớn nhất. Trong bất kỳ giải pháp hợp lệ nào, nó phải được ghép nối với một số phần tử khác. Đối tác tốt nhất có thể để giảm thiểu sự đóng góp chi phí là phần tử lớn nhất còn lại, càng nhỏ càng tốt trong khi vẫn tạo thành một cặp hợp lệ. Ghép nối nó với bất kỳ thứ gì nhỏ hơn không làm giảm chi phí, vì bản thân mức tối đa vẫn là phần tử lớn nhất. Tuy nhiên, việc ghép các phần tử lớn lại với nhau sẽ tránh lãng phí phần tử lớn thứ hai dưới dạng mức tối đa riêng biệt khi nó có thể được ghép với phần tử nhỏ hơn một chút. Việc lặp lại đối số này theo cách quy nạp cho thấy rằng việc ghép các phần tử liên tiếp theo thứ tự đã sắp xếp sẽ mang lại một sự sắp xếp trong đó mức tối đa của mỗi cặp chính xác là một trong những “phần tử thứ hai tính từ cuối” đã chọn và không có sự sắp xếp lại nào có thể làm giảm tổng hơn nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    a.sort()
    
    ans = 0
    for i in range(2 * n - 1, -1, -2):
        ans += a[i]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Lời giải bắt đầu bằng cách đọc giá trị n và 2n. Việc sắp xếp là cần thiết vì nó căn chỉnh các phần tử sao cho việc ghép cặp tối ưu trở thành một quyết định cục bộ chứ không phải là một vấn đề tìm kiếm toàn cầu. 

Vòng lặp đi từ cuối mảng theo hai bước, tích lũy trực tiếp các phần tử đóng vai trò là cực đại trong mỗi cặp tối ưu. Chúng tôi không bao giờ xây dựng các cặp một cách rõ ràng vì chỉ đóng góp tối đa cho câu trả lời cuối cùng. 

Một lỗi triển khai phổ biến là lặp lại không chính xác trên các chỉ mục, đặc biệt là trộn n và 2n hoặc dừng ở ranh giới sai. Phạm vi chính xác là chính xác 2n phần tử và sải bước phải là hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [5, 3, 4, 1, 2, 6]
```Sau khi sắp xếp:```
[1, 2, 3, 4, 5, 6]
```Chúng tôi ghép các phần tử liền kề: 

(1,2), (3,4), (5,6) 

| Bước | Chỉ mục được chọn | Giá trị | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 5 | 6 | 6 | 
| 2 | 3 | 4 | 10 | 
| 3 | 1 | 2 | 12 | 

Đầu ra là 12. 

Dấu vết này cho thấy rằng việc lấy từng phần tử thứ hai từ cuối sẽ nắm bắt chính xác mức tối đa của mỗi cặp tối ưu. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [10, 1, 9, 2]
```Sau khi sắp xếp:```
[1, 2, 9, 10]
```Các cặp là: 

(1,2), (9,10) 

| Bước | Chỉ mục được chọn | Giá trị | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 3 | 10 | 10 | 
| 2 | 1 | 2 | 12 | 

Đầu ra là 12. 

Điều này xác nhận rằng các giá trị lớn được tách biệt một cách hiệu quả và mỗi giá trị đóng góp chính xác một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính sau | 
| Không gian | O(1) đến O(n) | Sắp xếp tại chỗ cộng với lưu trữ đầu vào | 

Kích thước đầu vào lên tới 200.000 phần tử khiến việc sắp xếp trở thành hoạt động nặng nề duy nhất khả thi. Quá trình quét tuyến tính sau khi phân loại đảm bảo giải pháp vẫn hoạt động tốt trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n = int(input().strip())
        a = list(map(int, input().split()))
        a.sort()
        ans = 0
        for i in range(2 * n - 1, -1, -2):
            ans += a[i]
        print(ans)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample-like cases
assert run("3\n5 3 4 1 2 6\n") == "12"
assert run("2\n10 1 9 2\n") == "12"

# minimum case
assert run("1\n5 1\n") == "5"

# all equal
assert run("3\n4 4 4 4 4 4\n") == "12"

# already sorted
assert run("2\n1 2 3 4\n") == "6"

# reverse sorted
assert run("2\n4 3 2 1\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 1 | 5 | trường hợp cạnh tối thiểu | 
| tất cả đều bình đẳng | 12 | tính đúng đắn đối xứng | 
| sắp xếp / sắp xếp ngược | 6 | ổn định theo thứ tự | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, việc sắp xếp không tạo ra sự thay đổi cấu trúc có ý nghĩa. Thuật toán vẫn ghép các phần tử liền kề và tính tổng mọi phần tử thứ hai từ cuối, mang lại giá trị chính xác gấp n lần cùng một giá trị. Đối với đầu vào 4 4 4 4 4 4, mảng được sắp xếp không thay đổi và tổng trở thành 12 với n = 3, khớp với bất kỳ cặp hợp lệ nào. 

Khi mảng đã được sắp xếp theo thứ tự tăng dần, chẳng hạn như 1 2 3 4, thuật toán sẽ ghép cặp (1,2) và (3,4). Việc chọn 2 và 4 làm cực đại phù hợp với việc duyệt qua từng phần tử thứ hai từ cuối và không có cặp thay thế nào có thể làm giảm tổng vì bất kỳ ghép chéo nào như (1,4) sẽ tạo lực (2,3) và làm tăng tổng. 

Khi các giá trị được sắp xếp ngược, chẳng hạn như 4 3 2 1, việc sắp xếp sẽ chuẩn hóa cấu trúc trước tiên. Logic tương tự được áp dụng và thuật toán hoạt động giống hệt nhau sau khi sắp xếp, đảm bảo rằng thứ tự đầu vào không bao giờ ảnh hưởng đến tính chính xác.
