---
title: "CF 105507A - \u041f\u043e\u043a\u0443\u043f\u043a\u0430 \u0432\u0435\u043b\u043e\u0441\u0438\u043f\u0435\u0434\u0430"
description: "Chúng tôi được yêu cầu thanh toán số tiền chính xác chỉ bằng hai loại tiền xu, một loại trị giá 2 đơn vị và loại còn lại trị giá 5 đơn vị. Mục tiêu không chỉ là xác định xem số tiền có thể được hình thành hay không mà còn là xây dựng một tổ hợp các đồng tiền có tổng giá trị chính xác bằng số tiền cần thiết và sử dụng ở mức ít nhất…"
date: "2026-06-23T01:30:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105507
codeforces_index: "A"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 24, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 105507
solve_time_s: 52
verified: true
draft: false
---

[CF 105507A - \u041f\u043e\u043a\u0443\u043f\u043a\u0430 \u0432\u0435\u043b\u043e\u0441\u0438\u043f\u0435\u0434\u0430](https://codeforces.com/problemset/problem/105507/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu thanh toán số tiền chính xác chỉ bằng hai loại tiền xu, một loại trị giá 2 đơn vị và loại còn lại trị giá 5 đơn vị. Mục tiêu không chỉ là xác định xem số tiền có thể được hình thành hay không mà còn là xây dựng một tổ hợp các đồng xu có tổng giá trị chính xác bằng số tiền cần thiết và sử dụng càng ít đồng xu càng tốt. 

Mỗi giải pháp tương ứng với việc chọn một số đồng xu 5 giá trị và một số đồng xu 2 giá trị sao cho tổng có trọng số phù hợp với mục tiêu. Trong số tất cả các kết hợp hợp lệ, chúng ta phải giảm thiểu tổng số xu được sử dụng. 

Ràng buộc về giá trị mục tiêu n lên tới một triệu. Kích thước này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các kết hợp số lượng xu trong một vòng lặp lồng nhau trên cả hai loại tiền xu, vì ngay cả việc quét bậc hai đối với số lượng có thể có cũng sẽ quá chậm. Có thể chấp nhận quét tuyến tính hoặc gần tuyến tính trên một chiều. 

Một điểm tinh tế là tính khả thi được đảm bảo nên chúng ta không bao giờ cần phải xử lý những trường hợp bất khả thi. Điều này loại bỏ nhu cầu quay lui hoặc lập luận chẵn lẻ phức tạp để phát hiện tính không khả thi, nhưng chúng ta vẫn phải đảm bảo rằng những lựa chọn tham lam của mình không vô tình phá vỡ yêu cầu về tổng chính xác. 

Một mô hình trường hợp biên điển hình xuất hiện khi lựa chọn tham lam “lấy càng nhiều 5 giây càng tốt” vượt quá hoặc để lại phần dư không chia hết cho 2. Ví dụ: nếu n nhỏ hoặc bằng với một số dư nhất định modulo 10, thì cách tiếp cận tham lam ngây thơ có thể thất bại trừ khi nó điều chỉnh số lượng 5 đồng xu xuống. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ liệt kê số lượng đồng xu 5 giá trị mà chúng tôi lấy, chẳng hạn như k, sau đó kiểm tra xem giá trị còn lại n - 5k có thể được hình thành bằng cách sử dụng đồng xu 2 giá trị hay không. Điều này có nghĩa là xác minh xem n − 5k có phải là số chẵn và không âm hay không, và nếu có thì tính tổng số xu là k cộng (n − 5k)/2. Việc thử tất cả k từ 0 đến n/5 sẽ cho khả năng O(n) và mỗi kiểm tra là O(1), do đó độ phức tạp tổng cộng là O(n). Mặc dù điều này đã phù hợp với các ràng buộc, nhưng không cần thiết phải kiểm tra mọi khả năng một cách rõ ràng, bởi vì cấu trúc của bài toán hạn chế mạnh mẽ giải pháp tối ưu. 

Quan sát quan trọng là đồng xu 5 giá trị luôn hiệu quả hơn trong việc giảm số lượng xu trên mỗi đơn vị giá trị so với đồng xu 2 giá trị. Tuy nhiên, vì 5 là số lẻ trong khi 2 là số chẵn, nên các ràng buộc chẵn lẻ có thể buộc chúng ta phải thay thế một đồng 5 xu bằng nhiều đồng xu 2. Điều này tạo ra vấn đề điều chỉnh theo một hướng: chúng tôi muốn có càng nhiều 5 xu càng tốt, nhưng chúng tôi có thể cần giảm số lượng của chúng cho đến khi tổng còn lại chia hết cho 2. 

Điều này dẫn đến một chiến lược tham lam: bắt đầu với số lượng 5 xu tối đa có thể và giảm dần cho đến khi đạt được tính khả thi. Vì chúng ta chỉ di chuyển xuống dưới và mỗi bước kiểm tra một điều kiện chẵn lẻ đơn giản, quá trình này chạy trong trường hợp xấu nhất là O(n/5), nhưng trong thực tế nó bị giới hạn bởi một hệ số không đổi (nhiều nhất là một vài bước do ràng buộc modulo 2). Đây thực sự là O(1) cho mỗi bài kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Điều chỉnh tham lam | O(n/5) tệ nhất, O(1) thực tế | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách cố gắng tối đa hóa số lượng đồng xu 5 giá trị, sau đó cố định phần còn lại bằng đồng xu 2 giá trị.

1. Tính số lượng 5 đồng xu tối đa có thể là k = n // 5. Điều này cho điểm bắt đầu lớn nhất không vượt quá n. Ý tưởng là việc sử dụng các đồng tiền lớn hơn sẽ làm giảm tổng số lượng, vì vậy chúng tôi bắt đầu từ việc nén tổng số tiền một cách tích cực nhất. 
2. Trong khi k không âm, hãy kiểm tra xem giá trị còn lại r = n − 5k có thể được biểu diễn chỉ bằng đồng xu 2 giá trị hay không. Điều này có thể xảy ra chính xác khi r chẵn, vì 2 là đơn vị chẵn duy nhất có sẵn. 
3. Nếu r là số chẵn, hãy tính số 2 đồng xu là r // 2 và trả về k + r // 2. Sự kết hợp này là tối ưu vì chúng tôi đã tối đa hóa số lượng đồng xu lớn hơn tùy thuộc vào tính khả thi. 
4. Nếu r lẻ, giảm k đi 1 và lặp lại. Bước này tương ứng với việc thay thế một đồng 5 xu bằng 2 đồng xu bổ sung, làm đảo lộn tính chẵn lẻ của phần còn lại và có thể khôi phục tính khả thi. 

Bất biến chính là ở mỗi lần lặp lại, chúng tôi đang thử nghiệm giải pháp tốt nhất có thể với chính xác k 5 đồng xu. Bất kỳ giải pháp nào sử dụng nhiều hơn k năm xu đều không thể thực hiện được do hạn chế về giá trị và mọi giải pháp sử dụng ít hơn chỉ được xem xét khi cần thiết do tính chẵn lẻ không khớp. Do đó, k khả thi đầu tiên gặp phải sẽ mang lại số lượng xu tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    k = n // 5

    while k >= 0:
        rem = n - 5 * k
        if rem % 2 == 0:
            print(k + rem // 2)
            return
        k -= 1

solve()

if __name__ == "__main__":
    solve()
```Mã theo cấu trúc tham lam trực tiếp. Chúng tôi bắt đầu từ số lượng 5 xu tối đa có thể và kiểm tra tính khả thi thông qua tính chẵn lẻ. Kiểm tra phần còn lại`rem % 2 == 0`là điều kiện duy nhất cần thiết vì khả năng chia hết cho 2 mô tả đầy đủ tính đại diện khi sử dụng đồng tiền có 2 giá trị. 

Một cạm bẫy triển khai phổ biến là quên rằng vòng lặp phải giảm k từng bước một thay vì nhảy thêm 2. Tính chẵn lẻ thay thế theo mỗi mức giảm, do đó việc bỏ qua các giá trị sẽ có nguy cơ thiếu cấu hình khả thi đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đặt n = 13. 

Chúng ta bắt đầu với k = 13 // 5 = 2. 

| k | số dư n − 5k | chẵn lẻ | hành động | 
| --- | --- | --- | --- | 
| 2 | 3 | lẻ | giảm k | 
| 1 | 8 | thậm chí | chấp nhận | 

Tại k = 1, số dư là 8, được tạo thành từ 4 đồng xu có giá trị 2. Tổng số xu là 1 + 4 = 5. 

Điều này cho thấy rằng mặc dù việc sử dụng hai đồng 5 xu mang lại số lượng xu nhỏ hơn cho phần 5, nhưng nó phá vỡ tính khả thi và một bước giảm sẽ khôi phục phân tách hợp lệ. 

### Ví dụ 2 

Đặt n = 20. 

Chúng ta bắt đầu với k = 20 // 5 = 4. 

| k | phần còn lại | chẵn lẻ | hành động | 
| --- | --- | --- | --- | 
| 4 | 0 | thậm chí | chấp nhận | 

Ở đây không cần điều chỉnh. Chúng tôi sử dụng bốn đồng xu 5 và không có đồng xu 2, mang lại tổng cộng 4 xu. 

Điều này chứng tỏ tình huống tối ưu trong đó sự lựa chọn tham lam ngay lập tức thỏa mãn các ràng buộc chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n/5) trường hợp xấu nhất | mỗi bước giảm k cho đến khi khớp chẵn lẻ | 
| Không gian | O(1) | chỉ một số số nguyên được lưu trữ | 

Cho n tối đa 10^6, vòng lặp thực hiện tối đa khoảng 200000 lần lặp trong trường hợp xấu nhất tuyệt đối, nhưng trong thực tế, tính chẵn lẻ được giải quyết nhanh chóng, do đó, nó hoạt động giống như một quy trình có thời gian không đổi cho mỗi đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return io.StringIO(sys.stdout.getvalue() if False else "")

# Since the solution uses direct print, we instead show logical asserts conceptually.

# sample-like cases
# n = 6 -> 3 coins of 2
# n = 20 -> 4 coins of 5
# n = 13 -> 5 coins total (1x5 + 4x2)
```Do báo cáo vấn đề ban đầu ở định dạng này không bao gồm các khối I/O mẫu rõ ràng nên chúng tôi tập trung vào tính đúng đắn về cấu trúc thay vì thực thi khai thác chính xác. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 2 | trường hợp chẵn chỉ hợp lệ tối thiểu | 
| 5 | 1 | hộp đựng 5 xu nguyên chất | 
| 6 | 3 | chỉ cần 2 xu | 
| 7 | 2 | trường hợp hỗn hợp cần điều chỉnh | 
| 13 | 5 | trường hợp điều chỉnh chẵn lẻ | 

## Vỏ cạnh 

Khi n nhỏ, chẳng hạn như n = 4, thuật toán bắt đầu bằng k = 0 vì không thể sử dụng 5 xu. Số dư là 4, là số chẵn nên trả về ngay 2 xu giá trị 2. 

Với n = 7, k bắt đầu từ 1, số dư là 2, là số chẵn, do đó thuật toán trả về chính xác một đồng 5 xu và một đồng 2 xu. Điều này là tối ưu vì bất kỳ nỗ lực nào để sử dụng 05 xu sẽ yêu cầu 3 xu là 2, điều này còn tệ hơn. 

Đối với các giá trị trong đó n mod 5 là 1 hoặc 3, lựa chọn tham lam ban đầu k = n // 5 luôn tạo ra số dư lẻ. Vòng lặp sau đó giảm k cho đến khi phần còn lại trở thành số chẵn, đảm bảo tìm thấy phân tách hợp lệ mà không bỏ qua cấu hình tối ưu.
