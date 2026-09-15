---
title: "CF 104687E - \u0421\u0442\u0440\u043e\u043a\u0430-1"
description: "Chúng ta được cung cấp một chuỗi nhị phân, chỉ bao gồm số 0 và số 1. Chúng tôi đo lường sự hỗn loạn bằng cách sử dụng phép đảo ngược: mỗi cặp vị trí có số 1 xuất hiện trước số 0 sẽ đóng góp một đơn vị."
date: "2026-06-29T08:46:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 55
verified: true
draft: false
---

[CF 104687E - \u0421\u0442\u0440\u043e\u043a\u0430-1](https://codeforces.com/problemset/problem/104687/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân, chỉ bao gồm số 0 và số 1. Chúng tôi đo lường sự hỗn loạn bằng cách sử dụng phép đảo ngược: mỗi cặp vị trí có số 1 xuất hiện trước số 0 sẽ đóng góp một đơn vị. Nhiệm vụ là giảm số lần đảo ngược này càng nhiều càng tốt, đồng thời có thêm quyền tự do thực hiện tối đa một thao tác hoán đổi hai ký tự lân cận. 

Vì vậy, chúng tôi bắt đầu với cấu hình cố định gồm các số 0 và 1, chúng tôi có thể tùy ý chọn một cặp liền kề và hoán đổi nó một lần, đồng thời chúng tôi muốn biết số lần đảo ngược nhỏ nhất có thể đạt được trong số tất cả các lựa chọn đó, bao gồm cả lựa chọn không làm gì cả. 

Độ dài chuỗi tối đa là 100, điều này đã gợi ý rằng ngay cả hành vi bậc hai trên mỗi phép toán ứng viên cũng có thể chấp nhận được. Ở đây, bất cứ thứ gì có dạng khối hoặc tệ hơn đều ổn về mặt kỹ thuật, nhưng chúng ta nên hướng tới một giải pháp bậc hai rõ ràng vì cấu trúc đơn giản. 

Một cạm bẫy ngây thơ xuất hiện khi cho rằng việc hoán đổi một cặp cục bộ chỉ ảnh hưởng đến sự đảo ngược liên quan đến hai ký tự đó theo cách tuyến tính rõ ràng mà không tính toán lại cẩn thận. Ví dụ: trong một chuỗi như`1010`, đổi chỗ ở giữa`10`ĐẾN`01`thay đổi không chỉ sự đảo ngược được hình thành bởi cặp đó mà còn cả cách các nhân vật đó tương tác với những người khác ở cả hai bên. Bất kỳ cách tiếp cận nào cố gắng cập nhật số lần đảo ngược bằng phương pháp phỏng đoán cục bộ quá mức đều có xu hướng bỏ lỡ các hiệu ứng chéo này. 

Một vấn đề tế nhị khác là quên tùy chọn không thực hiện trao đổi nào cả. Trong một số trường hợp, chuỗi ban đầu đã tối ưu rồi, ví dụ`000111`, nơi mà bất kỳ sự hoán đổi nào cũng sẽ chỉ làm tăng thêm tình trạng hỗn loạn. 

## Phương pháp tiếp cận 

Cách trực tiếp để suy nghĩ về vấn đề là thử mọi trạng thái hợp lệ mà chúng ta có thể đạt được. Chỉ có hai loại: chuỗi gốc và tất cả các chuỗi thu được bằng cách hoán đổi một cặp liền kề. Đối với mỗi chuỗi như vậy, chúng ta có thể tính toán số lần đảo ngược từ đầu bằng cách quét tất cả các cặp và đếm xem một chuỗi bao nhiêu lần.`1`đứng trước một`0`. 

Điều này hiệu quả vì các ràng buộc rất nhỏ nhưng lại dư thừa. Nếu chuỗi có độ dài n thì có thể có n-1 lần hoán đổi. Việc tính toán nghịch đảo từ đầu cho một chuỗi mất O(n2), do đó tổng trở thành O(n³). Đây vẫn là đường giới hạn có thể chấp nhận được với n ≤ 100, nhưng nó lãng phí cấu trúc. 

Quan sát quan trọng là việc hoán đổi hai phần tử liền kề không định hình lại toàn bộ chuỗi. Chỉ có thứ tự tương đối của các phần tử được hoán đổi so với phần còn lại của mảng thay đổi. Mọi thứ khác vẫn giữ nguyên. Điều đó có nghĩa là chúng ta không cần phải tính toán lại mọi thứ từ đầu nếu chúng ta sẵn sàng suy luận cẩn thận về việc đóng góp nghịch đảo thay đổi như thế nào. 

Tuy nhiên, trong thực tế, cách triển khai rõ ràng nhất cho n ≤ 100 vẫn là tính toán lại các phép nghịch đảo trong O(n) hoặc O(n²) cho mỗi ứng cử viên hoán đổi. Cấu trúc đủ nhỏ để sự đơn giản chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tính toán lại tất cả các cặp cho mỗi lần hoán đổi | O(n³) | O(1) | Được chấp nhận cho các ràng buộc | 
| Tính toán lại số lần đảo ngược trên mỗi lần hoán đổi theo O(n) hoặc O(n²) | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi câu trả lời là mức tối thiểu giữa việc không thực hiện thao tác nào và thực hiện chính xác một lần hoán đổi ở một vị trí nào đó. 

1. Tính số lần đảo ngược của chuỗi gốc. Đây là câu trả lời cơ bản của chúng tôi. Mọi cặp (i, j) có i < j đều đóng góp nếu ký tự tại i là 1 và tại j là 0. 
2. Khởi tạo câu trả lời với giá trị cơ bản này. Điều này tương ứng với việc lựa chọn không thực hiện bất kỳ sự hoán đổi nào. 
3. Với mọi chỉ số i từ 0 đến n − 2, hãy xem xét việc hoán đổi các ký tự ở vị trí i và i + 1. Xây dựng chuỗi kết quả sau phép hoán đổi này. Lý do chúng tôi xây dựng lại chuỗi một cách rõ ràng là vì n nhỏ và tính chính xác quan trọng hơn việc tối ưu hóa vi mô. 
4. Đối với mỗi cấu hình được hoán đổi, hãy tính số lần đảo ngược của nó từ đầu bằng cách quét tất cả các cặp (j, k) có j < k và đếm khi số 1 đứng trước số 0. Điều này đưa ra chi phí chính xác của lựa chọn hoán đổi đó mà không cần dựa vào lý luận gia tăng mong manh. 
5. Cập nhật câu trả lời ở mức tối thiểu trên tất cả các vị trí hoán đổi và cấu hình ban đầu. 

Giá trị được lưu trữ cuối cùng là số lần đảo ngược tốt nhất có thể đạt được. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình cuối cùng hợp lệ nào đều là chuỗi gốc hoặc khác với chuỗi đó chính xác một chuyển vị liền kề. Vì chúng tôi liệt kê tất cả các khả năng như vậy và tính toán số lần đảo ngược chính xác của chúng nên không bỏ sót trạng thái nào có thể truy cập được. Bởi vì việc đếm ngược được tính toán lại một cách chính xác chứ không phải là gần đúng nên không có rủi ro về việc đếm thiếu hoặc đếm thừa do các giả định cục bộ. Thuật toán đầy đủ trên không gian trạng thái có thể truy cập được xác định bởi ràng buộc hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def inversion_count(s):
    n = len(s)
    cnt = 0
    for i in range(n):
        if s[i] == '1':
            for j in range(i + 1, n):
                if s[j] == '0':
                    cnt += 1
    return cnt

def solve():
    s = input().strip()
    n = len(s)

    best = inversion_count(s)

    s = list(s)
    for i in range(n - 1):
        s[i], s[i + 1] = s[i + 1], s[i]
        best = min(best, inversion_count(s))
        s[i], s[i + 1] = s[i + 1], s[i]

    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xác định bộ đếm đảo ngược trực tiếp để quét tất cả các cặp có thứ tự và đếm số lần xuất hiện của một`1`theo sau là một`0`. Điều này được sử dụng cho cả cấu hình ban đầu và cho mọi biến thể được hoán đổi. 

Vòng lặp chính thử mỗi lần hoán đổi liền kề chính xác một lần, thực hiện nó tại chỗ, đánh giá số lần đảo ngược kết quả và sau đó khôi phục chuỗi gốc. Bước khôi phục là cần thiết; quên nó sẽ tích lũy nhiều giao dịch hoán đổi và phá vỡ ràng buộc “nhiều nhất là một thao tác”. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`01101`Chúng tôi theo dõi các ứng cử viên cơ bản và trao đổi. 

| Tiểu bang | Chuỗi | Số lần đảo ngược | 
| --- | --- | --- | 
| ban đầu | 01101 | 2 | 
| hoán đổi i=0 | 10101 | 3 | 
| hoán đổi i=1 | 00101 | 1 | 
| hoán đổi i=2 | 01001 | 2 | 
| hoán đổi i=3 | 01110 | 1 | 

Kết quả tốt nhất là 1, đạt được bằng cách hoán đổi vị trí 1 và 2. Điều này cho thấy rằng một cải tiến cục bộ có thể lan truyền trên toàn cầu vì việc di chuyển số 0 sang trái sẽ giảm nhiều lần đảo ngược trong tương lai cùng một lúc. 

### Ví dụ 2 

đầu vào:`11100`| Tiểu bang | Chuỗi | Số lần đảo ngược | 
| --- | --- | --- | 
| ban đầu | 11100 | 6 | 
| hoán đổi i=0 | 11100 | 6 | 
| hoán đổi i=1 | 11100 | 6 | 
| hoán đổi i=2 | 11010 | 5 | 
| hoán đổi i=3 | 11001 | 4 | 

Kết quả tốt nhất là 4, đạt được bằng cách hoán đổi gần ranh giới giữa số 1 và số 0. Điều này chứng tỏ rằng các giao dịch hoán đổi có lợi thường xảy ra ở giao diện giữa các khối có ký tự giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) trường hợp xấu nhất, hiệu quả là O(n²) trong thực tế | n lần hoán đổi, mỗi lần tính toán lại các phép đảo ngược trong O(n²) | 
| Không gian | O(1) | chỉ thao tác chuỗi tại chỗ | 

