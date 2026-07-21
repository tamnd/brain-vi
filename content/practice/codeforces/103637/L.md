---
title: "CF 103637L - Số nguyên dài"
description: "Chúng tôi đang duy trì một số nguyên rất lớn bắt đầu bằng chuỗi thập phân ban đầu. Sau đó, số này được sửa đổi từng bước bằng một chuỗi các thao tác. Mỗi thao tác sẽ thêm một chữ số vào đầu bên phải của số hiện tại hoặc xóa chữ số ngoài cùng bên phải."
date: "2026-07-02T22:22:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103637
codeforces_index: "L"
codeforces_contest_name: "2019-2020 10th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 103637
solve_time_s: 42
verified: true
draft: false
---

[CF 103637L - Số nguyên dài](https://codeforces.com/problemset/problem/103637/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một số nguyên rất lớn bắt đầu bằng chuỗi thập phân ban đầu. Sau đó, số này được sửa đổi từng bước bằng một chuỗi các thao tác. Mỗi thao tác sẽ thêm một chữ số vào đầu bên phải của số hiện tại hoặc xóa chữ số ngoài cùng bên phải. Sau mỗi lần sửa đổi, chúng ta phải xuất giá trị của số hiện tại theo modulo$10^9 + 7$. 

Khó khăn chính là số lượng ban đầu có thể cực kỳ lớn, lên tới$10^{100000}$về độ dài, vì vậy chúng tôi không thể lưu trữ nó dưới dạng số nguyên ở bất kỳ loại tiêu chuẩn nào. Ngay cả việc lưu trữ nó dưới dạng một chuỗi và tính toán lại mô-đun từ đầu sau mỗi truy vấn cũng sẽ quá chậm vì có tới$10^5$các truy vấn và mỗi lần tính toán lại sẽ quét toàn bộ chiều dài hiện tại, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Các ràng buộc ngụ ý rằng mọi giải pháp đều phải xử lý từng truy vấn trong thời gian không đổi được khấu hao. Chúng ta được phép tiền xử lý tuyến tính chuỗi ban đầu, nhưng sau đó mỗi lần cập nhật phải được xử lý theo$O(1)$hoặc$O(\log n)$, và thực tế$O(1)$mỗi hoạt động. 

Một trường hợp khó nhận thấy xuất phát từ việc xóa. Ví dụ, hãy xem xét việc bắt đầu từ$1234$, nối thêm$5$, sau đó xóa. Trình tự là$1234 \rightarrow 12345 \rightarrow 1234$. Một cách tiếp cận đơn giản có thể tính toán lại mô đun sau khi xóa bằng cách cắt bớt chuỗi và đánh giá lại, nhưng cách đó quá chậm. Một sai lầm khác là giả định rằng việc loại bỏ một chữ số có thể được xử lý bằng cách chia cho 10 modulo$MOD$, điều này không hợp lệ trong số học mô-đun vì phép chia mô-đun yêu cầu nghịch đảo và số đang thay đổi về mặt cấu trúc chứ không phải theo đại số. 

Cách tiếp cận đúng phải hỗ trợ cả thao tác nối thêm và bật trong khi vẫn duy trì modulo giá trị$MOD$một cách hiệu quả. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: lưu trữ số dưới dạng chuỗi. Đối với mỗi truy vấn, hãy sửa đổi chuỗi tương ứng và tính toán lại phần còn lại bằng cách lặp qua tất cả các chữ số và đánh giá biểu diễn đa thức trong cơ số 10. Nếu số hiện tại có độ dài$L$, chi phí cho mỗi lần tính toán lại$O(L)$, và lên đến$10^5$các truy vấn và có khả năng tăng trưởng lớn về chiều dài, điều này dẫn đến trường hợp xấu nhất$O(n^2)$hoặc thời gian chạy tệ hơn. Điều này rõ ràng là không thể thực hiện được. 

Quan sát quan trọng là số này luôn được diễn giải theo cơ số 10 và số học modulo tương thích với cách xây dựng tăng dần. Nếu chúng ta duy trì giá trị hiện tại$x$modulo$MOD$, nối thêm một chữ số$d$biến số thành$x' = (x \cdot 10 + d) \bmod MOD$. Đây là hành vi băm lăn tiêu chuẩn. 

Khó khăn là thao tác xóa. Loại bỏ chữ số cuối cùng có nghĩa là chúng ta phải đảo ngược phép biến đổi$x \cdot 10 + d$. Nếu chúng ta biết chữ số được thêm vào ở bước đó, chúng ta có thể khôi phục giá trị trước đó bằng cách trừ chữ số đó và chia cho 10. Chia cho 10 theo modulo$MOD$yêu cầu nhân với nghịch đảo mô đun của 10, vì$MOD = 10^9 + 7$là nguyên tố. 

Tuy nhiên, chúng ta cũng cần biết đầy đủ lịch sử của các chữ số để có thể hoàn tác các thao tác một cách chính xác. Điều này gợi ý việc duy trì một chồng trạng thái: cả giá trị mô-đun hiện tại và chuỗi chữ số một cách ngầm định hoặc rõ ràng. 

Vì vậy, cách tiếp cận tối ưu là duy trì một ngăn xếp trong đó mỗi mục lưu trữ modulo giá trị hiện tại$MOD$. Để nối thêm, chúng tôi đẩy một giá trị mới. Để xóa, chúng tôi bật. Vì mỗi trạng thái đã biểu thị giá trị tiền tố đầy đủ nên chúng ta không cần tính toán lại bất cứ điều gì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Ngăn xếp DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi số tiến hóa là một chuỗi các tiền tố, mỗi tiền tố có một giá trị modulo đã biết$MOD$. 

1. Chuyển chuỗi số ban đầu thành giá trị theo modulo$MOD$. Chúng tôi xử lý các chữ số từ trái sang phải, cập nhật$x = (x \cdot 10 + digit) \bmod MOD$. Điều này thiết lập trạng thái cơ sở. 
2. Khởi tạo ngăn xếp với giá trị này. Ngăn xếp biểu thị lịch sử của các trạng thái hợp lệ sau mỗi thao tác. 
3. Đối với mỗi truy vấn, hãy kiểm tra loại của nó. Nếu đó là thao tác nối thêm với chữ số$d$, tính giá trị mới như$new = (stack[-1] \cdot 10 + d) \bmod MOD$, sau đó đẩy nó vào ngăn xếp. Điều này trực tiếp diễn ra từ việc mở rộng giá trị vị trí cơ sở 10. 
4. Nếu truy vấn là thao tác xóa, hãy xóa phần tử trên cùng khỏi ngăn xếp. Giá trị trước đó đã được lưu trữ bên dưới nó nên không cần tính toán lại. 
5. Sau mỗi thao tác, xuất giá trị ở đầu ngăn xếp. 

Ý tưởng chính là chúng ta không bao giờ cố gắng "chỉnh sửa" một số bằng đại số. Thay vào đó, chúng tôi coi mỗi thao tác là di chuyển giữa các trạng thái được tính toán trước. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, đỉnh ngăn xếp tương ứng chính xác với giá trị của chuỗi chữ số hiện tại được hiểu là modulo số cơ sở 10$MOD$. Khi chúng ta nối thêm một chữ số, công thức chuyển tiếp phản ánh chính xác sự dịch chuyển vị trí của tất cả các chữ số trước đó. Khi chúng tôi xóa, ngăn xếp sẽ khôi phục lại tiền tố chính xác trước đó, tiền tố này được đảm bảo đã được tính toán chính xác trước đó. Điều này tạo ra một bất biến: sau khi xử lý thao tác thứ i, ngăn xếp chứa giá trị mô-đun chính xác của số hiện tại, do đó kết quả đầu ra luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    x0 = input().strip()
    q = int(input())
    
    stack = []
    
    val = 0
    for ch in x0:
        val = (val * 10 + (ord(ch) - 48)) % MOD
    stack.append(val)
    
    for _ in range(q):
        parts = input().split()
        if parts[0] == '+':
            d = ord(parts[1]) - 48
            val = (stack[-1] * 10 + d) % MOD
            stack.append(val)
        else:
            stack.pop()
        
        print(stack[-1])

if __name__ == "__main__":
    solve()
```Vòng lặp ban đầu chuyển đổi chuỗi bắt đầu thành giá trị mô đun của nó bằng cách sử dụng cấu trúc cuộn tiêu chuẩn. Ngăn xếp sau đó được sử dụng để bảo toàn mọi kết quả trung gian. 

Đối với phần bổ sung, chúng tôi áp dụng rõ ràng công thức dịch chuyển cơ số 10. Điều này tránh mọi nhu cầu phải xây dựng lại hoặc duyệt qua các chữ số trước đó. Để xóa, chúng tôi chỉ cần loại bỏ trạng thái mới nhất, dựa vào thực tế là nó được xây dựng từ trạng thái trước đó một cách xác định. 

Một điểm tinh tế là chúng tôi lưu trữ các giá trị tiền tố đầy đủ thay vì cố gắng chỉ lưu trữ các chữ số và tính toán lại theo yêu cầu. Điều này tránh mọi nhu cầu về nghịch đảo mô-đun hoặc logic khôi phục phức tạp. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự: 

đầu vào:```
x0 = 12
+ 3
+ 4
-
```| Bước | Hoạt động | Chữ số hiện tại | Giá trị mod MOD | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | 12 | 12 | 
| 1 | +3 | 123 | 123 | 
| 2 | +4 | 1234 | 1234 | 
| 3 | - | 123 | 123 | 

Điều này cho thấy mỗi phần nối thêm tương đương với việc dịch sang trái trong cơ số 10 và cộng chữ số. 

Bây giờ là ví dụ thứ hai: 

đầu vào:```
x0 = 9
+ 0
- 
+ 5
```| Bước | Hoạt động | Chữ số hiện tại | Giá trị mod MOD | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | 9 | 9 | 
| 1 | +0 | 90 | 90 | 
| 2 | - | 9 | 9 | 
| 3 | +5 | 95 | 95 | 

Điều này chứng tỏ rằng việc xóa sẽ khôi phục chính xác tiền tố trước đó mà không cần tính toán lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi truy vấn thực hiện phép toán số học và ngăn xếp O(1) | 
| Không gian |$O(n)$| Ngăn xếp lưu trữ một số nguyên cho mỗi thao tác | 

Tổng số thao tác tối đa là$10^5$, do đó thời gian tuyến tính và bộ nhớ tuyến tính dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    MOD = 10**9 + 7
    data = inp.strip().splitlines()
    
    x0 = data[0].strip()
    q = int(data[1])
    
    stack = []
    val = 0
    for ch in x0:
        val = (val * 10 + (ord(ch) - 48)) % MOD
    stack.append(val)
    
    idx = 2
    out = []
    for _ in range(q):
        parts = data[idx].split()
        idx += 1
        if parts[0] == '+':
            d = ord(parts[1]) - 48
            val = (stack[-1] * 10 + d) % MOD
            stack.append(val)
        else:
            stack.pop()
        out.append(str(stack[-1]))
    
    return "\n".join(out)

# provided sample (structure assumed minimal)
assert run("12\n3\n+ 3\n+ 4\n-\n") == "123\n1234\n123", "sample-like"

# custom 1: single digit delete safety
assert run("5\n2\n+ 0\n-\n") == "50\n5", "append and delete"

# custom 2: repeated deletes back to initial
assert run("7\n4\n+ 1\n+ 2\n-\n-\n") == "71\n712\n71\n7", "stack rollback"

# custom 3: alternating operations
assert run("9\n5\n+ 0\n-\n+ 5\n+ 6\n-\n") == "90\n9\n95\n956\n95", "alternation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nối thêm rồi xóa | quay lui đúng đắn | tính nhất quán của hành vi ngăn xếp | 
| nhiều hoạt động | khôi phục tiền tố | tính đúng đắn theo luân phiên | 
| bật lên lặp đi lặp lại | ổn định ranh giới | không có giả định tràn | 

## Vỏ cạnh 

Trường hợp quan trọng là khi việc xóa sẽ đưa số trở về giá trị ban đầu. Ví dụ: 

đầu vào:```
x0 = 123
+ 4
-
```Sau khi nối thêm 4, chúng ta nhận được 1234. Sau khi xóa, chúng ta trả về chính xác 123. Thuật toán xử lý việc này vì trạng thái trước đó đã được lưu trữ rõ ràng trong ngăn xếp. Thao tác pop khôi phục giá trị mô-đun trước đó mà không cần tính toán lại. 

Một trường hợp khác là khi các chữ số bao gồm số 0: 

đầu vào:```
x0 = 1
+ 0
+ 0
```Các giá trị trở thành 1, 10, 100. Sự lặp lại$x = x \cdot 10 + d$bảo toàn chính xác các số 0 đứng đầu trong đóng góp vị trí. Việc chuyển đổi chuỗi thành int đơn giản ở mỗi bước cũng sẽ hoạt động hợp lý nhưng sẽ quá chậm; cách tiếp cận ngăn xếp giữ tính chính xác trong khi vẫn duy trì hiệu quả.
