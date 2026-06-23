---
title: "CF 105400A - Sữa Đổ I"
description: "Chúng ta được cho một số nguyên $N$, là tích của một chuỗi tung xúc xắc ẩn nào đó. Mỗi lần tung xúc xắc là một con súc sắc tiêu chuẩn, vì vậy mọi thừa số trong dãy ẩn là một số nguyên từ 1 đến 6."
date: "2026-06-22T17:38:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105400
codeforces_index: "A"
codeforces_contest_name: "Fall 2024 Cupertino Informatics Tournament"
rating: 0
weight: 105400
solve_time_s: 94
verified: true
draft: false
---

[CF 105400A - Sữa đổ I](https://codeforces.com/problemset/problem/105400/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$N$, là sản phẩm của một chuỗi tung xúc xắc ẩn nào đó. Mỗi lần tung là một con súc sắc tiêu chuẩn, vì vậy mọi thừa số trong dãy ẩn là một số nguyên từ 1 đến 6. Nhiệm vụ là xây dựng lại bất kỳ tập hợp hợp lệ nào của các giá trị xúc xắc có tích bằng$N$và trong số tất cả các bản dựng lại hợp lệ, hãy chọn bản dựng lại có tổng giá trị cuộn lớn nhất có thể. 

Nếu không có nhiều tập hợp số trong phạm vi từ 1 đến 6 có thể tạo ra sản phẩm$N$, câu trả lời là không thể và chúng tôi xuất ra$-1$. Ngoài ra còn có giới hạn đầu ra: nếu tổng tốt nhất có thể trở nên rất lớn, chúng ta phải xuất$10^9 + 7$thay vì. 

Khó khăn chính là chúng ta không được yêu cầu xây dựng lại một hệ số hợp lệ mà phải tối ưu hóa tất cả các hệ số hợp lệ dưới một ràng buộc nhân. Điều này ngay lập tức làm cho nó trở thành một bài toán phân tích nhân tử và tối ưu hóa tổ hợp hơn là một sự phân rã đơn giản. 

Ràng buộc$N \le 10^5$ngụ ý rằng chúng ta có thể tự do phân tích nhân tử$N$TRONG$O(\sqrt{N})$hoặc$O(\log N)$-loại hành vi và vẫn được an toàn. Bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả các tập hợp hoặc kết hợp tìm kiếm các giá trị xúc xắc sẽ bùng nổ, vì ngay cả các sản phẩm vừa phải cũng có thể có nhiều cách phân tách. 

Một vấn đề tế nhị xuất phát từ giá trị 1 trên một con súc sắc. Vì nhân với 1 không làm thay đổi tích mà làm tăng tổng, nên việc cho phép các số 1 tùy ý sẽ khiến câu trả lời không bị giới hạn. Điều này có nghĩa là 1 phải được coi là không liên quan trong bất kỳ cách xây dựng tối ưu nào và chúng ta bỏ qua nó một cách hiệu quả khi suy luận về cấu trúc sản phẩm. 

Các trường hợp cạnh thường phá vỡ các giải pháp ngây thơ là các đầu vào chứa các thừa số nguyên tố bên ngoài$\{2,3,5\}$, đặc biệt là 7, chẳng hạn như$N=7$, nơi không tồn tại sự phân hủy. Một trường hợp phức tạp khác là$N=1$, trong đó chúng ta phải quyết định xem có cho phép sản phẩm trống hay không. Giải thích đúng là không cần cuộn, cho tổng bằng 0, vì việc đưa vào bất kỳ số 1 nào đều không phải là cấu trúc tối ưu có giới hạn hợp lệ. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng tạo ra tất cả nhiều tập giá trị từ 1 đến 6 mà tích của nó là$N$, sau đó tính tổng của chúng và lấy giá trị lớn nhất. Ngay cả khi giới hạn bản thân ở các số từ 2 đến 6, điều này vẫn có nghĩa là khám phá một quy trình phân nhánh trong đó mỗi bước chia sản phẩm còn lại và số lượng trạng thái tăng theo cấp số nhân theo số lượng các yếu tố. Ví dụ: ngay cả những con số vừa phải như$N = 2^{20}$đã tương ứng với một số lượng lớn các phân vùng thành 2, 4 và kết hợp, khiến cho việc liệt kê không thể thực hiện được trong thời gian giới hạn. 

Quan sát quan trọng là cấu trúc này hoàn toàn có tính nhân và mọi nghiệm hợp lệ đều tương ứng với việc phân tích nhân tử của$N$thành số từ$\{2,3,4,5,6\}$. Bản thân các số này được xây dựng từ các số nguyên tố 2, 3 và 5, với 4 số đóng góp$2^2$và 6 đóng góp$2 \cdot 3$. Bất kỳ hệ số hóa nào của$N$ngoài các số nguyên tố 2, 3, 5 thì không thể trả lời ngay được. 

Một khi chúng ta biểu diễn mọi thứ theo số mũ nguyên tố, vấn đề sẽ trở thành nhiệm vụ phân phối lại: chúng ta quyết định cách gộp 2 và 3 thành 6 bất cứ khi nào có lợi, còn nếu không thì để chúng ở dạng 2 và 3. Việc tối ưu hóa xoay quanh việc so sánh các nhóm khác nhau ảnh hưởng như thế nào đến tổng trong khi vẫn bảo toàn cùng một tích số. Giao dịch không tầm thường duy nhất là giữa$6$và cặp$3 + 2$, vì cả hai đều biểu diễn cùng một sản phẩm nguyên tố nhưng có tổng khác nhau. 

Điều này làm giảm vấn đề từ tìm kiếm tổ hợp sang xây dựng tham lam trên số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với tất cả các yếu tố | Hàm mũ | Hàm mũ | Quá chậm | 
| Thừa số nguyên tố + nhóm tham lam |$O(\sqrt{N})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Phân tích nhân tử$N$thành số nguyên tố, đếm số lần 2, 3 và 5 xuất hiện. Nếu bất kỳ số nguyên tố nào khác ngoài 2, 3 hoặc 5 xuất hiện thì việc xây dựng là không thể, vì vậy câu trả lời là$-1$. Bước này đảm bảo chúng tôi chỉ làm việc trong phạm vi các hệ số xúc xắc có thể biểu thị. 
2. Hãy để$c_2, c_3, c_5$lần lượt là số các số nguyên tố 2, 3 và 5. Chúng mô tả đầy đủ cấu trúc nhân của$N$. 
3. Xây dựng càng nhiều số 6 càng tốt. Mỗi số 6 tiêu tốn một số 2 và một số 3. Số 6 là$k = \min(c_2, c_3)$. Điều này là tối ưu vì 6 có cùng giá gốc như (2,3) nhưng mang lại tổng lớn hơn 2+3. 
4. Giảm số lượng còn lại sau khi hình thành 6s:$c_2 \leftarrow c_2 - k$,$c_3 \leftarrow c_3 - k$. 
5. Chuyển đổi trực tiếp các số nguyên tố còn lại thành mặt xúc xắc. Mỗi 3 còn lại trở thành 3, mỗi 2 còn lại trở thành 2 và mỗi 5 đóng góp thành 5. 
6. Tính tổng số tiền như sau$6k + 3c_3 + 2c_2 + 5c_5$. 
7. Nếu$N = 1$, trả về 0 trực tiếp vì không có cuộn nào được yêu cầu. 
8. Nếu số tiền tính toán vượt quá$10^9 + 7$, đầu ra$10^9 + 7$. 

### Tại sao nó hoạt động 

Mọi cách xây dựng hợp lệ đều tương ứng chính xác với phân bố thừa số nguyên tố của$N$thành các nhóm tạo thành giá trị xúc xắc. Quyền tự do duy nhất làm thay đổi tổng mà không làm thay đổi tích là cách chúng ta nhóm 2 và 3 thành các mặt riêng biệt hoặc kết hợp 6. Vì 6 hoàn toàn lấn át tổng 2 + 3 trong khi vẫn giữ nguyên hệ số, nên mọi giải pháp tối ưu đều phải tối đa hóa số lượng các cặp như vậy. Tất cả các yếu tố còn lại không có cách biểu diễn thay thế nào giúp cải thiện tổng mà không thay đổi tính khả thi, do đó, việc ghép cặp tham lam của (2,3) mô tả đầy đủ tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input().strip())
    
    if n == 1:
        print(0)
        return
    
    c2 = c3 = c5 = 0
    x = n
    
    while x % 2 == 0:
        c2 += 1
        x //= 2
    while x % 3 == 0:
        c3 += 1
        x //= 3
    while x % 5 == 0:
        c5 += 1
        x //= 5
    
    if x != 1:
        print(-1)
        return
    
    k = min(c2, c3)
    c2 -= k
    c3 -= k
    
    total = 6 * k + 3 * c3 + 2 * c2 + 5 * c5
    
    if total > MOD:
        print(MOD)
    else:
        print(total)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xử lý trường hợp đặc biệt$N=1$, trực tiếp trả về 0. Sau đó, nó thực hiện phân tích thành thừa số nguyên tố hoàn chỉnh nhưng chỉ theo dõi các số 2, 3 và 5. Bất kỳ thừa số còn sót lại nào ngay lập tức ngụ ý một cấu trúc không hợp lệ. 

