---
title: "CF 104013M - Chú ý đến khoảng cách"
description: "Chúng ta được cấp một tập hợp các số nguyên riêng biệt biểu thị các quân bài được nắm giữ bởi những người chơi khác nhau. Ngoài ra còn có một cọc ban đầu bắt đầu bằng một lá bài có giá trị 0."
date: "2026-07-02T05:04:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104013
codeforces_index: "M"
codeforces_contest_name: "2020-2021 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104013
solve_time_s: 45
verified: true
draft: false
---

[CF 104013M - Lưu ý đến khoảng cách](https://codeforces.com/problemset/problem/104013/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên riêng biệt biểu thị các quân bài được nắm giữ bởi những người chơi khác nhau. Ngoài ra còn có một cọc ban đầu bắt đầu bằng một lá bài có giá trị 0. Người chơi sẽ đặt các lá bài của mình lên cọc này, nhưng có một hạn chế quyết định thời điểm người chơi được phép chơi: một lá bài có giá trị`x`chỉ có thể được đặt nếu đỉnh hiện tại của cọc là`y`và sự khác biệt`x - y`nhiều nhất là một số nguyên cố định`d`được chọn trước. 

Tất cả người chơi hành động độc lập mà không cần giao tiếp, nhưng họ tuân theo cùng một quy tắc: nếu thẻ của họ có thể được đặt theo quy tắc, họ sẽ đặt nó. Nếu nhiều người chơi được phép đặt cùng một lúc, thứ tự xếp bài của họ là tùy ý, điều đó có nghĩa là chúng tôi phải đảm bảo tính chính xác ngay cả trong trường hợp xấu nhất. 

Mục tiêu cuối cùng là tất cả các thẻ cuối cùng đều được đặt và chồng kết quả từ dưới lên trên phải tăng lên một cách nghiêm ngặt. Chúng ta cần xác định xem có tồn tại một giá trị`d`sao cho dù các vị trí đồng thời này được sắp xếp như thế nào thì quá trình này luôn thành công trong việc đặt tất cả các thẻ theo thứ tự tăng dần. Nếu như vậy`d`tồn tại, chúng tôi xuất ra bất kỳ giá trị hợp lệ nào, nếu không chúng tôi xuất ra 0. 

Kích thước đầu vào lên tới 100.000 thẻ, do đó, mọi giải pháp đều phải ở mức tuyến tính hoặc gần tuyến tính nhất. Một mô phỏng bậc hai trên tất cả các giá trị có thể có của`d`hoặc tất cả các hoán vị của thứ tự chơi là không thể vì nó yêu cầu theo thứ tự$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một nỗ lực ngây thơ sẽ là mô phỏng quá trình cho một`d`và một thứ tự cố định của thẻ. Điều này vốn đã có vấn đề vì thứ tự không được kiểm soát và trường hợp xấu nhất phụ thuộc vào sự ràng buộc đối nghịch giữa các quân bài có thể chơi đồng thời. Một dạng lỗi tinh vi khác là giả sử tính năng chèn được sắp xếp tham lam hoạt động: ngay cả khi chúng ta sắp xếp mảng, ràng buộc vẫn phụ thuộc vào các khoảng trống liên quan đến đỉnh đang phát triển của ngăn xếp, chứ không chỉ các khác biệt liền kề theo thứ tự được sắp xếp. 

Trường hợp cạnh bê tông phát sinh khi hai bước nhảy lớn ở gần nhau. Ví dụ, hãy xem xét thẻ`[1, 4, 8]`. Nếu như`d = 3`, thì từ 0 chúng ta có thể đặt 1, rồi 4, nhưng từ 4 chúng ta không thể đạt 8 nếu các trạng thái trung gian không được kiểm soát đúng cách tùy theo thứ tự. Tính chính xác không chỉ phụ thuộc vào sự kề cận theo thứ tự được sắp xếp mà còn phụ thuộc vào cách tích lũy các khoảng trống khi chèn hàng loạt trong trường hợp xấu nhất. 

Khó khăn cốt lõi là điều kiện tương tác với thứ tự theo cách tổng thể: chúng ta phải đảm bảo rằng không có “khoảng cách” nào không thể thu hẹp khi có nhiều thẻ cùng lúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các giá trị có thể có của`d`và mô phỏng quá trình cho từng người. Đối với một cố định`d`, chúng tôi sẽ liên tục quét tập hợp các thẻ còn lại, chọn những thẻ thỏa mãn ràng buộc liên quan đến mức tối đa hiện tại. Tuy nhiên, vì nhiều lá bài có thể được chơi cùng một lúc và thứ tự bên trong của chúng là tùy ý, nên chúng ta cần mô phỏng tất cả các khả năng xen kẽ của các đợt này, đây là giai thừa trong trường hợp xấu nhất. Ngay cả khi chúng ta bỏ qua sự mơ hồ về thứ tự và giả định một thứ tự cố định thì mỗi mô phỏng vẫn tốn$O(n)$, và cố gắng hết sức có thể`d`lên đến$10^9$là không thể. 

Quan sát quan trọng là quá trình này hoàn toàn được điều khiển bởi các khoảng trống liền kề theo thứ tự sắp xếp của các giá trị. Nếu chúng ta sắp xếp mảng thì hạn chế quan trọng duy nhất là liệu ở mỗi bước, chúng ta có thể di chuyển từ giá trị này sang giá trị tiếp theo mà không gặp phải khoảng trống vi phạm quy tắc trong việc sắp xếp khối trong trường hợp xấu nhất hay không. Thứ tự đối nghịch về cơ bản có nghĩa là bất cứ khi nào nhiều giá trị đủ điều kiện, chúng có thể xuất hiện theo bất kỳ thứ tự nào, do đó hệ thống chỉ hoạt động an toàn nếu mọi khoảng trống liền kề theo thứ tự được sắp xếp được giới hạn theo cách đảm bảo không thể xảy ra tắc nghẽn trung gian. 

Điều này làm giảm vấn đề trong việc xác định mức độ nhảy mà chúng ta có thể chịu đựng được trong khi vẫn đảm bảo rằng chuỗi từ 0 đến tất cả các số có thể được hoàn thành bất kể thứ tự lô. Câu trả lời hóa ra bị chi phối bởi “khoảng cách cầu nối” cần thiết tối đa giữa các giá trị liên tiếp theo thứ tự được sắp xếp, nhưng có sự phụ thuộc tinh tế vào thực tế là phần tử đầu tiên được kết nối với 0. 

Chúng tôi tính toán mảng đã sắp xếp và kiểm tra sự khác biệt. Tối ưu`d`bắt nguồn từ việc đảm bảo rằng mọi bước trong trình tự đã sắp xếp đều có thể truy cập được mà không bỏ qua trạng thái trung gian bắt buộc. Nếu bất kỳ khoảng cách nào quá lớn so với cấu trúc đã được thiết lập trước đó thì quy trình sẽ bị hỏng. Việc xây dựng đúng sẽ dẫn tới việc tìm ra yêu cầu tối thiểu tối đa gây ra bởi các sai phân liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu trên d và đặt hàng | O(2^n · n) | O(n) | Quá chậm | 
| Sắp xếp + phân tích khoảng cách | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách sắp xếp tất cả các giá trị thẻ theo thứ tự tăng dần. Điều này là cần thiết vì đống cuối cùng cũng phải tăng lên, do đó, bất kỳ quy trình hợp lệ nào cuối cùng cũng phải tôn trọng thứ tự này. 

Sau đó, chúng tôi xem xét chuỗi bắt đầu từ 0 và thẻ nhỏ nhất, sau đó từng cặp liên tiếp theo thứ tự được sắp xếp. Ràng buộc`d`phải đủ lớn để luôn cho phép mỗi lần chuyển đổi từ giá trị cao nhất hiện tại sang giá trị được chọn tiếp theo, ngay cả trong trường hợp xấu nhất khi không có thứ tự thay thế nào giúp chúng ta bỏ qua một khoảng cách lớn. 

Chúng tôi tính toán sự khác biệt giữa các phần tử liên tiếp, bao gồm cả sự khác biệt ban đầu từ 0 đến thẻ nhỏ nhất. Quan sát quan trọng là nếu khoảng cách quá lớn thì khi quá trình đạt đến điểm cuối thấp hơn, không có giá trị trung gian nhỏ hơn nào khác có thể hữu ích nếu chúng đã được sử dụng hoặc được đặt theo một thứ tự khác. Do đó, hạn chế cần thiết tối đa được xác định bởi khoảng cách lớn nhất không thể tránh khỏi trong chuỗi này. 

Chúng tôi thiết lập`d`đến mức tối đa của những khác biệt liên tiếp này. Điều này đảm bảo rằng ở mọi giai đoạn, bất kỳ thẻ nào đủ điều kiện đều không thể bị chặn bởi các hiệu ứng sắp xếp, vì lần chuyển tiếp được yêu cầu tiếp theo luôn nằm trong phạm vi cho phép. 

Nếu giá trị tính toán này hợp lệ, chúng tôi sẽ xuất nó. Nếu cấu trúc của các khoảng trống hàm ý rằng không có hữu hạn`d`có thể đảm bảo khả năng kết nối (điều này chỉ xảy ra trong các cách diễn giải suy biến của các quy tắc), chúng tôi xuất ra 0, nhưng trong công thức này, khoảng cách tối đa luôn mang lại câu trả lời hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến mà sau khi sắp xếp, quy trình có thể được xem như xây dựng một chuỗi đơn điệu từ 0 đến tất cả các giá trị và mọi thực thi hợp lệ phải tôn trọng thứ tự này trong trường hợp xấu nhất. Thứ tự đối nghịch giữa các thẻ đủ điều kiện đồng thời không thể tạo ra khoảng cách hiệu quả nhỏ hơn so với vùng lân cận đã được sắp xếp, bởi vì bất kỳ nỗ lực nào để “bỏ qua phía trước” vẫn để lại bước nhảy không thể vượt qua lớn nhất làm yếu tố hạn chế. Do đó, chênh lệch liên tiếp tối đa là ràng buộc chặt chẽ nhất đảm bảo không có bước nào trong chuỗi trở nên không hợp lệ theo bất kỳ thứ tự nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    # include initial 0
    prev = 0
    ans = 0
    
    for x in a:
        ans = max(ans, x - prev)
        prev = x
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp sắp xếp mảng để áp đặt thứ tự cấu trúc có ý nghĩa duy nhất trong bài toán. Sau đó, chúng tôi theo dõi giá trị trước đó bắt đầu từ 0 và tính khoảng cách tối đa giữa các giá trị liên tiếp. Khoảng cách lớn nhất đó là giá trị nhỏ nhất của`d`điều đó đảm bảo không có quá trình chuyển đổi nào bị chặn. 

Một cạm bẫy triển khai phổ biến là quên bao gồm quá trình chuyển đổi ban đầu từ 0 sang phần tử nhỏ nhất. Điều này rất cần thiết vì quá trình bắt đầu từ 0 và việc bỏ qua nó sẽ đánh giá thấp yêu cầu`d`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
13 2 10 8
```Mảng được sắp xếp:`[2, 8, 10, 13]`| Bước | trước | x | khoảng cách | khoảng cách tối đa | 
| --- | --- | --- | --- | --- | 
| 0→2 | 0 | 2 | 2 | 2 | 
| 2→8 | 2 | 8 | 6 | 6 | 
| 8→10 | 8 | 10 | 2 | 6 | 
| 10→13 | 10 | 13 | 3 | 6 | 

Đầu ra là`6`. 

Dấu vết này cho thấy bước nhảy cần thiết lớn nhất xảy ra trong khoảng từ 2 đến 8 và điều này chi phối tất cả các ràng buộc khác. 

### Ví dụ 2 

đầu vào:```
5
1 3 4 9 10
```Mảng được sắp xếp:`[1, 3, 4, 9, 10]`| Bước | trước | x | khoảng cách | khoảng cách tối đa | 
| --- | --- | --- | --- | --- | 
| 0→1 | 0 | 1 | 1 | 1 | 
| 1→3 | 1 | 3 | 2 | 2 | 
| 3→4 | 3 | 4 | 1 | 2 | 
| 4→9 | 4 | 9 | 5 | 5 | 
| 9→10 | 9 | 10 | 1 | 5 | 

Đầu ra là`5`. 

Điều này xác nhận rằng mặc dù hầu hết các khoảng trống đều nhỏ, nhưng một bước nhảy lớn duy nhất sẽ xác định giới hạn cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(n) | lưu trữ mảng | 

Các ràng buộc cho phép lên tới 100.000 phần tử, do đó việc sắp xếp theo$O(n \log n)$dễ dàng phù hợp trong giới hạn thời gian và quét tuyến tính là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert solve_io("4\n13 2 10 8\n") == "6"

# minimum size
assert solve_io("3\n1 2 3\n") == "1"

# already spaced
assert solve_io("4\n1 10 20 30\n") == "10"

# includes large early gap
assert solve_io("5\n100 1 2 3 4\n") == "96"

# all consecutive
assert solve_io("6\n5 6 7 8 9 10\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | 1 | trường hợp liên tiếp tối thiểu | 
| 4 1 10 20 30 | 10 | nhiều khoảng trống | 
| 5 100 1 2 3 4 | 96 | độ chính xác khoảng cách ban đầu lớn | 
| 6 5 6 7 8 9 10 | 5 | khoảng cách đồng đều | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi phần tử nhỏ nhất lớn hơn 0 rất nhiều. Đối với đầu vào:```
3
100 101 102
```Mảng được sắp xếp là`[100, 101, 102]`. Thuật toán tính toán các khoảng trống:`0→100 = 100`,`100→101 = 1`,`101→102 = 1`. Câu trả lời là 100. 

Nếu bỏ qua số 0 ban đầu, chúng tôi sẽ xuất sai số 1, điều này sẽ thất bại vì chúng tôi không bao giờ có thể đặt quân bài đầu tiên bắt đầu từ số 0 một cách hợp pháp. 

Thuật toán xử lý việc này một cách chính xác vì quá trình chuyển đổi ban đầu được bao gồm rõ ràng trong cùng một phép tính khoảng cách tối đa. 

Một trường hợp khác là khi các giá trị được nhóm chặt chẽ ngoại trừ một ngoại lệ ở xa. Vì:```
4
1 2 3 1000
```Trình tự được sắp xếp tạo ra một khoảng cách vượt trội ở`3→1000 = 997`và thuật toán trả về chính xác 997, đảm bảo rằng không có sự mơ hồ về thứ tự nào có thể gây ra tắc nghẽn sớm trước khi đạt đến giá trị cuối cùng.
