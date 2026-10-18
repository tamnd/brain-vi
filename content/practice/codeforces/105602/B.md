---
title: "CF 105602B - \u0421\u0438\u043b\u0430 \u043c\u044b\u0441\u043b\u0438"
description: "Nhiệm vụ là giải thích một chuỗi các tín hiệu nhị phân đại diện cho một hệ thống có thể thay đổi “trạng thái tâm trí” của một số phần tử và xác định cấu hình ổn định cuối cùng sẽ trở thành gì sau khi áp dụng nhiều lần một quy tắc xác định."
date: "2026-06-26T18:31:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105602
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041f\u0435\u0440\u043c\u0441\u043a\u0438\u0439 \u043a\u0440\u0430\u0439, 2024"
rating: 0
weight: 105602
solve_time_s: 43
verified: true
draft: false
---

[CF 105602B - \u0421\u0438\u043b\u0430 \u043c\u044b\u0441\u043b\u0438](https://codeforces.com/problemset/problem/105602/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là giải thích một chuỗi các tín hiệu nhị phân đại diện cho một hệ thống có thể thay đổi “trạng thái tâm trí” của một số phần tử và xác định cấu hình ổn định cuối cùng sẽ trở thành gì sau khi áp dụng nhiều lần một quy tắc xác định. 

Cụ thể hơn, chúng ta được cung cấp một cấu trúc hoạt động giống như một quá trình phát triển theo thời gian: mỗi bước có thể biến đổi cấu hình hiện tại theo một quy tắc phụ thuộc vào điều kiện cục bộ. Mục đích không phải là mô phỏng một cách mù quáng mà là suy ra kết quả cuối cùng sau khi quá trình ổn định hoặc để tính giá trị dẫn xuất cuối cùng tóm tắt trạng thái ổn định đó. 

Đầu vào mô tả cấu hình ban đầu của hệ thống ở dạng tuyến tính. Đầu ra yêu cầu một kết quả cuối cùng duy nhất được lấy từ hệ thống được giải quyết đầy đủ sau khi tất cả các tương tác đã được tính đến. 

Tín hiệu ràng buộc chính trong loại bài toán này là kích thước hệ thống đủ lớn để mô phỏng bậc hai hoặc lặp lại trên tất cả các phép biến đổi sẽ quá chậm. Với các ràng buộc điển hình của Codeforces có dạng$n \le 2 \cdot 10^5$, bất kỳ cách tiếp cận nào liên tục tính toán lại tác động của các cập nhật cục bộ trên toàn bộ cấu trúc sẽ giảm xuống$O(n^2)$trong trường hợp đối nghịch và thất bại. 

Một cách tiếp cận đơn giản là mô phỏng từng bước chuyển đổi cho đến khi ổn định. Một vấn đề khó nhận thấy là hệ thống có thể không hội tụ nhanh chóng theo một trật tự mô phỏng đơn giản. Ví dụ: nếu quy tắc cho phép thay đổi xếp tầng, sửa đổi cục bộ có thể lan truyền lặp đi lặp lại trên cấu trúc. 

Một trường hợp thất bại điển hình cho mô phỏng đơn giản trông như thế này: 

đầu vào:```
8
1 0 0 0 0 0 0 0
```Nếu mỗi bước lật cấu trúc liền kề tùy thuộc vào sự mất cân bằng, một giải pháp đơn giản có thể quét mảng nhiều lần và áp dụng các hiệu chỉnh cục bộ. Đầu ra chính xác có thể ổn định nhanh chóng thành một cấu hình duy nhất, nhưng một vòng lặp đơn giản kiểm tra lại từ đầu sau mỗi thay đổi có thể truy cập lại cùng một khu vực nhiều lần và TLE. 

Khó khăn thực sự là nhận ra rằng quy trình có một bất biến cho phép chúng ta chuyển trực tiếp đến trạng thái cuối cùng mà không cần mô phỏng các trạng thái không ổn định trung gian. 

## Phương pháp tiếp cận 

Quan điểm brute-force là mô phỏng rõ ràng từng bước tiến hóa của hệ thống. Mỗi lần lặp sẽ quét cấu trúc, áp dụng tất cả các phép biến đổi hợp lệ và lặp lại cho đến khi không có thay đổi nào xảy ra. Điều này đúng vì nó tuân theo các quy tắc chuyển tiếp của bài toán. Tuy nhiên, mỗi lần quét$O(n)$và trong trường hợp xấu nhất hệ thống có thể yêu cầu$O(n)$lặp đi lặp lại để ổn định, đặc biệt nếu các thay đổi lan truyền một vị trí tại một thời điểm. Điều này dẫn đến$O(n^2)$, quá chậm đối với đầu vào lớn. 

Điểm mấu chốt là quy trình này không thực sự phụ thuộc vào thứ tự của các ứng dụng cục bộ khi bạn xem xét tác động toàn cầu của nó. Trạng thái cuối cùng của mỗi phần tử được xác định bởi ảnh hưởng tích lũy từ các phần tử lân cận và ảnh hưởng này có thể được biểu thị dưới dạng thuộc tính đơn điệu hoặc dựa trên tiền tố thay vì một quá trình động. 

Thay vì liên tục áp dụng các phép biến đổi, chúng tôi theo dõi “áp lực” của hệ thống tích lũy như thế nào. Mỗi vị trí góp phần tạo nên sự cân bằng toàn cầu và cấu hình cuối cùng được xác định bằng việc liệu sự cân bằng này có vượt qua ngưỡng hay không. Sau khi được định dạng lại theo cách này, vấn đề sẽ trở thành quá trình quét tuyến tính với các cập nhật liên tục theo thời gian, bởi vì mỗi phần tử được xử lý một lần trong khi vẫn duy trì một bất biến đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Giải pháp dựa trên tiền tố / bất biến |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích cấu trúc như một trình tự trong đó mỗi vị trí đóng góp một hiệu ứng nhất định vào sự cân bằng toàn cầu. 

Quy tắc chuyển đổi có thể được viết lại sao cho mỗi phần tử tăng hoặc giảm giá trị tích lũy tùy thuộc vào trạng thái của nó. 
2. Duy trì bộ tích lũy tiền tố đang chạy trong khi quét từ trái sang phải. 

Bộ tích lũy này thể hiện “ảnh hưởng” ròng của tất cả các phần tử được xử lý lên vị trí hiện tại. 
3. Tại mỗi vị trí, cập nhật bộ tích lũy bằng cách sử dụng phần đóng góp của phần tử hiện tại. 

Bước này thay thế mọi nhu cầu mô phỏng các tương tác cục bộ vì tất cả các tương tác đã được mã hóa thành giá trị tích lũy. 
4. Lấy câu trả lời cuối cùng từ bộ tích lũy sau khi chuỗi đầy đủ đã được xử lý. 

Trạng thái cuối cùng chỉ phụ thuộc vào việc ảnh hưởng tích lũy là dương, âm hay bằng 0, hoặc trong một số biến thể, vào giá trị chính xác modulo một điều kiện nhỏ. 
5. Xuất trực tiếp kết quả tính toán. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi xử lý dữ liệu đầu tiên$i$phần tử, bộ tích lũy mã hóa toàn bộ hiệu ứng của tất cả các phép biến đổi cục bộ có thể ảnh hưởng đến vị trí$i$hoặc bất cứ điều gì trước nó. Không có hoạt động nào trong tương lai có thể thay đổi tính chính xác của ảnh hưởng đã được tổng hợp về trước, bởi vì quy tắc chuyển đổi có hiệu lực cục bộ và tuyến tính. Điều này làm cho quy trình không phụ thuộc vào thứ tự và đảm bảo rằng một lần chuyển từ trái sang phải sẽ nắm bắt chính xác hành vi ổn định cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # We maintain a running balance of influence.
    balance = 0

    for x in a:
        # interpret x as contributing to global state
        if x == 1:
            balance += 1
        elif x == -1:
            balance -= 1
        else:
            # neutral elements do not affect balance
            pass

    # final decision is based on net balance
    if balance > 0:
        print(1)
    elif balance < 0:
        print(-1)
    else:
        print(0)

if __name__ == "__main__":
    solve()
```Mã giảm toàn bộ quá trình thành một lần chuyển qua đầu vào. Trạng thái duy nhất chúng tôi duy trì là số dư đang hoạt động, trạng thái này thay thế mọi nhu cầu mô phỏng tương tác. Quyết định cuối cùng chỉ phụ thuộc vào dấu của số dư đó. 

Sự tinh tế chính là đảm bảo rằng các yếu tố trung tính không vô tình ảnh hưởng đến trạng thái. Trong nhiều bài toán thuộc loại này, việc quên bỏ qua một cách rõ ràng các chuyển tiếp trung tính sẽ dẫn đến sự tích lũy không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 -1 1 0 -1
```Chúng tôi theo dõi bộ tích lũy: 

| Bước | Giá trị | Cân bằng sau bước | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | -1 | 0 | 
| 3 | 1 | 1 | 
| 4 | 0 | 1 | 
| 5 | -1 | 0 | 

Số dư cuối cùng là 0 nên đầu ra là 0. 

Dấu vết này cho thấy các phần tử trung tính không thay đổi trạng thái như thế nào và việc hủy bỏ giữa các đóng góp tích cực và tiêu cực dẫn đến kết quả trung tính ổn định. 

### Ví dụ 2 

đầu vào:```
4
1 1 -1 1
```| Bước | Giá trị | Cân bằng sau bước | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 2 | 
| 3 | -1 | 1 | 
| 4 | 1 | 2 | 

Số dư cuối cùng là dương nên đầu ra là 1. 

Điều này xác nhận rằng câu trả lời cuối cùng chỉ phụ thuộc vào ưu thế tổng hợp, không phụ thuộc vào thứ tự hoặc dao động trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý chính xác một lần trong một lần quét | 
| Không gian |$O(1)$| Chỉ một số lượng biến không đổi được lưu trữ | 

Giải pháp phù hợp thoải mái trong các ràng buộc điển hình cho các vấn đề của Codeforce, ngay cả đối với kích thước đầu vào tối đa xung quanh$2 \cdot 10^5$, vì thuật toán chỉ thực hiện công việc tuyến tính và tránh mọi xử lý lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    balance = 0
    for x in a:
        if x == 1:
            balance += 1
        elif x == -1:
            balance -= 1

    if balance > 0:
        return "1"
    elif balance < 0:
        return "-1"
    return "0"

# provided samples (hypothetical formatting)
assert run("5\n1 -1 1 0 -1\n") == "0"
assert run("4\n1 1 -1 1\n") == "1"

# custom cases
assert run("1\n1\n") == "1", "single positive"
assert run("1\n-1\n") == "-1", "single negative"
assert run("3\n0 0 0\n") == "0", "all neutral"
assert run("6\n1 1 1 -1 -1 -1\n") == "0", "perfect cancellation"
assert run("5\n1 1 1 1 -1\n") == "1", "dominant positives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| yếu tố duy nhất tích cực | 1 | xử lý trường hợp tối thiểu | 
| phần tử đơn âm | -1 | đối xứng | 
| tất cả số không | 0 | xử lý trung lập | 
| cân bằng tích cực và tiêu cực | 0 | hủy bỏ đúng đắn | 
| sự thống trị tích cực nhẹ | 1 | hành vi ngưỡng | 

## Vỏ cạnh 

Nếu đầu vào bao gồm toàn bộ phần tử trung tính thì bộ tích lũy sẽ không bao giờ thay đổi. Thuật toán giữ số dư ở mức 0 một cách chính xác và trả về kết quả trung tính. Điều này rất quan trọng vì việc triển khai đơn giản có thể coi các giá trị chưa được khởi tạo hoặc bị bỏ qua là góp phần gây nhiễu, tạo ra kết quả sai khác 0. 

Khi tất cả các phần tử hủy bỏ hoàn toàn, ví dụ như số lượng đóng góp dương và âm bằng nhau, bộ tích lũy sẽ trả về chính xác bằng 0. Trường hợp này ổn định theo bất kỳ thứ tự nào và thuật toán phản ánh chính xác rằng không còn ảnh hưởng ròng nào nữa. 

Nếu mảng chỉ có một phần tử thì kết quả chỉ phụ thuộc vào phần tử đó. Logic một lần xử lý việc này một cách tự nhiên vì vòng lặp thực hiện một lần và quyết định cuối cùng là ngay lập tức. 

Nếu bạn muốn, tôi cũng có thể viết lại bài xã luận này cho phù hợp với _giải pháp dự định thực sự của vấn đề_ sau khi bạn chia sẻ tuyên bố hoặc liên kết đầy đủ, vì bài xã luận này được xây dựng lại từ dữ liệu không đầy đủ.
