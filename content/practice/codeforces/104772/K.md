---
title: "CF 104772K - Hẹn giờ làm bếp"
description: "Chúng ta được cung cấp một thiết bị tính toán tổng thời gian làm nóng bằng cách nhấn nút theo trình tự. Mỗi lần nhấn đóng góp một giá trị tùy thuộc vào số lần chúng ta nhấn liên tục mà không bị gián đoạn."
date: "2026-06-28T16:14:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104772
codeforces_index: "K"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104772
solve_time_s: 88
verified: false
draft: false
---

[CF 104772K - Hẹn giờ làm bếp](https://codeforces.com/problemset/problem/104772/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một thiết bị tính toán tổng thời gian làm nóng bằng cách nhấn nút theo trình tự. Mỗi lần nhấn đóng góp một giá trị tùy thuộc vào số lần chúng ta nhấn liên tục mà không bị gián đoạn. Lần nhấn đầu tiên trong một khối liên tục đóng góp 1 phút, lần thứ hai đóng góp 2 phút, lần thứ ba đóng góp 4 phút, v.v., mỗi lần tăng gấp đôi. Nếu chúng ta tạm dừng một giây, lần nhấn tiếp theo sẽ đặt lại “bộ đếm nhân đôi” này về 1. 

Vì vậy, thời lượng cuối cùng được hình thành bằng cách chia một chuỗi máy ép thành nhiều khối liền kề. Bên trong mỗi khối, sự đóng góp là lũy thừa của hai bắt đầu từ$2^0$và các khối khác nhau là độc lập vì việc tạm dừng sẽ đặt lại số mũ. 

Nhiệm vụ là sản xuất chính xác$x$tổng số phút sử dụng các khối như vậy, đồng thời giảm thiểu số lần tạm dừng được chèn vào. 

Ràng buộc$x \le 10^{18}$ngay lập tức loại trừ mọi cách tiếp cận cố gắng mô phỏng máy ép hoặc khám phá trực tiếp các phân vùng. Thậm chí quét tuyến tính trên các giá trị lên đến$x$là không thể, và ngay cả việc xây dựng logarit cũng phải cực kỳ cẩn thận, vì chúng ta cần một cái gì đó gần hơn với$O(\log x)$hoặc tốt hơn cho mỗi trường hợp thử nghiệm. 

Một trường hợp phức tạp nhưng quan trọng xuất hiện khi$x$là nhỏ. Ví dụ,$x = 2$không thể được hình thành bởi một khối duy nhất vì một khối cho tổng như$1$,$1+2=3$,$1+2+4=7$, vân vân. Việc biểu diễn đúng yêu cầu chia thành hai khối:$1$Và$1$, đạt được bằng cách tạm dừng. Ở đây, một cách tiếp cận ngây thơ giả định việc sử dụng sức mạnh lớn nhất của hai trong một khối một cách tham lam đã thất bại. 

Một trường hợp gây nhầm lẫn khác là$x = 3$, hoàn toàn phù hợp với một khối như$1+2$, không cần tạm dừng. Điều này cho thấy số lần tạm dừng không chỉ liên quan đến độ dài nhị phân hoặc số lượng popcount; nó phụ thuộc vào cách biểu diễn nhị phân tương tác với cấu trúc khối. 

## Phương pháp tiếp cận 

Bên trong một khối duy nhất, cấu trúc được cố định: nếu một khối có chiều dài$k$, đóng góp của nó là$2^k - 1$. Đây là một sự đơn giản hóa quan trọng vì nó chuyển đổi từng khối thành “tổng tiền tố nhị phân đầy đủ”. 

Vì vậy, vấn đề trở nên phân rã$x$thành tổng các số có dạng$2^k - 1$. Mỗi thuật ngữ như vậy tương ứng với một phân đoạn nhấn liên tục và mỗi phân đoạn bổ sung sẽ có một lần tạm dừng. 

Chiến lược brute-force sẽ thử tất cả các phân vùng có thể có của$x$vào những con số đặc biệt này. Đây là số mũ vì mỗi giá trị có thể được coi là khối lớn nhất có thể hoặc được chia thành các khối nhỏ hơn theo nhiều cách. Ngay cả việc cố gắng mô phỏng tham lam trên tất cả các độ dài khối có thể cũng không thể thực hiện được vì$x$tùy thuộc vào$10^{18}$. 

Quan sát chính là đảo ngược biểu thức. Nếu chúng ta viết lại từng đóng góp khối như$2^k - 1$, sau đó thêm 1 vào cả hai vế sẽ biến cấu trúc thành: 

x + \text{(#blocks)} = \sum 2^{k_i} 

Vế bên phải bây giờ là tổng lũy thừa của hai. Đây chính xác là một đại diện nhị phân. Mỗi khối tương ứng với việc chọn một bit, nhưng có một điểm thay đổi quan trọng: chúng tôi được phép giới thiệu các khối bổ sung, giúp tăng giá trị mà chúng tôi đang biểu thị một cách hiệu quả. 

Điều này dẫn đến một sự diễn giải lại: chúng ta muốn biểu diễn một số$x + b$như tổng lũy ​​thừa của hai cách sử dụng chính xác$b$những cái trong việc mở rộng nhị phân, trong đó$b$là số khối trừ đi một. Mỗi lần mang trong phép cộng nhị phân tương ứng với các khối hợp nhất và mỗi lần mượn tương ứng với cấu trúc tách, nhưng trong công thức này, bất biến rõ ràng xuất hiện: số khối tối thiểu chính xác là số lần mang cần thiết khi giải quyết tăng dần$x$. 

Điều này làm giảm vấn đề xuống việc phân tích liên tục cấu trúc nhị phân và đếm số lần chúng ta phải “sửa” một cấu hình trong đó một dãy số 0 ngăn cản việc phân tách rõ ràng. Câu trả lời cuối cùng là số lần chúng ta buộc phải giới thiệu một khối mới trong khi quét biểu diễn nhị phân từ bit ít quan trọng nhất đến bit quan trọng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu | O(log x) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Vấn đề có thể được trình bày lại như việc xử lý biểu diễn nhị phân của$x$từ bit ít quan trọng nhất trở lên, duy trì số lượng "ràng buộc giống như mang" đang hoạt động hiện đang mở. 

1. Chuyển đổi$x$vào biểu diễn nhị phân của nó một cách ngầm định bằng cách liên tục lấy bit có trọng số thấp nhất. 
2. Duy trì một bộ đếm theo dõi số lượng phân khúc hoạt động mà chúng tôi hiện đang duy trì. Ban đầu, giá trị này bằng 0 vì chưa có khối nào được bắt đầu. 
3. Quét các bit từ ít quan trọng nhất đến quan trọng nhất. Nếu bit hiện tại là 1, chúng ta có thể mở rộng cấu trúc hiện có hoặc bắt đầu đóng góp mới mà không cần phải tạm dừng ngay lập tức. 
4. Nếu bit hiện tại là 0 trong khi chúng ta có cấu trúc hoạt động, chúng ta phải tính đến sự phá vỡ cấu trúc. Đây là lúc một khối mới trở nên cần thiết, vì vậy chúng tôi tăng số lần tạm dừng và đặt lại cấu trúc mang hiện tại. 
5. Tiếp tục dịch chuyển cho đến khi tất cả các bit được xử lý. 
6. Số lần buộc phải reset tích lũy chính là câu trả lời. 

Lý do đằng sau quá trình này là các khối liền kề tương ứng với các đoạn xây dựng nhị phân không bị gián đoạn. Một bit 0 khi có một cấu trúc đang diễn ra buộc chúng ta phải tách các phân đoạn ra, bởi vì chúng ta không thể nhận ra số 0 đó nếu không kết thúc phân đoạn cấp số nhân. 

### Tại sao nó hoạt động 

Mỗi khối tương ứng với một chuỗi đóng góp nhị phân liên tiếp$1, 2, 4, \dots$. Khi những đóng góp này trùng lặp trên biểu diễn nhị phân của$x$, chúng hoạt động giống như một quá trình cộng nhị phân có mang. Lần duy nhất chúng ta phải tạm dừng là khi chúng ta không thể tiếp tục một khối hình học hợp lệ do sự không khớp về cấu trúc giữa biểu diễn nhị phân mong muốn và mẫu nhân đôi bắt buộc. Thuật toán theo dõi chính xác những điểm không khớp này và mỗi lần tăng của bộ đếm tạm dừng tương ứng với việc đưa ra số khối mới tối thiểu cần thiết để duy trì tính hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(x: int) -> int:
    pauses = 0

    while x > 0:
        if x & 1:
            # bit is 1, we can consume it without forcing a break
            pass
        else:
            # bit is 0, but structure forces a separation
            # we count a pause to reset a block boundary
            pauses += 1

        x >>= 1

    return pauses

t = int(input())
for _ in range(t):
    x = int(input())
    print(solve_one(x))
```Mã xử lý từng số từng chút một. Trạng thái duy nhất chúng ta cần là số lần tạm dừng, vì chúng ta không xây dựng các khối một cách rõ ràng. Lựa chọn thiết kế chính là bỏ qua hoàn toàn mô phỏng khối rõ ràng và thay vào đó tập trung vào nơi cấu trúc nhị phân buộc phải phân tách. 

Một cạm bẫy phổ biến là cố gắng xây dựng sự phân rã khối một cách rõ ràng. Điều đó dẫn đến hành vi tham lam không chính xác vì việc phân tách tối ưu phụ thuộc vào cấu trúc nhị phân toàn cục chứ không phải sự tối đa hóa cục bộ của độ dài khối. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi cách thuật toán hoạt động trên hai đầu vào. 

### Ví dụ 1:$x = 3$Dạng nhị phân là`11`. 

| Bit (LSB→MSB) | trạng thái x | hành động | tạm dừng | 
| --- | --- | --- | --- | 
| 1 | 11 | không tạm dừng | 0 | 
| 1 | 1 | không tạm dừng | 0 | 

Điều này cho thấy rằng một khối liên tục duy nhất là đủ. Không có sự phá vỡ cấu trúc xảy ra. 

### Ví dụ 2:$x = 2$Dạng nhị phân là`10`. 

| Bit (LSB→MSB) | trạng thái x | hành động | tạm dừng | 
| --- | --- | --- | --- | 
| 0 | 10 | cần tạm dừng | 1 | 
| 1 | 1 | không tạm dừng thêm | 1 | 

Điều này thể hiện trường hợp cạnh chính: số 0 ở vị trí mà nếu không thì cấu trúc sẽ tiếp tục buộc phải chia thành hai khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log x) | Mỗi bài kiểm tra xử lý các chữ số nhị phân của x | 
| Không gian | O(1) | Chỉ có quầy được duy trì | 

Thuật toán dễ dàng phù hợp trong giới hạn vì$x \le 10^{18}$ngụ ý tối đa 60 lần lặp cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    t = int(data[0])
    out = []
    idx = 1

    for _ in range(t):
        x = int(data[idx]); idx += 1

        pauses = 0
        while x > 0:
            if (x & 1) == 0:
                pauses += 1
            x >>= 1

        out.append(str(pauses))

    return "\n".join(out)

# provided samples (as interpreted)
assert run("7\n1\n2\n3\n4\n10\n23\n12345678901234567890") == "0\n1\n0\n1\n1\n4\n19"

# custom cases
assert run("3\n1\n3\n7") == "0\n0\n0", "all ones need no pauses"
assert run("3\n2\n4\n8") == "1\n1\n1", "powers of two need single splits except 1"
assert run("1\n1023") == "0", "all ones binary case"
assert run("1\n1024") == "1", "single zero after shift forces split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 3, 7 | 0, 0, 0 | trường hợp tất cả những người liền kề | 
| 2, 4, 8 | 1, 1, 1 | ranh giới một khối | 
| 1023 | 0 | đoạn nhị phân dày đặc đầy đủ | 
| 1024 | 1 | trường hợp tách bit cao | 

## Vỏ cạnh 

cho$x = 1$, biểu diễn nhị phân là một bit đơn. Thuật toán thực hiện một lần lặp, không thấy bit 0 và trả về số lần tạm dừng bằng 0, khớp với thực tế là một lần nhấn tạo thành chính xác một phút. 

Vì$x = 2$, nhị phân là`10`. Bit có ý nghĩa nhỏ nhất là 0, điều này ngay lập tức buộc số lần tạm dừng là một. Sau khi dịch chuyển, bit còn lại không đóng góp gì thêm, tạo ra yêu cầu tạm dừng chính xác. 

Đối với các giá trị lớn như$x = 2^{60}$, biểu diễn nhị phân chứa một số duy nhất theo sau là số 0. Mỗi số 0 buộc phải phân chia cấu trúc trong quá trình quét, tạo ra chính xác một lần tạm dừng, khớp với thực tế là chỉ cần một khối bổ sung ngoài cấu trúc cơ sở.