Sau đó, nó thực hiện bước tối ưu hóa có ý nghĩa duy nhất, ghép 2 và 3 thành 6 một cách tham lam. Điều này được thực hiện bằng cách sử dụng số lượng tối thiểu của chúng, đảm bảo không lãng phí cơ hội ghép đôi. 

Cuối cùng, nó tính tổng bằng công thức dẫn xuất. Kiểm tra giới hạn được áp dụng ở cuối vì tổng tăng tuyến tính theo số lượng và không thể vượt quá số nguyên Python, nhưng vấn đề cần phải cắt bớt. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$N = 7$| Bước | c2 | c3 | c5 | Còn lại x | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | 7 | Hệ số hóa | 
| Sau khi nhân tố hóa | 0 | 0 | 0 | 7 | 7 không hợp lệ | 

Số 7 không thể biểu thị bằng cách sử dụng bất kỳ mặt xúc xắc nào từ 1 đến 6 ngoại trừ chính nó và vì số 7 không được phép tung ra nên không có cấu trúc nào tồn tại. Đầu ra là$-1$. 

Điều này xác nhận tính đúng đắn của bước xác nhận chính. 

### Ví dụ 2:$N = 60$| Bước | c2 | c3 | c5 | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | Hệ số hóa | 
| Sau khi nhân tố hóa | 2 | 1 | 1 |$60 = 2^2 \cdot 3 \cdot 5$| 
| Ghép nối | 1 | 0 | 1 | Một (2,3) tạo thành 6 | 
| Tổng cuối cùng | - | - | - |$6 + 2 + 5 = 13$| 

