---
title: "CF 105631A - Lễ kỷ niệm"
description: "Chúng tôi được đưa ra nhiều kịch bản độc lập. Trong mỗi kịch bản, có ba loại bóng bay: loại có nhãn S, loại có nhãn Y và loại có nhãn U. Từ những quả bóng bay này, chúng tôi muốn lắp ráp nhiều lần các gói trang trí giống hệt nhau được gọi là bộ SYSU."
date: "2026-06-22T14:56:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105631
codeforces_index: "A"
codeforces_contest_name: "SYSU Collegiate Programming Contest 2024 (SYSUCPC 2024), Final"
rating: 0
weight: 105631
solve_time_s: 55
verified: true
draft: false
---

[CF 105631A - Lễ kỷ niệm](https://codeforces.com/problemset/problem/105631/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều kịch bản độc lập. Trong mỗi kịch bản, có ba loại bóng bay: loại có nhãn S, loại có nhãn Y và loại có nhãn U. Từ những quả bóng bay này, chúng tôi muốn lắp ráp nhiều lần các gói trang trí giống hệt nhau được gọi là bộ SYSU. 

Một bộ SYSU hợp lệ sử dụng một mẫu chữ cái cố định: nó yêu cầu hai bong bóng S, một bong bóng Y và một bong bóng U. Mỗi bong bóng có thể được sử dụng tối đa một lần, do đó, khi bong bóng được gán cho một bộ, nó sẽ không thể được sử dụng lại trong một bộ khác. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là xác định xem có thể hình thành bao nhiêu bộ SYSU hoàn chỉnh từ số lượng bóng bay S, Y và U có sẵn. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm và mỗi trường hợp lên tới 10.000. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm đều dễ dàng đủ. Ngay cả một O(x + y + z) cho mỗi trường hợp thử nghiệm vẫn không đáng kể, nhưng chúng ta nên hướng tới một giải pháp số học trực tiếp. 

Một sai lầm phổ biến trong những vấn đề như thế này là cho rằng mỗi chữ cái đóng góp độc lập mà không tôn trọng yêu cầu nhân đôi đối với S. Một vấn đề tế nhị khác là quên rằng cả ba tài nguyên phải có sẵn đồng thời, vì vậy yếu tố hạn chế luôn là thành phần bị hạn chế nhất. 

Các trường hợp cạnh chủ yếu là số lượng suy biến. Nếu bất kỳ x, y hoặc z nào bằng 0 thì không thể hình thành tập hợp nào. Một ranh giới thú vị khác là khi S dồi dào nhưng Y hoặc U nhỏ hoặc ngược lại. Ví dụ: nếu x = 100, y = 1, z = 1, chỉ có thể có một bộ mặc dù S có vẻ dồi dào. Ngược lại, nếu y và z lớn nhưng S nhỏ thì S sẽ trở thành nút cổ chai vì cần có hai lần cho mỗi bộ. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng từng tòa nhà một. Đối với mỗi lần thử, chúng tôi kiểm tra xem có sẵn ít nhất hai quả bóng S, một quả bóng Y và một quả bóng chữ U hay không. Nếu vậy, chúng tôi trừ đi số lượng đó và tăng câu trả lời của mình. Chúng tôi lặp lại cho đến khi không còn có thể tạo thành một bộ nữa. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa của quy trình. Tuy nhiên, hiệu suất của nó phụ thuộc vào số lượng bộ được hình thành. Trong trường hợp xấu nhất, nếu x, y, z lớn, chúng ta có thể trừ liên tục tới 10.000 lần cho mỗi trường hợp thử nghiệm. Với 100 trường hợp kiểm thử, điều này vẫn có thể chấp nhận được ở đây, nhưng nó không cần thiết và che giấu cấu trúc thực sự của vấn đề. 

Quan sát quan trọng là mỗi bộ tiêu thụ tài nguyên cố định một cách độc lập với các lựa chọn trước đó. Không có lợi ích ghép nối hoặc sắp xếp lại qua các bước. Điều này có nghĩa là số lượng bộ tối đa được xác định hoàn toàn bằng số lần mỗi tài nguyên có thể hỗ trợ mức sử dụng cần thiết. S giới hạn chúng tôi bởi x // 2, Y giới hạn chúng tôi bởi y và U giới hạn chúng tôi bởi z. Câu trả lời cuối cùng phải đồng thời thỏa mãn tất cả các ràng buộc nên nó là mức tối thiểu trong ba khả năng độc lập này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T*trả lời) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Công thức trực tiếp tối ưu | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi test case, hãy đọc các giá trị x, y và z. Chúng đại diện cho số lượng bóng bay S, Y và U tương ứng có sẵn. Mục tiêu là tính toán xem có thể tạo được bao nhiêu gói SYSU đầy đủ từ những nguồn cung cấp cố định này. 
2. Tính xem có bao nhiêu bó bóng bay S có thể đỡ được một mình. Vì mỗi gói yêu cầu hai bong bóng S nên số lần đóng góp có thể có của S là x chia cho 2 bằng phép chia số nguyên. Điều này thể hiện số lượng tập hợp tối đa mà S có thể duy trì mà không cần xem xét các ràng buộc khác. 
3. Coi y là giới hạn trên trực tiếp của số tập hợp. Mỗi bộ yêu cầu chính xác một quả bóng Y nên số lượng bộ không thể vượt quá y. 
4. Coi z như một giới hạn trên trực tiếp khác. Mỗi bộ yêu cầu chính xác một quả bóng chữ U, vì vậy z cũng giới hạn tổng số bộ có thể có. 
5. Câu trả lời cuối cùng là giá trị nhỏ nhất trong ba giá trị này vì mọi tập hợp lệ phải đáp ứng đồng thời cả ba ràng buộc và việc vượt quá bất kỳ một tài nguyên nào sẽ khiến việc xây dựng thêm không thể thực hiện được. 

### Tại sao nó hoạt động 

Mỗi bộ SYSU tiêu thụ tài nguyên một cách độc lập và không thể đảo ngược. Điều này có nghĩa là tính khả thi của việc hình thành k bộ chỉ phụ thuộc vào tổng số tài nguyên cần thiết, cụ thể là các bong bóng 2k S, các bong bóng k Y và các bong bóng k U, có sẵn hay không. Do đó, điều kiện khả thi là 2k  x, k  y và k  z. K lớn nhất thỏa mãn mọi bất đẳng thức chính xác là giá trị nhỏ nhất của x // 2, y và z, đảm bảo tính tối ưu mà không cần mô phỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        x, y, z = map(int, input().split())
        out.append(str(min(x // 2, y, z)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp chuyển điều kiện khả thi về mặt toán học thành mã. Phép chia số nguyên x // 2 nắm bắt được yêu cầu ghép nối của S bong bóng. Việc lấy mức tối thiểu trên ba giới hạn được tính toán sẽ thực thi đồng thời tất cả các ràng buộc. Việc sử dụng đầu ra được đệm thông qua danh sách và kết nối sẽ tránh được chi phí I/O lặp lại, mặc dù điều đó không thực sự cần thiết ở quy mô này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

x = 10, y = 2, z = 4 

| Bước | x // 2 | y | z | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| Tính toán ban đầu | 5 | 2 | 4 | - | 
| Lấy tối thiểu | - | - | - | 2 | 

Điều này chứng tỏ trường hợp Y là hệ số giới hạn. Mặc dù S và U là đủ cho nhiều bộ hơn nhưng chỉ tồn tại 2 quả bóng Y, giới hạn tổng số ở 2 gói hoàn chỉnh. 

### Ví dụ 2 

đầu vào: 

x = 11, y = 45, z = 14 

| Bước | x // 2 | y | z | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| Tính toán ban đầu | 5 | 45 | 14 | - | 
| Lấy tối thiểu | - | - | - | 5 | 

Ở đây S trở thành nút thắt cổ chai vì nó được yêu cầu hai lần cho mỗi bộ. Mặc dù Y và U rất nhiều nhưng chỉ có thể hình thành được 5 cặp S hoàn chỉnh. 

Những dấu vết này cho thấy câu trả lời luôn được kiểm soát bởi sự ràng buộc chặt chẽ nhất giữa ba nguồn lực độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện các phép tính số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng bất kể kích thước đầu vào | 

Lời giải dễ dàng nằm trong giới hạn vì ngay cả ở T = 100, việc tính toán chỉ bao gồm một vài phép tính số nguyên cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        x, y, z = map(int, input().split())
        res.append(str(min(x // 2, y, z)))
    return "\n".join(res)

# provided sample-style tests (based on statement examples)
assert run("3\n10 2 4\n11 45 14\n1924 100 2024\n") == "2\n5\n100", "sample tests"

# minimum values
assert run("1\n0 0 0\n") == "0", "all zero"

# S bottleneck
assert run("1\n1 100 100\n") == "0", "not enough S"

# Y bottleneck
assert run("1\n100 1 100\n") == "1", "Y limits"

# U bottleneck
assert run("1\n100 100 2\n") == "2", "U limits"

# all equal large
assert run("1\n20 20 20\n") == "10", "balanced case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | 0 | không có tài nguyên | 
| 1 100 100 | 0 | Ràng buộc ghép nối S | 
| 100 1 100 | 1 | Nút cổ chai Y | 
| 100 100 2 | 2 | Bạn thắt cổ chai | 
| 20 20 20 | 10 | cân bằng tỷ lệ | 

## Vỏ cạnh 

Khi x nhỏ hơn 2, thuật toán tạo ra số 0 một cách chính xác vì x // 2 trở thành 0, ngay lập tức chặn mọi hình thành có thể xảy ra bất kể y hay z. Ví dụ: đầu vào 1 10 10 mang lại x // 2 = 0, vì vậy câu trả lời là 0, phù hợp với thực tế là không thể hình thành một tập hợp SYSU nếu không có hai bong bóng S. 

Khi một trong y hoặc z bằng 0, thao tác tối thiểu đảm bảo kết quả bằng 0 ngay cả khi có nhiều bong bóng S. Ví dụ: đầu vào 100 0 100 mang lại min(50, 0, 100) = 0, phản ánh chính xác rằng mọi tập hợp đều yêu cầu đồng thời cả Y và U. 

Trong trường hợp S cực lớn nhưng các ràng buộc theo cặp chiếm ưu thế, chẳng hạn như 10000 1 1, việc tính toán giảm xuống min(5000, 1, 1) = 1, cho thấy nút cổ chai được xác định chính xác mà không cần mô phỏng.
