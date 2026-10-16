---
title: "CF 105486G - Mảng mở rộng"
description: "Chúng tôi bắt đầu với một danh sách các số nguyên và chúng tôi được phép mở rộng danh sách đó nhiều lần bằng cách lấy bất kỳ cặp lân cận nào và chèn một giá trị bắt nguồn từ chúng bằng các phép toán theo bit: AND, OR hoặc XOR."
date: "2026-06-23T19:02:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105486
codeforces_index: "G"
codeforces_contest_name: "2024 ICPC Asia Chengdu Regional Contest (The 3rd Universal Cup. Stage 15: Chengdu)"
rating: 0
weight: 105486
solve_time_s: 70
verified: true
draft: false
---

[CF 105486G - Mảng mở rộng](https://codeforces.com/problemset/problem/105486/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một danh sách các số nguyên và chúng tôi được phép mở rộng danh sách đó nhiều lần bằng cách lấy bất kỳ cặp lân cận nào và chèn một giá trị bắt nguồn từ chúng bằng các phép toán theo bit: AND, OR hoặc XOR. Giá trị được chèn nằm giữa hai phần tử được chọn và quá trình có thể tiếp tục vô thời hạn trên mảng mới được tạo. 

Mục tiêu không phải là theo dõi bản thân mảng mà là để hiểu những giá trị nào cuối cùng có thể xuất hiện ở bất kỳ đâu trong mảng đó và đặc biệt là để tối đa hóa tổng số giá trị riêng biệt có thể được tạo ra. 

Phần quan trọng của các ràng buộc là kích thước của mảng, lên tới 100.000 phần tử, với các giá trị lên tới khoảng 2³⁰. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng việc chèn thêm hoặc duy trì mảng đang phát triển một cách rõ ràng, vì mảng có thể phát triển mà không bị giới hạn. Thay vào đó, giải pháp phải suy luận về _closure_ của các giá trị: tập hợp số nào cuối cùng có thể truy cập được, bất kể thứ tự thực hiện các thao tác. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng chỉ có các giá trị ban đầu và kết quả theo cặp ngay lập tức của chúng mới quan trọng. Ví dụ, đưa ra`[1, 2, 3]`, người ta có thể chỉ nghĩ đến những kết hợp như`1 & 2`,`2 | 3`, v.v. vấn đề. Nhưng một khi một giá trị mới được chèn vào, nó sẽ trở nên liền kề với các phần tử khác và tham gia vào các hoạt động tiếp theo, tạo ra các kết hợp bậc hai và bậc cao hơn không xuất hiện trực tiếp trong mảng ban đầu. 

Điều này về cơ bản tạo ra vấn đề về việc hiểu cấu trúc nào được tạo ra khi một tập hợp số được đóng dưới ứng dụng lặp lại AND, OR và XOR theo bit. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ mô phỏng quá trình theo đúng nghĩa đen: duy trì mảng, chọn mọi cặp liền kề có thể có, chèn tất cả ba giá trị dẫn xuất và tiếp tục cho đến khi không có giá trị mới nào xuất hiện. Điều này nhanh chóng trở nên không khả thi vì mỗi thao tác đều tăng kích thước mảng và số lượng các chuỗi thao tác có thể tăng lên một cách bùng nổ. Ngay cả khi chúng tôi cố gắng loại bỏ các giá trị trùng lặp, sự tương tác giữa các phần tử mới được chèn sẽ tạo ra sự bùng nổ tổ hợp cả về thời gian và bộ nhớ. 

Quan sát chính là chúng ta không quan tâm đến _thứ tự_ của các phần chèn thêm hoặc cấu trúc của mảng, chỉ quan tâm đến giá trị nào có thể tồn tại. Khi một giá trị được tạo, nó có thể được sử dụng giống như bất kỳ giá trị nào khác. Điều này có nghĩa là chúng tôi đang nghiên cứu một cách hiệu quả việc đóng tập hợp ban đầu theo ba phép toán theo bit. 

Bây giờ chúng ta phóng to hành vi của các hoạt động này từng chút một. Mỗi bit của một số phát triển độc lập theo AND, OR và XOR. Nếu chúng ta tách biệt một vị trí bit đơn thì mỗi số sẽ đóng góp 0 hoặc 1 tại vị trí đó và các phép toán sẽ hoạt động như sau trên các bit: 

Từ bất kỳ cặp bit nào: 

- AND chỉ cho 1 nếu cả hai đều bằng 1 
- HOẶC cho 1 nếu có ít nhất một là 1 
- XOR cho kết quả 1 nếu chính xác 1 là 1 

Bây giờ hãy xem xét điều gì sẽ xảy ra nếu chúng ta có cả 0 và 1 ở cùng một vị trí bit trên một số giá trị có thể truy cập. Từ một cặp (0,1), chúng ta có thể tạo lại cả 0 và 1. Từ (1,1), XOR tạo ra 0, đưa lại giá trị còn thiếu. Từ (0,0), mọi thứ vẫn ở mức 0, do đó bit đó được cố định vĩnh viễn. 

Điều này có nghĩa là mỗi bit hoạt động độc lập: nếu mảng ban đầu chứa ít nhất một số 0 và ít nhất một số 1 ở một vị trí bit nhất định thì bit đó sẽ hoàn toàn có thể được kiểm soát khi đóng. Mặt khác, nó không đổi trên tất cả các giá trị có thể truy cập. 

Khi các bit độc lập theo cách này, bất kỳ sự kết hợp nào của các bit có thể điều khiển tự do đều có thể được thực hiện bằng cách lặp lại việc xây dựng các giá trị trung gian, bởi vì chúng ta có thể tổng hợp 0 ​​hoặc 1 ở mỗi vị trí bit tự do một cách độc lập và sau đó kết hợp chúng thông qua các hoạt động tiếp theo. 

Vì vậy, vấn đề giảm xuống còn việc đếm xem có bao nhiêu vị trí bit được "hỗn hợp" trong đầu vào, nghĩa là vị trí đó chứa cả 0 và 1 trong số các số ban đầu. Nếu k bit được trộn lẫn thì tập hợp có thể truy cập cuối cùng chứa tất cả 2^k kết hợp của các bit đó, trong khi các bit cố định không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tăng trưởng khổng lồ, không giới hạn) | O(rất lớn) | Quá chậm | 
| Phân tích độc lập bit | O(n log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét tất cả các số và ghi lại, đối với từng vị trí bit, xem chúng ta có thấy số 0 hay không và liệu chúng ta có thấy số 1 hay không. 

Điều này nắm bắt chính xác thông tin cần thiết để xác định xem một bit là cố định hay miễn phí khi đóng. 
2. Đối với mỗi vị trí bit từ 0 đến 30, hãy kiểm tra xem cả hai trạng thái có xuất hiện trong số các số đầu vào hay không. Nếu có, hãy đánh dấu phần này là “miễn phí”. 

Lý do là chỉ khi cả hai trạng thái bit cùng tồn tại mới có thể tái tạo cả hai giá trị thông qua tương tác XOR và AND/OR. 
3. Đếm xem có bao nhiêu bit trống và biểu thị số lượng này là k. 

Mỗi bit như vậy có thể thay đổi độc lập ở bất kỳ giá trị nào có thể truy cập được. 
4. Trả về 2^k làm câu trả lời. 

Điều này thể hiện tất cả các kết hợp có thể có của các bit tự do, trong khi các bit cố định không góp phần thay đổi. 

### Tại sao nó hoạt động

Thuật toán dựa trên bất biến cấu trúc: mỗi vị trí bit phát triển độc lập theo các phép toán được phép và trở ngại duy nhất đối với tính linh hoạt hoàn toàn ở một bit là tính đồng nhất trong tập hợp ban đầu. Khi một bit ban đầu có cả 0 và 1, các ứng dụng lặp lại AND, OR và XOR đảm bảo rằng không có giá trị nào bị mất vĩnh viễn, vì mỗi thao tác sẽ bảo toàn hoặc đưa lại cả hai trạng thái. Điều này ngăn cản việc thu gọn thành một tập hợp con bị hạn chế và việc đóng trên bit đó trở thành lựa chọn nhị phân đầy đủ. Vì các bit không tương tác theo cách hạn chế khả năng thay đổi của các bit khác, nên không gian trạng thái toàn cục là tích Descartes của các quyền tự do bit độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    has0 = [True] * 31
    has1 = [True] * 31

    # initialize: we will refine by checking actual presence
    has0 = [False] * 31
    has1 = [False] * 31

    for x in a:
        for b in range(31):
            if x & (1 << b):
                has1[b] = True
            else:
                has0[b] = True

    free_bits = 0
    for b in range(31):
        if has0[b] and has1[b]:
            free_bits += 1

    print(1 << free_bits)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ theo dõi số liệu thống kê bit, không bao giờ mô phỏng bất kỳ việc mở rộng mảng nào. Đối với mỗi số, nó cập nhật bit nào xuất hiện dưới dạng 0 và bit nào xuất hiện dưới dạng 1. Vòng lặp cuối cùng đếm xem có bao nhiêu vị trí được trộn lẫn. Câu trả lời được tính là lũy thừa của hai bằng cách sử dụng dịch chuyển bit, điều này an toàn vì số mũ nhiều nhất là 31. 

Một cạm bẫy triển khai phổ biến là quên khởi tạo cả hai mảng hiện diện một cách chính xác hoặc diễn giải sai các bước kiểm tra bit; mỗi bit phải được kiểm tra độc lập cho cả hai trạng thái trên tất cả các số. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`a = [1, 2]`. 

Đây là 1`01`và 2 là`10`. Đối với mỗi vị trí bit, cả 0 và 1 đều xuất hiện. Chúng tôi theo dõi: 

| Chút | has0 | có1 | 
| --- | --- | --- | 
| 0 | vâng | vâng | 
| 1 | vâng | vâng | 

Tất cả các bit đều miễn phí nên k = 2 và câu trả lời là 4. 

Điều này cho thấy hai giá trị bổ sung ngay lập tức mở khóa tính linh hoạt của bit đầy đủ như thế nào. 

Bây giờ hãy xem xét`a = [7, 3]`. 

Nhị phân: 

- 7 = 111 
- 3 = 011 

| Chút | has0 | có1 | 
| --- | --- | --- | 
| 0 | không | vâng | 
| 1 | không | vâng | 
| 2 | vâng | vâng | 

Chỉ có bit 2 là trống nên k = 1 và đáp án là 2. 

Điều này chứng tỏ rằng ngay cả khi hầu hết các bit được cố định, một bit hỗn hợp đơn lẻ đã tăng gấp đôi không gian có thể truy cập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 31) | Mỗi số được quét một lần và kiểm tra từng bit | 
| Không gian | O(1) | Chỉ sử dụng các mảng có kích thước cố định để theo dõi bit | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện vài triệu thao tác bit ngay cả đối với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out.strip()

