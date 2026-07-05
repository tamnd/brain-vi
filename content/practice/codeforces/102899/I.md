---
title: "CF 102899I - KK \u4e70\u80a1\u7968"
description: "Chúng ta được cho một chuỗi giá cổ phiếu trong n ngày. Vào mỗi ngày thứ i, cổ phiếu có giá xác định p[i]. KK được phép thực hiện tối đa một giao dịch hoàn chỉnh: anh ta có thể mua một lần vào một ngày nào đó và sau đó bán một lần vào một ngày sau đó."
date: "2026-07-04T08:21:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "I"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 34
verified: true
draft: false
---

[CF 102899I - KK \u4e70\u80a1\u7968](https://codeforces.com/problemset/problem/102899/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi giá cổ phiếu trong n ngày. Vào mỗi ngày thứ i, cổ phiếu có giá xác định p[i]. KK được phép thực hiện tối đa một giao dịch hoàn chỉnh: anh ta có thể mua một lần vào một ngày nào đó và sau đó bán một lần vào một ngày sau đó. Anh ta bắt đầu với số tiền mặt không giới hạn, vì vậy mục tiêu hoàn toàn là tối đa hóa lợi nhuận, được định nghĩa bằng giá bán trừ đi giá mua. Nếu mọi giao dịch có thể đều dẫn đến lợi nhuận không dương thì câu trả lời được xác định là bằng 0. 

Cấu trúc này là một bài toán quét mảng đơn trong đó chúng ta đang cố gắng chọn hai chỉ số i < j để tối đa hóa p[j] − p[i]. 

Ràng buộc n lên tới 100000 ngay lập tức loại trừ mọi phép liệt kê O(n²) của tất cả các cặp. Một giải pháp bậc hai sẽ yêu cầu khoảng 10¹⁰ so sánh trong trường hợp xấu nhất, vượt xa giới hạn 1 giây trong Python. Điều này thúc đẩy chúng ta hướng tới một cách tiếp cận tuyến tính hoặc gần tuyến tính để xử lý mảng trong một lượt hoặc một số lượng nhỏ lượt. 

Một trường hợp phức tạp xuất hiện khi giá không bao giờ tăng. Ví dụ: nếu chuỗi là 5 4 3 2, thì bất kỳ cặp mua-bán nào cũng tạo ra kết quả âm, nhưng vì KK không bị buộc phải giao dịch nên câu trả lời đúng là 0. Một cách tiếp cận ngây thơ, mù quáng lấy chênh lệch tốt nhất có thể trả về sai số âm thay vì kẹp về 0. 

Một trường hợp khác là khi đạt được lợi nhuận tốt nhất bằng cách mua ở mức tối thiểu toàn cầu xảy ra muộn. Ví dụ: trong 10 1 7 3 8 6, mức mua tốt nhất là ở mức 1 và bán ở mức 8, nhưng mức tối thiểu có thể xuất hiện sau một đợt bán hàng tốt tại địa phương nếu chúng ta không cẩn thận trong việc theo dõi lịch sử. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: hãy thử mọi cặp ngày mua và bán có thể. Với mỗi i, chúng ta xem xét tất cả j > i và tính p[j] − p[i], theo dõi mức tối đa. Điều này đúng vì nó làm cạn kiệt tất cả các giao dịch hợp lệ. Tuy nhiên, đối với mỗi n điểm bắt đầu, chúng tôi có thể quét tối đa n điểm tiếp theo, dẫn đến khoảng n(n−1)/2 phép toán, tức là khoảng 5×10⁹ khi n là 100000. Thao tác này không thể chạy trong giới hạn thời gian. 

Quan sát quan trọng là khi chúng ta ấn định ngày bán j, ngày mua tốt nhất có thể chỉ đơn giản là ngày có giá tối thiểu trong số tất cả các ngày trước đó. Thay vì tính toán lại mức tối thiểu này nhiều lần, chúng tôi duy trì nó trong khi quét từ trái sang phải. Ở mỗi bước, chúng tôi biết giá rẻ nhất từng thấy cho đến nay và chúng tôi so sánh giá hiện tại dưới dạng giá bán tiềm năng với giá mua tốt nhất có thể. 

Điều này làm giảm vấn đề xuống còn một lần trong đó chúng tôi liên tục cập nhật hai phần thông tin: giá trị tiền tố tối thiểu và lợi nhuận tốt nhất thu được cho đến nay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Theo dõi một lượt tối thiểu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`min_price`có giá trị rất lớn và`best_profit`bằng 0.`min_price`đại diện cho giá cổ phiếu thấp nhất tính đến thời điểm hiện tại, đây là điểm mua tốt nhất có thể có cho đến nay. 
2. Lặp lại mảng giá từ trái sang phải. 
3. Với mỗi mức giá p[i], hãy tính lợi nhuận nếu chúng ta bán vào ngày thứ i sau khi mua với giá rẻ nhất vào ngày trước đó, đó là p[i] − min_price. 
4. Cập nhật`best_profit`nếu lợi nhuận tính toán này lớn hơn lợi nhuận tốt nhất hiện tại. 
5. Cập nhật`min_price`nếu p[i] nhỏ hơn mức tối thiểu hiện tại, vì doanh số bán hàng trong tương lai nên coi đây là một lựa chọn mua tốt hơn. 
6. Sau khi xử lý tất cả các ngày, đầu ra`best_profit`. 

Điểm quan trọng là thứ tự bên trong vòng lặp: chúng ta phải tính lợi nhuận bằng cách sử dụng mức tối thiểu trước đó trước khi có thể cập nhật nó với giá hiện tại. Nếu không, chúng tôi sẽ cho phép mua và bán trong cùng một ngày một cách không chính xác. 

### Tại sao nó hoạt động 

Tại mọi chỉ số i,`min_price`lưu trữ giá trị tối thiểu của p[0..i−1]. Điều này đảm bảo rằng khi đánh giá ngày i là điểm bán, chúng tôi sẽ xem xét nghiêm ngặt tất cả các ngày mua hợp lệ trước i. Bất kỳ giải pháp tối ưu nào (i, j) đều phải có j là ngày bán hàng và quá trình quét của chúng tôi đảm bảo rằng khi chúng tôi đạt đến j, tôi đã nắm bắt được điều tốt nhất có thể.`min_price`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    
    min_price = p[0]
    best = 0
    
    for i in range(1, n):
        price = p[i]
        
        # sell today using best previous buy
        best = max(best, price - min_price)
        
        # update best buy candidate
        if price < min_price:
            min_price = price
    
    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng một lần truyền qua mảng, duy trì mức lợi nhuận tối thiểu và lợi nhuận tốt nhất. Việc khởi tạo với p[0] là an toàn vì giao dịch đầu tiên có thể xảy ra phải liên quan đến ít nhất một ngày trước đó và chúng tôi chỉ bắt đầu tính toán lợi nhuận từ ngày thứ 2 trở đi. 

Một lỗi phổ biến là cập nhật`min_price`trước khi tính toán lợi nhuận cho ngày hiện tại. Điều đó sẽ cho phép sử dụng cùng một ngày cho cả mua và bán, vi phạm ràng buộc vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1: 1 7 3 8 6 

Chúng tôi theo dõi sự tiến hóa từng ngày. 

| Ngày | Giá | Tối thiểu cho đến nay | Lợi nhuận hôm nay | Lợi nhuận tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | - | 0 | 
| 2 | 7 | 1 | 6 | 6 | 
| 3 | 3 | 1 | 2 | 6 | 
| 4 | 8 | 1 | 7 | 7 | 
| 5 | 6 | 1 | 5 | 7 | 

Lợi nhuận tối đa xảy ra khi mua ở mức 1 và bán ở mức 8, cho 7. Bảng cho thấy mức tối thiểu ổn định sớm như thế nào và tất cả những cải thiện sau này đều đến từ giá bán cao hơn. 

### Ví dụ 2: 3 2 8 

| Ngày | Giá | Tối thiểu cho đến nay | Lợi nhuận hôm nay | Lợi nhuận tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | - | 0 | 
| 2 | 2 | 2 | - | 0 | 
| 3 | 8 | 2 | 6 | 6 | 

Điều này chứng tỏ tầm quan trọng của việc cập nhật mức tối thiểu trước ngày đóng góp cuối cùng với tư cách là ứng cử viên mua. Lựa chọn mua tốt nhất sẽ chuyển sang ngày thứ 2 khi chúng ta gặp phải mức giá thấp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá được xử lý một lần và cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai biến được duy trì | 

Quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn 100000 phần tử. Các thao tác là các phép so sánh và cập nhật số nguyên đơn giản, trong giới hạn thời gian thông thường đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))
    
    min_price = p[0]
    best = 0
    
    for i in range(1, n):
        best = max(best, p[i] - min_price)
        min_price = min(min_price, p[i])
    
    return str(best)

