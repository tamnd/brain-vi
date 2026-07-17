---
title: "CF 103446E - Số nguyên lạ"
description: "Chúng ta được cho một dãy số nguyên và giá trị ngưỡng $k$. Từ chuỗi này, chúng ta muốn chọn một chuỗi con (bảo toàn các chỉ số gốc) sao cho mọi cặp giá trị được chọn đều có giá trị “gần giống nhau”: chênh lệch tuyệt đối giữa hai phần tử được chọn bất kỳ phải lớn nhất là $k$."
date: "2026-07-03T07:35:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103446
codeforces_index: "E"
codeforces_contest_name: "The 2021 ICPC Asia Shanghai Regional Programming Contest"
rating: 0
weight: 103446
solve_time_s: 38
verified: true
draft: false
---

[CF 103446E - Số nguyên lạ](https://codeforces.com/problemset/problem/103446/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và một giá trị ngưỡng$k$. Từ chuỗi này, chúng tôi muốn chọn một chuỗi con (bảo toàn các chỉ số ban đầu) sao cho mọi cặp giá trị được chọn đều có giá trị gần nhau: sự khác biệt tuyệt đối giữa hai phần tử được chọn bất kỳ phải lớn nhất$k$. Mục tiêu là tối đa hóa số lượng phần tử chúng ta có thể chọn theo ràng buộc này. 

Hạn chế chính là toàn cục trên tập hợp đã chọn, không chỉ giữa các phần tử liên tiếp. Nếu chúng ta chọn ba giá trị$x, y, z$, thì cả ba điểm khác biệt theo cặp phải nằm trong$k$, không chỉ$|x-y|$Và$|y-z|$. Điều này làm cho cấu trúc chỉ phụ thuộc vào phạm vi giá trị trong tập hợp đã chọn. 

Kích thước đầu vào đạt$n = 10^5$, do đó bất kỳ bậc hai hoặc thậm chí$O(n \log n)$giải pháp so sánh nhiều lần tất cả các cặp sẽ gặp khó khăn nếu nó không được cấu trúc cẩn thận. Giải pháp lý tưởng nhất là sắp xếp một lần rồi quét tuyến tính hoặc bằng cửa sổ trượt. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Trong trường hợp đó, chúng ta chỉ có thể chọn các giá trị giống hệt nhau. Câu trả lời trở thành tần số tối đa của bất kỳ số nào. Ví dụ, nếu mảng là$[1, 2, 2, 3]$Và$k = 0$, câu trả lời đúng là$2$. Một kẻ tham lam ngây thơ cố gắng mở rộng một dãy con dựa trên sự kề cận sẽ chọn sai nhiều phần tử hơn. 

Một trường hợp cạnh khác phát sinh khi tất cả các giá trị nằm trong một khoảng nhỏ. Ví dụ, nếu tất cả$A_i = 10^9$, thì bất kỳ tập hợp con nào cũng hợp lệ và câu trả lời là$n$. Một giải pháp đúng không được vô tình loại bỏ các bản sao hoặc dựa vào các giả định bất đẳng thức nghiêm ngặt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi tập hợp con của mảng và kiểm tra xem nó có thỏa mãn điều kiện là tất cả các khác biệt theo cặp nhiều nhất là không.$k$. Đối với mỗi tập hợp con, việc xác minh tính hợp lệ yêu cầu quét tất cả các phần tử đã chọn và theo dõi giá trị tối thiểu và tối đa. Một tập hợp con hợp lệ chính xác khi$\max - \min \le k$. 

Điều này dẫn đến số lượng tập hợp con theo cấp số nhân, đại khái là$2^n$và mỗi lần xác nhận có giá lên tới$O(n)$. Ngay cả khi giảm xuống chỉ kiểm tra các tập hợp con một cách ngầm định, cách tiếp cận này gần như không thể thực hiện được ngay lập tức. 

Cấu trúc của điều kiện cho thấy sự đơn giản hóa: điều kiện chỉ phụ thuộc vào phạm vi giá trị được chọn. Khi chúng tôi sắp xếp mảng, mọi tập hợp con hợp lệ sẽ tương ứng với một phân đoạn liền kề theo thứ tự được sắp xếp trong đó chênh lệch giữa phần tử nhỏ nhất và phần tử lớn nhất là nhiều nhất$k$. Điều này chuyển vấn đề thành việc tìm đoạn dài nhất như vậy. 

Sau khi sắp xếp, chúng ta có thể sử dụng cửa sổ trượt hai con trỏ. Chúng tôi mở rộng điểm cuối bên phải và duy trì vị trí ngoài cùng bên trái sao cho cửa sổ vẫn hợp lệ. Bất cứ khi nào sự khác biệt vượt quá$k$, chúng ta di chuyển con trỏ trái về phía trước cho đến khi điều kiện được khôi phục. Kích thước cửa sổ tối đa nhìn thấy trong quá trình này là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Sắp xếp + Cửa sổ trượt |$O(n \log n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Các bước 

1. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp đảm bảo rằng bất kỳ tập hợp con hợp lệ nào cũng có thể được coi là một khoảng liền kề theo thứ tự này, vì việc mở rộng phạm vi chỉ làm tăng chênh lệch tối đa-tối thiểu. 
2. Khởi tạo hai con trỏ$l = 0$,$r = 0$, và một biến$ans = 1$. Cửa sổ$[l, r]$đại diện cho tập ứng cử viên hiện tại. 
3. Di chuyển$r$từ trái sang phải trên mảng. Ở mỗi bước, bao gồm$A[r]$vào cửa sổ hiện tại. 
4. Trong khi cửa sổ hiện tại vi phạm điều kiện$A[r] - A[l] > k$, tăng$l$. Thao tác này sẽ thu nhỏ cửa sổ từ bên trái cho đến khi nó hợp lệ trở lại. Lý do là bất kỳ phần tử nào ở quá xa$A[r]$trong một mảng được sắp xếp không thể vẫn ở cùng một tập hợp lệ. 
5. Sau khi sửa xong cửa sổ, hãy cập nhật$ans = \max(ans, r - l + 1)$. Điều này theo dõi tập hợp con hợp lệ lớn nhất được thấy cho đến nay. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, phần tử lớn nhất và nhỏ nhất của bất kỳ tập hợp nào được chọn phải đến từ các điểm cuối của tập hợp đó theo thứ tự được sắp xếp. Nếu một cửa sổ vi phạm ràng buộc, phần tử ngoài cùng bên trái luôn là ứng cử viên tốt nhất để loại bỏ, vì nó đóng góp nhiều nhất cho phạm vi. Việc loại bỏ bất kỳ thành phần bên trong nào cũng không làm giảm phạm vi hoạt động một cách hiệu quả. Thuật toán duy trì một cửa sổ luôn tối đa cho điểm cuối bên phải hiện tại và do đó khám phá tất cả các tập hợp con tối đa khả thi mà không bỏ sót các ứng cử viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    l = 0
    ans = 1
    
    for r in range(n):
        while a[r] - a[l] > k:
            l += 1
        ans = max(ans, r - l + 1)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng, điều này rất cần thiết vì nó biến ràng buộc theo cặp toàn cục thành ràng buộc phạm vi. Sau khi được sắp xếp, chúng tôi chỉ cần theo dõi các giá trị nhỏ nhất và lớn nhất trong tập ứng cử viên, tương ứng với các điểm cuối của một đoạn liền kề. 

Vòng lặp hai con trỏ duy trì một cửa sổ hợp lệ. Vòng lặp while bên trong là nơi duy nhất mà con trỏ bên trái di chuyển và mỗi phần tử được loại bỏ tối đa một lần, đảm bảo độ phức tạp tuyến tính sau khi sắp xếp. 

Một lỗi phổ biến là quên sắp xếp và thử mở cửa sổ trượt trên mảng ban đầu. Điều đó không thành công vì điều kiện phụ thuộc vào phạm vi giá trị chứ không phải khoảng cách chỉ mục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 2
a = [3, 1, 4, 1, 5]
```Mảng được sắp xếp:$[1, 1, 3, 4, 5]$| r | một [r] | tôi | cửa sổ | kích thước hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1] | 1 | 
| 1 | 1 | 0 | [1,1] | 2 | 
| 2 | 3 | 0 | [1,1,3] | 3 | 
| 3 | 4 | 1 | [1,3,4] → điều chỉnh l=1 | 3 | 
| 4 | 5 | 2 | [3,4,5] | 3 | 

Câu trả lời cuối cùng là 3. 

Dấu vết này cho thấy con trỏ bên trái chỉ di chuyển như thế nào khi phạm vi vượt quá$k$và cách các giá trị nhỏ cũ hơn bị loại bỏ khi chúng không tương thích với mức tối đa hiện tại. 

### Ví dụ 2 

đầu vào:```
n = 6, k = 0
a = [2, 2, 1, 3, 2, 2]
```Mảng được sắp xếp:$[1, 2, 2, 2, 2, 3]$| r | một [r] | tôi | cửa sổ | kích thước hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1] | 1 | 
| 1 | 2 | 1 | [2] | 1 | 
| 2 | 2 | 1 | [2,2] | 2 | 
| 3 | 2 | 1 | [2,2,2] | 3 | 
| 4 | 2 | 1 | [2,2,2,2] | 4 | 
| 5 | 3 | 5 | [3] | 1 | 

Câu trả lời là 4. 

Điều này thể hiện hành vi đặc biệt khi$k=0$, trong đó cửa sổ thực sự trở thành “tất cả các giá trị giống hệt nhau”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, cửa sổ trượt là tuyến tính | 
| Không gian |$O(1)$hoặc$O(n)$| Phụ thuộc vào việc thực hiện sắp xếp | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, do đó$O(n \log n)$giải pháp nằm trong giới hạn. Quét tuyến tính đảm bảo không có chi phí bổ sung ngoài việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    data = inp.strip().split()
    n = int(data[0])
    k = int(data[1])
    a = list(map(int, data[2:]))

    a.sort()
    l = 0
    ans = 1
    for r in range(n):
        while a[r] - a[l] > k:
            l += 1
        ans = max(ans, r - l + 1)
    return str(ans)

# provided sample (conceptual, since original sample output missing in prompt)
assert run("5 2\n3 1 4 1 5") == "3"

# all equal
assert run("4 10\n7 7 7 7") == "4"

# k = 0
assert run("5 0\n1 2 2 2 3") == "3"

# strictly increasing but small window
assert run("6 1\n1 2 3 4 5 6") == "2"

# single element
assert run("1 100\n42") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | n | trường hợp cạnh chấp nhận đầy đủ | 
| k = 0 trùng lặp | tần số tối đa | ràng buộc bình đẳng | 
| trình tự tăng dần | 2 | cửa sổ trượt thu nhỏ | 
| phần tử đơn | 1 | ranh giới tối thiểu | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, ví dụ đầu vào$[5, 5, 5, 5]$với bất kỳ$k \ge 0$, mảng được sắp xếp không đổi và cửa sổ không bao giờ co lại. Thuật toán tiếp tục mở rộng$r$và luôn tạo ra$ans = n$, cho kết quả đúng. 

Khi$k = 0$, chẳng hạn như$[1, 2, 2, 3, 2]$, sắp xếp cho$[1, 2, 2, 2, 3]$. Cửa sổ cho giá trị 2 mở rộng để bao gồm tất cả các lần xuất hiện, trong khi giá trị 1 và 3 bị loại trừ ngay lập tức do vi phạm. Thuật toán cô lập khối tần số lớn nhất một cách tự nhiên. 

Khi các giá trị có khoảng cách rộng rãi, chẳng hạn như$[1, 100, 200, 300]$với$k = 50$, mọi cửa sổ đều nhỏ. Thuật toán dịch chuyển liên tục$l$chuyển tiếp, đảm bảo rằng không có tập hợp phạm vi lớn không hợp lệ nào đóng góp vào câu trả lời và trả về chính xác 1.
