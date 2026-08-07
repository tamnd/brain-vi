---
title: "CF 103973C - Lăn vòng tròn"
description: "Chúng ta có hai đường tròn có bán kính $a$ và $b$. Một vòng tròn được cố định tại chỗ và vòng tròn còn lại được đặt tiếp tuyến với nó từ bên ngoài."
date: "2026-07-02T06:18:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103973
codeforces_index: "C"
codeforces_contest_name: "2022 Huazhong University of Science and Technology Freshmen Cup"
rating: 0
weight: 103973
solve_time_s: 47
verified: true
draft: false
---

[CF 103973C - Xoay vòng tròn](https://codeforces.com/problemset/problem/103973/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đường tròn có bán kính$a$Và$b$. Một vòng tròn được cố định tại chỗ và vòng tròn còn lại được đặt tiếp tuyến với nó từ bên ngoài. Sau đó, vòng tròn thứ hai được lăn quanh vòng tròn đầu tiên, luôn tiếp tuyến và cuối cùng nó quay trở lại vị trí ban đầu sau khi hoàn thành một vòng quanh vòng tròn cố định. 

Trong khi chuyển động này xảy ra, vòng tròn lăn cũng quay quanh tâm của chính nó. Nhiệm vụ là xác định xem vòng tròn chuyển động hoàn thành bao nhiêu vòng quay trong một vòng quay quanh vòng tròn cố định. Câu trả lời không nhất thiết phải là số nguyên, vì vậy nó phải được đưa ra dưới dạng phân số rút gọn. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập và mỗi trường hợp thử nghiệm chỉ là một cặp bán kính. Từ$T$có thể lớn như$10^5$, mỗi trường hợp kiểm thử phải được xử lý trong thời gian không đổi. Bản thân bán kính có thể lên tới$10^{18}$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên quan đến mô phỏng chuyển động hoặc bước đi lặp lại dọc theo con đường. 

Một cách giải thích ngây thơ có thể gợi ý mô phỏng chuyển động lăn, theo dõi góc quay theo từng bước nhỏ dọc theo đường đi. Điều này thất bại vì đường đi là liên tục và vì quy mô chuyển động sẽ yêu cầu theo thứ tự$10^{18}$bước nếu rời rạc theo thang bán kính. 

Trường hợp cạnh tinh tế xuất hiện khi cả hai bán kính đều bằng nhau. Ví dụ,$a = b = 1$. Một trực giác ngây thơ có thể nói rằng vòng tròn lăn đúng một lần, nhưng lý luận hình học cho thấy nó thực sự hoàn thành hai vòng quay. Điều này đã gợi ý rằng có liên quan đến một cái gì đó mang tính toàn cầu hơn là tốc độ lăn cục bộ. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là tưởng tượng vòng tròn nhỏ hơn di chuyển dọc theo chu vi của vòng tròn lớn hơn và liên tục cập nhật góc quay của nó. Ở mỗi chuyển động vô cùng nhỏ dọc theo đường đi bên ngoài, vòng tròn lăn sẽ quay tỉ lệ với chiều dài cung mà nó di chuyển chia cho bán kính của chính nó. Điều này sẽ yêu cầu tích phân chuyển động quay trên một vòng quay đầy đủ chiều dài$2\pi(a + b)$, vì tâm của đường tròn chuyển động vẽ đường tròn có bán kính$a + b$. 

Mặc dù lý do này là đúng về nguyên tắc, nhưng việc mô phỏng trực tiếp hoặc rời rạc hóa nó là không thể trong những điều kiện ràng buộc. Điểm kém hiệu quả chính là chúng ta sẽ cần xử lý toàn bộ đường dẫn liên tục, thực hiện một cách hiệu quả số lượng cập nhật nhỏ không giới hạn. 

Cái nhìn sâu sắc quan trọng là chuyển động phân hủy rõ ràng thành hai phần đóng góp. Đầu tiên, tâm của đường tròn chuyển động di chuyển dọc theo một đường tròn bán kính$a + b$, đóng góp một vòng quay của$\frac{a + b}{b}$. Thứ hai, có một hiệu ứng bổ sung gây ra bởi thực tế là vòng tròn đang lăn quanh một bề mặt cong khác chứ không phải là đường thẳng, điều này tạo ra một vòng quay đầy đủ hơn trên mỗi vòng quay của đường tâm. Đây là hiện tượng “lăn không trượt trên một đường cong kín” cổ điển, trong đó độ cong tăng thêm một phần$1$hết lượt. 

Việc kết hợp các hiệu ứng này sẽ tạo ra một vòng quay tổng cộng:$$\frac{a + b}{b} + 1 = \frac{a + b + b}{b} = \frac{a + 2b}{b}$$Tuy nhiên, điều này vẫn chưa phù hợp với hành vi mẫu và việc phân tách hình học cẩn thận hơn cho thấy rằng bất biến đúng là đối xứng trong$a$Và$b$. Thay vì phân chia các đóng góp một cách không đối xứng, chúng tôi nhận thấy rằng mỗi vòng tròn đóng góp như nhau vào hình học lăn hiệu quả. 

Công thức cuối cùng đúng sẽ đơn giản hóa thành:$$\frac{a + b}{\gcd(a, b)} \Big/ \frac{b}{\gcd(a, b)} = \frac{a + b}{b} \text{ reduced properly via gcd}$$Một cách ổn định hơn để suy ra nó là lưu ý rằng trong một vòng quay hoàn toàn, độ dài cung được vạch bởi điểm tiếp xúc tương ứng với$2\pi(a + b)$và mỗi vòng quay đầy đủ của vòng tròn chuyển động tương ứng với độ dài cung$2\pi b$. Do đó số vòng quay thô là:$$\frac{a + b}{b}$$Nhưng do đường dẫn bị đóng, số lượng thực tế phải được giảm xuống mức thấp nhất khi được biểu thị dưới dạng phân số, đó là lý do tại sao đầu ra được yêu cầu ở dạng rút gọn. 

Do đó, toàn bộ nhiệm vụ giảm xuống còn tính toán một phân số và đơn giản hóa nó bằng cách sử dụng gcd của tử số và mẫu số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(lớn) | O(1) | Quá chậm | 
| Công thức trực tiếp + GCD | O(log min(a,b)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$a$Và$b$cho từng trường hợp thử nghiệm. Chúng xác định hình dạng của các vòng tròn cố định và lăn, đồng thời xác định đầy đủ chuyển động. 
2. Tính số vòng quay thô như$a + b$. Điều này thể hiện sự đóng góp tổng cộng hiệu quả của cả hai vòng tròn vào chuyển động lăn trên một đường đi hoàn chỉnh. 
3. Đặt mẫu số là$b$, vì mỗi vòng quay đầy đủ của vòng tròn lăn tương ứng với độ dài cung tỉ lệ với bán kính của nó. 
4. Tính toán$g = \gcd(a + b, b)$. Bước này đảm bảo phân số được biểu thị ở dạng tối giản bằng cách loại bỏ bất kỳ hệ số tỷ lệ chung nào giữa tử số và mẫu số. 
5. Đầu ra$\frac{a + b}{g} / \frac{b}{g}$. Đây là phần rút gọn biểu thị số vòng quay. 

Ý tưởng chính đằng sau việc giảm ở cuối là tỷ lệ hình học của độ dài xác định góc quay và chỉ có tỷ lệ tương đối mới là vấn đề chứ không phải đơn vị tuyệt đối. 

### Tại sao nó hoạt động 

Chuyển động lăn chỉ phụ thuộc vào tỉ số độ dài cung. Trong một vòng quay hoàn toàn, tâm của vòng tròn chuyển động vạch ra một đường cong khép kín có chiều dài tỉ lệ với$a + b$. Vòng tròn lăn chuyển đổi chiều dài cung đã di chuyển thành các phép quay bằng cách chia cho thang đo chu vi của chính nó, tỉ lệ với$b$. Bất kỳ ước số chung nào giữa hai đại lượng này đều tương ứng với cấu trúc lặp lại trong chuyển động mà không ảnh hưởng đến số đếm cuối cùng, đó chính xác là những gì việc loại bỏ gcd nắm bắt được. Điều này đảm bảo phân số thu được vừa tối thiểu vừa chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def solve():
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        num = a + b
        den = b
        g = gcd(num, den)
        num //= g
        den //= g
        print(f"{num}/{den}")

if __name__ == "__main__":
    solve()
```Mã đọc từng trường hợp thử nghiệm một cách độc lập và tính toán trực tiếp phần rút gọn. Phép toán không cần thiết duy nhất là gcd, đảm bảo phân số là tối thiểu. Phép chia số nguyên là an toàn vì tất cả các giá trị đều nằm trong giới hạn 64 bit ngay cả khi được thêm vào. 

Một lỗi phổ biến là quên giảm phân số, điều này sẽ dẫn đến định dạng không chính xác ngay cả khi giá trị số là chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$a = 1, b = 1$Chúng tôi tính toán$a + b = 2$, mẫu số$b = 1$. 

| bước | một | b | tử số | mẫu số | gcd | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | - | - | - | - | 
| tính toán | 1 | 1 | 2 | 1 | - | - | 
| giảm | 1 | 1 | 2 | 1 | 1 | 1/2 | 

Điều này chứng tỏ rằng ngay cả khi cả hai đường tròn giống hệt nhau, thì vòng tròn lăn thực hiện hai vòng quay hoàn toàn, phù hợp với hiệu ứng nhân đôi hình học đã biết của việc lăn quanh một vòng kín. 

### Ví dụ 2:$a = 4, b = 6$Chúng tôi tính toán$a + b = 10$, mẫu số$6$. 

| bước | một | b | tử số | mẫu số | gcd | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 4 | 6 | - | - | - | - | 
| tính toán | 4 | 6 | 10 | 6 | - | - | 
| giảm | 4 | 6 | 10 | 6 | 2 | 3/5 | 

Điều này xác nhận rằng phân số được đơn giản hóa do các yếu tố được chia sẻ giữa đóng góp độ dài đường đi và bán kính lăn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|---|---|---|---| 

| Thời gian |$O(T \log \min(a,b))$| Mỗi trường hợp thử nghiệm yêu cầu tính toán gcd trên số nguyên 64 bit | 

| Không gian |$O(1)$| Chỉ một vài biến số nguyên được sử dụng cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép lên đến$10^5$các trường hợp thử nghiệm và hoạt động gcd đủ nhanh trong Python nhờ thuật toán Euclid. Giải pháp thoải mái phù hợp trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        num = a + b
        den = b
        g = gcd(num, den)
        out.append(f"{num//g}/{den//g}")
    return "\n".join(out)

# provided samples
assert run("2\n1 1\n4 6") == "2/1\n5/3"

# minimum size
assert run("1\n1 1") == "2/1"

# co-prime case
assert run("1\n2 3") == "5/3"

# equal large values
assert run("1\n1000000000000000000 1000000000000000000") == "2/1"

# boundary asymmetry
assert run("1\n1 1000000000000000000") == "1000000000000000001/1000000000000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1/2 | hiệu ứng nhân đôi bán kính giống hệt nhau | 
| 2 3 | 3/5 | giảm không hề nhỏ | 
| 1 10^18 | (10^18+1)/10^18 | mất cân bằng cực độ | 
| 10^18 10^18 | 1/2 | độ ổn định giá trị lớn bằng nhau | 

## Vỏ cạnh 

cho$a = b$, thuật toán tạo ra tử số$2a$và mẫu số$a$, làm giảm đến$2/1$. Bước gcd ở đây rất cần thiết vì nếu không giảm, đầu ra sẽ vẫn giữ nguyên không chính xác.$2a/a$, không có định dạng bắt buộc. 

Đối với các giá trị cực lớn như$a = 10^{18}, b = 1$, tử số trở thành$10^{18} + 1$. Vì đây là nguyên tố cùng với$1$, gcd là$1$, và phân số không đổi. Thuật toán xử lý vấn đề này mà không gặp vấn đề tràn vì số nguyên Python hỗ trợ độ chính xác tùy ý một cách tự nhiên. 

Khi$a$Và$b$chia sẻ một gcd lớn, chẳng hạn như$a = 6, b = 4$, tử số là$10$và mẫu số là$4$và việc giảm gcd sẽ đơn giản hóa nó một cách chính xác thành$5/2$. Điều này khẳng định rằng bước đơn giản hóa không mang tính thẩm mỹ nhưng cần thiết về mặt cấu trúc để đảm bảo tính chính xác.
