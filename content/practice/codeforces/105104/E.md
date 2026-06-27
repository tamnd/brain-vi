---
title: "CF 105104E - Bầu cử"
description: "Chúng ta được cung cấp một danh sách các khu vực bầu cử, mỗi khu vực bầu cử chỉ được mô tả bằng một số nguyên duy nhất biểu thị số phiếu bầu mà khu vực đó chứa."
date: "2026-06-27T20:09:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105104
codeforces_index: "E"
codeforces_contest_name: "2024 HNMU@XTU"
rating: 0
weight: 105104
solve_time_s: 48
verified: true
draft: false
---

[CF 105104E - Cuộc bầu cử](https://codeforces.com/problemset/problem/105104/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các khu vực bầu cử, mỗi khu vực bầu cử chỉ được mô tả bằng một số nguyên duy nhất biểu thị số phiếu bầu mà khu vực đó chứa. Ở mỗi khu vực bầu cử, một trong hai ứng cử viên giành được tất cả phiếu bầu từ khu vực bầu cử đó, do đó, mỗi khu vực bầu cử hoạt động giống như một “khối” duy nhất đóng góp toàn bộ giá trị của nó cho bên này hoặc bên kia. 

Câu hỏi ẩn không phải là mô phỏng một cuộc bầu cử. Thay vào đó, nó hỏi liệu cấu trúc của các khối bỏ phiếu này có đủ cứng nhắc để bất kỳ cách hợp pháp nào để chia chúng thành hai nhóm đều dẫn đến một mô hình kết quả tổng thể duy nhất hay không. Tương tự, chúng ta muốn biết liệu có tồn tại hai phép gán ký hiệu khác nhau hay không, một phép gán tương ứng với “ứng cử viên A thắng khu vực bầu cử này” và phép gán còn lại là “ứng cử viên B thắng khu vực bầu cử đó”, sao cho tổng trọng số thu được không thể phân biệt được dưới mọi biến đổi pháp lý. Ví dụ trong tuyên bố gợi ý điều này: việc gán các dấu hiệu khác nhau có thể tạo ra hiệu ứng tổng thể như nhau, điều đó có nghĩa là có sự mơ hồ trong việc xác định kết quả bầu cử. 

Kích thước đầu vào lên tới 100000 giá trị, mỗi giá trị lớn bằng 2^31 − 1. Điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng liệt kê các tập hợp con hoặc so sánh tất cả các phân vùng có thể. Ngay cả cách tiếp cận bậc hai đối với các cặp khu vực bầu cử cũng sẽ quá chậm. Giải pháp về cơ bản phải là tuyến tính hoặc tuyến tính. 

Một vấn đề tế nhị phát sinh khi nhiều giá trị bằng nhau hoặc khi các giá trị nhỏ có thể được tạo thành từ những giá trị lớn hơn. Trong những trường hợp đó, việc gán dấu khác nhau có thể triệt tiêu lẫn nhau theo những cách không ngờ tới. Ví dụ: nếu mảng chứa các giá trị như 1, 2, 3, 4, chúng ta có thể kết hợp các dấu hiệu để tạo ra tổng bằng nhau theo nhiều cách, tương ứng với sự mơ hồ. Một trực giác ngây thơ cho rằng “các số khác nhau đảm bảo tính duy nhất” không thành công vì các tổ hợp tuyến tính trên các dấu hiệu vẫn có thể xung đột với nhau. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều là lũy thừa của hai hoặc tuân theo cấu trúc tăng dần. Trong những trường hợp như vậy, tính duy nhất thường được giữ nhưng chỉ khi mỗi giá trị đủ lớn so với tổng của tất cả các giá trị trước đó. Mặt khác, các giá trị trước đó có thể mô phỏng các giá trị sau thông qua việc hủy bỏ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng kiểm tra xem liệu có tồn tại hai cách gán dấu hiệu khác nhau để tạo ra kết quả bầu cử giống hệt nhau hay không. Một cách để suy nghĩ về nó là xem xét tất cả các tập hợp con của các khu vực bầu cử và tính toán tất cả các tổng có thể có. Nếu mỗi tập hợp con tạo ra một giá trị riêng biệt thì hệ thống là rõ ràng. Tuy nhiên, điều này ngay lập tức dẫn đến 2^n khả năng, và ngay cả với n = 40 thì điều này cũng không khả thi. 

Một nỗ lực khác là sắp xếp các giá trị và cố gắng phát hiện xung đột giữa các tổng tập hợp con bằng cách sử dụng quy hoạch động. Điều này vẫn bùng nổ vì phạm vi tổng rất lớn, lên tới n * 2^31 và DP trên tổng là không khả thi về cả thời gian hoặc bộ nhớ. 

Cái nhìn sâu sắc quan trọng là diễn giải lại vấn đề dưới dạng điều kiện duy nhất trên tổng tập hợp con của nhiều tập hợp trong đó mỗi phần tử có thể được lấy bằng dấu cộng hoặc dấu trừ. Điều này tương đương với việc hỏi liệu tập hợp có “độc lập tuyến tính với các hệ số trong {−1, 0, 1}” theo nghĩa rất hạn chế hay không. Quan sát quan trọng là sự mơ hồ xuất hiện chính xác khi một số tập hợp con của các phần tử nhỏ hơn có thể kết hợp để khớp với phần tử lớn hơn. 

Khi chúng ta sắp xếp mảng, điều kiện sẽ đơn giản hóa đáng kể. Nếu tại bất kỳ điểm nào, phần tử lớn nhất còn lại không lớn hơn tổng của tất cả các phần tử nhỏ hơn được xem xét cho đến nay, thì chúng ta có thể xây dựng hai phép gán dấu khác nhau va chạm nhau. Ngược lại, nếu mọi phần tử đều lớn hơn tổng của tất cả các phần tử trước đó thì mỗi phần tử sẽ đưa ra một thang đo mới không thể sao chép bằng cách kết hợp các phần tử trước đó, đảm bảo tính duy nhất. 

Điều này biến vấn đề thành quét tuyến tính tham lam sau khi sắp xếp.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về nhiệm vụ đăng nhập | O(2^n) | O(n) | Quá chậm | 
| Đã sắp xếp kiểm tra tổng tiền tố tham lam | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm để chúng ta luôn cân nhắc xem liệu các giá trị nhỏ hơn có thể xây dựng được các giá trị lớn hơn hay không. Việc sắp xếp là cần thiết vì thuộc tính khóa phụ thuộc vào khả năng tiếp cận tích lũy từ các phần tử nhỏ hơn. 
2. Khởi tạo biến tổng hiện hành là 0. Tổng này biểu thị giá trị tối đa có thể được tạo bằng cách sử dụng kết hợp các phần tử đã được xử lý. 
3. Lặp lại mảng được sắp xếp từ phần tử nhỏ nhất đến phần tử lớn nhất. 
4. Với mỗi phần tử ai, kiểm tra xem ai có nhỏ hơn hoặc bằng tổng chạy hiện tại hay không. Nếu điều kiện này đúng, chúng ta ngay lập tức kết luận rằng có sự mơ hồ và dừng lại. 
5. Nếu ai lớn hơn tổng hiện tại, chúng ta cập nhật tổng hiện có bằng cách thêm ai, vì phần tử này mở rộng phạm vi các giá trị có thể xây dựng được. 
6. Nếu vòng lặp kết thúc mà không gây ra tình trạng lỗi, chúng ta kết luận rằng không thể xảy ra xung đột giữa các phép gán dấu. 

### Tại sao nó hoạt động 

Ở mỗi bước, tổng hiện có thể hiện đầy đủ các giá trị có thể được tạo bằng cách sử dụng các kết hợp đã ký của các phần tử đã xử lý trước đó. Nếu một phần tử mới nhiều nhất bằng tổng này, điều đó có nghĩa là nó có thể được biểu diễn dưới dạng sự kết hợp của các phần tử trước đó với các dấu hiệu thích hợp, do đó nó không đưa ra một hướng độc lập mới. Điều này ngụ ý rằng hai phép gán dấu khác nhau có thể tạo ra hiệu ứng tổng thể giống nhau, phá vỡ tính duy nhất. Nếu mọi phần tử mới vượt quá phạm vi có thể truy cập hiện tại, thì phần tử đó không thể được sao chép bởi các phần tử trước đó, do đó, mỗi phần tử đóng góp một thang đo độc lập mới, ngăn chặn mọi xung đột dựa trên sự hủy bỏ trên toàn bộ tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    s = 0
    for x in a:
        if x <= s:
            print("NO")
            return
        s += x

    print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo chính xác việc xây dựng tham lam. Việc sắp xếp đảm bảo chúng tôi luôn kiểm tra tính khả thi theo thứ tự mức độ tăng dần. Tổng số tiền chạy`s`theo dõi những gì đã có thể được hình thành bởi các phần tử trước đó. Thời điểm chúng tôi gặp phải một phần tử không vượt quá ranh giới này, chúng tôi sẽ phát hiện ra sự xung đột tiềm ẩn về khả năng biểu diễn và chấm dứt sớm. 

Một lỗi triển khai phổ biến là sử dụng`<`thay vì`<=`. Đẳng thức phải thất bại vì một phần tử bằng tổng hiện tại có thể được sao chép chính xác bằng cách kết hợp có dấu của các phần tử trước đó, tạo ra sự mơ hồ. Một điều tinh tế khác là chúng tôi chỉ duy trì một khoản tiền duy nhất; không cần phải theo dõi phạm vi dương và âm riêng biệt vì tính đối xứng tiềm ẩn trong cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 2, 3, 4]
```Đã sắp xếp mảng rồi`[1, 2, 3, 4]`. 

| Bước | x | tổng số chạy trước | điều kiện (x ≤ s) | hành động | tổng số tiền chạy sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | sai | thêm | 1 | 
| 2 | 2 | 1 | sai | thêm | 3 | 
| 3 | 3 | 3 | đúng | dừng lại | - | 

Ở bước 3, giá trị 3 bằng tổng của các phần tử trước đó. Điều này có nghĩa là 3 có thể được hình thành dưới dạng 1 + 2, do đó nó gây ra sự mơ hồ. Thuật toán xuất ra NO. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 3, 6, 13]
```| Bước | x | tổng số chạy trước | điều kiện (x ≤ s) | hành động | tổng số tiền chạy sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | sai | thêm | 1 | 
| 2 | 3 | 1 | sai | thêm | 4 | 
| 3 | 6 | 4 | sai | thêm | 10 | 
| 4 | 13 | 10 | sai | thêm | 23 | 

