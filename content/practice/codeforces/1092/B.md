---
title: "CF 1092B - Thành lập đội"
description: "Chúng ta được cung cấp một mảng các số nguyên biểu thị trình độ kỹ năng của học sinh. Các học sinh phải được ghép thành các cặp rời rạc chính xác $frac{n}{2}$, để mỗi học sinh được sử dụng đúng một lần. Một cặp chỉ “hợp lệ” nếu cả hai học sinh trong đó đều có trình độ kỹ năng giống hệt nhau."
date: "2026-06-13T04:29:18+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1092
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 527 (Div. 3)"
rating: 800
weight: 1092
solve_time_s: 171
verified: true
draft: false
---

[CF 1092B - Thành lập nhóm](https://codeforces.com/problemset/problem/1092/B) 

**Đánh giá:** 800 
**Thẻ:** sắp xếp 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên biểu thị trình độ kỹ năng của học sinh. Các học sinh phải được ghép thành một cách chính xác$\frac{n}{2}$cặp rời rạc, vì vậy mỗi học sinh được sử dụng đúng một lần. Một cặp chỉ “hợp lệ” nếu cả hai học sinh trong đó đều có trình độ kỹ năng giống hệt nhau. Vì các kỹ năng chỉ có thể được tăng lên (không bao giờ giảm), nên chúng tôi được phép tăng từng học sinh bất kỳ số lần nào, trả một đơn giá cho mỗi lần tăng. 

Nhiệm vụ là chọn cấu trúc ghép nối và các số gia tăng sao cho mỗi cặp bao gồm các giá trị bằng nhau, đồng thời giảm thiểu tổng số các gia số được thực hiện trên tất cả học sinh. 

Quan sát chính từ các ràng buộc là$n \le 100$, do đó, ngay cả lý luận bậc hai hoặc bậc ba theo cặp hoặc sắp xếp cũng hoàn toàn an toàn. Chúng tôi không lo lắng về việc tối ưu hóa tiệm cận ngoài$O(n^2)$, điều này gợi ý rằng chúng ta nên tập trung vào chiến lược tham lam hoặc ghép nối hơn là bất kỳ cấu trúc dữ liệu hoặc DP phức tạp nào trên các trạng thái lớn. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là trước tiên hãy thử khớp các giá trị bằng nhau rồi sau đó tham lam sửa chữa những phần còn lại. Ví dụ: nếu các giá trị bị mất cân bằng nghiêm trọng, việc sửa các kết quả khớp cục bộ sớm có thể buộc phải điều chỉnh tốn kém trong tương lai. Một cạm bẫy khác là ghép nối không có phân loại hoặc cấu trúc, điều này có thể dễ dàng bỏ lỡ các ghép nối tối ưu toàn cầu. 

Ví dụ, nếu chúng ta lấy`1 100 2 3`, một cặp đôi ngây thơ như`(1,100)`Và`(2,3)`có vẻ hợp lý về mặt cấu trúc, nhưng chi phí rất lớn. Một sự ghép nối toàn cầu tốt hơn là`(1,2)`Và`(3,100)`giúp trải rộng việc điều chỉnh giá trị lớn hiệu quả hơn. 

Điều này cho thấy rằng vấn đề đặt hàng về cơ bản là quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cách có thể để ghép cặp học sinh và đối với mỗi cặp, hãy tính chi phí để làm cho mỗi cặp bằng nhau. Vì có$(n-1)!!$các cách ghép đôi$n$các mặt hàng, điều này phát triển cực kỳ nhanh chóng. Ngay cả đối với$n = 20$, điều này trở nên không thể thực hiện được. Mỗi cặp vẫn yêu cầu tính toán chi phí cân bằng các cặp, do đó, ngay cả khi tính toán chi phí nhanh, việc liệt kê vẫn chiếm ưu thế. 

Cái nhìn sâu sắc quan trọng là sắp xếp mảng. Sau khi được sắp xếp, việc ghép nối tối ưu sẽ có cấu trúc: chúng tôi ghép các phần tử liền kề. Điều này có tác dụng vì việc ghép hai giá trị gần sẽ giảm thiểu chi phí nâng giá trị nhỏ hơn để khớp với giá trị lớn hơn và việc sắp xếp đảm bảo chúng ta tránh ghép một giá trị nhỏ với một giá trị rất lớn khi tồn tại các lựa chọn thay thế cục bộ tốt hơn. 

Sau khi sắp xếp, ta chỉ cần xét cặp$(a_0, a_1), (a_2, a_3), \dots$. Đối với mỗi cặp, chi phí chỉ đơn giản là$a_{i+1} - a_i$, vì chúng ta chỉ tăng phần tử nhỏ hơn. 

Điều này làm giảm vấn đề từ vấn đề ghép nối tổ hợp sang quét tuyến tính trên một mảng đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê ghép đôi Brute Force |$O(n!)$|$O(n)$| Quá chậm | 
| Sắp xếp + Ghép liền kề tham lam |$O(n \log n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị kỹ năng của học sinh vào một mảng. Mục tiêu là giảm thiểu tổng số gia tăng cần thiết để tạo thành các cặp bằng nhau. 
2. Sắp xếp mảng theo thứ tự không giảm. Bước này đảm bảo rằng các giá trị gần nhau về số lượng sẽ trở thành lân cận, điều này rất cần thiết để giảm thiểu chi phí điều chỉnh. 
3. Khởi tạo một biến`answer = 0`để tích lũy tổng số lượng gia tăng cần thiết. 
4. Lặp lại mảng đã sắp xếp theo từng bước hai, xem xét các cặp liên tiếp. 
5. Cho mỗi cặp`(a[i], a[i+1])`, tính chi phí như`a[i+1] - a[i]`. Điều này thể hiện việc tăng giá trị nhỏ hơn để phù hợp với giá trị lớn hơn. 
6. Thêm chi phí này vào tổng chi phí đang hoạt động. 
7. Xuất chi phí tích lũy cuối cùng. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành giải pháp trong đó việc ghép nối được thực hiện giữa các phần tử liền kề mà không làm tăng chi phí. Nếu hai cặp giao nhau, chẳng hạn như ghép đôi`(a_i, a_k)`Và`(a_j, a_l)`với$i < j < k < l$, hoán đổi chúng thành`(a_i, a_j)`Và`(a_k, a_l)`không bao giờ làm tăng tổng chi phí vì nó làm giảm khoảng cách lớn giữa các cặp. Việc áp dụng lập luận này nhiều lần sẽ loại bỏ tất cả các điểm giao nhau, dẫn đến việc ghép cặp liền kề là tối ưu. 

Điều này chứng tỏ rằng việc ghép nối liền kề tham lam không chỉ thuận tiện mà còn tối ưu về mặt cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

ans = 0
for i in range(0, n, 2):
    ans += a[i+1] - a[i]

print(ans)
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào và sắp xếp mảng kỹ năng. Việc sắp xếp là cần thiết vì nó tạo ra một cấu trúc nơi các cặp tối ưu trở thành cục bộ. 

Vòng lặp xử lý mảng theo từng phần hai. Mỗi lần lặp lại ghép hai giá trị được sắp xếp liền kề, đảm bảo chi phí tối thiểu cho cặp đó theo cấu trúc tối ưu toàn cục. Sự khác biệt được thêm trực tiếp vì chỉ cho phép gia số, vì vậy chúng tôi luôn tăng giá trị nhỏ hơn để khớp với giá trị lớn hơn. 

Không có vấn đề về ranh giới vì$n$thậm chí được đảm bảo, vì vậy`a[i+1]`luôn luôn hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
5 10 2 3 14 5
```Mảng được sắp xếp:```
2 3 5 5 10 14
```| tôi | Cặp | Chi phí | Tổng số chạy | 
| --- | --- | --- | --- | 
| 0 | (2,3) | 1 | 1 | 
| 2 | (5,5) | 0 | 1 | 
| 4 | (10,14) | 4 | 5 | 

Đầu ra là 5. 

Dấu vết này cho thấy cách sắp xếp đảm bảo sự khác biệt cục bộ tương ứng với mức tăng tối thiểu cần thiết. 

### Ví dụ 2 

đầu vào:```
4
1 100 2 3
```Mảng được sắp xếp:```
1 2 3 100
```| tôi | Cặp | Chi phí | Tổng số chạy | 
| --- | --- | --- | --- | 
| 0 | (1,2) | 1 | 1 | 
| 2 | (3.100) | 97 | 98 | 

Đầu ra là 98. 

Ví dụ này nêu bật lý do tại sao việc ghép nối cục bộ tham lam hoạt động tốt hơn so với ghép nối tùy ý: giá trị lớn buộc phải chịu chi phí nhưng việc ghép nối nó với phần tử có sẵn gần nhất sẽ giảm thiểu tổng chi phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, chuyển tuyến tính đơn sau đó | 
| Không gian |$O(1)$hoặc$O(n)$| Phụ thuộc vào việc triển khai sắp xếp tại chỗ | 

Những hạn chế$n \le 100$làm cho điều này là quá đủ. Ngay cả việc phân loại ngây thơ hoặc quét lặp đi lặp lại cũng có thể vượt qua một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = 0
    for i in range(0, n, 2):
        ans += a[i+1] - a[i]
    return str(ans)

# provided sample
assert run("6\n5 10 2 3 14 5\n") == "5"

# all equal
assert run("4\n7 7 7 7\n") == "0"

# increasing sequence
assert run("4\n1 2 3 4\n") == "2"

# large gap case
assert run("4\n1 100 2 3\n") == "98"

# minimum case
assert run("2\n1 10\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 0 | tính chính xác của việc ghép nối không tốn phí | 
| 1 2 3 4 | 2 | tối ưu ghép nối liền kề | 
| 1 100 2 3 | 98 | xử lý sự mất cân bằng lớn | 
| 1 10 | 9 | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị đều bằng nhau. Thuật toán sắp xếp chúng nhưng mọi chênh lệch liền kề đều bằng 0, do đó kết quả vẫn bằng 0. Đối với đầu vào`4 / 5 5 5 5`, mảng được sắp xếp không thay đổi và tất cả sự khác biệt được tính toán đều biến mất. 

Một trường hợp cạnh khác là khi các giá trị bị sai lệch nhiều, chẳng hạn như`1 1 1 100`. Sau khi sắp xếp, các cặp trở thành`(1,1)`Và`(1,100)`. Cái đầu tiên đóng góp chi phí bằng 0 và cái thứ hai đóng góp 99, điều này là không thể tránh khỏi vì chỉ tồn tại một giá trị lớn. 

Trường hợp cạnh cuối cùng là đầu vào nhỏ nhất có thể`n = 2`. Thuật toán chỉ trả về sự khác biệt giữa hai giá trị sau khi sắp xếp, khớp trực tiếp với cặp hợp lệ duy nhất.
