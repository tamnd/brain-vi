---
title: "CF 104010E - Giống như dưa chua"
description: "Chúng ta đang đứng ở vị trí 0 trên một trục số vô hạn và muốn đạt đến tọa độ đích $x$ nào đó, có thể dương, âm hoặc bằng 0. Trong một lần di chuyển, chúng ta chọn một số nguyên không âm $k$, sau đó nhảy sang trái hoặc sang phải chính xác $2^k$."
date: "2026-07-02T05:20:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104010
codeforces_index: "E"
codeforces_contest_name: "2022-2023 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 22)"
rating: 0
weight: 104010
solve_time_s: 47
verified: true
draft: false
---

[CF 104010E - Giống như dưa chua](https://codeforces.com/problemset/problem/104010/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đứng ở vị trí số 0 trên một trục số vô hạn và muốn đạt tới tọa độ đích nào đó$x$, có thể dương, âm hoặc bằng 0. Trong một lần di chuyển, chúng ta chọn một số nguyên không âm$k$, sau đó nhảy sang trái hoặc sang phải một cách chính xác$2^k$. Vì vậy, độ dài nước đi có sẵn là lũy thừa của hai và mỗi nước đi độc lập cho phép chúng ta chọn hướng. 

Nhiệm vụ là tính toán số lần nhảy tối thiểu cần thiết để tiếp đất chính xác trên$x$. Mỗi trường hợp thử nghiệm là độc lập và có thể có tới 100000 truy vấn, trong khi$x$có thể lớn như$10^{18}$về độ lớn. 

Sự ràng buộc về$t$buộc chúng ta phải trả lời từng truy vấn trong thời gian gần như không đổi hoặc theo logarit. Bất kỳ giải pháp nào cố gắng mô phỏng đường đi hoặc thực hiện tìm kiếm trên các trạng thái đều không khả thi ngay lập tức, vì ngay cả hệ số phân nhánh vừa phải cũng bùng nổ theo cấp số nhân. Độ lớn của$x$gợi ý một cấu trúc biểu diễn nhị phân, vì$10^{18}$phù hợp trong khoảng 60 bit. 

Một trường hợp cạnh tinh tế phát sinh khi$x = 0$. Trong trường hợp đó, không cần phải di chuyển, nhưng lý luận ngây thơ dựa trên biểu diễn nhị phân có thể chỉ định sai ít nhất một nước đi nếu nó luôn cố gắng phân tách thành lũy thừa của hai. 

Một trường hợp góc khác là âm tính$x$. Vì chúng ta có thể tự do lựa chọn hướng cho mỗi bước di chuyển nên tính đối xứng gợi ý rằng chỉ$|x|$vấn đề, nhưng điều này phải được chứng minh bằng cấu trúc của các hoạt động hơn là giả định. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ coi mỗi trạng thái là một tọa độ trên đường thẳng và mỗi chuyển động sẽ phân nhánh thành hai trạng thái mới: cộng hoặc trừ$2^k$cho bất kỳ$k \ge 0$. Tìm kiếm đường đi ngắn nhất từ ​​0 tới$x$về mặt khái niệm là đúng, vì tất cả các bước di chuyển đều tốn một bước. Tuy nhiên, không gian trạng thái là vô hạn và có tính kết nối cao, thậm chí còn hạn chế tọa độ ở$[-10^{18}, 10^{18}]$để lại quá nhiều trạng thái. Mỗi trạng thái cũng có vô số chuyển tiếp đi do tùy ý$k$, do đó, ngay cả việc liệt kê các chuyển tiếp cũng không thể thực hiện được. Cách tiếp cận này thất bại ngay lập tức vì hệ số phân nhánh không bị giới hạn và độ sâu của các giải pháp tối ưu có thể đạt tới khoảng 60. 

Quan sát quan trọng là mỗi bước di chuyển tương ứng với việc chuyển đổi một vị trí bit đơn trong biểu diễn nhị phân của tọa độ, vì$2^k$chỉ ảnh hưởng một chút$k$. Tuy nhiên, không giống như phép cộng nhị phân tiêu chuẩn, chúng tôi được phép chọn dấu độc lập cho mỗi lần di chuyển, điều đó có nghĩa là chúng tôi không bị hạn chế thực hiện việc truyền bá theo cách thông thường. Thay vào đó, vấn đề trở nên tương đương với việc biểu diễn$x$dưới dạng tổng các quyền hạn đã ký của hai, giảm thiểu số lượng điều khoản. 

Đây chính xác là khái niệm cổ điển về biểu diễn số nguyên dạng không liền kề (NAF), trong đó chúng ta viết lại một số trong cơ số 2 nhưng cho phép các chữ số$-1, 0, 1$, giảm thiểu số lượng chữ số khác 0. Mỗi chữ số khác 0 tương ứng với một lần nhảy và việc giảm thiểu các bước nhảy tương đương với việc giảm thiểu các chữ số khác 0 trong bản khai triển nhị phân có dấu này. 

Cấu trúc tham lam của NAF hoạt động từng chút một, quyết định có nên sử dụng một$\pm 1$tại bit hiện tại hoặc mang sang bit tiếp theo khi tính chẵn lẻ buộc nó. Điều này giúp loại bỏ các chuỗi dài các chuỗi trong biểu diễn nhị phân, điều này tạo ra sự kém hiệu quả trong việc phân rã nhị phân đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm biểu đồ) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (tham lam NAF) | (O(\log | x | )) mỗi lần kiểm tra | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng số một cách độc lập, giảm nó xuống giá trị tuyệt đối vì dấu hiệu không liên quan. 

