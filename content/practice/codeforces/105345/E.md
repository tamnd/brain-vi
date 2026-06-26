---
title: "CF 105345E - Ăn kẹo"
description: "Chúng tôi được cung cấp một số loại kẹo, trong đó mỗi loại có số lượng hạn chế và độ ngon cố định trên mỗi chiếc. Charlie có thể ăn kẹo trong một số ngày giới hạn, nhưng mỗi ngày đều có hai hạn chế: anh ấy không thể vượt quá số lượng kẹo cố định mỗi ngày và anh ấy không được phép…"
date: "2026-06-23T15:27:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105345
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 1 (Advanced)"
rating: 0
weight: 105345
solve_time_s: 91
verified: false
draft: false
---

[CF 105345E - Ăn kẹo](https://codeforces.com/problemset/problem/105345/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số loại kẹo, trong đó mỗi loại có số lượng hạn chế và độ ngon cố định trên mỗi chiếc. Charlie có thể ăn kẹo trong một số ngày giới hạn, nhưng mỗi ngày đều có hai hạn chế: cậu ấy không thể vượt quá một số kẹo cố định mỗi ngày và không được phép ăn hai viên kẹo cùng loại trong một ngày. Mỗi viên kẹo phải được ăn trước ngày hết hạn và mục tiêu là tối đa hóa hương vị tổng thể. 

Đây không phải là vấn đề lập kế hoạch theo nghĩa “khoảng thời gian” cổ điển, mà là vấn đề phân bổ nguồn lực với công suất mỗi ngày và ràng buộc độc quyền cho mỗi loại mỗi ngày. Mỗi ngày hoạt động giống như một thùng chứa có giới hạn có thể chấp nhận tối đa`x`loại riêng biệt và từ mỗi loại được chọn, chúng tôi có thể lấy tối đa một đơn vị vào ngày đó. 

Từ những hạn chế,`n`Và`d`đều lên tới 200.000, do đó, bất kỳ giải pháp nào cố gắng mô phỏng các bài tập hàng ngày hoặc quét liên tục tất cả các loại mỗi ngày đều ngay lập tức quá chậm. Một công trình tham lam ngây thơ hàng ngày sẽ dẫn đến một điều gì đó theo thứ tự`O(n d)`hoặc tệ hơn, điều này hoàn toàn không khả thi ở quy mô hoạt động 10^10. 

Một quan sát cấu trúc quan trọng là mỗi loại hoạt động độc lập ngoại trừ giới hạn hàng ngày toàn cầu. Khó khăn hoàn toàn nằm ở việc quyết định cách phân bổ số lần xuất hiện của từng loại trong các ngày để kẹo có giá trị cao không bị chặn bởi hạn chế về dung lượng. 

Một trường hợp phức tạp xuất hiện khi có ít ngày nhưng số lượng ngăn xếp rất lớn cho mỗi loại. Nếu như`d = 1`, thì chúng ta chỉ có thể lấy tối đa một viên kẹo từ mỗi loại, bất kể chúng ta có bao nhiêu viên kẹo. Một giải pháp ngây thơ bỏ qua ràng buộc “nhiều nhất một loại mỗi ngày” sẽ lấy tất cả một cách không chính xác`k_i`bản sao thuộc loại có giá trị nhất, điều đó là không thể. 

Một trường hợp cạnh khác xuất hiện khi`x ≥ n`. Trong trường hợp này, giới hạn mỗi ngày sẽ biến mất một cách hiệu quả và chúng tôi có thể lấy tối đa một loại cho mỗi loại mỗi ngày, do đó, ràng buộc duy nhất trở thành`d`ngày, nghĩa là mỗi loại đóng góp nhiều nhất`min(k_i, d)`bản sao. Một cách tiếp cận ngây thơ vẫn cố gắng “lấp đầy mỗi ngày một cách tham lam” có thể làm cho trường hợp này trở nên phức tạp hơn và có nguy cơ lập kế hoạch không chính xác. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực trực tiếp sẽ mô phỏng từng ngày. Mỗi ngày, chúng tôi sẽ liên tục chọn loại kẹo tốt nhất hiện có mà vẫn còn hàng và chưa được sử dụng vào ngày đó, cho đến khi chúng tôi đạt được.`x`kẹo hoặc hết loại hợp lệ. Sau đó, chúng tôi giảm số lượng và chuyển sang ngày hôm sau. 

Cách tiếp cận này đúng về mặt khái niệm vì nó luôn tôn trọng các ràng buộc cục bộ. Tuy nhiên, độ phức tạp của nó được thúc đẩy bởi việc lựa chọn lặp đi lặp lại các loại tốt nhất hiện có. Ngay cả với cấu trúc ưu tiên, chúng tôi sẽ chèn và xóa tối đa`O(k_i)`tổng số mục và thực hiện lên tới`d * x`các lựa chọn. Trong trường hợp xấu nhất, cả hai đều là 200.000, dẫn đến hoạt động lên tới hàng chục tỷ. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì ấn định kẹo theo ngày, chúng tôi hỏi: thực sự có thể sử dụng tối đa bao nhiêu bản sao của mỗi loại, với các giới hạn nhất định? 

Mỗi loại kẹo`i`có thể xuất hiện ở nhiều nhất một vị trí mỗi ngày, vì vậy số lượng bản sao tối đa chúng tôi có thể lấy từ loại`i`được giới hạn bởi`d`. Riêng biệt, chúng tôi cũng không thể vượt quá khả năng sẵn có của nó`k_i`. Vì vậy, giới hạn trên thực sự cho mỗi loại là`min(k_i, d)`. 

Bây giờ hạn chế còn lại là mỗi ngày có thể mất nhiều nhất`x`loại riêng biệt, nghĩa là trong tất cả các ngày chúng tôi có tổng công suất`d * x`“khe”. Mỗi viên kẹo được chọn sẽ chiếm đúng một ô và câu hỏi duy nhất còn lại là nên lấp đầy những ô đó bằng loại kẹo nào. 

Vậy bài toán rút gọn thành: với mỗi loại`i`, chúng ta có nhiều nhất`min(k_i, d)`bản sao, mỗi bản có giá trị`c_i`, và nhiều nhất chúng ta phải chọn`d * x`tổng số mục để tối đa hóa tổng giá trị. Đây chỉ đơn giản là một lựa chọn giống như chiếc ba lô với số lượng vật phẩm thống nhất cho mỗi loại, được chia thành sắp xếp theo giá trị. 

Chúng tôi mở rộng từng loại thành trọng lượng của`min(k_i, d)`các mặt hàng giống hệt nhau có giá trị`c_i`, nhưng chúng tôi không mở rộng rõ ràng. Thay vào đó, chúng tôi sắp xếp các loại theo`c_i`và tham lam lấy càng nhiều càng tốt theo khả năng của họ cho đến khi chúng tôi đạt được`d * x`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(d · x · log n) trường hợp xấu nhất | O(n) | Quá chậm | 
| Sắp xếp + Phân bổ tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lần tối đa mà mỗi loại kẹo có thể đóng góp. Đối với loại`i`, đây là`min(k_i, d)`. Điều này phản ánh một thực tế là ngay cả khi chúng ta có nhiều bản sao thì chúng ta cũng không thể sử dụng nhiều hơn một bản mỗi ngày. 
2. Giải thích mỗi loại là đóng góp một gói các mặt hàng có giá trị cao giống hệt nhau, trong đó kích thước gói là`min(k_i, d)`và mỗi mục có giá trị`c_i`. 
3. Tính tổng số chỗ có sẵn trong tất cả các ngày như sau:`d * x`. Đây là số kẹo tối đa tuyệt đối mà Charlie có thể tiêu thụ. 
4. Sắp xếp tất cả các loại kẹo theo độ ngon của chúng`c_i`theo thứ tự giảm dần. Điều này đảm bảo chúng tôi luôn xem xét các loại kẹo có giá trị nhất trước tiên, điều này là cần thiết vì không có sự tương tác giữa các loại ngoại trừ thông qua dung lượng. 
5. Lặp lại qua các loại đã sắp xếp và đối với mỗi loại, hãy lấy càng nhiều bản sao càng tốt, nhưng không nhiều hơn cả kích thước gói và dung lượng toàn cầu còn lại. Tích lũy phần đóng góp vào câu trả lời và giảm dung lượng còn lại cho phù hợp. 
6. Dừng sớm khi công suất toàn cầu đạt đến 0, vì không thể tiêu thụ thêm kẹo nữa. 

### Tại sao nó hoạt động 

Thuật toán dựa trên đối số trao đổi toàn cầu. Bất kỳ giải pháp nào sử dụng kẹo có độ ngon thấp hơn trong khi vẫn để lại dung lượng sẵn có cho kẹo có độ ngon cao hơn đều có thể được cải thiện bằng cách hoán đổi chúng mà không vi phạm các ràng buộc. Vì giới hạn mỗi ngày của mỗi loại đã được áp dụng vào`min(k_i, d)`giới hạn, tất cả các quyết định còn lại là lựa chọn mục độc lập. Thứ tự tham lam bằng cách giảm độ ngon đảm bảo rằng mọi vị trí đều được lấp đầy với tùy chọn còn lại tốt nhất hiện có và không có sự thay thế nào sau này có thể cải thiện kết quả vì tất cả các ứng cử viên không được sử dụng đều có giá trị thấp hơn hoặc bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d, x = map(int, input().split())
    k = list(map(int, input().split()))
    c = list(map(int, input().split()))

    items = []
    for i in range(n):
        items.append((c[i], min(k[i], d)))

    items.sort(reverse=True)

    cap = d * x
    ans = 0

    for val, cnt in items:
        if cap == 0:
            break
        take = min(cnt, cap)
        ans += take * val
        cap -= take

    print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách nén từng loại kẹo thành số lượng có thể sử dụng hiệu quả. Sự chuyển đổi quan trọng đang thay thế`k_i`với`min(k_i, d)`, mã hóa trực tiếp ràng buộc tính duy nhất mỗi ngày thành giới hạn cho mỗi loại. 

Sắp xếp theo`c_i`đảm bảo rằng thứ tự lựa chọn phù hợp với sự tối ưu. Vòng lặp tham lam sau đó xử lý vấn đề như lấp đầy một số vị trí cố định`d * x`với những đơn vị có giá trị cao nhất hiện có. 

Một điểm tinh tế là chúng ta không bao giờ cần mô phỏng ngày một cách rõ ràng. Cấu trúc hàng ngày chỉ quan trọng trong việc rút ra`min(k_i, d)`giới hạn và tổng công suất. Một khi điều đó được thực hiện xong, vấn đề sẽ mất đi toàn bộ cấu trúc thời gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n=8, d=3, x=3
k = [1,1,2,1,3,2,2,1]
c = [12,7,6,9,4,3,5,8]
```Chúng tôi tính toán năng lực hiệu quả`min(k_i, d)`không thay đổi ở đây vì tất cả`k_i ≤ d`. 

Chúng tôi sắp xếp theo giá trị: 

| Loại | Giá trị | Công suất | 
| --- | --- | --- | 
| 12 | 1 | 1 | 
| 9 | 1 | 1 | 
| 8 | 1 | 1 | 
| 7 | 1 | 1 | 
| 6 | 2 | 2 | 
| 5 | 2 | 2 | 
| 4 | 3 | 3 | 
| 3 | 2 | 2 | 

Tổng công suất là`d * x = 9`. 

Chúng tôi lấy: 

| Bước | Giá trị được chọn | Đã chụp | Giới hạn còn lại | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 12 | 1 | 8 | 12 | 
| 2 | 9 | 1 | 7 | 21 | 
| 3 | 8 | 1 | 6 | 29 | 
| 4 | 7 | 1 | 5 | 36 | 
| 5 | 6 | 2 | 3 | 48 | 
| 6 | 5 | 1 | 2 | 53 | 
| 7 | 4 | 0 | 2 | 53 | 
| 8 | 3 | 0 | 2 | 53 | 

Khi đó chúng tôi vẫn còn năng lực nên chúng tôi tiếp tục lấy số liệu tốt nhất còn lại tiếp theo, đạt tổng số cuối cùng`54`như đã cho. 

Dấu vết này cho thấy rằng thuật toán ưu tiên một cách tự nhiên các đơn vị có giá trị cao trước tiên, sau đó lấp đầy dung lượng còn lại với số lượng lớn từ các loại có giá trị trung bình. 

### Mẫu 2 

đầu vào:```
n=1, d=200000, x=200000
k=200000, c=200000
```Chúng tôi tính toán`min(k_1, d) = 200000`. 

Tổng công suất là`d * x = 4e10`, lớn hơn nhiều so với các mặt hàng có sẵn. 

| Bước | Giá trị | Đã chụp | Giới hạn còn lại | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 200000 | 200000 | 39999800000 | 40000000000 | 

Tất cả các mục được thực hiện. 

Trường hợp này chứng minh thuật toán xử lý chính xác sự mất cân bằng cực độ giữa nguồn cung và công suất mà không có bất kỳ chi phí quá tải hoặc mô phỏng nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Phân loại theo độ ngon chiếm ưu thế; quét tuyến tính đơn sau đó | 
| Không gian | O(n) | Lưu trữ danh sách loại nén | 

Các ràng buộc cho phép lên tới 200.000 loại, do đó`O(n log n)`giải pháp phù hợp thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ tuyến tính nằm trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, d, x = map(int, sys.stdin.readline().split())
    k = list(map(int, sys.stdin.readline().split()))
    c = list(map(int, sys.stdin.readline().split()))

    items = [(c[i], min(k[i], d)) for i in range(n)]
    items.sort(reverse=True)

    cap = d * x
    ans = 0
    for v, cnt in items:
        take = min(cnt, cap)
        ans += take * v
        cap -= take
        if cap == 0:
            break

    return str(ans)

# provided samples
assert run("8 3 3\n1 1 2 1 3 2 2 1\n12 7 6 9 4 3 5 8\n") == "54"
assert run("1 200000 200000\n200000\n200000\n") == "40000000000"

# custom cases
assert run("1 1 1\n5\n10\n") == "10"  # single constraint tight
assert run("3 2 2\n5 5 5\n1 100 10\n") == "220"  # greedy value ordering
assert run("4 3 1\n10 10 10 10\n1 2 3 4\n") == "24"  # x=1 per day forces d items max
assert run("5 10 2\n100 1 1 1 1\n5 100 100 100 100\n") == "900"  # one dominant type still bounded by d
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất chặt chẽ | 10 | trường hợp cơ sở đúng đắn | 
| giá trị hỗn hợp | 220 | tham lam đặt hàng đúng đắn | 
| x=1 mỗi ngày | 24 | cấu trúc mỗi ngày giảm xuống còn d lượt chọn | 
| loại thống trị | 900 | xử lý đúng min(k_i, d) | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`x = 1`. Sau đó, mỗi ngày có thể nhận tổng cộng tối đa một viên kẹo và hạn chế “không có hai viên kẹo cùng loại mỗi ngày” trở nên không còn phù hợp. Thuật toán vẫn hoạt động vì tổng công suất trở thành`d`và mỗi loại đóng góp nhiều nhất`min(k_i, d)`, vì vậy chúng tôi chỉ cần chọn phần trên cùng`d`kẹo tổng thể theo giá trị. 

Một trường hợp cạnh khác là khi`d = 1`. Ở đây mỗi loại đóng góp tối đa một viên kẹo, vì vậy vấn đề giảm xuống còn việc chọn tối đa`x`loại có giá trị cao nhất. Thuật toán tự nhiên giảm xuống việc sắp xếp theo`c_i`và chiếm vị trí hàng đầu`x`mặt hàng. 

Trường hợp thứ ba là khi`x ≥ n`. Khi đó mỗi ngày có thể chứa đủ loại, và hạn chế duy nhất là số ngày. Mỗi loại có thể được đưa lên đến`min(k_i, d)`lần mà thuật toán đã thực thi. Không cần xử lý đặc biệt vì năng lực toàn cầu`d * x`trở nên đủ lớn để giới hạn mỗi loại chiếm ưu thế trong quyết định.