# minimum size
assert run("2\n0 1\n") == "2", "basic mixed bits"

# all equal
assert run("3\n5 5 5\n") == "1", "no bit variation"

# fully mixed small
assert run("2\n1 2\n") == "4", "all bits free"

# single bit difference
assert run("3\n0 1 1\n") == "2", "one free bit"

# maximum like stress pattern
assert run("5\n0 1 2 3 4\n") == str(1 << 3), "multiple mixed bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 1 | 2 | trộn bit cơ bản | 
| 3 5 5 5 | 1 | không có trường hợp biến đổi | 
| 2 1 2 | 4 | tự do hoàn toàn | 
| 3 0 1 1 | 2 | tự do một phần | 
| 5 0 1 2 3 4 | 8 | kích hoạt đa bit | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các số đều giống hệt nhau. Ví dụ, đầu vào`a = [8, 8, 8]`. Mỗi bit luôn luôn là 0 hoặc luôn là 1, do đó không có vị trí bit nào bị trộn lẫn. Thuật toán đánh dấu không có bit trống nào, dẫn đến k = 0 và câu trả lời là 1. Điều này phù hợp với thực tế vì không có thao tác nào có thể đưa ra giá trị mới khi cả hai đầu vào giống hệt nhau. 

Một trường hợp cạnh khác là khi chỉ có một bit khác nhau trên toàn bộ mảng. Ví dụ,`a = [0, 1, 1, 1]`. Chỉ có bit thấp nhất được trộn lẫn, vì vậy k = 1 và câu trả lời là 2. Xem qua thuật toán, chỉ vị trí bit đó có cả has0 và has1 được đặt, và tất cả các bit khác vẫn cố định, xác nhận rằng chỉ tồn tại một bậc tự do trong lần đóng cuối cùng.
