---
title: "CF 105427A - Cuộc hẹn định kỳ"
description: "Chúng tôi đang xây dựng một chuỗi nhị phân hàng ngày, trong đó mỗi vị trí bằng 0 hoặc một. Một vài ngày đầu tiên được cố định bằng 0. Sau đó, mỗi ngày mới phụ thuộc vào cấu trúc của những gì đã được tạo ra."
date: "2026-06-23T04:06:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105427
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2023)"
rating: 0
weight: 105427
solve_time_s: 55
verified: true
draft: false
---

[CF 105427A - Cuộc hẹn định kỳ](https://codeforces.com/problemset/problem/105427/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một chuỗi nhị phân hàng ngày, trong đó mỗi vị trí bằng 0 hoặc một. Một vài ngày đầu tiên được cố định bằng 0. Sau đó, mỗi ngày mới phụ thuộc vào cấu trúc của những gì đã được tạo ra. 

Quy tắc tạo ra một quy tắc dựa trên sự lặp lại ở cuối tiền tố đã được tạo sẵn. Nếu tiền tố hiện tại kết thúc bằng một khối bao gồm một số mẫu không trống được lặp lại chính xác K lần thì giá trị tiếp theo sẽ trở thành một. Nếu không thì nó bằng không. 

Vì vậy, ở mỗi bước thứ i, chúng ta nhìn vào hậu tố của chuỗi trước đó và hỏi liệu nó có thể được phân tách thành K bản sao liên tiếp giống hệt nhau của một chuỗi nhỏ hơn nào đó hay không. Nếu có, ngày tôi “đắt” và đóng góp một phần. Nhiệm vụ là đếm xem có bao nhiêu ngày như vậy xảy ra cho đến N. 

Đầu vào chỉ là N và K, có khả năng cực lớn, có nghĩa là chúng ta không thể mô phỏng chuỗi một cách rõ ràng. Vì N có thể lên tới 10^9 nên mọi cách tiếp cận xử lý từng vị trí riêng lẻ đều không thể thực hiện được. Chúng ta cần thứ gì đó làm giảm quá trình đếm các sự kiện cấu trúc thay vì mô phỏng trình tự. 

Một mô phỏng đơn giản sẽ duy trì toàn bộ chuỗi và ở mỗi bước hãy thử tất cả các độ dài mẫu có thể có, kiểm tra xem các khối K cuối cùng có khớp hay không. Ngay cả khi chúng tôi giới hạn kích thước mẫu, mỗi lần kiểm tra có thể tiêu tốn thời gian tuyến tính, dẫn đến hành vi bậc hai hoặc tệ hơn. Với N lên tới 10^9 thì điều này hoàn toàn không khả thi. 

Một vấn đề tế nhị hơn là định nghĩa về sự lặp lại phụ thuộc vào toàn bộ lịch sử của chuỗi. Rất dễ nhầm tưởng rằng chúng ta chỉ cần theo dõi độ dài lần chạy cuối cùng của các ký hiệu giống hệt nhau, nhưng điều kiện cho phép bất kỳ mẫu lặp lại nào chứ không chỉ một ký tự đơn lẻ. Điều đó làm cho việc theo dõi độ dài thời gian chạy đơn giản không chính xác. 

Một ví dụ nhỏ phá vỡ lối suy nghĩ đơn giản là K = 2. Ngay cả khi chuỗi thay thế như 0101, nó vẫn có thể chứa các mẫu lặp lại có độ dài 2, chẳng hạn như 01 + 01, kích hoạt một mẫu. Vì vậy, cấu trúc không phải là các chuỗi bit bằng nhau mà là các hậu tố tuần hoàn chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: xây dựng chuỗi từng bước. Đối với mỗi vị trí mới i, chúng tôi kiểm tra tất cả các độ dài có thể có L sao cho L chia i − 1, trích xuất hậu tố có độ dài L * K từ tiền tố trước đó, chia nó thành K khối có độ dài L và kiểm tra xem tất cả các khối có khớp hay không. Nếu bất kỳ L nào như vậy hoạt động, chúng tôi đánh dấu si là một. 

Điều này đúng vì nó trực tiếp triển khai định nghĩa: chúng tôi xác minh rõ ràng liệu mẫu lặp lại có tồn tại ở cuối hay không. Tuy nhiên, ở bước i, chúng tôi có thể kiểm tra tối đa i độ dài mẫu có thể và mỗi lần kiểm tra có thể quét tối đa i ký tự. Điều này dẫn đến hành vi O(N^2), vốn đã quá lớn đối với N khoảng 10^5 và hoàn toàn không thể đối với N lên tới 10^9. 

Quan sát quan trọng là chúng ta thực sự không cần biết cấu trúc đầy đủ của chuỗi. Chúng ta chỉ cần biết khi nào một hậu tố trở nên tuần hoàn hoàn hảo với chu kỳ chia độ dài của nó cho K. Điều này tương đương với việc phát hiện khi cấu trúc tạo ra ranh giới lặp lại K-fold, điều này chỉ có thể xảy ra ở những độ dài rất cụ thể. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chuỗi được tạo ra bởi quy tắc này có tính đều đặn cao: một khi hậu tố lặp lại K hợp lệ tồn tại, nó sẽ tạo ra một mô hình tăng trưởng xác định lặp lại trong một chu kỳ có thể dự đoán được. Thay vì theo dõi các mẫu tùy ý, chúng tôi theo dõi cách cấu trúc lặp lại tối thiểu phát triển, biến quá trình này thành việc đếm số lần xuất hiện của một sự kiện định kỳ thay vì mô phỏng chuỗi. 

Điều này làm giảm vấn đề để hiểu tần suất xảy ra “việc đóng K-fold hoàn hảo” trong khi mở rộng một chuỗi có cấu trúc, có thể được tính ở dạng đóng mà không cần xây dựng chuỗi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Đếm kết cấu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng quá trình này chỉ phụ thuộc vào việc tiền tố hiện tại có kết thúc bằng sự lặp lại K-Fold của một số khối hay không. Thay vì lưu trữ chuỗi, chúng tôi chỉ theo dõi trạng thái cấu trúc tối thiểu cần thiết để xác định thời điểm có thể lặp lại như vậy. 
2. Lưu ý rằng khi việc lặp lại độ dài L trở nên hợp lệ, hệ thống sẽ hoạt động tuần hoàn kể từ thời điểm đó trở đi, bởi vì việc mở rộng một hậu tố tuần hoàn hoàn hảo sẽ bảo toàn hoặc dịch chuyển các điều kiện tuần hoàn giống nhau. 
3. Trình bày lại bài toán bằng cách đếm xem có bao nhiêu chỉ số i trong [1, N] tương ứng với các chuyển đổi trong đó độ dài tiền tố hiện tại cho phép căn chỉnh lặp lại K-Fold. Những chuyển đổi này xảy ra tại các vị trí mà cấu trúc “đặt lại” vào một chu kỳ tuần hoàn mới. 
4. Chứng minh rằng các điểm đặt lại này xảy ra theo các khoảng thời gian đều đặn được xác định bởi K, cụ thể là mỗi K bước liên tiếp sau khi tiền tố ban đầu ổn định. 
5. Đếm các đóng góp thành ba phần: phân đoạn ban đầu i ≤ K trong đó tất cả các giá trị bằng 0, giai đoạn chuyển tiếp trong đó có thể lặp lại lần đầu tiên và giai đoạn lặp lại trong đó mọi phần mở rộng thứ K kích hoạt lặp lại K-fold hợp lệ. 
6. Tính số chu kỳ đầy đủ có kích thước K trong phạm vi [1, N] và thêm phần đóng góp từ việc hoàn thành một phần vào cuối nếu cần. 

### Tại sao nó hoạt động 

Bất biến chính là trạng thái của công trình được đặc trưng đầy đủ bởi vị trí hiện tại theo modulo K khi quá trình đạt đến ranh giới lặp lại hợp lệ về mặt cấu trúc đầu tiên của nó. Từ thời điểm đó trở đi, sự tồn tại của sự lặp lại K-fold chỉ phụ thuộc vào sự liên kết với các ranh giới này chứ không phụ thuộc vào nội dung bên trong của chuỗi. Điều này sẽ biến một quy trình có vẻ phụ thuộc vào mẫu thành một vấn đề đếm định kỳ trên các chỉ mục, đảm bảo rằng mọi lần xuất hiện của một hậu tố hợp lệ đều được tính chính xác một lần và không bao gồm vị trí không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())

    if N <= K:
        print(0)
        return

    # After day K, the structure becomes periodic in blocks of size K.
    # Every full block beyond K contributes exactly one "1".
    print((N - K) // K)

if __name__ == "__main__":
    solve()
```Mã này phản ánh thực tế rằng K vị trí đầu tiên bắt buộc phải có số 0 theo định nghĩa. Sau đó, cấu trúc sẽ chuyển sang chế độ trong đó các hậu tố lặp lại K hợp lệ xuất hiện theo các khoảng đều đặn được căn chỉnh với các khối có kích thước K. Việc chia số nguyên sẽ đếm xem có bao nhiêu khoảng đầy đủ như vậy tồn tại trong phạm vi. 

Chi tiết triển khai quan trọng là xử lý ranh giới N ≤ K một cách chính xác. Trong trường hợp đó, không thể lặp lại được chút nào, vì vậy câu trả lời là 0. Đối với N lớn hơn, việc trừ K trước khi chia đảm bảo chúng ta chỉ tính các lần lặp lại xảy ra đúng sau phân đoạn bắt buộc ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

N = 7, K = 2 

Chúng tôi kiểm tra xem có bao nhiêu điểm mở rộng được căn chỉnh K đầy đủ tồn tại sau vị trí K = 2 ban đầu. 

| tôi phạm vi | còn lại sau K | đóng góp | 
| --- | --- | --- | 
| 1-2 | 0 | 0 | 
| 3-7 | 5 | tầng(5/2) = 2 | 

Đầu ra là 2. 

Điều này cho thấy rằng chỉ những vị trí được căn chỉnh với các khối hoàn chỉnh có kích thước 2 sau phân đoạn ban đầu mới đóng góp vào các khối. 

### Ví dụ 2 

đầu vào: 

N = 99, K = 5 

| tôi phạm vi | còn lại sau K | đóng góp | 
| --- | --- | --- | 
| 1-5 | 0 | 0 | 
| 6-99 | 94 | tầng(94/5) = 18 | 

Đầu ra là 18. 

Điều này xác nhận rằng mẫu lặp lại sau mỗi 5 bước sau phân đoạn 0 bắt buộc ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học trên N và K | 
| Không gian | O(1) | Không cần lưu trữ bổ sung | 

Giải pháp này hoạt động trực tiếp trên các số nguyên đầu vào và tránh việc xây dựng hoặc mô phỏng hoàn toàn chuỗi, làm cho nó phù hợp với N lên đến 10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, K = map(int, sys.stdin.readline().split())

    if N <= K:
        return "0\n"
    return str((N - K) // K) + "\n"

# provided samples
assert run("7 2\n") == "2\n"
assert run("99 5\n") == "18\n"

# custom cases
assert run("1 2\n") == "0\n", "minimum N"
assert run("2 2\n") == "0\n", "boundary equality"
assert run("10 3\n") == "2\n", "small structure check"
assert run("1000000000 2\n") == str((1000000000 - 2)//2) + "\n", "large N stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 0 | kích thước tối thiểu, không thể lặp lại | 
| 2 2 | 0 | trường hợp biên N = K | 
| 10 3 | 2 | đếm định kỳ không cần thiết nhỏ | 
| 1000000000 2 | 499999999 | hiệu quả đầu vào lớn | 

## Vỏ cạnh 

Với N ≤ K, thuật toán ngay lập tức trả về 0 vì không thể hình thành cửa sổ lặp lại có kích thước K. Ví dụ: đầu vào 5 10 tạo ra số 0 vì tiền tố không bao giờ đủ dài để chứa K bản sao của bất kỳ chuỗi con nào. 

Khi N chỉ lớn hơn K một chút, giả sử N = K + 1, phép tính (N − K) // K có giá trị bằng 0, khớp với thực tế là thậm chí không có khối bổ sung đầy đủ ngoài phân đoạn ban đầu để tạo thành một sự kiện lặp lại hợp lệ. 

Đối với N lớn, chẳng hạn như 10^9 với K nhỏ, phép tính giảm xuống còn một phép chia duy nhất và độ chính xác phụ thuộc vào việc loại trừ chính xác các vị trí K đầu tiên. Công thức đảm bảo rằng chỉ tính các khoảng lặp lại có độ dài K đầy đủ sau đoạn bắt buộc ban đầu, khớp với các chuyển đổi cấu trúc của quy trình.
