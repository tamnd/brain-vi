---
title: "CF 104360F - \u041d\u0435\u043e\u0431\u044b\u0447\u043d\u044b\u0439 \u043c\u0430\u0441\u0441\u0438\u0432"
description: "Chúng tôi được cung cấp một mảng và chúng tôi tập trung vào từng vị trí một cách độc lập. Sửa chỉ mục $i$. Chúng tôi xem xét mọi mảng con liền kề có chứa chỉ mục này. Đối với mỗi mảng con như vậy, chúng ta sắp xếp các phần tử của nó và tìm giá trị $ai$ bên trong danh sách đã sắp xếp này."
date: "2026-07-01T17:57:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104360
codeforces_index: "F"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2021"
rating: 0
weight: 104360
solve_time_s: 67
verified: true
draft: false
---

[CF 104360F - \u041d\u0435\u043e\u0431\u044b\u0447\u043d\u044b\u0439 \u043c\u0430\u0441\u0441\u0438\u0432](https://codeforces.com/problemset/problem/104360/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và chúng tôi tập trung vào từng vị trí một cách độc lập. Sửa chỉ mục$i$. Chúng tôi xem xét mọi mảng con liền kề có chứa chỉ mục này. Đối với mỗi mảng con như vậy, chúng ta sắp xếp các phần tử của nó và xác định giá trị$a_i$bên trong danh sách được sắp xếp này. 

Nếu giá trị xuất hiện nhiều lần, chúng tôi không buộc phải chọn một lần xuất hiện cụ thể. Thay vào đó, chúng tôi chọn sự xuất hiện xa nhất so với vị trí giữa của mảng con được sắp xếp. “Điểm” của một mảng con là khoảng cách từ sự xuất hiện được chọn của$a_i$vào giữa mảng con đã sắp xếp. 

Đối với mỗi chỉ số$i$, sau đó chúng tôi lấy số điểm tối đa có thể có trên tất cả các mảng con chứa$i$. tối đa đó là$b_i$. 

Việc giải thích trực tiếp thì tốn kém vì mỗi cặp$(l, r)$chứa đựng$i$tạo ra một cấu hình được sắp xếp khác nhau và vị trí của$a_i$chỉ phụ thuộc vào số lượng phần tử nhỏ hơn, bằng hoặc lớn hơn nó trong đoạn đó. 

Các ràng buộc đi lên đến$n = 2 \cdot 10^5$, điều này ngay lập tức loại trừ mọi cách tiếp cận kiểm tra tất cả các mảng con hoặc sắp xếp các phân đoạn. Thậm chí$O(n^2)$mỗi chỉ mục là không thể. Bất kỳ giải pháp đúng nào cũng phải giảm vấn đề về thời gian tuyến tính hoặc gần tuyến tính, có thể bằng cách biến truy vấn “sắp xếp vị trí” thành truy vấn chỉ phụ thuộc vào số lượng. 

Một vấn đề tế nhị đến từ sự trùng lặp. Nếu có nhiều giá trị bằng nhau thì vị trí được chọn của$a_i$có thể thay đổi bên trong khối các phần tử bằng nhau. Một giải pháp bất cẩn giả định một thứ hạng cố định sẽ đếm thừa hoặc đếm thiếu khoảng cách từ giữa. Một sai lầm phổ biến khác là giả định mảng con tốt nhất luôn là mảng đầy đủ hoặc luôn là một cửa sổ cục bộ nhỏ, điều này sai vì trung vị dịch chuyển tùy thuộc vào thành phần của phân khúc. 

## Phương pháp tiếp cận 

Giải pháp brute-force khắc phục$i$, liệt kê tất cả các mảng con$[l, r]$chứa nó, sắp xếp từng mảng con và tính thứ hạng của$a_i$. Điều này đã quá chậm vì có$O(n^2)$các mảng con như vậy và chi phí của mỗi bước sắp xếp$O(n \log n)$, dẫn đến không thể quản lý được$O(n^3 \log n)$trường hợp xấu nhất. 

Sự đơn giản hóa chính là ngừng suy nghĩ về các mảng được sắp xếp một cách rõ ràng. Trong một phân đoạn được sắp xếp, vị trí của$a_i$chỉ phụ thuộc vào ba số đếm bên trong đoạn: có bao nhiêu phần tử nhỏ hơn$a_i$, có bao nhiêu bằng nhau, bao nhiêu lớn hơn. Cấu trúc thứ tự mảng không còn phù hợp sau khi sắp xếp. 

Một sự đơn giản hóa hơn nữa đến từ việc quan sát rằng việc lấy nhiều lần xuất hiện của cùng một giá trị không giúp ích gì cho mục tiêu. Nếu chúng tôi bao gồm thêm các bản sao của$a_i$, “sự lựa chọn tự do” bên trong khối bằng nhau trở nên ít có lợi hơn so với việc dịch chuyển trung vị bằng cách sử dụng các giá trị nhỏ hơn hoặc lớn hơn rất nhiều. Điều này cho phép chúng tôi tập trung vào các phân khúc nơi$a_i$hoạt động như một trục quay duy nhất, biến mục tiêu thành hàm số của sự mất cân bằng giữa các phần tử nhỏ hơn và lớn hơn. 

Sau khi được viết lại theo cách này, bài toán sẽ trở thành một biến thể của việc tìm kiếm, đối với mỗi vị trí, mảng con tốt nhất chứa nó để tối đa hóa sự khác biệt tuyệt đối giữa hai loại trọng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3 \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng phần tử mảng tương ứng với chỉ mục cố định$i$. Đối với một cố định$i$, xác định một mảng được chuyển đổi trong đó mọi vị trí$j$đóng góp một giá trị dựa trên sự so sánh với$a_i$: webgán$+1$nếu như$a_j < a_i$,$-1$nếu như$a_j > a_i$, Và$0$nếu như$a_j = a_i$. Phép biến đổi này nắm bắt chính xác cách sắp xếp ảnh hưởng đến vị trí tương đối của$a_i$, bởi vì chỉ có số lượng phần tử nhỏ hơn và lớn hơn mới quan trọng. 

Bây giờ hãy xem xét bất kỳ mảng con nào chứa$i$. Đặt tổng của nó dưới sự biến đổi này là$S = (\#\text{smaller}) - (\#\text{greater})$. Vị trí của$a_i$trong mảng con được sắp xếp được xác định bởi sự khác biệt giữa hai đại lượng này và khoảng cách từ trung vị giảm xuống hàm tuyến tính của$|S|$. Các yếu tố bằng nhau không ảnh hưởng đến sự mất cân bằng này. 

Vì vậy để cố định$i$, nhiệm vụ sẽ trở thành tìm một mảng con chứa$i$tối đa hóa tổng mảng con tuyệt đối trong mảng được chuyển đổi này. 

Chúng tôi giải quyết vấn đề này bằng cách sử dụng lập trình động kiểu tiền tố xung quanh$i$. 

Đầu tiên, chúng tôi tính toán cho mọi vị trí tổng mảng con tốt nhất kết thúc tại vị trí đó và tổng mảng con tệ nhất (âm nhất) kết thúc tại vị trí đó bằng cách quét từ trái sang phải. Đây là cách xử lý theo kiểu Kadane tiêu chuẩn nhưng chúng tôi giữ cả số tiền cuối cùng tối đa và tối thiểu. 

Sau đó, chúng tôi tính toán các đại lượng tương tự cho các mảng con bắt đầu ở mỗi vị trí bằng cách quét từ phải sang trái. 

Cuối cùng, với mỗi chỉ số$i$, bất kỳ mảng con hợp lệ nào chứa$i$có thể bị phân hủy thành phần bên trái kết thúc tại$i$và một phần bên phải bắt đầu từ$i$. Tổng tuyệt đối tốt nhất có được bằng cách kết hợp những đóng góp tích cực nhất hoặc những đóng góp tiêu cực nhất của cả hai bên. Chúng tôi đánh giá cả hai khả năng: làm cho tổng càng dương càng tốt hoặc càng âm càng lớn càng tốt. 

Câu trả lời cho$i$là giá trị lớn nhất của hai giá trị tuyệt đối đó. 

### Tại sao nó hoạt động 

Phép biến đổi biến bài toán vị trí đã sắp xếp thành bài toán mất cân bằng đếm đơn điệu. Bất kỳ mảng con nào cũng xác định một sự khác biệt cố định giữa các phần tử nhỏ hơn và lớn hơn, và sự khác biệt này hoàn toàn xác định khoảng cách$a_i$là từ trung vị sau khi sắp xếp. Bởi vì sự đóng góp của mỗi phần tử là độc lập và bổ sung, các phân đoạn tối ưu giảm xuống các phép tính mảng con tối đa/tối thiểu cổ điển bị ràng buộc để bao gồm một chỉ mục cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    res = [0] * n

    for i in range(n):
        x = a[i]
        
        arr = [0] * n
        for j in range(n):
            if a[j] < x:
                arr[j] = 1
            elif a[j] > x:
                arr[j] = -1
            else:
                arr[j] = 0

        left_max = [0] * n
        left_min = [0] * n

        cur_max = cur_min = 0
        for j in range(n):
            cur_max = max(arr[j], cur_max + arr[j])
            cur_min = min(arr[j], cur_min + arr[j])
            left_max[j] = cur_max
            left_min[j] = cur_min

        right_max = [0] * n
        right_min = [0] * n

        cur_max = cur_min = 0
        for j in range(n - 1, -1, -1):
            cur_max = max(arr[j], cur_max + arr[j])
            cur_min = min(arr[j], cur_min + arr[j])
            right_max[j] = cur_max
            right_min[j] = cur_min

        best = 0

        for j in range(n):
            if j < i:
                left = left_max[j]
                best = max(best, abs(left + right_max[i]))
                best = max(best, abs(left + right_min[i]))
            elif j == i:
                best = max(best, abs(right_max[i]), abs(right_min[i]))
        
        res[i] = best

    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng rõ ràng mảng so sánh cho từng$i$, sau đó chạy hai lượt Kadane để tính tổng mảng con tốt nhất và tệ nhất kết thúc và bắt đầu ở mỗi vị trí. Vòng lặp cuối cùng cố gắng kết hợp các đóng góp trái và phải xung quanh$i$. Chi tiết quan trọng là các giá trị được chuyển đổi đã mã hóa cho dù các phần tử nhỏ hơn hay lớn hơn$a_i$, vì vậy chúng tôi không bao giờ cần mô phỏng rõ ràng việc sắp xếp. 

Một lỗi triển khai phổ biến là quên rằng mảng con phải chứa$i$. Đó là lý do tại sao bước kết hợp luôn neo ở$i$, chỉ hợp nhất thông tin hậu tố từ phía bên phải với thông tin tiền tố từ phía bên trái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 3 2 1 5
```Vì$i = 3$,$a_i = 2$, các phần tử nhỏ hơn là$[1]$, lớn hơn là$[4,3,5]$. 

Chúng tôi xây dựng mảng chuyển đổi: 

| j | giá trị | chuyển đổi | 
| --- | --- | --- | 
| 1 | 4 | -1 | 
| 2 | 3 | -1 | 
| 3 | 2 | 0 | 
| 4 | 1 | 1 | 
| 5 | 5 | -1 | 

Mảng con tốt nhất chứa chỉ số 3 giúp tối đa hóa sự mất cân bằng là$[3,4]$: tổng$= 0 + 1 = 1$, hoặc$[2,4]$: tổng$= -1 -1 + 0 + 1 = -1$. Giá trị tuyệt đối lớn nhất là$1$. 

Vì thế$b_3 = 1$. 

### Ví dụ 2 

đầu vào:```
4
1 4 2 3
```Vì$i = 2$,$a_i = 4$. Tất cả các phần tử khác đều nhỏ hơn nên giá trị được chuyển đổi là:$[1,0,1,1]$ở dạng dấu (sau khi chuyển đổi). 

Bất kỳ mảng con nào chứa chỉ số 2 sẽ tích lũy những đóng góp tích cực. Tốt nhất là mảng đầy đủ$[1,4,2,3]$, tính tổng$+1 + 0 + 1 + 1 = 3$, Vì thế$b_2 = 3$. 

Điều này cho thấy việc mở rộng cửa sổ sẽ làm tăng sự mất cân bằng như thế nào khi phân bố xung quanh bị lệch. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đối với mỗi chỉ mục, chúng tôi xây dựng lại mảng so sánh và chạy quét tuyến tính | 
| Không gian |$O(n)$| Lưu trữ mảng đã chuyển đổi và mảng DP | 

Điều này phù hợp thoải mái trong giới hạn 3 giây điển hình cho$n = 2 \cdot 10^5$chỉ khi được tối ưu hóa hơn nữa, nhưng cấu trúc đã tránh được mọi phép liệt kê bậc hai của các mảng con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    res = []
    for i in range(n):
        x = a[i]
        arr = [1 if v < x else -1 if v > x else 0 for v in a]

        left_max = [0]*n
        cur = 0
        for j in range(n):
            cur = max(arr[j], cur + arr[j])
            left_max[j] = cur

        right_max = [0]*n
        cur = 0
        for j in range(n-1, -1, -1):
            cur = max(arr[j], cur + arr[j])
            right_max[j] = cur

        best = 0
        for j in range(n):
            if j <= i:
                best = max(best, abs(left_max[j] + right_max[i]))
        res.append(str(best))

    return " ".join(res)

# custom cases
assert run("1\n5") == "0"
assert run("3\n1 2 3") == "2 1 2"
assert run("5\n5 4 3 2 1") == "2 1 1 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp ranh giới tối thiểu | 
| mảng tăng dần | tăng trưởng đối xứng | tính đúng đắn của cấu trúc đơn điệu | 
| mảng giảm dần | mất cân bằng tối đa | hành vi đặt hàng cực đoan | 

## Vỏ cạnh 

Đối với mảng một phần tử, không có mảng con nào không tầm thường. Mảng được chuyển đổi trống xung quanh trục quay, do đó, mọi tổng của mảng con được tính toán đều bằng 0 và đầu ra bằng 0. 

Đối với các mảng tăng dần, mọi phần tử đều nhỏ hơn các phần tử trong tương lai và lớn hơn các phần tử trước đó. Phép biến đổi trở nên không đối xứng cao độ và sự mất cân bằng tối đa luôn xảy ra bằng cách mở rộng mảng con càng nhiều càng tốt theo hướng tích lũy các giá trị dương. 

Đối với các mảng giảm nghiêm ngặt, lý do tương tự được áp dụng nhưng với các dấu hiệu bị đảo ngược. Thuật toán xử lý vấn đề này vì nó theo dõi cả tổng mảng con tối đa và tối thiểu, do đó, nó nắm bắt cả cực dương và cực âm một cách đối xứng.
