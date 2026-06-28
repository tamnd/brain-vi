---
title: "CF 105129C - LCIS"
description: "Chúng ta có hai dãy, mỗi dãy là một hoán vị của cùng một tập hợp các số nguyên từ 1 đến n. Nhiệm vụ là tìm độ dài của dãy con dài nhất xuất hiện trong cả hai hoán vị và tăng dần."
date: "2026-06-27T18:52:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "C"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 46
verified: true
draft: false
---

[CF 105129C - LCIS](https://codeforces.com/problemset/problem/105129/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dãy, mỗi dãy là một hoán vị của cùng một tập hợp các số nguyên từ 1 đến n. Nhiệm vụ là tìm độ dài của dãy con dài nhất xuất hiện trong cả hai hoán vị và tăng dần. 

Dãy con có nghĩa là chúng ta chọn các phần tử mà không thay đổi thứ tự tương đối của chúng trong mỗi mảng. Tăng lên có nghĩa là bản thân các giá trị phải tăng trưởng một cách nghiêm ngặt. Vì vậy, chúng tôi đang tìm kiếm một chuỗi các giá trị tôn trọng hai ràng buộc cùng một lúc: nó phải xuất hiện theo thứ tự bên trong cả hai hoán vị và các giá trị được chọn phải tăng dần. 

Một cách hữu ích để diễn giải lại vấn đề là coi một hoán vị là xác định một thứ tự và hoán vị thứ hai là một chuỗi trong đó chúng ta muốn trích xuất một mẫu tăng dài nhất khi chiếu lên thứ tự đầu tiên. 

Các ràng buộc cho phép n lên tới 100000 cho mỗi trường hợp thử nghiệm với nhiều trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận bậc hai đối với tất cả các cặp vị trí. Một giải pháp phải gần tuyến tính hoặc n log n cho mỗi trường hợp thử nghiệm. Bất cứ điều gì như lập trình động trên các cặp chỉ số hoặc kiểm tra tất cả các chuỗi con sẽ quá chậm. 

Một sai lầm ngây thơ là trước tiên coi đây là dãy con chung dài nhất rồi áp đặt thứ tự tăng dần sau đó. Điều đó sẽ bỏ qua cấu trúc mà cả hai mảng đều là hoán vị. Một cách tiếp cận không chính xác phổ biến khác là tính toán LIS trực tiếp trên một hoán vị bằng cách sử dụng các giá trị từ hoán vị kia mà không ánh xạ vị trí chính xác, điều này phá vỡ điều kiện căn chỉnh. 

## Phương pháp tiếp cận 

Quan điểm brute-force bắt đầu từ dãy con chung dài nhất cổ điển. Chúng ta có thể định nghĩa một DP trong đó dp[i][j] là câu trả lời cho tiền tố a[1..i] và b[1..j]. Điều đó hoạt động với các chuỗi chung nhưng chi phí O(n^2) cho mỗi trường hợp thử nghiệm, vượt xa giới hạn khi n là 100000. 

Sự đơn giản hóa chính xuất phát từ thực tế là cả hai mảng đều là hoán vị. Điều này có nghĩa là mọi giá trị xuất hiện chính xác một lần trong mỗi mảng. Thay vì nghĩ đến việc so khớp các vị trí, chúng ta có thể chuyển bài toán thành một bài toán về một dãy duy nhất. 

Chúng tôi sửa mảng b làm thứ tự tham chiếu. Chúng ta ánh xạ từng giá trị trong a tới vị trí của nó trong b. Sau đó, chúng ta chuyển đổi a thành một mảng p trong đó p[i] là chỉ số của a[i] bên trong b. Bây giờ bất kỳ dãy con nào của a đều tương ứng với dãy con của p, và yêu cầu nó phải chung cho cả hai hoán vị có nghĩa là chúng ta đang chọn các giá trị có vị trí trong b tăng dần. Điều kiện là các giá trị được chọn đang tăng giá trị đã được tự động thỏa mãn nếu chúng ta chọn chúng theo thứ tự từ một dãy con tôn trọng thứ tự b. Điều này làm giảm vấn đề tìm dãy con tăng dài nhất của mảng được ánh xạ p. 

Toàn bộ vấn đề trở thành một vấn đề LIS tiêu chuẩn trên một mảng có độ dài n, có thể giải được trong O(n log n) bằng cách sử dụng mảng đuôi tham lam với tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu LCS DP | O(n^2) | O(n^2) | Quá chậm | 
| Lập bản đồ vị trí + LIS | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Xây dựng mảng pos trong đó pos[x] là chỉ số của giá trị x trong hoán vị b. Điều này cho chúng ta một cách nhanh chóng để so sánh thứ tự trong b. 
2. Thay thế từng phần tử a[i] bằng pos[a[i]]. Sau phép biến đổi này, chúng ta nhận được một dãy số p biểu thị vị trí mỗi phần tử của a xuất hiện trong b. Vấn đề bây giờ được rút gọn thành việc tìm dãy con tăng dài nhất trong p. 
3. Duy trì một mảng tails, trong đó tails[len] lưu trữ giá trị cuối cùng nhỏ nhất có thể có của một dãy con tăng dần có độ dài len được tìm thấy cho đến nay. Cấu trúc này đảm bảo chúng ta giữ được các chuỗi con dễ mở rộng nhất. 
4. Với mỗi giá trị x trong p, hãy tìm vị trí đầu tiên trong đuôi mà x có thể mở rộng hoặc thay thế đuôi hiện có bằng cách sử dụng tìm kiếm nhị phân. Nếu x lớn hơn tất cả các đuôi hiện tại, chúng ta sẽ mở rộng chuỗi; nếu không chúng tôi cập nhật vị trí thích hợp. 
5. Kích thước cuối cùng của đuôi là độ dài của dãy con tăng dài nhất, chính là đáp án. 

Lý do tìm kiếm nhị phân hợp lệ là vì các đuôi luôn được sắp xếp. Mỗi vị trí đại diện cho giá trị kết thúc tốt nhất có thể có cho một chuỗi con có độ dài đó, do đó chúng tôi không bao giờ mất các tiện ích mở rộng tối ưu tiềm năng. 

### Tại sao nó hoạt động 

Bất kỳ dãy con tăng chung nào đều tương ứng với một dãy giá trị có vị trí trong b tăng nghiêm ngặt. Vì chúng ta mã hóa từng giá trị theo vị trí của nó trong b, nên chúng ta đang chọn chính xác một dãy con tăng dần trong p. Ngược lại, bất kỳ dãy con tăng dần nào trong p đều tương ứng với các giá trị xuất hiện theo thứ tự tăng dần trong cả hai hoán vị. Do đó, LIS trên p ghi lại chính xác độ dài LCIS. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, x in enumerate(b):
            pos[x] = i

        p = [pos[x] for x in a]

        tails = []

        for x in p:
            lo, hi = 0, len(tails)
            while lo < hi:
                mid = (lo + hi) // 2
                if tails[mid] < x:
                    lo = mid + 1
                else:
                    hi = mid
            if lo == len(tails):
                tails.append(x)
            else:
                tails[lo] = x

        print(len(tails))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng ánh xạ vị trí nghịch đảo cho b, điều này rất cần thiết vì nó chuyển đổi so sánh giá trị thành so sánh chỉ số. Nếu không có bước này, chúng ta không thể thống nhất hai hoán vị thành một bài toán về dãy duy nhất. 

Phần thứ hai xây dựng p, là biểu diễn được biến đổi của a trong hệ tọa độ của b. Mọi bước sau đều phụ thuộc vào mức giảm này. 

Việc triển khai LIS sử dụng ý tưởng sắp xếp kiên nhẫn tiêu chuẩn. Tìm kiếm nhị phân đảm bảo chúng tôi duy trì các đuôi tối ưu một cách hiệu quả và việc cập nhật các đuôi thay vì lưu trữ các chuỗi đầy đủ giúp giữ cho bộ nhớ tuyến tính. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó a = [1, 3, 2, 4] và b = [1, 2, 3, 4]. Khi đó pos trở thành {1:0, 2:1, 3:2, 4:3} và p trở thành [0, 2, 1, 3]. 

| Bước | x | đuôi trước | hành động | đuôi sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [] | nối thêm | [0] | 
| 2 | 2 | [0] | nối thêm | [0, 2] | 
| 3 | 1 | [0, 2] | thay thế chỉ số 1 | [0, 1] | 
| 4 | 3 | [0, 1] | nối thêm | [0, 1, 3] | 

Câu trả lời cuối cùng là 3. Điều này tương ứng với dãy con [1, 2, 4]. 

Bây giờ hãy xem xét trường hợp ngược lại a = [4, 3, 2, 1], b = [1, 2, 3, 4]. Khi đó p = [3, 2, 1, 0]. 

| Bước | x | đuôi trước | hành động | đuôi sau | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | [] | nối thêm | [3] | 
| 2 | 2 | [3] | thay thế | [2] | 
| 3 | 1 | [2] | thay thế | [1] | 
| 4 | 0 | [1] | thay thế | [0] | 

Câu trả lời cuối cùng là 1, vì không tồn tại cấu trúc tăng dần. 

Dấu vết đầu tiên cho thấy các thay thế cục bộ có thể cải thiện khả năng mở rộng trong tương lai như thế nào, đây là cơ chế cốt lõi đằng sau LIS. Điều thứ hai cho thấy các chuỗi biến đổi giảm nghiêm ngặt sẽ thu gọn về độ dài một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử trong số n phần tử được xử lý bằng tìm kiếm nhị phân trên đuôi | 
| Không gian | O(n) | Mảng để ánh xạ vị trí và đuôi LIS | 

Tổng độ phức tạp phù hợp thoải mái trong giới hạn ngay cả đối với 100000 phần tử cho mỗi trường hợp thử nghiệm, vì log n nhỏ và tất cả các hoạt động đều tuyến tính hoặc gần tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import check_output
    return check_output(["python3", "solution.py"], input=inp.encode()).decode()

# sample-like tests
assert run("1\n1\n1\n1\n") == "1\n"

assert run("1\n4\n1 3 2 4\n1 2 3 4\n") == "3\n"

assert run("1\n4\n4 3 2 1\n1 2 3 4\n") == "1\n"

# identical permutations
assert run("1\n5\n1 2 3 4 5\n1 2 3 4 5\n") == "5\n"

# shifted order
assert run("1\n6\n2 3 4 5 6 1\n1 2 3 4 5 6\n") == "5\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | 1 | ranh giới tối thiểu | 
| hoán vị danh tính | n | trường hợp trận đấu đầy đủ | 
| hoán vị ngược | 1 | sự sụp đổ trật tự tồi tệ nhất | 
| sự dịch chuyển theo chu kỳ | 5 | cấu trúc LIS không tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một hoán vị bị đảo ngược chính xác. Trong trường hợp đó, ánh xạ tạo ra một chuỗi giảm nghiêm ngặt và thuật toán LIS phải giảm nó một cách chính xác thành một chuỗi mà không có bất kỳ phần mở rộng ngẫu nhiên nào. Mảng tails liên tục được thay thế thay vì mở rộng, duy trì tính chính xác. 

Một trường hợp khác là khi cả hai hoán vị đều giống hệt nhau. Sau đó, pos ánh xạ từng phần tử vào chỉ mục riêng của nó, tạo ra một chuỗi đã tăng dần, do đó mọi phần tử đều mở rộng đuôi. Thuật toán không được kích hoạt thay thế trong trường hợp này. 

Trường hợp khó phát hiện thứ ba là khi dãy con tốt nhất không liền kề nhau trong cả hai mảng. Phép biến đổi LIS xử lý việc này một cách tự nhiên vì nó chỉ hoạt động theo thứ tự tương đối trong b, không phải theo thứ tự liền kề, đảm bảo các mẫu như vậy vẫn được phát hiện chính xác.