1. Nếu$x = 0$, trả về 0 ngay lập tức. Không cần phải nhảy vì chúng ta đã ở điểm gốc. 
2. Thay thế$x$qua$|x|$. Điều này có hiệu quả vì mọi bước di chuyển đều cho phép chọn hướng, do đó, bất kỳ chuỗi bước di chuyển nào đạt tới$x$có thể được nhân đôi để đạt được$-x$có chiều dài giống hệt nhau. 
3. Khởi tạo bộ đếm cho các phép toán về 0. Điều này sẽ biểu thị số bit có dấu khác 0 mà chúng ta sẽ sử dụng. 
4. Trong khi$x > 0$, kiểm tra xem$x$là số lẻ hoặc số chẵn. Nếu như$x$chẵn, chúng ta chia cho 2 và chuyển sang bit tiếp theo. Điều này tương ứng với một chữ số 0 trong biểu diễn nhị phân có dấu ở vị trí hiện tại. 
5. Nếu$x$là số lẻ, chúng ta quyết định cộng hay trừ 1 ở bit này. Trong xây dựng NAF nhị phân, nếu$x \bmod 4 = 1$, chúng tôi sử dụng chữ số$+1$, ngược lại nếu$x \bmod 4 = 3$, chúng tôi sử dụng chữ số$-1$. Sau khi chọn, chúng ta tăng bộ đếm hoạt động và cập nhật$x = (x - d)/2$, Ở đâu$d \in \{+1, -1\}$. 
6. Lặp lại cho đến khi tất cả các bit được xử lý. 

Ý tưởng chính ở bước 5 là việc chọn$\pm 1$đảm bảo chúng ta tránh tạo ra các bit khác 0 liên tiếp trong biểu diễn, điều này sẽ làm tăng số lần nhảy cần thiết. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng ta đang xây dựng một biểu diễn của$x$trong cơ số 2 với các chữ số trong$\{-1, 0, 1\}$. Mỗi phép trừ hoặc cộng 1 tương ứng với việc thực hiện bước nhảy kích thước$2^k$. Lựa chọn tham lam đảm bảo rằng bất cứ khi nào một bit được đặt theo cách không thuận lợi (tạo hai chữ số khác 0 liền kề trong hệ nhị phân tiêu chuẩn), chúng tôi sẽ giải quyết nó ngay lập tức bằng cách lật nó thành một chữ số có dấu và đẩy số mang về phía trước. Điều này đảm bảo rằng không còn hai chữ số khác 0 liền kề nào, đây là thuộc tính xác định của biểu diễn nhị phân có dấu có độ dài tối thiểu. Vì mỗi chữ số khác 0 tương ứng với chính xác một nước đi nên việc giảm thiểu chúng sẽ trực tiếp giảm thiểu số lần nhảy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(x):
    if x == 0:
        return 0
    x = abs(x)
    ops = 0

    while x > 0:
        if x & 1 == 0:
            x >>= 1
        else:
            if x == 1:
                ops += 1
                break
            if x & 3 == 1:
                x -= 1
            else:
                x += 1
            ops += 1
            x >>= 1

    return ops

