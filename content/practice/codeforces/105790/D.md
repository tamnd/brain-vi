---
title: "CF 105790D - Độ lệch hướng"
description: "Một con tàu vũ trụ đang tiến đến bãi đáp. Giữa tàu và bãi đáp có dãy núi $N$. Con tàu chuyển động về phía trước với vận tốc không đổi 1 km/s và đồng thời lao xuống với vận tốc không đổi 1 km/s."
date: "2026-06-26T03:49:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "D"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 34
verified: true
draft: false
---

[CF 105790D - Độ lệch lộ trình](https://codeforces.com/problemset/problem/105790/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một con tàu vũ trụ đang tiến đến bãi đáp. Giữa tàu và bãi đáp có$N$núi. Con tàu chuyển động về phía trước với vận tốc không đổi 1 km/s và đồng thời lao xuống với vận tốc không đổi 1 km/s. 

Nếu con tàu bắt đầu hạ độ cao từ độ cao$H$, sau đó một giây nó vượt qua ngọn núi đầu tiên ở độ cao$H-1$, sau hai giây nó vượt qua ngọn núi thứ hai ở độ cao$H-2$và nói chung nó vượt qua$i$-ngọn núi thứ ở độ cao$H-(i+1)$khi sử dụng lập chỉ mục dựa trên số không. 

Mỗi ngọn núi có độ cao$h_i$. Con tàu phải ở trên mọi ngọn núi mà nó đi qua. Nhiệm vụ là tìm độ cao ban đầu nhỏ nhất$H$đảm bảo không có va chạm. 

Đầu vào bao gồm số lượng núi và độ cao của chúng. Đầu ra là một số nguyên duy nhất, độ cao khởi đầu an toàn tối thiểu. 

Độ cao của núi có thể lớn tới$10^9$. Điều này ngay lập tức loại trừ mọi mô phỏng trên các giá trị độ cao hoặc bất kỳ phương pháp tiếp cận nào thử tất cả các độ cao bắt đầu có thể có. Số lượng núi có thể đủ lớn để giải pháp chỉ xử lý mỗi núi một lần, điều này gợi ý$O(N)$quét. 

Nguồn gốc chính của sai lầm là sự bất bình đẳng nghiêm ngặt. Con tàu không thể chỉ đến được đỉnh núi mà nó phải ở trên đỉnh núi. 

Coi như:```
1
10
```Nếu độ cao ban đầu là 11 thì tàu đạt độ cao 10 khi vượt núi. Đó là một vụ va chạm vì độ cao không hoàn toàn lớn hơn độ cao của ngọn núi. Câu trả lời đúng là:```
12
```Một sai lầm dễ mắc phải khác là quên rằng con tàu sẽ mất độ cao trước khi đến được ngọn núi đầu tiên. 

Ví dụ:```
3
2 2 1
```Ở độ cao 4, tàu tới ngọn núi thứ nhất ở độ cao 3, ngọn núi thứ hai ở độ cao 2 và va chạm với ngọn núi thứ hai. Câu trả lời đúng là:```
5
```Cạm bẫy thứ ba là sử dụng các chỉ số dựa trên một và dựa trên 0 một cách không nhất quán. 

Ví dụ:```
2
1 100
```Đối với ngọn núi thứ hai, con tàu ở độ cao$H-2$, không$H-1$. Điều kiện đúng sẽ trở thành$H > 102$, đưa ra câu trả lời:```
103
```Sơ đồ lập chỉ mục sai sẽ tạo ra 102. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử từng độ cao xuất phát của ứng viên. Đối với mỗi độ cao$H$, mô phỏng chuyến bay và xác minh xem mọi ngọn núi đã được dọn sạch hay chưa. Mô phỏng này đúng vì nó kiểm tra chính xác các điều kiện được mô tả trong bài toán. 

Vấn đề là độ cao yêu cầu có thể vượt quá$10^9$. Việc kiểm tra mọi độ cao có thể cho đến câu trả lời sẽ cần hàng tỷ lần kiểm tra, điều này hoàn toàn không khả thi. 

Một phiên bản thông minh hơn một chút sẽ thực hiện tìm kiếm nhị phân trên câu trả lời. Đối với độ cao cố định$H$, chúng tôi có thể xác minh sự an toàn trong$O(N)$thời gian. Vì câu trả lời nằm trong phạm vi gần đúng$10^9$, tìm kiếm nhị phân cần khoảng 31 lần lặp, cho$O(N \log 10^9)$. 

Quan sát chính loại bỏ ngay cả tìm kiếm nhị phân. 

Khi đi qua núi$i$, độ cao của tàu bằng:$$H - (i+1)$$Để tránh va chạm:$$H - (i+1) > h_i$$Sắp xếp lại:$$H > h_i + i + 1$$Vì điều này phải được áp dụng cho mọi ngọn núi,$H$phải lớn hơn giá trị lớn nhất của$h_i + i + 1$. 

Số nguyên nhỏ nhất thỏa mãn yêu cầu đó là:$$H = \max(h_i + i + 1) + 1$$mà cũng có thể được viết là:$$H = \max(h_i + i + 2)$$Bây giờ câu trả lời có được bằng một lần quét qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot A)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân |$O(N \log A)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(N)$|$O(1)$| Đã chấp nhận | 

Đây$A$biểu thị tầm quan trọng của câu trả lời. 

## Hướng dẫn thuật toán 

1. Đọc$N$và độ cao của núi. 
2. Khởi tạo`answer = 0`. 
3. Đối với mỗi ngọn núi tại chỉ số`i`: 

Tính toán`h[i] + i + 2`. 