Chúng ta tạo thành một số 6 từ (2,3), để lại một số 2 và một số 5. ​​Tổng tối ưu là$6 + 2 + 5 = 13$. 

Điều này chứng tỏ cách ghép nối cải thiện cách biểu diễn trong khi vẫn duy trì tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{N})$| Yếu tố hóa chiếm ưu thế; tất cả các hoạt động khác là không đổi | 
| Không gian |$O(1)$| Chỉ có một vài quầy được lưu trữ | 

Các ràng buộc cho phép lên đến$N = 10^5$, do đó phép chia thử dễ dàng phù hợp với giới hạn thời gian và phần còn lại của quá trình tính toán là công việc liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline().strip()  # placeholder; replace with solve() capture if needed

# provided sample
# assert run("7\n") == "-1", "sample 1"

# custom cases
# N = 1
# assert run("1\n") == "0", "single empty product"

# N = 5 (valid single roll)
# assert run("5\n") == "5", "single 5"

# N = 12 = 2^2 * 3
# expected 6 + 2 = 8
# assert run("12\n") == "8", "pairing 2 and 3 once"

# N = 30 = 2 * 3 * 5
# expected 6 + 5 = 11
# assert run("30\n") == "11", "mix with 5"

# N with invalid prime
# assert run("14\n") == "-1", "contains 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | xử lý sản phẩm rỗng | 
| 5 | 5 | khuôn mặt hợp lệ duy nhất | 
| 12 | 8 | ghép nối tối ưu (2,3) | 
| 14 | -1 | phát hiện số nguyên tố không hợp lệ | 

## Vỏ cạnh 

Vụ án$N = 7$kích hoạt kiểm tra số nguyên tố không hợp lệ. Trong quá trình nhân tố hóa, giá trị còn lại trở thành 7 sau khi loại bỏ các đóng góp 2, 3 và 5. Vì nó không phải là 1 nên thuật toán sẽ loại bỏ nó một cách chính xác và đưa ra$-1$. Một cách tiếp cận ngây thơ cố gắng chia 7 thành các mặt được phép một cách tham lam sẽ thất bại vì không tồn tại sự phân tách hợp lệ. 

Vụ án$N = 1$được xử lý trước logic phân tích nhân tử. Nếu không có phần bảo vệ này, thuật toán sẽ coi nó như có các số nguyên tố bằng 0 và ngầm tạo ra tổng 0, điều này vẫn đúng, nhưng việc xử lý nó một cách rõ ràng sẽ tránh được sự mơ hồ về ngữ nghĩa của tích trống và đảm bảo tính nhất quán với cách diễn giải dự định.