t = int(input())
for _ in range(t):
    x = int(input())
    print(solve_one(x))
```Việc triển khai phản ánh trực tiếp việc xây dựng nhị phân đã ký. Vòng lặp xử lý số từng bit từ phía ít quan trọng nhất. Khi bit hiện tại bằng 0, chúng ta chỉ cần dịch chuyển. Khi nó bằng một, chúng tôi quyết định nên trừ hay thêm một dựa trên ngữ cảnh bit tiếp theo được mã hóa trong$x \bmod 4$, điều này ngăn chặn sự liền kề trong tương lai của các chữ số khác 0. 

Trường hợp đặc biệt$x = 1$được xử lý rõ ràng vì không cần điều chỉnh số nhớ ngoài việc đếm một thao tác cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét$x = 7$. 

| Bước | x (trước) | x mod 2 | x mod 4 | Hoạt động | hoạt động | x (sau ca) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 1 | 3 | +1 | 1 | 4 | 
| 2 | 4 | 0 | - | ca | 1 | 2 | 
| 3 | 2 | 0 | - | ca | 1 | 1 | 
| 4 | 1 | 1 | - | +1 | 2 | 0 | 

Thuật toán tạo ra 2 nước đi. Điều này phản ánh rằng 7 có thể được viết là$8 - 1$, tương ứng với hai lũy thừa có dấu của hai. 

Bây giờ hãy xem xét$x = 10$. 

| Bước | x (trước) | x mod 2 | x mod 4 | Hoạt động | hoạt động | x (sau ca) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 0 | - | ca | 0 | 5 | 
| 2 | 5 | 1 | 1 | -1 | 1 | 2 | 
| 3 | 2 | 0 | - | ca | 1 | 1 | 
| 4 | 1 | 1 | - | +1 | 2 | 0 | 

Điều này cho thấy cách thuật toán tránh các thuật toán liên tiếp trong cấu trúc nhị phân, tạo ra biểu diễn có chữ ký tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log | x | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính theo số bit được xử lý. Vì mỗi số có nhiều nhất$10^{18}$, vòng lặp chạy tối đa khoảng 60 lần lặp cho mỗi lần kiểm tra, dễ dàng nằm trong giới hạn cho 100000 truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        x = int(input())
        if x == 0:
            print(0)
            continue
        x = abs(x)
        ops = 0
        while x > 0:
            if x & 1 == 0:
                x >>= 1
            else:
                if x == 1:
                    ops += 1
                    break
                if x & 3 == 1:
                    x -= 1
                else:
                    x += 1
                ops += 1
                x >>= 1
        print(ops)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample (conceptual, since formatting is broken)
assert run("1\n0\n") == "0", "zero case"

# custom cases
assert run("1\n1\n") == "1", "smallest non-zero"
assert run("1\n2\n") == "1", "power of two"
assert run("1\n3\n") == "2", "needs decomposition"
assert run("1\n7\n") == "2", "binary chain case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | trường hợp cơ bản không di chuyển | 
| 1 | 1 | bước nhảy đơn hoạt động | 
| 2 | 1 | sức mạnh chính xác của hai | 
| 3 | 2 | phân hủy không tầm thường | 
| 7 | 2 | dây xích nhỏ tệ nhất | 

## Vỏ cạnh 

cho$x = 0$, thuật toán ngay lập tức trả về 0 trước khi vào vòng lặp, điều này tránh việc đếm sai một thao tác ảo từ vòng xử lý nhị phân. 

Vì$x = 1$, vòng lặp kết thúc với một thao tác duy nhất và không dịch chuyển thêm nữa, vì không còn bit cao hơn. Điều này tránh được một bước điều chỉnh bổ sung không cần thiết có thể cố gắng truy cập các bit cao hơn không tồn tại. 

Đối với lũy thừa lớn của hai như$x = 2^{60}$, thuật toán chỉ thực hiện các dịch chuyển cho đến bit cuối cùng, tạo ra chính xác một thao tác, phù hợp với trực giác rằng một bước nhảy có kích thước đó là đủ.
