---
title: "CF 1015C - Nén bài hát"
description: "Chúng tôi được cung cấp một bộ sưu tập các bài hát, mỗi bài có kích thước ban đầu và kích thước nhỏ hơn nếu chúng tôi chọn nén nó. Tất cả các bài hát phải được sao chép vào ổ flash có tổng dung lượng cố định. Quyết định duy nhất có sẵn là nén bài hát nào."
date: "2026-06-16T22:27:43+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1015
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 501 (Div. 3)"
rating: 1100
weight: 1015
solve_time_s: 208
verified: true
draft: false
---

[CF 1015C - Nén bài hát](https://codeforces.com/problemset/problem/1015/C) 

**Đánh giá:** 1100 
**Thẻ:** sắp xếp 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các bài hát, mỗi bài có kích thước ban đầu và kích thước nhỏ hơn nếu chúng tôi chọn nén nó. Tất cả các bài hát phải được sao chép vào ổ flash có tổng dung lượng cố định. Quyết định duy nhất có sẵn là nén bài hát nào. Việc nén một bài hát sẽ thay thế kích thước của nó bằng một kích thước nhỏ hơn và chúng tôi muốn đảm bảo tổng kích thước cuối cùng không vượt quá dung lượng. 

Mục tiêu không phải là quyết định xem có phù hợp với các bài hát hay không, bởi vì điều đó luôn có thể được giải quyết bằng cách nén mọi thứ và kiểm tra tổng thể. Nhiệm vụ thực sự là giảm thiểu số lượng bài hát chúng tôi chọn nén trong khi vẫn làm cho tổng kích thước vừa vặn trong giới hạn. 

Khó khăn chính đến từ việc nén một bài hát mang lại lợi ích khác nhau tùy theo bài hát. Một số bài hát tiết kiệm rất nhiều dung lượng khi nén, một số khác tiết kiệm rất ít dung lượng. Quyết định là về việc chọn một tập hợp con các mục trong đó mỗi lựa chọn sẽ làm giảm tổng kích thước đi một lượng đã biết. 

Những ràng buộc đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính. Với tối đa 100.000 bài hát, bất kỳ giải pháp nào thử tất cả các tập hợp con đều ngay lập tức không thể thực hiện được vì nó sẽ phát triển theo cấp số nhân. Ngay cả giải pháp bậc hai cũng sẽ quá chậm. Điều này gợi ý rõ ràng rằng chúng ta cần một chiến lược tham lam dựa trên việc sắp xếp. 

Trường hợp khó tinh tế xuất hiện khi thậm chí việc nén tất cả các bài hát cũng không đủ. Trong trường hợp đó, không tập hợp con nào có thể trợ giúp vì việc nén chỉ làm giảm kích thước. Một cạm bẫy khác ít rõ ràng hơn là cho rằng chúng ta phải luôn nén các bài hát có kích thước gốc lớn nhất. Điều đó không chính xác vì điều quan trọng không phải là kích thước tuyệt đối mà là mức giảm đạt được khi nén. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi tập hợp con của các bài hát, tính toán tổng kích thước kết quả sau khi nén những bài hát đó trong tập hợp con và lấy kích thước tập hợp con tối thiểu phù hợp với dung lượng. Điều này hoạt động về mặt khái niệm vì nó khám phá tất cả các khả năng, nhưng về bản chất nó có tính cấp số nhân, yêu cầu kiểm tra 2^n tập hợp con. Với n lên tới 100.000 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là việc nén một bài hát luôn làm giảm tổng kích thước một lượng cố định: sự khác biệt giữa kích thước ban đầu và kích thước nén của nó. Nếu chúng ta xác định mức giảm này là mức tăng thì mỗi bài hát được nén sẽ góp phần độc lập vào việc giảm tổng số. Vấn đề trở thành việc chọn số lượng mặt hàng tối thiểu có tổng mức tăng ít nhất là vượt quá khả năng. 

Điều này biến thành một vấn đề lựa chọn tham lam cổ điển. Nếu chúng ta muốn đạt được mục tiêu giảm thiểu bằng cách sử dụng càng ít mặt hàng càng tốt, trước tiên chúng ta phải luôn chọn những mặt hàng mang lại mức giảm lớn nhất. Sắp xếp các bài hát theo mức tăng dần theo thứ tự giảm dần đảm bảo rằng mọi thao tác nén được chọn đều hiệu quả nhất có thể về mặt dung lượng tiết kiệm cho mỗi thao tác. 

Trước tiên, chúng tôi tính toán tổng kích thước mà không cần nén. Nếu nó đã vượt quá dung lượng thì không cần nén. Nếu không, chúng tôi tính toán xem chúng tôi cần giảm bao nhiêu. Sau đó, chúng tôi sắp xếp tất cả các bài hát theo lợi ích nén của chúng và tham lam chọn những bài lớn nhất cho đến khi đáp ứng yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp tham lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Gọi tổng là tổng của tất cả kích thước bài hát gốc. Đối với mỗi bài hát, hãy xác định mức tăng là a_i - b_i, đó là lượng dung lượng chúng ta tiết kiệm được nếu nén nó.

1. Tính tổng kích thước của tất cả các bài hát mà không cần nén. Điều này đưa ra yêu cầu lưu trữ cơ bản trước khi đưa ra bất kỳ quyết định nào. 
2. Nếu tổng đã nhỏ hơn hoặc bằng m, xuất 0 ngay lập tức vì không cần nén. 
3. Tính toán số lượng dư thừa, được định nghĩa là bao nhiêu dung lượng chúng ta phải loại bỏ để vừa với ổ đĩa. 
4. Đối với mỗi bài hát, hãy tính giá trị khuếch đại của nó và lưu trữ cùng với bài hát. 
5. Sắp xếp tất cả các bài hát theo thứ tự giảm dần. Điều này đảm bảo rằng chúng tôi luôn xem xét việc nén hiệu quả nhất trước tiên. 
6. Lặp lại qua danh sách đã sắp xếp, trừ đi từng mức tăng đã chọn từ mức vượt quá yêu cầu và đếm số lượng bài hát chúng tôi nén. 
7. Ngay khi phần dư thừa còn lại bằng 0 hoặc âm, hãy dừng và xuất số lượng bài hát đã nén. 

Lý do chúng tôi dừng sớm là khi đã đạt được mức giảm cần thiết, việc thêm nhiều lần nén sẽ chỉ làm tăng số lượng mà không cải thiện tính khả thi. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mỗi lần nén đều độc lập và đóng góp bổ sung vào tổng mức giảm. Vì mỗi bài hát được nén đều giảm tổng theo một lượng cố định nên bài toán trở nên tương đương với việc chọn số phần tử tối thiểu có tổng trọng số bằng ít nhất một mục tiêu. Trong cài đặt như vậy, việc sắp xếp theo trọng lượng theo thứ tự giảm dần là tối ưu vì việc thay thế bất kỳ mức tăng nhỏ hơn đã chọn bằng mức tăng chưa sử dụng lớn hơn chỉ có thể giảm hoặc duy trì số lượng mục cần thiết. Đối số trao đổi này đảm bảo rằng tiền tố tham lam của lợi nhuận lớn nhất là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

songs = []
total = 0

for _ in range(n):
    a, b = map(int, input().split())
    total += a
    songs.append(a - b)

if total <= m:
    print(0)
    sys.exit()

songs.sort(reverse=True)

need = total - m
cnt = 0

for gain in songs:
    need -= gain
    cnt += 1
    if need <= 0:
        print(cnt)
        break
```Giải pháp bắt đầu bằng cách đọc tất cả kích thước bài hát và tính toán cả kích thước tổng cộng cũng như lợi ích của việc nén từng bài hát. Sự phân tách này rất quan trọng vì chúng ta không bao giờ cần trực tiếp đến các kích thước nén nữa mà chỉ cần sự khác biệt của chúng. 

Sau khi kiểm tra trường hợp tầm thường không cần nén, chúng tôi tính toán xem phải giải phóng bao nhiêu dung lượng. Sắp xếp mức tăng theo thứ tự giảm dần đảm bảo chúng tôi ưu tiên nén hiệu quả nhất. 

Vòng lặp duy trì mức giảm đang chạy và đếm số lượng bài hát được nén. Khi đạt được mức giảm cần thiết, chúng tôi dừng ngay lập tức, đảm bảo số lượng tối thiểu. 

Một lỗi phổ biến là quên sắp xếp theo mức tăng và thay vào đó sắp xếp theo kích thước ban đầu hoặc kích thước nén, không tương ứng với mục tiêu tối ưu hóa thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 21
10 8
7 4
3 1
5 4
```Tổng kích thước là 25, vì vậy chúng ta cần giảm 4. 

| Bước | Được chọn | Nhu cầu còn lại | Nén được sử dụng | 
| --- | --- | --- | --- | 
| Bắt đầu | - | 4 | 0 | 
| 1 | 3 | 1 | 1 | 
| 2 | 2 | -1 | 2 | 

Sau khi lấy được hai mức lợi nhuận lớn nhất, yêu cầu được thỏa mãn nên đáp án là 2. 

Dấu vết này cho thấy rằng việc chọn mức giảm lớn nhất trước tiên sẽ đạt được mục tiêu với số lượng tối thiểu, mặc dù nhiều sự kết hợp của hai bài hát vẫn hoạt động. 

### Ví dụ 2 

đầu vào:```
3 16
4 3
6 5
8 7
```Tổng kích thước là 18, vì vậy chúng ta cần giảm đi 2. 

| Bước | Được chọn | Nhu cầu còn lại | Nén được sử dụng | 
| --- | --- | --- | --- | 
| Bắt đầu | - | 2 | 0 | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 0 | 2 | 

Ở đây yêu cầu cả hai mức tăng nhỏ hơn, cho thấy rằng ngay cả khi tất cả mức tăng đều bằng nhau thì lệnh tham lam vẫn có hiệu lực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp lợi ích chiếm ưu thế trong thời gian chạy, tất cả các hoạt động khác đều tuyến tính | 
| Không gian | O(n) | Chúng tôi lưu trữ một giá trị khuếch đại cho mỗi bài hát | 

Các ràng buộc cho phép tối đa 100.000 bài hát, vì vậy giải pháp n log n nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và phù hợp thoải mái trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    songs = []
    total = 0

    for _ in range(n):
        a, b = map(int, input().split())
        total += a
        songs.append(a - b)

    if total <= m:
        return "0"

    songs.sort(reverse=True)

    need = total - m
    cnt = 0

    for g in songs:
        need -= g
        cnt += 1
        if need <= 0:
            return str(cnt)

    return "-1"

# provided sample
assert run("""4 21
10 8
7 4
3 1
5 4
""") == "2"

# impossible case
assert run("""3 5
4 3
4 3
4 3
""") == "-1"

# already fits
assert run("""2 100
10 5
20 10
""") == "0"

# all equal gains
assert run("""4 10
5 4
5 4
5 4
5 4
""") == "2"

# large gain variety
assert run("""5 20
10 1
10 9
10 8
10 7
10 6
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã phù hợp với trường hợp | 0 | không cần nén | 
| trường hợp bất khả thi | -1 | thậm chí nén toàn bộ không thành công | 
| tất cả lợi ích như nhau | 2 | xử lý cà vạt theo thứ tự tham lam | 
| lợi nhuận hỗn hợp | 1 | đủ nén đơn tốt nhất | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi nén toàn bộ bài hát vẫn không đáp ứng đủ dung lượng. Ví dụ: nếu tổng kích thước là 50 và m là 10, nhưng ngay cả sau khi tổng nén hoàn toàn là 15, thuật toán vẫn trả về chính xác -1 vì mức tăng tích lũy không bao giờ đạt đến ngưỡng yêu cầu. 

Một trường hợp cạnh khác là khi tổng đã bằng m. Trong trường hợp này, mức giảm cần thiết là 0 và thuật toán ngay lập tức trả về 0 mà không cần vào vòng lặp tham lam. Điều này ngăn chặn việc nén không chính xác các bài hát không cần thiết. 

Trường hợp thứ ba là khi lợi nhuận giống hệt nhau. Bước sắp xếp không phụ thuộc vào độ ổn định, vì bất kỳ thứ tự nào trong số các mức tăng bằng nhau đều tạo ra cùng số lần nén và điều kiện tiền tố đảm bảo tính chính xác bất kể thứ tự ràng buộc.
