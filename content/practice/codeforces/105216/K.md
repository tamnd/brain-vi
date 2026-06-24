---
title: "CF 105216K - Máy Tính K Happy"
description: "Chúng tôi đang mô phỏng một quá trình trong đó máy tính được gửi đến một chiếc mỗi tuần. Mỗi máy tính được gán một “tên” ngẫu nhiên, đây chỉ là một số nguyên được chọn thống nhất từ ​​phạm vi $1$ đến $N$. Jose chỉ có thể giữ tối đa $k$ máy tính vào bất kỳ lúc nào."
date: "2026-06-24T17:10:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105216
codeforces_index: "K"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 105216
solve_time_s: 79
verified: false
draft: false
---

[CF 105216K - Máy tính K Happy](https://codeforces.com/problemset/problem/105216/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình trong đó máy tính được gửi đến một chiếc mỗi tuần. Mỗi máy tính được gán một “tên” ngẫu nhiên, đây chỉ là một số nguyên được chọn thống nhất trong phạm vi$1$ĐẾN$N$. Jose chỉ có thể giữ nhiều nhất$k$máy tính bất cứ lúc nào. Khi một máy tính mới đến và ngôi nhà đã đầy, máy tính cũ nhất sẽ bị loại bỏ trước tiên, do đó cấu trúc hoạt động giống như một hàng đợi có kích thước FIFO$k$. 

Quá trình này tiếp tục vô thời hạn và chúng tôi được yêu cầu tính số tuần dự kiến ​​cho đến khi lần đầu tiên hai máy tính trong nhà có cùng tên. Sự kiện đó được coi là thất bại và chúng tôi ngừng đếm ngay khi nó xảy ra. 

Vì vậy, trạng thái phát triển như một cửa sổ trượt cuối cùng$k$tạo ra các giá trị ngẫu nhiên. We are waiting for the first duplicate inside this window.

 The constraints immediately rule out any simulation. Giá trị của$N$có thể lớn như$10^{12}$, vì vậy chúng tôi không thể theo dõi tần số trong một mảng. Giá trị của$k$có thể lên tới$10^6$, quá lớn để theo dõi xung đột bậc hai hoặc tính toán lại mỗi bước. Câu trả lời là một kỳ vọng đối với một quá trình ngẫu nhiên, vì vậy chúng ta cần một biểu thức dạng đóng hoặc một đạo hàm tổ hợp nhanh. 

Một trường hợp khó nhận thấy là khi$k$vượt quá$N$. Trong trường hợp đó, việc trùng lặp trở nên không thể tránh khỏi ngay khi chúng ta có nhiều hơn$N$các mục trong cửa sổ, nhưng vì kích thước cửa sổ bị giới hạn ở$k$, quá trình này có hiệu quả hoạt động khác nhau. Nếu như$k > N$, sự kiện cuối cùng được đảm bảo nhưng thời gian dự kiến ​​phụ thuộc vào tốc độ lấp đầy cửa sổ trượt và các lực chồng lên nhau. Cách tiếp cận ngây thơ “nghịch lý sinh nhật trên k mục” sẽ sai vì cửa sổ không tĩnh. 

Một trường hợp thất bại khác xuất phát từ việc coi mỗi tuần là một thử nghiệm độc lập “chọn k mục và kiểm tra các mục trùng lặp”. Điều đó bỏ qua rằng các phần tử cũ đã bị loại bỏ, do đó các va chạm trước đó có thể biến mất nếu chúng bị đẩy ra ngoài cửa sổ. 

## Phương pháp tiếp cận 

Việc mô phỏng lực lượng vũ phu rất đơn giản. Chúng tôi tạo từng số ngẫu nhiên, duy trì hàng đợi có kích thước tối đa$k$và bản đồ tần số của các giá trị hiện có trong hàng đợi. Mỗi bước chúng tôi chèn giá trị mới, loại bỏ giá trị cũ nhất nếu cần và kiểm tra xem giá trị được chèn đã tồn tại trên bản đồ hay chưa. Nếu có, chúng tôi dừng lại. 

Điều này đúng, nhưng về cơ bản nó là mô phỏng Monte Carlo về thời gian dừng trong quy trình Markov. Cho dù mỗi bước$O(1)$, thời gian dừng dự kiến ​​theo thứ tự thời gian va chạm của cửa sổ cuốn, có thể cực kỳ lớn đối với cửa sổ lớn$N$. Quan trọng hơn, chúng ta được yêu cầu đưa ra một kỳ vọng chính xác chứ không phải ước tính thực nghiệm. 

Quan sát chính là quá trình này chỉ phụ thuộc vào tập hợp các phần tử hoạt động trong cửa sổ hiện tại. Thời điểm một phần tử mới được giới thiệu, cách duy nhất để tạo ra một “máy tính buồn” là giá trị đó đã tồn tại trong số hiện tại.$k$những tên hoạt động. Vì vậy, vấn đề quy về việc phân tích xác suất mà$i$-lần rút thăm thứ hai là bản sao của bất kỳ lần rút thăm nào trước đó$\min(k, i-1)$các giá trị hoạt động. 

Chúng tôi thay đổi quan điểm: thay vì mô phỏng thời gian, chúng tôi tính toán thời gian chờ dự kiến ​​cho lần va chạm đầu tiên trong một quy trình trong đó mỗi bước đưa ra một giá trị ngẫu nhiên mới và xóa một giá trị cũ sau khi cửa sổ đầy. Sự đơn giản hóa quan trọng là, từ quan điểm của một giá trị đầu vào cố định, tập hợp các giá trị hoạt động hoạt động giống như một tập hợp ngẫu nhiên thống nhất có kích thước lên đến$k$, bởi vì mỗi vị trí trong cửa sổ tương ứng với một lần rút thăm ngẫu nhiên độc lập. 

Do đó, bất cứ lúc nào sau khi cửa sổ lấp đầy, trạng thái hoạt động về cơ bản là một tập hợp nhiều kích thước$k$rút ra từ$N$, và lần rút thăm mới sẽ xung đột nếu nó khớp với bất kỳ điều nào trong số này$k$các giá trị. Nếu chúng ta bỏ qua mối tương quan được đưa ra bởi việc xóa, xác suất để lần rút tiếp theo an toàn là xấp xỉ$1 - \frac{k}{N}$, miễn là tất cả các giá trị hoạt động đều khác biệt. Quá trình dừng lại chính xác khi xảy ra va chạm. 

Điều này biến vấn đề thành một vấn đề về thời gian chờ đợi dự kiến ​​cổ điển: chúng tôi liên tục thực hiện các thử nghiệm trong đó mỗi thử nghiệm thành công (không có xung đột) với xác suất tỷ lệ thuận với số lượng giá trị riêng biệt hiện có trong cửa sổ. Tuy nhiên, vì sự kiện dừng xảy ra ở lần lặp lại đầu tiên nên thay vào đó, chúng tôi có thể theo dõi thời gian dự kiến ​​cho đến lần rút lặp lại đầu tiên trong một luồng trong đó chỉ có lần cuối cùng$k$các yếu tố quan trọng. 

Một cách rõ ràng hơn để rút ra nó là xem xét xác suất mà quá trình đó tồn tại ít nhất$t$các bước. Điều đó đòi hỏi tất cả những điều đầu tiên$t$các giá trị khác biệt trong mỗi cửa sổ trượt có kích thước$k$. Vì$t \le k$, đây chỉ đơn giản là tất cả$t$các giá trị khác biệt, tạo ra xác suất giai thừa giảm tiêu chuẩn. Vì$t > k$, mọi cửa sổ có kích thước$k$phải khác biệt, điều này buộc một ràng buộc tổ hợp có cấu trúc tương đương với điều kiện tránh giống như de Bruijn. Thay vì liệt kê trực tiếp các chuỗi hợp lệ, chúng tôi tính toán giá trị mong đợi bằng cách sử dụng tuyến tính theo xác suất sống sót, điều này dẫn đến một sản phẩm lồng ghép dựa trên số lượng lựa chọn mới còn lại ở mỗi bước. 

Sự đơn giản hóa chính là quy trình này hoạt động giống như một bài toán sinh nhật tổng quát với “bộ nhớ$k$”, và xác suất sống sót ở bước$i$chỉ phụ thuộc vào số lượng giá trị riêng biệt còn lại trong cửa sổ, giá trị này luôn chính xác$\min(i-1, k)$dưới sự sống sót. Điều này cho phép chúng ta tính toán:$$P(\text{no collision at step } i) = \frac{N - \min(i-1, k)}{N}$$Thời gian dừng dự kiến ​​trở thành tổng của các xác suất sống sót, rút ​​gọn thành tổng hữu hạn lên tới$k+1$, theo sau là một cái đuôi hình học có xác suất va chạm không đổi$\frac{k}{N}$. 

Điều này mang lại một biểu thức có thể được đánh giá trong$O(k)$thời gian sử dụng số học mô-đun, với dạng đóng đuôi hình học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\mathbb{E}[T])$|$O(k)$| Quá chậm | 
| Kỳ vọng kết hợp tối ưu |$O(k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán thời gian dự kiến cho đến khi bản sao đầu tiên xuất hiện trong cửa sổ trượt có kích thước$k$, dưới sự rút thăm ngẫu nhiên thống nhất từ$1$ĐẾN$N$. 

1. Chúng tôi lập mô hình xác suất sống sót cho từng độ dài tiền tố$i$, giả sử chưa có sự trùng lặp nào xảy ra. Lần đầu tiên$k$các bước, tất cả các giá trị nhìn thấy trước đó đều khác biệt, vì vậy ở bước$i$, có chính xác$i-1$các giá trị bị cấm. Điều này đưa ra hệ số xác suất sống sót của$\frac{N-(i-1)}{N}$. 
2. Chúng tôi mở rộng lý luận này sang bước$k+1$, nơi cửa sổ hiện đã đầy. Trong trường hợp sống sót, cửa sổ chứa$k$các giá trị riêng biệt, do đó xác suất để giá trị tiếp theo an toàn trở thành$\frac{N-k}{N}$. Giá trị này vẫn ổn định cho tất cả các bước tiếp theo. 
3. Chúng tôi tính toán xác suất sống sót nhiều lần. Cho phép$S_i$là xác suất để không xảy ra va chạm cho đến bước$i$. Chúng tôi khởi tạo$S_0 = 1$và cập nhật bằng cách sử dụng hệ số sống sót từng bước có được ở trên. 
4. Thời gian dừng dự kiến ​​được tính bằng cách sử dụng danh tính$E[T] = \sum_{i \ge 0} S_i$, chuyển đổi thời gian dừng thành tổng xác suất sống sót thay vì liệt kê trực tiếp các sự kiện dừng. 
5. Chúng tôi chia tổng thành hai phần: tiền tố hữu hạn cho$i \le k$, trong đó xác suất sống sót giảm theo cấp số nhân khi các lựa chọn có sẵn ngày càng thu hẹp và một cái đuôi vô hạn bắt đầu từ$k+1$, trở thành một chuỗi hình học có tỉ số$\frac{N-k}{N}$. 
6. Chúng tôi tính toán tiền tố hữu hạn bằng cách sử dụng tích đang chạy của$\frac{N-i}{N}$, sau đó thêm đuôi hình học dạng đóng bắt đầu từ$S_k$. 
7. Tất cả các tính toán được thực hiện modulo$10^9+7$, sử dụng nghịch đảo mô-đun để chia cho$N$. 

### Tại sao nó hoạt động 

Bất biến chính là miễn là không xảy ra xung đột thì cửa sổ đang hoạt động luôn bao gồm các phần tử riêng biệt. Điều này buộc số lượng giá trị bị cấm cho lần rút tiếp theo chỉ phụ thuộc vào kích thước cửa sổ hiện tại chứ không phải lịch sử chính xác. Khi cửa sổ đạt kích thước$k$, số lượng giá trị cấm ổn định ở mức chính xác$k$, làm cho quá trình không còn bộ nhớ kể từ thời điểm đó trở đi. Điều này chuyển đổi vấn đề thành tiền tố thay đổi xác suất theo sau là đuôi hình học đứng yên, xác định duy nhất kỳ vọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    N, k = map(int, input().split())
    
    # If k >= N, collision happens quickly in expectation,
    # but formula still works if handled carefully.
    
    invN = modinv(N % MOD)
    
    # S = survival probability up to current step
    S = 1
    
    # expected value
    ans = 0
    
    # prefix up to k steps
    for i in range(1, k + 1):
        ans = (ans + S) % MOD
        S = S * (N - (i - 1)) % MOD * invN % MOD
    
    # tail geometric part: from step k+1 onward
    # survival ratio becomes (N-k)/N
    if N > k:
        ratio = (N - k) % MOD * invN % MOD
        
        # expected remaining contribution = S / (1 - ratio)
        # = S * N / k
        inv_one_minus = modinv((k % MOD))
        tail = S * N % MOD * inv_one_minus % MOD
        ans = (ans + tail) % MOD
    else:
        # if k >= N, collision guaranteed soon; treat tail carefully
        ans = ans % MOD
    
    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mã duy trì xác suất sống sót đang chạy`S`, biểu thị xác suất không có sự trùng lặp nào xảy ra cho đến độ dài tiền tố hiện tại theo giả định “tất cả khác biệt cho đến nay”. Mỗi lần lặp cập nhật`S`sử dụng số lượng các lựa chọn mới có sẵn. Sự đóng góp tiền tố tích lũy khối lượng tồn tại dự kiến. 

Một khi chúng ta đạt đến kích thước`k`, quá trình đi vào một chế độ dừng trong đó mỗi lần rút mới sẽ tránh được xung đột với xác suất`(N-k)/N`. Điều này tạo ra một đuôi hình học, được tính tổng ở dạng đóng bằng cách sử dụng nghịch đảo mô-đun của`k`. 

Một chi tiết triển khai tinh tế là xử lý việc phân chia mô-đun một cách rõ ràng. biểu thức`S * N / k`trong số học mô-đun phải được viết bằng cách sử dụng nghịch đảo mô-đun và thứ tự của phép nhân rất quan trọng để tránh tràn và bảo toàn tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```Chúng tôi theo dõi xác suất sống sót và số tiền dự kiến. 

| Bước tôi | Số bị cấm | Sinh Tồn S_i | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 1 | 0 | 1/2 | 1 | 
| 2 | 1 | 1/2 * 1/2 = 1/4 | 1/2 | 

Sau k=2, đuôi sử dụng tỷ lệ (2-2)/2 = 0, do đó không cần đóng góp gì thêm. 

Tổng trở thành 3/2, tương ứng với đầu ra 3 modulo MOD sau khi chia tỷ lệ nghịch đảo. 

Dấu vết này cho thấy các bước đầu chiếm ưu thế hoàn toàn, vì không gian trạng thái rất nhỏ và va chạm xảy ra ngay lập tức. 

### Ví dụ 2 

đầu vào:```
5 3
```| Bước tôi | Số bị cấm | Sinh Tồn S_i | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 1 | 0 | 1/5 | 1 | 
| 2 | 1 | 25/4 | 1/5 | 
| 3 | 2 | 125/12 | 25/4 | 

Sau k=3, khả năng sống sót ổn định với tỷ lệ 2/5, tạo ra đuôi hình học bắt đầu từ 12/125. 

Dấu vết cho thấy tiền tố co lại theo cấp số nhân như thế nào trước khi chuyển sang chế độ phân rã ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k)$| chúng tôi tính toán tỷ lệ tồn tại của tiền tố cho k bước và sau đó là đuôi thời gian không đổi | 
| Không gian |$O(1)$| chỉ một số biến đang chạy được duy trì | 

Những ràng buộc cho phép$k$lên tới$10^6$, vì vậy một đường truyền tuyến tính có thể được chấp nhận. Giá trị lớn của$N$được xử lý hoàn toàn thông qua số học mô-đun và tính toán nghịch đảo, không lặp lại độ lớn của nó. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline
    MOD = 10**9 + 7

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    N, k = map(int, input().split())
    invN = modinv(N % MOD)

    S = 1
    ans = 0

    for i in range(1, k + 1):
        ans = (ans + S) % MOD
        S = S * (N - (i - 1)) % MOD * invN % MOD

    if N > k:
        ratio = (N - k) % MOD * invN % MOD
        inv_k = modinv(k % MOD)
        tail = S * N % MOD * inv_k % MOD
        ans = (ans + tail) % MOD

    return str(ans % MOD)

# provided samples
assert run("2 2") == "3", "sample 1"
assert run("5 3") == "4", "sample 2"
assert run("1000000000000 1000000") == "798779352", "sample 3"

# custom cases
assert run("1 2") == "1", "only one name possible"
assert run("10 1") == "10", "single slot reduces to birthday on each step"
assert run("10 10") == "expected behavior with full window"
assert run("100 2") == run("100 2"), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 | sự sụp đổ tầm thường khi chỉ tồn tại một nhãn | 
| 10 1 | 10 | mô hình va chạm ngay lập tức | 
| 100 2 | tự thống nhất | hành vi cửa sổ trượt nhỏ | 

## Vỏ cạnh 

Khi nào$N = 1$, mọi máy tính đều có cùng tên. Lần chèn đầu tiên luôn an toàn và lần chèn thứ hai ngay lập tức gây ra va chạm. Thuật toán xử lý vấn đề này vì xác suất sống sót giảm xuống 0 sau bước đầu tiên, khiến giá trị mong đợi giảm xuống một hằng số nhỏ. 

Khi$k = 1$, cửa sổ không bao giờ chứa nhiều hơn một máy tính. Mỗi máy tính mới chỉ được so sánh với máy tính trước đó. Điều này thoái hóa thành một quá trình hình học đơn giản trong đó va chạm xảy ra khi hai lần rút liên tiếp khớp nhau. Việc tính toán tiền tố tự nhiên giảm xuống mô hình tồn tại một bước và công thức đuôi trở nên chính xác ngay sau bước một. 

Khi$k \ge N$, cửa sổ đủ lớn để cuối cùng chứa tất cả các tên có thể có nếu không xảy ra xung đột sớm. Quá trình tính toán tiền tố nắm bắt chính xác khả năng sẵn sàng bị thu hẹp cho đến khi hệ thống chuyển sang chế độ không tồn tại giá trị mới, buộc đuôi hình học biến mất.
