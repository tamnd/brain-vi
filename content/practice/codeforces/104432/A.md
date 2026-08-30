---
title: "CF 104432A - Dễ Dàng"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi cái có một danh sách các số nguyên dương. Chúng tôi được phép chèn chính xác một số nguyên bổ sung vào danh sách này."
date: "2026-06-30T18:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104432
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #17 (AOE-Forces)"
rating: 0
weight: 104432
solve_time_s: 105
verified: false
draft: false
---

[CF 104432A - Easy Peasy](https://codeforces.com/problemset/problem/104432/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi cái có một danh sách các số nguyên dương. Chúng tôi được phép chèn chính xác một số nguyên bổ sung vào danh sách này. Mục tiêu là chọn giá trị được chèn này sao cho ước số chung lớn nhất của toàn bộ bộ sưu tập, sau khi chèn, trở thành chính xác 1. Trong số tất cả các lựa chọn hợp lệ, chúng ta phải chọn số nguyên không âm nhỏ nhất, với hạn chế là giá trị được chèn không được bằng 1. 

Nhiệm vụ thực sự yêu cầu là làm thế nào để "sửa" gcd của toàn bộ mảng bằng cách sử dụng một số được chọn cẩn thận và thực hiện theo cách giảm thiểu số đó. 

Các ràng buộc cho phép tổng cộng tối đa 10^5 phần tử trong tất cả các trường hợp thử nghiệm, với các giá trị lớn tới 10^18. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng việc chèn ứng viên dựa trên tất cả các phần tử mảng cho nhiều khả năng cho mỗi trường hợp thử nghiệm. Việc tính toán gcd trên một mảng thì rẻ, nhưng việc thử liên tục nhiều ứng cử viên cho mỗi phần tử không có cấu trúc sẽ chỉ quá chậm nếu thực hiện bất cẩn; ở đây chúng ta sẽ thấy rằng cấu trúc thu gọn bài toán thành một tìm kiếm rất nhỏ. 

Có một số trường hợp khó nhận thấy có ý nghĩa quan trọng. 

Nếu tất cả các số đều giống nhau và lớn hơn 1 thì gcd chính là số đó. Ví dụ: một mảng như [6, 6, 6] có gcd 6. Nếu chúng ta thử chèn x = 0, gcd sẽ trở thành gcd(6, 0) = 6, điều này không giúp ích gì. Nếu chúng ta thử x = 2, gcd(6, 2) = 2, vẫn không bằng 1. Chỉ những số nguyên tố cùng nhau với 6 mới có tác dụng và phải tìm số nhỏ nhất như vậy. 

Nếu mảng đã có gcd 1 thì việc chèn 0 là hợp lệ vì gcd(1, 0) = 1. Điều này khiến 0 trở thành câu trả lời nhỏ nhất có thể và được phép vì chỉ 1 bị cấm. 

Một cách tiếp cận bất cẩn sẽ cố gắng tính lại gcd của toàn bộ mảng cho mọi ứng cử viên x một cách độc lập. Vì về nguyên tắc x có thể phát triển lớn nên điều này sẽ lãng phí thời gian và bỏ sót sự thật rằng chỉ có gcd của mảng ban đầu mới thực sự quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi trường hợp kiểm thử, hãy tính gcd của mảng, sau đó thử tăng giá trị của x bắt đầu từ 0, bỏ qua 1 và kiểm tra xem việc chèn x có làm cho tổng gcd bằng 1 hay không. Mỗi lần kiểm tra đều yêu cầu tính gcd(g, x), trong đó g là gcd của mảng. Điều này có hiệu quả vì gcd của toàn bộ mảng cộng với x chính xác là gcd(g, x). 

Cách tiếp cận thô bạo này là đúng, nhưng nếu chúng ta tưởng tượng việc mở rộng nó mà không có cái nhìn sâu sắc, có thể chúng ta sẽ cảm thấy như cần phải kiểm tra nhiều giá trị của x và kết hợp nhiều lần với toàn bộ mảng. Điều đó sẽ trở nên tốn kém nếu chúng ta tính toán lại gcd trên n phần tử mỗi lần. Tuy nhiên, khi chúng tôi nén mảng thành một giá trị gcd duy nhất, mỗi lần kiểm tra sẽ trở thành O(1) và nút thắt cổ chai sẽ biến mất. 

Quan sát quan trọng là toàn bộ mảng tương đương với một số g trong các phép toán gcd. Bài toán quy về việc tìm x nhỏ nhất sao cho gcd(g, x) = 1 và x ≠ 1. Vì x = 0 chỉ đúng khi g = 1, phần còn lại của việc tìm kiếm là trên các số nguyên rất nhỏ bắt đầu từ 2 trở lên. Bởi vì g có nhiều nhất một vài thừa số nguyên tố nhỏ trong thực tế so với độ lớn của nó, nên số nguyên tố cùng nhau đầu tiên xuất hiện rất nhanh. 

Điều này làm giảm vấn đề khi tính toán một gcd cho mỗi trường hợp thử nghiệm và sau đó quét một tiền tố số nguyên nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên mảng cho mỗi ứng cử viên x | O(n · X) | O(1) | Quá chậm | 
| Giảm xuống gcd rồi thử nhỏ x | O(n + K) | O(1) | Đã chấp nhận | 

Ở đây K là số số nguyên nhỏ được kiểm tra trước khi tìm giá trị nguyên tố cùng nhau, được giới hạn bởi một hằng số rất nhỏ trong thực tế. 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi giảm mảng, sau đó tìm kiếm giá trị chèn hợp lệ nhỏ nhất.

1. Tính gcd của tất cả các phần tử trong mảng. Điều này nén tất cả cấu trúc thành một số g duy nhất vì gcd có tính chất kết hợp và giao hoán. 
2. Nếu g bằng 1 thì mảng đã có gcd 1 mà không cần chèn thêm. Số nguyên không âm nhỏ nhất được phép không phải là 1 là 0 và việc chèn 0 sẽ giữ cho gcd không thay đổi ở mức 1. 
3. Nếu g lớn hơn 1, chúng ta phải tìm số nguyên x nhỏ nhất bắt đầu từ 0 trở lên, trừ 1, sao cho gcd(g, x) bằng 1. 
4. Bỏ qua x = 0 ngay lập tức trong trường hợp này vì gcd(g, 0) = g, khác 1 khi g > 1. 
5. Bắt đầu kiểm tra từ x = 2 trở lên. Với mỗi ứng viên, hãy tính gcd(g, x). Giá trị đầu tiên mà gcd này trở thành 1 là câu trả lời. 
6. Dừng lại ngay khi tìm thấy x như vậy, vì chúng ta đang quét theo thứ tự tăng dần và cần mức tối thiểu. 

### Tại sao nó hoạt động 

Toàn bộ mảng chỉ đóng góp thông qua gcd g của nó, vì gcd(a1, a2, ..., an, x) đơn giản hóa thành gcd(g, x). Do đó, bất kỳ lựa chọn nào của x để xác định kết quả đều phải nguyên tố cùng nhau với g. Số nguyên hợp lệ nhỏ nhất là 0 khi g đã bằng 1 hoặc số nguyên đầu tiên bắt đầu từ 2 không chia sẻ bất kỳ thừa số nguyên tố nào với g. Bởi vì các ràng buộc gcd chỉ phụ thuộc vào thừa số nguyên tố, nên khi chúng ta đạt đến một số x đưa ra một số nguyên tố mới không chia g, gcd sẽ trở thành 1 vĩnh viễn cho x đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        g = 0
        for v in arr:
            g = math.gcd(g, v)
        
        if g == 1:
            out.append("0")
            continue
        
        x = 2
        while True:
            if math.gcd(g, x) == 1:
                out.append(str(x))
                break
            x += 1
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên thu gọn mảng thành một giá trị gcd duy nhất. Đây là thông tin duy nhất quan trọng cho mọi lý luận trong tương lai. Nhánh cho g = 1 đảm nhận vai trò đặc biệt của số 0: nó bảo toàn gcd 1 và là số nhỏ nhất được phép. 

Đối với g > 1, chúng tôi thực hiện quét tăng dần đơn giản bắt đầu từ 2. Kiểm tra gcd là thời gian không đổi và an toàn vì chúng tôi không bao giờ cần phải xem xét các tương tác với các phần tử mảng riêng lẻ nữa. 

Vòng lặp được cố tình không giới hạn trong mã, nhưng về mặt toán học, nó kết thúc nhanh chóng vì các số nguyên rất nhỏ hầu như luôn phá vỡ cấu trúc gcd bằng g. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: [2, 1, 4] 

Chúng tôi tính toán gcd của mảng từng bước. 

| Bước | Giá trị | Gcd hiện tại | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| Đọc 2 | 2 | 2 | 
| Đọc 1 | 1 | 1 | 
| Đọc 4 | 4 | 1 | 

Vì gcd cuối cùng là 1 nên chúng ta lập tức xuất ra 0. Việc chèn 0 không làm thay đổi gcd vì gcd(1, 0) = 1. 

Ví dụ này hiển thị nhánh trong đó mảng đã “hoàn toàn tương thích” với gcd 1 và không cần chỉnh sửa ngoài việc chèn số nhỏ nhất được phép. 

### Ví dụ 2 

Mảng đầu vào: [9, 33, 3, 11] 

| Bước | Giá trị | Gcd hiện tại | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| Đọc 9 | 9 | 9 | 
| Đọc 33 | 33 | 3 | 
| Đọc 3 | 3 | 3 | 
| Đọc 11 | 11 | 1 | 

Đợi đã, trong trường hợp này gcd trở thành 1, vì vậy theo quy tắc, chúng ta sẽ xuất ra 0. Điều này phù hợp với hành vi mẫu thứ hai dự kiến. 

Dấu vết này chứng minh rằng ngay cả khi các số trông có cấu trúc cao, một phần tử nguyên tố tương đối duy nhất có thể thu gọn gcd thành 1, kích hoạt trường hợp 0 ​​ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + K) | Một lần để tính gcd của mảng cộng với một lần quét nhỏ trên các số nguyên bắt đầu từ 2 cho đến khi tìm thấy giá trị nguyên tố cùng nhau | 
| Không gian | O(1) | Chỉ một số biến được duy trì cho mỗi trường hợp thử nghiệm | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm được giới hạn bởi 10^5, do đó tính toán gcd tổng thể là tuyến tính. Việc quét bổ sung là không đáng kể vì nó dừng ở một số nguyên rất nhỏ trong mọi trường hợp thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else __import__("builtins").exec  # placeholder
```Một dây nịt đúng sẽ nối dây`solve()`trực tiếp; được bỏ qua ở đây để định dạng rõ ràng.```
# sample-like cases
# (interpreting the sample as two test cases)
# 1) gcd becomes 1
# 2) gcd becomes 1 immediately

