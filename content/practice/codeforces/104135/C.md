---
title: "CF 104135C - \u041a\u0440\u043e\u0448 \u0438 \u0443\u0434\u0430\u043b\u0435\u043d\u0438\u044f"
description: "Chúng ta được cấp một dãy số biểu thị một hàng phần tử, mỗi phần tử có hai thuộc tính: một giá trị và một chi phí loại bỏ. Trò chơi cho phép chúng ta liên tục chọn hai phần tử liền kề và xóa phần tử có giá trị nhỏ hơn, trả chi phí loại bỏ liên quan."
date: "2026-07-02T01:40:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104135
codeforces_index: "C"
codeforces_contest_name: "\u0417\u0438\u043c\u043d\u0438\u0439 \u043b\u0438\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 2023"
rating: 0
weight: 104135
solve_time_s: 44
verified: true
draft: false
---

[CF 104135C - \u041a\u0440\u043e\u0448 \u0438 \u0443\u0434\u0430\u043b\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/104135/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số biểu thị một hàng phần tử, mỗi phần tử có hai thuộc tính: một giá trị và một chi phí loại bỏ. Trò chơi cho phép chúng ta liên tục chọn hai phần tử liền kề và xóa phần tử có giá trị nhỏ hơn, trả chi phí loại bỏ liên quan. Cấu trúc của mảng thay đổi sau mỗi lần xóa vì các phần tử nằm gần nhau nên tính kề luôn luôn động. 

Quá trình tiếp tục bao nhiêu lần cũng được, nhưng chúng ta phải luôn để lại ít nhất một phần tử trong mảng. Chúng tôi cũng có ngân sách cố định và mỗi lần xóa sẽ tiêu tốn một phần ngân sách đó. Mục tiêu là tối đa hóa giá trị nhỏ nhất còn lại trong mảng sau khi thực hiện một số chuỗi xóa theo ràng buộc ngân sách. 

Từ góc độ ràng buộc, kích thước mảng có thể lên tới 100000 và ngân sách có thể lên tới 10^18. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào mô phỏng các hoạt động từng bước hoặc cố gắng liệt kê các chuỗi xóa. Bất kỳ cách tiếp cận nào là bậc hai tính theo n hoặc thậm chí n log n với các hằng số nặng đều có khả năng được chấp nhận, nhưng bất kỳ cách tiếp cận nào tệ hơn O(n log n) sẽ quá chậm. 

Khó khăn chính là việc xóa không phải là tùy ý: chúng phụ thuộc vào sự kề cận và mỗi thao tác sẽ loại bỏ phần nhỏ hơn của cặp liền kề đã chọn. Điều này có nghĩa là việc loại bỏ một phần tử cụ thể không độc lập với các phần tử khác; thay vào đó, tính khả thi phụ thuộc vào việc liệu chúng ta có thể "phơi bày" phần tử đó thành phần tử nhỏ hơn trong một số cặp liền kề hay không. 

Một số trường hợp phức tạp cần lưu ý. Nếu tất cả các phần tử đều đã lớn nhưng chi phí bằng 0, chúng ta có thể xóa mạnh mẽ và để lại mức tối thiểu rất cao. Nếu chi phí rất lớn, chúng ta có thể không xóa được bất kỳ thứ gì, vì vậy câu trả lời chỉ đơn giản là phần tử tối thiểu của mảng. Một trường hợp góc khác là khi chiến lược tối ưu chỉ để lại một phần tử; vì vấn đề yêu cầu còn lại ít nhất một phần tử nên chúng tôi phải đảm bảo không bao giờ xóa mọi thứ. 

Một ví dụ đơn giản gây hiểu lầm là trường hợp một phần tử giá rẻ trên toàn cầu nằm ở giữa nhưng được bao quanh bởi những phần tử đắt tiền. Ngay cả khi việc loại bỏ cục bộ là rẻ, cấu trúc có thể chặn việc loại bỏ nó vì nó không bao giờ có thể nhỏ hơn trong một cặp liền kề hợp lệ nếu không loại bỏ các hàng xóm trước. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng mô phỏng tất cả các trình tự xóa có thể xảy ra. Ở mỗi bước, chúng tôi chọn bất kỳ cặp liền kề nào, loại bỏ cặp nhỏ hơn và trừ chi phí của nó khỏi ngân sách. Điều này tạo thành một không gian trạng thái khổng lồ vì mảng thay đổi hình dạng sau mỗi thao tác và số lượng các chuỗi có thể tăng theo cấp số nhân với n. Ngay cả với n = 40, điều này vẫn không thể thực hiện được. 

Quan sát quan trọng là chúng ta thực sự không được yêu cầu mô phỏng quy trình mà chỉ để xác định xem liệu chúng ta có thể loại bỏ đủ phần tử để đảm bảo duy trì một giá trị tối thiểu nhất định hay không. Điều này gợi ý đảo ngược quan điểm: thay vì mô phỏng việc xóa, chúng tôi kiểm tra xem liệu câu trả lời x của ứng viên có đạt được hay không. 

Cố định một giá trị x. Chúng tôi muốn biết liệu có thể loại bỏ mọi phần tử có giá trị nhỏ hơn x trong khi chi tiêu tối đa s hay không. Nếu chúng ta có thể làm được điều này thì tất cả các phần tử còn lại đều có giá trị ít nhất là x, nên giá trị tối thiểu ít nhất là x. Điều này biến bài toán thành một bài toán quyết định có thể được giải độc lập với mỗi x, điều này đương nhiên dẫn đến tìm kiếm nhị phân cho câu trả lời. 

Phần không tầm thường là xác định tính khả thi của một giá trị x cố định. Một phần tử có giá trị ít nhất x không bao giờ có thể bị xóa nếu chúng tôi chọn không xóa phần tử đó, vì vậy chúng tôi coi những phần tử đó là "được bảo vệ". Các phần tử có giá trị nhỏ hơn x phải bị loại bỏ. Mỗi phần tử như vậy đều có một chi phí, nhưng liệu nó có thực sự được loại bỏ hay không còn phụ thuộc vào việc liệu nó có thể được ghép nối với một số phần tử lân cận trong quá trình hay không.

Một sự đơn giản hóa cấu trúc quan trọng là bất kỳ phần tử di động nào cuối cùng cũng có thể bị xóa miễn là chúng tôi luôn đảm bảo nó tham gia vào một cặp liền kề hợp lệ trước khi bị chặn bởi các phần tử còn sót lại. Điều này làm giảm vấn đề thành sự tích lũy chi phí trên tất cả các phần tử có giá trị nhỏ hơn x, bởi vì chúng ta luôn có thể sắp xếp việc xóa theo thứ tự cho phép xóa chúng độc lập với vị trí chính xác của nhau, miễn là có ít nhất một hàng xóm tồn tại tại thời điểm xóa. 

Do đó, đối với x cố định, tính khả thi giảm xuống bằng cách tính tổng chi phí của tất cả các phần tử có giá trị nhỏ hơn x và kiểm tra xem tổng này có nằm trong ngân sách s hay không, đồng thời đảm bảo rằng không phải tất cả các phần tử đều bị loại bỏ (phải tồn tại ít nhất một phần tử có giá trị ≥ x). 

Điều này biến bài toán thành một vị từ đơn điệu trên x, cho phép tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra tính khả thi | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp hoặc coi các giá trị là ứng cử viên cho câu trả lời tối thiểu cuối cùng, vì câu trả lời phải bằng một số giá trị phần tử hiện có. Điều này làm giảm không gian tìm kiếm xuống còn tối đa n khả năng. 
2. Tìm kiếm nhị phân trên ngưỡng ứng cử viên x đại diện cho giá trị tối thiểu được phép trong mảng cuối cùng. 
3. Đối với x cố định, hãy lặp qua mảng và xác định tất cả các phần tử có giá trị nhỏ hơn x, vì đây là những phần tử chúng ta phải loại bỏ. 
4. Tích lũy tổng chi phí loại bỏ các yếu tố này. Điều này thể hiện mức chi tiêu tối thiểu có thể cần thiết để loại bỏ tất cả các yếu tố vi phạm ngưỡng. 
5. Kiểm tra xem tổng chi phí này có nằm trong ngân sách hay không. Nếu vượt quá s thì x không khả thi vì chúng ta không thể loại bỏ hết các phần tử vi phạm. 
6. Đảm bảo tồn tại ít nhất một phần tử có giá trị ≥ x. Nếu không tồn tại thì ngay cả khi có đủ ngân sách, chúng tôi cũng không thể để phần tử cuối cùng hợp lệ đáp ứng ngưỡng. 
7. Nếu cả hai điều kiện đều được thỏa mãn, hãy đánh dấu x là khả thi và thử tăng nó lên; nếu không thì giảm nó. 
8. Sau khi tìm kiếm nhị phân hoàn tất, xuất ra giá trị x lớn nhất có thể. 

### Tại sao nó hoạt động 

Bất biến chính là tính khả thi chỉ phụ thuộc vào việc liệu chúng ta có thể loại bỏ tất cả các yếu tố dưới ngưỡng mà không vượt quá ngân sách hay không chứ không phụ thuộc vào thứ tự xóa chính xác. Bất kỳ trình tự xóa hợp lệ nào loại bỏ một phần tử nhất định luôn trả chi phí của nó chính xác một lần và ràng buộc kề không làm tăng tổng chi phí cần thiết mà chỉ hạn chế thứ tự. Vì chúng ta luôn có thể lên lịch xóa để mỗi phần tử có thể tháo rời cuối cùng được ghép nối và loại bỏ trước khi nó bị chặn bởi các phần tử cao hơn còn sót lại, nên tổng chi phí chính xác là tổng chi phí của tất cả các phần tử dưới ngưỡng. Do đó, bài toán quyết định là đơn điệu trong x, đảm bảo tính đúng đắn của tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def feasible(a, c, s, x):
    total = 0
    has_good = False
    n = len(a)

    for i in range(n):
        if a[i] >= x:
            has_good = True
        else:
            total += c[i]

    return has_good and total <= s

def solve():
    n, s = map(int, input().split())
    a = list(map(int, input().split()))
    c = list(map(int, input().split()))

    vals = sorted(set(a))
    
    lo, hi = 0, len(vals) - 1
    ans = vals[0]

    while lo <= hi:
        mid = (lo + hi) // 2
        x = vals[mid]

        if feasible(a, c, s, x):
            ans = x
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc mảng và cấu trúc chi phí, sau đó nén các câu trả lời của ứng viên thành các giá trị riêng biệt của mảng. Hàm khả thi tính toán chi phí loại bỏ tất cả các phần tử dưới ngưỡng và kiểm tra xem có tồn tại ít nhất một phần tử hợp lệ hay không. 

Tìm kiếm nhị phân duy trì tính bất biến mà tất cả các giá trị lên tới`ans`là khả thi và nó mở rộng lên trên bất cứ khi nào điểm giữa hiện tại hợp lệ. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ mô phỏng các hoạt động lân cận một cách rõ ràng. Điều này là có chủ ý vì tổng chi phí không phụ thuộc vào thứ tự xóa theo lịch trình tối ưu, mặc dù vấn đề ban đầu được diễn đạt một cách linh hoạt. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có giá trị`[3, 1, 4, 2, 5, 6]`và chi phí là`[3, 2, 5, 5, 2, 7]`với ngân sách`12`. 

Chúng tôi kiểm tra ngưỡng ứng viên x = 4. 

| tôi | một [tôi] | c[i] | một [tôi] < 4? | tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | vâng | 3 | 
| 1 | 1 | 2 | vâng | 5 | 
| 2 | 4 | 5 | không | 5 | 
| 3 | 2 | 5 | vâng | 10 | 
| 4 | 5 | 2 | không | 10 | 
| 5 | 6 | 7 | không | 10 | 

Tổng chi phí là 10, nằm trong ngân sách và chúng tôi vẫn có các yếu tố ≥ 4 nên 4 là khả thi. Điều này cho thấy cơ chế xác định chính xác rằng chúng ta có thể loại bỏ tất cả các phần tử nhỏ hơn. 

Bây giờ xét x = 5. 

| tôi | một [tôi] | c[i] | một [tôi] < 5? | tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | vâng | 3 | 
| 1 | 1 | 2 | vâng | 5 | 
| 2 | 4 | 5 | vâng | 10 | 
| 3 | 2 | 5 | vâng | 15 | 
| 4 | 5 | 2 | không | 15 | 
| 5 | 6 | 7 | không | 15 | 

Bây giờ tổng chi phí là 15, vượt quá 12, vì vậy 5 là không khả thi. Điều này cho thấy việc kiểm tra ngưỡng nắm bắt trực tiếp tình trạng không khả thi của ngân sách như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần kiểm tra tính khả thi là O(n) và tìm kiếm nhị phân chạy tối đa log n giá trị ứng cử viên | 
| Không gian | O(n) | Lưu trữ mảng và tập giá trị nén | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc cho n lên tới 100000, vì khoảng 20 lần kiểm tra tính khả thi được thực hiện, mỗi lần kiểm tra tuyến tính trên mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, s = map(int, input().split())
    a = list(map(int, input().split()))
    c = list(map(int, input().split()))

    def feasible(a, c, s, x):
        total = 0
        has_good = False
        for i in range(len(a)):
            if a[i] >= x:
                has_good = True
            else:
                total += c[i]
        return has_good and total <= s

    vals = sorted(set(a))
    lo, hi = 0, len(vals) - 1
    ans = vals[0]

    while lo <= hi:
        mid = (lo + hi) // 2
        x = vals[mid]
        if feasible(a, c, s, x):
            ans = x
            lo = mid + 1
        else:
            hi = mid - 1

    return str(ans)

# sample
assert run("""6 12
3 1 4 2 5 6
3 2 5 5 2 7
""") == "4"

# all equal
assert run("""5 0
2 2 2 2 2
1 1 1 1 1
""") == "2"

# cannot delete anything
assert run("""4 0
1 2 3 4
10 10 10 10
""") == "1"

# tight budget
assert run("""3 3
5 1 4
2 2 2
""") == "4"

# large budget
assert run("""3 100
1 2 3
1 1 1
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 4 | tính đúng đắn của các giá trị hỗn hợp | 
| tất cả đều bình đẳng | 2 | xử lý việc loại bỏ bằng không | 
| ngân sách eo hẹp | 4 | cắt tỉa dưới những hạn chế | 
| ngân sách lớn | 3 | khả năng xóa hoàn toàn | 

## Vỏ cạnh 

Khi tất cả các phần tử đều ở dưới ngưỡng ứng cử viên, việc kiểm tra tính khả thi sẽ thất bại ngay lập tức vì không có phần tử nào còn sót lại để neo ở mức tối thiểu cuối cùng. Thuật toán loại bỏ chính xác các ngưỡng đó vì`has_good`vẫn sai, ngăn chặn các câu trả lời không hợp lệ. 

Khi ngân sách bằng 0, câu trả lời khả thi duy nhất là phần tử tối thiểu, vì mọi nỗ lực loại bỏ phần tử có giá trị thấp hơn đều phải chịu chi phí dương. Việc kiểm tra thực thi điều này một cách tự nhiên bởi vì`total`trở nên tích cực bất cứ khi nào cần loại bỏ. 

Khi tất cả chi phí bằng 0, mọi phần tử dưới ngưỡng có thể được loại bỏ tự do, do đó câu trả lời sẽ trở thành giá trị phần tử lớn nhất có thể. Thuật toán nắm bắt được điều này bởi vì`total`không bao giờ vượt quá`s`và tính khả thi chỉ phụ thuộc vào sự tồn tại của một phần tử còn sót lại. 

Khi các giá trị tăng hoặc giảm nghiêm ngặt, tìm kiếm nhị phân vẫn hoạt động vì tính khả thi chỉ phụ thuộc vào phân vùng theo ngưỡng chứ không phụ thuộc vào cấu trúc hoặc độ kề, khiến thứ tự không liên quan đến tính toán chi phí.
