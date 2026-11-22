---
title: "CF 266A - Đá Trên Bàn"
description: "Chúng tôi được phát một hàng đá, mỗi viên được sơn một trong ba màu. Mục tiêu là loại bỏ càng ít viên đá càng tốt để sau khi loại bỏ, không có hai viên đá liền kề nào có cùng màu."
date: "2026-06-04T18:07:18+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 266
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 163 (Div. 2)"
rating: 800
weight: 266
solve_time_s: 61
verified: true
draft: false
---

[CF 266A - Những viên đá trên bàn](https://codeforces.com/problemset/problem/266/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một hàng đá, mỗi viên được sơn một trong ba màu. Mục tiêu là loại bỏ càng ít viên đá càng tốt để sau khi loại bỏ, không có hai viên đá liền kề nào có cùng màu. 

Nói cách khác, chúng ta muốn lấy chuỗi ký tự gốc và xóa các ký tự cho đến khi mỗi cặp ký tự liên tiếp còn lại khác nhau. Thứ tự của các viên đá còn lại không được thay đổi nên chúng tôi không sắp xếp lại mà chỉ loại bỏ các phần tử. 

Kích thước đầu vào rất nhỏ, tối đa 50 viên đá. Điều này có nghĩa là ngay cả việc quét tuyến tính đơn giản hoặc thậm chí lý luận đắt tiền hơn cũng sẽ hoạt động thoải mái trong giới hạn thời gian. Ở đây, bất kỳ giải pháp nào có độ phức tạp bậc hai đều có hiệu quả ngay lập tức, nhưng cấu trúc của bài toán cho thấy tồn tại một cái gì đó đơn giản hơn. 

Một sai lầm ngây thơ ở đây là nghĩ đến việc sắp xếp lại hoặc đếm tần số. Ví dụ, đã cho`RRG`, ai đó có thể nghĩ rằng vì có hai chữ R nên một chữ R luôn phải bị loại bỏ bất kể vị trí. Điều đó đúng trong trường hợp này, nhưng chỉ tần số nói chung là không đủ vì tính liền kề rất quan trọng. 

Một cách tiếp cận không chính xác phổ biến khác là cố gắng "cân bằng" màu sắc trên toàn cầu, ví dụ như cố gắng thay thế R, G, B theo một số mẫu cố định. Điều này không thành công trên các đầu vào như`RGRGRG`, trong đó không cần loại bỏ mặc dù bất kỳ giả định mô hình cứng nhắc nào cũng có thể loại bỏ các viên đá hợp lệ một cách không chính xác. 

Một giải pháp đúng phải chỉ phụ thuộc vào sự lân cận cục bộ chứ không phải sự phân bố toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là mô phỏng việc loại bỏ bằng cách thử tất cả các tập hợp con của các viên đá và kiểm tra xem tập hợp con nào thỏa mãn điều kiện không có hai viên đá liền kề nào bằng nhau. Đối với mỗi tập hợp con, chúng tôi xác minh tính hợp lệ theo thời gian tuyến tính. Vì có$2^n$tập hợp con, điều này dẫn đến$O(n \cdot 2^n)$sự phức tạp về mặt thời gian. Ngay cả với$n = 50$, điều này trở nên lớn về mặt thiên văn, khiến nó không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần chọn loại đá nào sẽ giữ trên toàn cầu. Thay vào đó, chúng ta có thể quyết định một cách tham lam từ trái sang phải xem liệu một hòn đá có phải được loại bỏ hay không chỉ dựa trên mối quan hệ của nó với hòn đá được giữ trước đó. 

Cấu trúc của bài toán là một sự giảm ràng buộc cục bộ cổ điển: điều kiện hợp lệ chỉ phụ thuộc vào các phần tử được giữ liền kề. Điều này có nghĩa là một khi chúng tôi sửa chữa những gì chúng tôi giữ nguyên vị trí$i-1$, quyết định tại vị trí$i$được xác định hoàn toàn bằng việc nó có khớp với màu được giữ cuối cùng hay không. 

Điều này làm giảm vấn đề từ lựa chọn tổ hợp sang quy trình lọc một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với bộ đếm được đặt về 0, thể hiện số lượng viên đá được loại bỏ. 
2. Theo dõi màu đá được lưu giữ cuối cùng. Ban đầu, điều này không được xác định vì chưa có viên đá nào được xử lý. 
3. Quét chuỗi từ trái sang phải, xử lý từng viên đá một. 
4. Đối với mỗi viên đá, so sánh màu của nó với màu được giữ cuối cùng. Nếu giống nhau thì viên đá này phải bỏ đi vì giữ lại sẽ vi phạm quy tắc liền kề. 
5. Nếu nó khác với màu được giữ cuối cùng, chúng tôi sẽ giữ lại và cập nhật màu được giữ cuối cùng lên màu hiện tại. 
6. Tiếp tục cho đến hết chuỗi, tích lũy số lượng đá đã loại bỏ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, trình tự được giữ lại được đảm bảo thỏa mãn điều kiện không có hai viên đá liền kề nào bằng nhau. Điều này được duy trì bởi vì chúng tôi chỉ thêm một viên đá khi nó khác với viên đá được giữ cuối cùng. Nếu nó khớp, việc loại bỏ nó luôn an toàn vì nó không thể giúp ích cho các quyết định trong tương lai: việc giữ nó sẽ chỉ tạo ra một vùng lân cận không hợp lệ mà không có lợi ích gì, vì tất cả các lần kiểm tra trong tương lai chỉ phụ thuộc vào viên đá được giữ gần đây nhất chứ không phải bất kỳ viên đá nào trước đó. Điều này thiết lập một bất biến tham lam rằng tiền tố được giữ luôn tối ưu cho độ dài của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()
    
    removed = 0
    last = None
    
    for c in s:
        if c == last:
            removed += 1
        else:
            last = c
    
    print(removed)

if __name__ == "__main__":
    solve()
```Mã duy trì một biến duy nhất`last`đại diện cho viên đá được giữ gần đây nhất. Khi ký tự hiện tại khớp`last`, nó sẽ bị loại bỏ và được tính là một lần loại bỏ. Ngược lại, nó sẽ trở thành cái mới`last`. Điều này trực tiếp thực hiện quy tắc tham lam từ hướng dẫn thuật toán trong một lần. 

Sự tinh tế quan trọng là khởi tạo:`last = None`đảm bảo ký tự đầu tiên luôn được giữ nguyên vì nó không thể khớp với bất kỳ ký tự nào trước nó. Không có vấn đề ranh giới nào ngoài phạm vi này vì mọi so sánh đều an toàn và thống nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
RRG
```| tôi | đá | cuối cùng trước | hành động | đã xóa | cuối cùng sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | R | Không có | giữ | 0 | R | 
| 2 | R | R | xóa | 1 | R | 
| 3 | G | R | giữ | 1 | G | 

Điều này cho thấy rằng chỉ các bản sao liên tiếp bị loại bỏ, trong khi các chuyển đổi hợp lệ được giữ nguyên. 

### Ví dụ 2 

đầu vào:```
5
RGBBB
```| tôi | đá | cuối cùng trước | hành động | đã xóa | cuối cùng sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | R | Không có | giữ | 0 | R | 
| 2 | G | R | giữ | 0 | G | 
| 3 | B | G | giữ | 0 | B | 
| 4 | B | B | xóa | 1 | B | 
| 5 | B | B | xóa | 2 | B | 

Điều này xác nhận rằng chỉ các khối liền kề lặp lại mới được thu gọn thành một đại diện duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Quét một lần từ trái sang phải qua chuỗi | 
| Không gian |$O(1)$| Chỉ có một biến theo dõi được sử dụng | 

Với$n \le 50$, thuật toán không đáng kể về mặt hiệu suất, nhưng cấu trúc quét tuyến tính vẫn là công thức rõ ràng nhất và có khả năng mở rộng vượt quá các ràng buộc nếu cần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("3\nRRG\n") == "1"

# all distinct, no removals
assert run("4\nRGBR\n") == "0"

# all same
assert run("5\nRRRRR\n") == "4"

# alternating pattern
assert run("6\nRGRGRG\n") == "0"

# single element
assert run("1\nB\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| RGRGR | 0 | không cần xóa | 
| RRRRR | 4 | sự sụp đổ hoàn toàn của các bản sao | 
| B | 0 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Đối với một hòn đá như`B`, bộ thuật toán`last = None`, giữ đá và không loại bỏ đá. Không có sự so sánh nào kích hoạt việc xóa, phù hợp với yêu cầu vì một phần tử duy nhất luôn hợp lệ. 

Đối với một chuỗi hoàn toàn thống nhất như`RRRRR`, đầu tiên`R`được giữ lại và mọi ký tự tiếp theo đều bằng`last`, vì vậy mỗi cái được tính là đã bị xóa. Quá trình quét không bao giờ cập nhật`last`, đảm bảo kết quả chính xác$n-1$, phù hợp với việc giữ một đại diện của khối giống hệt nhau tối đa. 

Đối với các chuỗi xen kẽ như`RGRGRG`, mỗi ký tự khác nhau`last`, vì vậy không có gì bị loại bỏ. Bất biến chỉ các cặp liền kề bằng nhau không hợp lệ đảm bảo cấu trúc này đã tối ưu và thuật toán bảo toàn chính xác tất cả các phần tử.
