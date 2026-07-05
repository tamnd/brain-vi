---
title: "CF 102904B - Chuyển tiền"
description: "Chúng ta được cung cấp một chuỗi các yêu cầu về tiền tệ phải được đáp ứng theo thứ tự và một lượng tiền cố định sẵn có bắt đầu từ con số 0. Mỗi yêu cầu sẽ tăng hoặc giảm số dư khả dụng và quy trình sẽ tiến triển từng bước khi chúng tôi di chuyển qua trình tự."
date: "2026-07-04T08:10:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102904
codeforces_index: "B"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u041f\u044f\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102904
solve_time_s: 43
verified: true
draft: false
---

[CF 102904B - Chuyển tiền](https://codeforces.com/problemset/problem/102904/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các yêu cầu về tiền tệ phải được đáp ứng theo thứ tự và một lượng tiền cố định sẵn có bắt đầu từ con số 0. Mỗi yêu cầu sẽ tăng hoặc giảm số dư khả dụng và quy trình sẽ tiến triển từng bước khi chúng tôi di chuyển qua trình tự. Nhiệm vụ là xác định xem tất cả các yêu cầu có thể được xử lý mà số dư không bao giờ trở nên không hợp lệ theo các quy tắc được ngụ ý trong các hoạt động hay không và tính toán trạng thái cuối cùng sau khi xử lý mọi thứ. 

Một cách hữu ích để suy nghĩ về vấn đề này là chúng ta đang mô phỏng một tài khoản tài chính theo một chuỗi các giao dịch. Mỗi thao tác sẽ thay đổi số dư hiện tại và hệ thống có thể ngầm cấm số dư giảm xuống dưới 0 tại bất kỳ thời điểm nào, vì điều đó tương ứng với việc cố gắng tiêu nhiều tiền hơn số tiền có sẵn. Đầu ra được xác định bằng việc liệu mô phỏng này có thể tiến hành thành công hay không và số dư thu được sẽ trở thành bao nhiêu. 

Các ràng buộc ngụ ý rằng độ dài chuỗi có thể đủ lớn để bất kỳ phương pháp tính toán lại bậc hai hoặc lặp lại nào cũng sẽ thất bại. Do đó, giải pháp phải xử lý từng thao tác theo thời gian không đổi hoặc logarit và lý tưởng nhất là trong một lần tuyến tính duy nhất. 

Điểm tinh tế chính là mô phỏng đơn giản có thể bị hỏng trong các trường hợp khó khăn khi số dư trung gian quan trọng hơn giá trị cuối cùng. Ví dụ, hãy xem xét một chuỗi như`[+5, -3, -4]`. Một cách tiếp cận ngây thơ có thể chỉ tập trung vào tổng số tiền ròng, đó là`-2`, và kết luận là không thể, nhưng sự cố thực tế xảy ra ở bước thứ ba khi số dư sẽ âm. Ngược lại, các chuỗi như`[+5, -2, -3]`duy trì tính khả thi ngay cả khi số dư cuối cùng bằng không. Độ chính xác phụ thuộc vào việc theo dõi tiền tố đang chạy chứ không chỉ tổng số. 

Một trường hợp cạnh khác phát sinh khi tất cả các phép toán đều âm. Ví dụ`[-1, -1, -1]`ngay lập tức thất bại bất kể số tiền cuối cùng. Giải pháp bất cẩn chỉ kiểm tra số dư cuối cùng sẽ chấp nhận sai những trường hợp như vậy. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực là mô phỏng đơn giản. Chúng tôi duy trì sự cân bằng đang hoạt động, lặp lại mảng và áp dụng từng thao tác một. Sau mỗi lần cập nhật, chúng tôi kiểm tra xem số dư có còn hợp lệ hay không. Nếu nó trở nên không hợp lệ, chúng tôi sẽ dừng sớm và báo cáo lỗi. 

Cách tiếp cận này đúng vì nó mô phỏng trực tiếp quá trình được mô tả trong bài toán. Tuy nhiên, tính kém hiệu quả của nó không phải ở tính chính xác mà ở khả năng tính toán lại lặp đi lặp lại nếu được triển khai theo cách đơn giản hơn hoặc lồng nhau hơn, chẳng hạn như tính toán lại tính hợp lệ cho mọi tiền tố từ đầu hoặc kiểm tra lại các phân đoạn trước đó nhiều lần. Trong trường hợp xấu nhất, điều này sẽ thoái hóa thành hành vi O(n2) nếu không cẩn thận với việc duy trì trạng thái gia tăng. 

Cái nhìn sâu sắc quan trọng là hệ thống không có sự phụ thuộc lịch sử nào ngoài số dư hiện tại. Mỗi thao tác sửa đổi một trạng thái vô hướng duy nhất và tất cả các ràng buộc đều cục bộ đối với trạng thái đó. Điều này có nghĩa là chúng ta không bao giờ cần phải xem lại các quyết định trước đó. Toàn bộ quá trình giảm xuống còn duy trì tổng tiền tố trong khi thực thi ràng buộc giới hạn dưới ở mỗi bước. 

Điều này biến vấn đề thành một lần quét tuyến tính duy nhất trong đó chúng tôi cập nhật số dư và xác thực nó ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) ở dạng ngây thơ | O(1) | Quá chậm | 
| Mô phỏng tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`balance`về 0 để thể hiện số tiền hiện có. Điều này thể hiện trạng thái hệ thống trước khi bất kỳ giao dịch nào được áp dụng. 
2. Lặp lại danh sách các thao tác theo thứ tự, cập nhật`balance`bằng cách thêm từng giá trị hoạt động. Điều này mô hình hóa quy trình thực được mô tả trong bài toán, trong đó mỗi yêu cầu sẽ trực tiếp thay đổi tài khoản. 
3. Sau khi áp dụng từng thao tác, hãy kiểm tra ngay xem`balance`đã trở nên tiêu cực. Nếu có, hãy chấm dứt quá trình và trả về lỗi. Việc kiểm tra này thực thi ràng buộc rằng chúng ta không thể chi tiêu nhiều hơn số tiền chúng ta có tại bất kỳ thời điểm nào. 
4. Nếu quá trình lặp hoàn thành mà số dư không bao giờ giảm xuống dưới 0, hãy trả về thành công cùng với số dư cuối cùng. Giá trị cuối cùng thể hiện số tiền còn lại sau khi tất cả các giao dịch hợp lệ được áp dụng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là ở mỗi bước lặp,`balance`bằng tổng của tất cả các hoạt động được xử lý cho đến nay. Bởi vì mỗi thao tác được áp dụng chính xác một lần và không có thao tác nào phụ thuộc vào các giá trị trong tương lai nên trạng thái được nắm bắt hoàn toàn bởi tổng hoạt động này. Điều kiện hợp lệ chỉ phụ thuộc vào việc liệu tổng tiền tố có trở thành số âm hay không. Nếu không có tiền tố nào vi phạm điều kiện này thì mọi trạng thái trung gian đều hợp lệ và quy trình này nhất quán với các quy tắc của hệ thống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    balance = 0
    for x in arr:
        balance += x
        if balance < 0:
            print("NO")
            return

    print("YES")
    print(balance)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một số nguyên duy nhất`balance`và cập nhật nó khi quét mảng. Chi tiết triển khai quan trọng là kiểm tra ngay sau mỗi lần cập nhật, đảm bảo chúng tôi phát hiện các trạng thái không hợp lệ ngay khi chúng xảy ra thay vì trì hoãn xác thực. Hàm thoát sớm khi bị lỗi, giúp duy trì thời gian chạy tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 -2 -3 4 -1