Giá trị này chính xác là độ cao xuất phát tối thiểu cần thiết để vượt qua ngọn núi cụ thể đó một cách an toàn. 
4. Cập nhật`answer`với giá trị tối đa được thấy cho đến nay. 

Mỗi ngọn núi áp đặt giới hạn dưới cho độ cao bắt đầu. Giới hạn dưới mạnh nhất quyết định câu trả lời cuối cùng. 
5. Đầu ra`answer`. 

### Tại sao nó hoạt động 

Đối với núi$i$, con tàu đạt đến độ cao$H-(i+1)$. Yêu cầu an toàn:$$H-(i+1) > h_i$$tương đương với:$$H \ge h_i+i+2$$bởi vì$H$là một số nguyên. 

Mỗi ngọn núi tạo ra một giới hạn dưới như vậy. Bất kỳ độ cao nào nhỏ hơn giới hạn tối đa đều không thành công đối với ít nhất một ngọn núi. Bản thân giới hạn cực đại thỏa mãn đồng thời mọi bất đẳng thức. Vì nó vừa đủ vừa cần thiết nên đây là câu trả lời hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
h = list(map(int, input().split()))

ans = 0

for i in range(n):
    ans = max(ans, h[i] + i + 2)

print(ans)
```Việc thực hiện phản ánh trực tiếp đạo hàm toán học. 

biểu hiện`h[i] + i + 2`xuất phát từ việc chuyển đổi bất đẳng thức nghiêm ngặt thành giới hạn dưới số nguyên. sử dụng`+1`thay vì`+2`sẽ không chính xác vì con tàu phải ở ngay trên núi. 

Vòng lặp tính toán yêu cầu của mỗi ngọn núi và chỉ giữ lại ngọn núi lớn nhất. Không cần bộ nhớ bổ sung ngoài mảng đầu vào và mức tối đa đang chạy. 

Số nguyên Python tự động xử lý các giá trị lớn hơn$10^9$, vì vậy tràn không phải là một mối quan tâm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 2 1
```| tôi | h[i] | h[i] + tôi + 2 | Tối đa hiện tại | 
| --- | --- | --- | --- | 
| 0 | 2 | 4 | 4 | 
| 1 | 2 | 5 | 5 | 
| 2 | 1 | 5 | 5 | 

Trả lời:```
5
```Ngọn núi thứ hai là yếu tố hạn chế. Xuất phát ở độ cao 5 sẽ quét sạch mọi ngọn núi, trong khi độ cao 4 va chạm với ngọn núi thứ hai. 

### Ví dụ 2 

đầu vào:```
5
4 2 3 1 5
```| tôi | h[i] | h[i] + tôi + 2 | Tối đa hiện tại | 
| --- | --- | --- | --- | 
| 0 | 4 | 6 | 6 | 
| 1 | 2 | 5 | 6 | 
| 2 | 3 | 7 | 7 | 
| 3 | 1 | 6 | 7 | 
| 4 | 5 | 11 | 11 | 

Trả lời:```
11
```Ngọn núi cuối cùng buộc phải có độ cao xuất phát cao nhất. Bất kỳ giá trị nhỏ hơn nào sẽ đặt con tàu ở độ cao tối đa là 5 khi đi qua ngọn núi đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Một lần quét qua những ngọn núi | 
| Không gian |$O(1)$| Chỉ lưu trữ mức tối đa đang chạy | 

Thuật toán thực hiện một lượng công việc không đổi trên mỗi ngọn núi và không bao giờ xem lại các phần tử. Điều này thoải mái phù hợp với giới hạn lập trình cạnh tranh điển hình ngay cả khi$N$là rất lớn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    h = list(map(int, input().split()))

    ans = 0
    for i in range(n):
        ans = max(ans, h[i] + i + 2)

    return str(ans)

# provided samples
assert run("3\n2 2 1\n") == "5", "sample 1"
assert run("1\n10\n") == "12", "sample 2"
assert run("5\n3 4 2 3 1\n") == "8", "sample 3"

# custom cases
assert run("1\n1\n") == "3", "minimum size"
assert run("4\n5 5 5 5\n") == "10", "all equal"
assert run("2\n1 100\n") == "103", "indexing boundary"
assert run("3\n1000000000 1000000000 1000000000\n") == "1000000004", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`3`| Ví dụ nhỏ nhất có thể | 
|`4 / 5 5 5 5`|`10`| Mọi chiều cao đều bằng nhau | 
|`2 / 1 100`|`103`| Giảm độ cao chính xác cho mỗi ngọn núi | 
|`3 / 10^9 10^9 10^9`|`1000000004`| Giá trị lớn và xử lý số nguyên | 

## Vỏ cạnh 

Hãy xem xét:```
1
10
```Thuật toán tính toán:$$10 + 0 + 2 = 12$$và đầu ra 12. 

Kiểm tra bằng tay, độ cao 11 chạm đến ngọn núi ở độ cao 10, không vượt quá nó một cách nghiêm ngặt. Sự bất bình đẳng nghiêm ngặt được xử lý chính xác. 

Coi như:```
3
2 2 1
```Các giá trị được tính toán là:$$4,\ 5,\ 5$$vậy đáp án là 5 

Ở độ cao 4, tàu vượt qua ngọn núi thứ hai ở độ cao 2 và va chạm. Yêu cầu tối đa xác định chính xác độ cao an toàn tối thiểu. 

Coi như:```
2
1 100
```Thuật toán tính toán:$$1+0+2=3$$Và$$100+1+2=103$$đưa ra câu trả lời 103. 

Ngọn núi thứ hai đạt được sau hai giây chứ không phải một giây. Công thức tự động kết hợp độ cao bị mất chính xác thông qua thuật ngữ chỉ số`i + 1`, tránh mắc phải những sai lầm nhỏ nhặt.