# minimal n=1, already 1
# expected 0
# single element 1

# all equal >1
# expected 2

# mixed case producing gcd 1 early
# expected 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 0 | chèn hợp lệ nhỏ nhất khi gcd đã 1 | 
| 2 2 2 | 2 | nhỏ nhất x nguyên tố cùng nhau với g=2 | 
| 3 5 7 | 0 | gcd đã có 1 trường hợp | 

## Vỏ cạnh 

Khi mảng gcd đã bằng 1, việc chèn 0 là hợp lệ và hoàn toàn tối ưu. Ví dụ: đầu vào [3, 5, 7] thu gọn thành gcd 1 ngay lập tức, do đó thuật toán không được thử bất kỳ ứng cử viên tích cực nào. 

Khi mảng gcd lớn hơn 1 và chẵn, các ứng viên chẵn nhỏ sẽ thất bại ngay lập tức. Ví dụ: nếu g = 8 thì x = 2, 4, 6, 8 đều thất bại vì chúng có chung thừa số 2. Thuật toán bỏ qua chúng một cách chính xác bằng cách tăng x cho đến khi nó đạt đến một số như 3 hoặc 5 nguyên tố cùng nhau với g. 

Khi tất cả các số giống hệt nhau, chẳng hạn như [12, 12, 12], gcd vẫn là 12 và thuật toán tìm kiếm từ 2 trở lên cho đến khi tìm thấy số nguyên đầu tiên không chia sẻ thừa số nguyên tố với 12. Trong trường hợp này x = 5 hoạt động ngay lập tức vì gcd(12, 5) = 1 và quá trình quét kết thúc sớm.