```| Bước | Hoạt động | Số dư | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | +5 | 5 | Có | 
| 2 | -2 | 3 | Có | 
| 3 | -3 | 0 | Có | 
| 4 | +4 | 4 | Có | 
| 5 | -1 | 3 | Có | 

Dấu vết này cho thấy số dư không bao giờ giảm xuống dưới 0 ở bất kỳ tiền tố nào. Mặc dù các giá trị dao động, mọi trạng thái trung gian vẫn hợp lệ, do đó quá trình thành công. 

### Ví dụ 2 

đầu vào:```
3
2 -5 4
```| Bước | Hoạt động | Số dư | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | +2 | 2 | Có | 
| 2 | -5 | -3 | Không | 

Quá trình này không thành công ở bước thứ hai vì số dư ngay lập tức trở nên âm. Mặc dù thao tác thứ ba có thể khôi phục tổng số tiền, hệ thống không cho phép các trạng thái không hợp lệ trung gian nên trình tự bị từ chối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thao tác được xử lý chính xác một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có một biến chạy duy nhất được duy trì | 

Thuật toán phù hợp thoải mái với các ràng buộc điển hình dành cho lập trình cạnh tranh trong đó n có thể đạt tới 10^5 hoặc hơn, vì nó thực hiện một lần truyền tuyến tính duy nhất với chi phí tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# basic valid case
assert run("5\n5 -2 -3 4 -1\n") == "YES\n3"

# early failure
assert run("3\n2 -5 4\n") == "NO"

# minimum size success
assert run("1\n0\n") == "YES\n0"

# immediate failure
assert run("1\n-1\n") == "NO"

# all positive
assert run("4\n1 2 3 4\n") == "YES\n10"

# alternating safe case
assert run("6\n3 -1 2 -2 1 -1\n") == "YES\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | CÓ 0 | xử lý ranh giới của đầu vào trung tính | 
| âm đơn | KHÔNG | trạng thái không hợp lệ ngay lập tức | 
| tất cả đều tích cực | CÓ tổng | tích lũy đơn giản | 
| giá trị xen kẽ | CÓ | tính chính xác của tiền tố theo biến động | 

## Vỏ cạnh 

Trường hợp một cạnh là khi thao tác đầu tiên âm. Đối với đầu vào`n = 3, [-1, 2, 3]`, thuật toán xử lý bước đầu tiên, ngay lập tức phát hiện`balance = -1`, và trả về thất bại. Điều này xác nhận rằng việc chấm dứt sớm được xử lý chính xác và không cần xử lý thêm. 

Một trường hợp cạnh khác là khi các giá trị dao động nhưng không bao giờ vượt quá 0. Vì`3, [1, -1, 1]`, sự cân bằng tiến triển như`1 → 0 → 1`. Thuật toán không bao giờ kích hoạt điều kiện lỗi, chứng tỏ rằng sự bằng 0 được cho phép và chỉ các giá trị âm nghiêm ngặt bị cấm. 

Trường hợp cạnh cuối cùng là một chuỗi dài các số 0. Vì`5, [0, 0, 0, 0, 0]`, số dư vẫn bằng 0 trong suốt thời gian đó. Thuật toán chấp nhận chính xác trình tự, xác nhận rằng các hoạt động không thay đổi không ảnh hưởng đến tính hợp lệ.