# provided samples
assert run("5\n1 7 3 8 6\n") == "7"
assert run("3\n3 2 8\n") == "6"

# custom cases
assert run("2\n5 4\n") == "0", "monotone decreasing"
assert run("2\n1 10\n") == "9", "simple increasing"
assert run("5\n5 5 5 5 5\n") == "0", "all equal"
assert run("6\n10 1 2 3 4 5\n") == "4", "min early then rising"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 4 | 0 | giao dịch không có lãi | 
| 1 10 | 9 | tính toán lợi nhuận cơ bản | 
| tất cả đều bình đẳng | 0 | trường hợp cạnh giá phẳng | 
| 10 1 2 3 4 5 | 4 | ca tối thiểu sớm | 

## Vỏ cạnh 

Đối với dãy giảm nghiêm ngặt như 5 4 3 2, thuật toán đặt`min_price`ban đầu là 5. Mỗi ngày tiếp theo tạo ra lợi nhuận âm hoặc bằng 0, và`best`vẫn là 0 trong suốt. Việc chạy tối thiểu cập nhật liên tục nhưng không bao giờ giúp tạo ra sự khác biệt tích cực, phù hợp với sản lượng yêu cầu. 

Đối với một chuỗi trong đó mức tối thiểu xảy ra muộn, chẳng hạn như 8 7 6 1 10, thuật toán sẽ tránh được việc cố định mức tối thiểu sớm một cách chính xác. Khi đạt tới 1,`min_price`trở thành 1 và ngày cuối cùng 10 mang lại lợi nhuận 9, được nắm bắt chính xác vì cập nhật`min_price`xảy ra sau khi tính toán lợi nhuận mỗi ngày.
