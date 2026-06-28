---
title: "CF 105139J - Điểm trên trục số A"
description: "Chúng ta được cho một tập hợp các điểm được đặt trên một trục số. Ở mỗi bước, chúng tôi liên tục chọn bất kỳ hai điểm hiện có nào, xóa chúng và chèn điểm giữa của chúng. Điều này tiếp tục cho đến khi chỉ còn lại một điểm và quá trình dừng lại."
date: "2026-06-27T16:59:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105139
codeforces_index: "J"
codeforces_contest_name: "The 2024 International Collegiate Programming Contest in Hubei Province, China"
rating: 0
weight: 105139
solve_time_s: 38
verified: true
draft: false
---

[CF 105139J - Điểm trên trục số A](https://codeforces.com/problemset/problem/105139/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm được đặt trên một trục số. Ở mỗi bước, chúng tôi liên tục chọn bất kỳ hai điểm hiện có nào, xóa chúng và chèn điểm giữa của chúng. Điều này tiếp tục cho đến khi chỉ còn lại một điểm và quá trình dừng lại. Chi tiết quan trọng là cặp điểm được chọn ngẫu nhiên thống nhất trong số tất cả các cặp điểm có thể có ở mỗi bước. 

Nhiệm vụ không phải là mô phỏng quá trình này mà là tính toán vị trí cuối cùng dự kiến ​​của điểm còn lại cuối cùng. Vì câu trả lời được đảm bảo là một số hữu tỷ nên chúng ta được yêu cầu xuất nó ở dạng mô-đun như sau:$p \cdot q^{-1} \bmod 998244353$, Ở đâu$\frac{p}{q}$là kỳ vọng ở dạng rút gọn. 

Kích thước đầu vào tăng lên$n = 10^6$, điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng hoặc chuỗi Markov dựa trên trạng thái. Ngay cả một thao tác đơn lẻ cũng làm giảm số điểm đi một và mỗi thao tác sẽ liên quan đến việc tính toán lại các lựa chọn theo cặp, do đó mô phỏng đơn giản ít nhất sẽ là bậc hai trong trường hợp xấu nhất. Điều đó vượt xa giới hạn khả thi. Thay vào đó, lời giải phải dựa vào một bất biến cấu trúc của quá trình. 

Một điểm tinh tế là tính ngẫu nhiên chỉ áp dụng cho thứ tự của phép hợp nhất chứ không áp dụng cho kết quả số học của phép hợp nhất. Điều này thường gợi ý rằng kỳ vọng cuối cùng có thể độc lập với tính ngẫu nhiên theo cách tuyến tính. Một vấn đề không rõ ràng khác là câu trả lời phụ thuộc vào tất cả các điểm cùng một lúc, do đó, lý luận tham lam về việc hợp nhất cục bộ không có tác dụng. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì nhiều tập hợp điểm và liên tục chọn một cặp ngẫu nhiên, thay thế nó bằng điểm giữa của chúng và tiếp tục. Mỗi lần hợp nhất là$O(1)$, nhưng số bước là$n-1$. Phần khó khăn là tính ngẫu nhiên: ở mỗi bước đều có$\binom{k}{2}$các lựa chọn, vì vậy việc liệt kê các chuyển đổi hoặc tính trung bình trên tất cả các chuỗi là theo cấp số nhân trong$n$. Ngay cả lý luận về sự phân bố của các trạng thái trung gian cũng nhanh chóng trở nên khó hiểu. 

Quan sát quan trọng là phép toán điểm giữa là tuyến tính. Nếu chúng ta viết một sự hợp nhất của$x_i$Và$x_j$BẰNG$(x_i + x_j)/2$, thì mọi thao tác đều là tổ hợp affine của các đầu vào. Vì kỳ vọng là tuyến tính nên chúng ta có thể theo dõi xem mỗi điểm ban đầu đóng góp như thế nào vào kết quả cuối cùng. 

Thay vì theo dõi vị trí, chúng tôi theo dõi các hệ số. Ban đầu mỗi điểm đóng góp trọng số$1$. Khi hai điểm có hệ số$a_i$Và$a_j$được hợp nhất, chúng tạo ra một điểm mới có hệ số trở thành$(a_i + a_j)/2$, được phân bổ trở lại số tiền đóng góp ban đầu. Tính ngẫu nhiên của việc lựa chọn không ảnh hưởng đến tính đối xứng: mọi cặp không có thứ tự đều có khả năng xảy ra như nhau ở mọi giai đoạn, vì vậy mọi điểm đều có thể trao đổi được. Sự đối xứng này buộc tất cả các điểm ban đầu phải có sự đóng góp dự kiến ​​giống hệt nhau cho kết quả cuối cùng. 

Vì điểm cuối cùng là tổ hợp lồi của tất cả các điểm ban đầu và tất cả các hệ số có kỳ vọng bằng nhau nên mỗi điểm đóng góp trọng số$1/n$trong sự mong đợi. Do đó, vị trí cuối cùng dự kiến ​​chỉ đơn giản là giá trị trung bình của tất cả các tọa độ ban đầu. 

Điều này làm giảm toàn bộ vấn đề thành tính toán trung bình số học mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (tuyến tính + đối xứng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của tất cả các giá trị đầu vào. Điều này nắm bắt tổng khối lượng của tất cả các điểm trước khi bất kỳ sự hợp nhất nào xảy ra. Vì tất cả các phép biến đổi đều tuyến tính nên việc theo dõi tổng là đủ để phục hồi kỳ vọng. 
2. Nhân tổng với nghịch đảo mô đun của$n$modulo 998244353. Điều này thực hiện phép chia trong số học mô-đun và tương ứng với việc lấy trung bình các phần đóng góp bằng nhau trên tất cả các điểm. 
3. Xuất kết quả dưới dạng tọa độ cuối cùng dự kiến. 

### Tại sao nó hoạt động 

Mỗi lần hợp nhất sẽ thay thế hai giá trị$x_i, x_j$với$(x_i + x_j)/2$. Nếu chúng ta mở rộng quy trình này theo cách quy nạp thì giá trị cuối cùng luôn là giá trị trung bình có trọng số của đầu vào ban đầu. Các trọng số phụ thuộc vào cây hợp nhất ngẫu nhiên, nhưng mọi cây hợp nhất nhị phân trên các lá được dán nhãn đều có khả năng như nhau do tính đối xứng của việc ghép ngẫu nhiên đồng nhất. 

Sự đối xứng này ngụ ý rằng mỗi phần tử ban đầu có trọng số dự kiến ​​giống hệt nhau trong biểu thức cuối cùng. Vì tổng trọng lượng phải có tổng bằng 1 nên mỗi trọng lượng dự kiến ​​sẽ là$1/n$. Do đó, kỳ vọng của giá trị cuối cùng chính xác là giá trị trung bình của các đầu vào, không phụ thuộc vào quá trình ngẫu nhiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    s = sum(arr) % MOD
    inv_n = modinv(n)
    print((s * inv_n) % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai là tối thiểu vì hiểu biết sâu sắc cốt lõi sẽ loại bỏ mọi nhu cầu mô phỏng. Bước không tầm thường duy nhất là chia mô-đun cho$n$, được xử lý bằng định lý nhỏ Fermat vì môđun là số nguyên tố. 

Một cách tinh tế là đảm bảo tổng được giảm modulo trước khi nhân, để tránh sự tăng trưởng không cần thiết. Cái khác là thế$n$bản thân nó có thể lớn, nhưng tính toán nghịch đảo mô-đun vẫn còn$O(\log MOD)$, không đáng kể dưới các ràng buộc. 

## Ví dụ đã hoạt động 

Xem xét đầu vào nơi có điểm$[1, 2, 4]$. 

| Bước | Tổng hợp | n | Giá trị hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 7 | 3 | - | 
| Cuối cùng | 7 | 3 |$7/3$| 

Kết quả là$7 \cdot 3^{-1} \bmod MOD$. 

Điều này xác nhận rằng mặc dù có sự hợp nhất ngẫu nhiên nhưng kết quả mong đợi chỉ phụ thuộc vào giá trị trung bình. 

Bây giờ hãy xem xét ví dụ thứ hai với các giá trị lặp lại$[5, 5, 5, 5]$. 

| Bước | Tổng hợp | n | Giá trị hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 20 | 4 | - | 
| Cuối cùng | 20 | 4 | 5 | 

Điều này cho thấy sự ổn định dưới các đầu vào đối xứng: tất cả các phép hợp nhất đều bảo toàn cùng một giá trị về mặt xác định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một giá trị tổng cộng với lũy thừa mô-đun cho nghịch đảo | 
| Không gian | O(1) | Chỉ lưu trữ tổng đang chạy và bộ đệm đầu vào | 

Thuật toán dễ dàng phù hợp với các ràng buộc ngay cả đối với$n = 10^6$, vì hoạt động chủ yếu là quét tuyến tính đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    MOD = 998244353
    
    n = int(input())
    arr = list(map(int, input().split()))
    s = sum(arr) % MOD
    inv_n = pow(n, MOD - 2, MOD)
    return str((s * inv_n) % MOD)

# provided sample
assert run("3\n1 2 4\n") == str((7 * pow(3, 998244351, 998244353)) % 998244353)

# minimum n
assert run("1\n42\n") == "42"

# all equal
assert run("5\n7 7 7 7 7\n") == "7"

# increasing sequence
assert run("4\n1 2 3 4\n") == str((10 * pow(4, 998244351, 998244353)) % 998244353)

# large uniform test
assert run("3\n0 0 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 giá trị đơn | giá trị của chính nó | trường hợp nhận dạng | 
| tất cả các giá trị bằng nhau | cùng giá trị | ổn định khi sáp nhập | 
| dãy số học | trung bình mô đun đúng | tính đúng đắn chung | 
| tất cả số không | đầu ra bằng không | xử lý không ranh giới | 

## Vỏ cạnh 

Đầu vào một phần tử như`n = 1`là trường hợp duy nhất không xảy ra sự hợp nhất. Thuật toán xử lý việc này một cách tự nhiên vì tổng bằng chính phần tử đó và nghịch đảo của 1 là 1, do đó đầu ra không thay đổi. 

Các mảng hoàn toàn bằng nhau hoạt động mang tính xác định trong quy trình vì mọi điểm giữa đều giống hệt với đầu vào. Thuật toán phản ánh điều này vì giá trị trung bình bằng cùng một giá trị và không có sự ngẫu nhiên nào ảnh hưởng đến kết quả. 

Đầu vào lớn có giá trị gần bằng$998244353$yêu cầu giảm mô đun trong quá trình tính tổng. Việc triển khai sẽ giảm tổng ngay lập tức, ngăn ngừa tràn và đảm bảo số học mô-đun chính xác trong suốt quá trình tính toán.