Không có phần tử nào có thể truy cập được bằng số tiền trước đó. Mỗi giá trị đưa ra một thang đo độc lập mới, do đó hệ thống vẫn rõ ràng. Đầu ra là CÓ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính sau | 
| Không gian | O(1) thêm | Chỉ có một khoản tiền được duy trì | 

Các ràng buộc cho phép tối đa 100000 phần tử, do đó, giải pháp n log n dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        s = 0
        for x in a:
            if x <= s:
                print("NO")
                return
            s += x
        print("YES")

    from io import StringIO
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided samples (interpreted)
assert run("4\n1 2 3 4\n") == "NO"
assert run("4\n114 515 1919 810\n") == "YES"

# custom cases
assert run("1\n1\n") == "YES"
assert run("2\n1 1\n") == "NO"
assert run("3\n1 2 4\n") == "YES"
assert run("5\n1 2 3 8 16\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1`| CÓ | trường hợp tối thiểu | 
|`2\n1 1`| KHÔNG | trường hợp thất bại bình đẳng | 
|`3\n1 2 4`| CÓ | chuỗi an toàn sức mạnh của hai | 
|`5\n1 2 3 8 16`| KHÔNG | va chạm sớm qua 1+2+3 | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là một khu vực bầu cử duy nhất. Với đầu vào`1`, bất kỳ giá trị nào cũng tạo thành một hệ thống duy nhất vì không thể so sánh được. Thuật toán khởi tạo tổng bằng 0 và ngay lập tức chấp nhận giá trị đầu tiên nếu nó dương. 

Đối với đầu vào`2\n1 1`, sắp xếp sản lượng`[1, 1]`. Phần tử đầu tiên đặt tổng bằng 1. Phần tử thứ hai bằng tổng hiện tại, gây ra tình trạng lỗi. Điều này phản ánh thực tế là hai khối giống hệt nhau có thể được hoán đổi giữa các bên mà không làm thay đổi kết quả chung, tạo ra sự mơ hồ. 

Đối với đầu vào`3\n1 2 4`, tổng chạy tiến triển thành 1, 3, 7. Mỗi phần tử hoàn toàn lớn hơn tổng trước nó, vì vậy không có phần tử nào có thể biểu thị bằng phần tử trước đó. Thuật toán xác nhận tính duy nhất một cách chính xác. 

Đối với đầu vào`5\n1 2 3 8 16`, tổng tiền tố đạt 6 sau khi xử lý 1, 2, 3. Phần tử tiếp theo 8 vẫn an toàn, nhưng 16 cuối cùng đã vượt quá cấu trúc tổng thể theo cách vẫn duy trì mức tăng trưởng nghiêm ngặt; tuy nhiên, nếu chúng tôi sửa đổi một chút để bao gồm 6 thay vì 8 thì lỗi sẽ xuất hiện ngay lập tức vì 6 bằng 1+2+3, chứng tỏ khả năng kết hợp tập hợp con phá vỡ tính duy nhất ngay cả khi các giá trị không bằng nhau.