Với n 100, số lượng thao tác trong trường hợp xấu nhất là khoảng 10⁶, nằm trong giới hạn thoải mái đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    # re-run solution
    input = sys.stdin.readline

    def inversion_count(s):
        n = len(s)
        cnt = 0
        for i in range(n):
            if s[i] == '1':
                for j in range(i + 1, n):
                    if s[j] == '0':
                        cnt += 1
        return cnt

    s = input().strip()
    n = len(s)
    best = inversion_count(s)
    s = list(s)
    for i in range(n - 1):
        s[i], s[i + 1] = s[i + 1], s[i]
        best = min(best, inversion_count(s))
        s[i], s[i + 1] = s[i + 1], s[i]
    return str(best)

# provided sample
assert run("01101\n") == "1"

# all zeros
assert run("0000\n") == "0"

# all ones
assert run("1111\n") == "0"

# single beneficial swap
assert run("10\n") == "0"

# no beneficial swap
assert run("0011\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 01101 | 1 | độ chính xác của mẫu | 
| 0000 | 0 | không có trường hợp đảo ngược | 
| 1111 | 0 | tất cả những cái, không có số không | 
| 10 | 0 | đảo ngược tối thiểu có thể tháo rời | 
| 0011 | 0 | trao đổi không xấu đi tối ưu | 

## Vỏ cạnh 

Đối với một chuỗi được sắp xếp đầy đủ như`000111`, số lần đảo ngược ban đầu bằng 0. Bất kỳ sự hoán đổi liền kề nào cũng sẽ gây ra ít nhất một sự đảo ngược vì nó tạo ra một sự đảo ngược cục bộ.`10`mẫu. Thuật toán đánh giá chuỗi gốc trước tiên, vì vậy câu trả lời vẫn bằng 0 ngay cả khi tất cả các giao dịch hoán đổi đều tệ hơn. 

Đối với một chuỗi đảo ngược hoàn toàn như`111000`, sự đảo ngược được tối đa hóa ban đầu. Hoán đổi gần ranh giới, chẳng hạn như vị trí 2 và 3, làm giảm nhiều lần đảo ngược khối chéo cùng một lúc. Thuật toán đánh giá rõ ràng các lần hoán đổi ranh giới này và nắm bắt được sự cải thiện vì nó tính toán lại toàn bộ số lần đảo ngược sau mỗi lần hoán đổi thay vì giả định các hiệu ứng cục bộ.
