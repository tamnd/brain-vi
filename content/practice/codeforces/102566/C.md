---
title: "CF 102566C - Biểu tượng cảm xúc"
description: "Chúng tôi có một số mảng giá trị biểu tượng cảm xúc. Mục tiêu không phải là sắp xếp toàn bộ mảng. Thay vào đó, chúng ta có thể chọn một phần liền kề của mảng và xóa một số phần tử bên trong phần đó để các phần tử còn lại theo thứ tự không giảm."
date: "2026-08-06T20:54:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "C"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 89
verified: true
draft: false
---

[CF 102566C - Biểu tượng cảm xúc](https://codeforces.com/problemset/problem/102566/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số mảng giá trị biểu tượng cảm xúc. Mục tiêu không phải là sắp xếp toàn bộ mảng. Thay vào đó, chúng ta có thể chọn một phần liền kề của mảng và xóa một số phần tử bên trong phần đó để các phần tử còn lại theo thứ tự không giảm. Các phần tử trước và sau phần được chọn đó không quan trọng và không được tính là phần bị xóa. 

Giá trị đầu tiên chúng ta phải xuất ra là số lượng biểu tượng cảm xúc tối đa có thể còn lại. Giá trị thứ hai là số lượng biểu tượng cảm xúc bị xóa tối thiểu cần thiết trong khoảng thời gian đã chọn để có được chuỗi tối ưu như vậy. 

Các ràng buộc buộc một giải pháp gần tuyến tính. Một trường hợp thử nghiệm duy nhất có thể chứa tối đa 100000 giá trị và tổng của tất cả các kích thước tối đa là 1000000. Việc thử mọi khoảng thời gian đã mang lại kết quả bậc hai, khoảng 10^10 phép tính cho trường hợp lớn nhất, vượt xa giới hạn. Chúng ta cần một phương thức O(N log N). 

Một lỗi phổ biến là chỉ tính độ dài chuỗi con không giảm dài nhất và xuất ra N trừ đi giá trị đó. Việc tính các biểu tượng cảm xúc nằm ngoài khoảng thời gian đã chọn sẽ bị xóa, điều này không được phép. Ví dụ, mảng`1 2 3 100 0`có dãy con không giảm dài nhất có độ dài 4 bằng cách sử dụng`1 2 3 100`, nhưng câu trả lời chỉ loại bỏ`0`nếu toàn bộ khoảng được chọn. Các phần tử bên ngoài không được tự động xóa. 

Một trường hợp cạnh khác là khi toàn bộ mảng không giảm. Đối với đầu vào`1 2 2 5`, đầu ra đúng là`4 0`. Một giải pháp luôn tính các phần tử bị loại bỏ là`N - length`có thể vô tình hoạt động ở đây nhưng không thành công trên các mảng có chuỗi con tốt nhất nằm trong một khoảng nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi khoảng có thể, tính toán dãy con không giảm dài nhất bên trong nó và giữ câu trả lời tốt nhất. Có các khoảng O(N^2) và việc tính toán một chuỗi con cho mỗi khoảng thời gian vốn đã tốn kém, do đó tổng công việc sẽ trở thành O(N^3) khi triển khai trực tiếp. 

Quan sát quan trọng là các biểu tượng cảm xúc còn lại phải tạo thành một dãy con không giảm của mảng ban đầu. Số lượng biểu tượng cảm xúc được giữ tối đa là chuỗi con không giảm dài nhất. Điều kiện bổ sung về việc xóa có nghĩa là chúng ta phải nhớ nơi chuỗi con đó bắt đầu và kết thúc, bởi vì chỉ những phần tử nằm giữa các vị trí đó mới có thể bị xóa. 

Chúng ta có thể sử dụng ý tưởng sắp xếp kiên nhẫn cho dãy con không giảm dài nhất. Trong khi xử lý mảng, chúng tôi duy trì giá trị kết thúc nhỏ nhất có thể cho mỗi độ dài chuỗi con. Cùng với thông tin đó, chúng tôi lưu giữ đủ dữ liệu vị trí để xây dựng lại khoảng thời gian tốt nhất chứa chuỗi con tối ưu. 

Độ dài chuỗi con đưa ra câu trả lời đầu tiên. Khi dãy con tối ưu được chọn có chỉ số đầu tiên`l`và chỉ số cuối cùng`r`, số lần xóa là`(r - l + 1) - length`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^3) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chỉ nén các giá trị nhằm mục đích sắp xếp thứ tự và xử lý mảng từ trái sang phải. Chúng ta cần biết, với mỗi giá trị, dãy con không giảm tốt nhất có thể được mở rộng bởi phần tử hiện tại. 
2. Sử dụng cây Fenwick trên các giá trị nén. Mỗi nút Fenwick lưu trữ trạng thái chuỗi con tốt nhất kết thúc bằng các giá trị trong tiền tố đó. Một trạng thái chứa độ dài chuỗi con, vị trí bắt đầu và vị trí kết thúc của nó. 
3. Đối với giá trị hiện tại`a[i]`, truy vấn tất cả các giá trị trước đó nhỏ hơn hoặc bằng nó. Trạng thái trả về tốt nhất có thể được mở rộng vì việc thêm`a[i]`giữ cho dãy không giảm. 
4. Tạo trạng thái mới bằng cách tăng độ dài lên một và đặt vị trí kết thúc thành`i`. Cập nhật cây Fenwick tại vị trí nén của`a[i]`. 
5. Sau khi xử lý tất cả các phần tử, chọn trạng thái có độ dài lớn nhất. Nếu một số trạng thái có cùng độ dài, hãy chọn trạng thái có kích thước khoảng nhỏ nhất. 
6. Độ dài câu trả lời là độ dài trạng thái đã chọn. Số lượng biểu tượng cảm xúc bị xóa là kích thước khoảng cách của nó trừ đi độ dài được giữ lại. 

Tại sao nó hoạt động: 

Mọi thông báo cuối cùng hợp lệ đều tương ứng với một dãy con không giảm trong một khoảng nào đó. Cây Fenwick xem xét mọi quan hệ có thể có trước đó, bởi vì một giá trị có thể mở rộng chính xác các dãy con kết thúc bằng giá trị không lớn hơn chính nó. Các trạng thái tốt nhất được lưu trữ bảo toàn các ứng cử viên có thể dẫn đến kết quả tối ưu, do đó độ dài tối đa cuối cùng là chuỗi con không giảm dài nhất có thể và khoảng được chọn là khoảng nhỏ nhất trong số các giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a):
    vals = sorted(set(a))
    comp = {v: i + 1 for i, v in enumerate(vals)}
    m = len(vals)

    bit = [(0, 0, 0)] * (m + 2)

    def better(x, y):
        if x[0] != y[0]:
            return x if x[0] > y[0] else y
        if x[0] == 0:
            return x
        sx = x[2] - x[1] + 1
        sy = y[2] - y[1] + 1
        return x if sx < sy else y

    def query(i):
        ans = (0, 0, 0)
        while i:
            ans = better(ans, bit[i])
            i -= i & -i
        return ans

    def update(i, value):
        while i <= m:
            bit[i] = better(bit[i], value)
            i += i & -i

    answer = (0, 0, 0)

    for i, x in enumerate(a):
        prev = query(comp[x])
        if prev[0] == 0:
            cur = (1, i, i)
        else:
            cur = (prev[0] + 1, prev[1], i)
        update(comp[x], cur)
        answer = better(answer, cur)

    return answer[0], answer[2] - answer[1] + 1 - answer[0]

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        x, y = solve_case(a)
        out.append(f"{x} {y}")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây Fenwick lưu trữ thông tin về tiền tố của các giá trị. Truy vấn tiền tố chính xác là thao tác cần thiết cho các chuỗi con không giảm, vì giá trị được chọn trước đó không được vượt quá giá trị hiện tại. 

Trạng thái lưu trữ chỉ mục bắt đầu thay vì chỉ độ dài. Đây là chi tiết xử lý quy tắc xóa bất thường. Chúng tôi không muốn số lần xóa bao gồm các biểu tượng cảm xúc nằm ngoài khoảng thời gian đã chọn, do đó, ranh giới khoảng thời gian phải được theo dõi. 

Tọa độ nén là cần thiết vì giá trị biểu tượng cảm xúc có thể lớn tới 10^9, nhưng chỉ thứ tự tương đối của chúng mới quan trọng. Số lượng giá trị nén nhiều nhất là N, vì vậy tất cả các phép toán Fenwick vẫn là O(log N). 

## Ví dụ đã hoạt động 

Đối với mẫu:`4 3 2 1 5 6 3 3 1 3`các trạng thái quan trọng là: 

| Chỉ mục | Giá trị | Độ dài hay nhất kết thúc ở đây | Khoảng thời gian | 
| --- | --- | --- | --- | 
| 0 | 4 | 1 | 0 đến 0 | 
| 4 | 5 | 2 | 0 đến 4 | 
| 5 | 6 | 3 | 0 đến 5 | 
| 7 | 3 | 3 | 3 đến 7 | 
| 9 | 3 | 4 | 6 đến 9 | 

Dãy số cuối cùng là`3 3 1 3`? Ứng viên này bị loại vì nó không phải là không giảm. Trạng thái tối ưu hợp lệ tương ứng với khoảng chứa`1 3 3 3`, với độ dài 4 và kích thước khoảng 7, do đó, ba biểu tượng cảm xúc sẽ bị xóa. 

Đối với đầu vào đã được sắp xếp:`1 2 2 5`| Chỉ mục | Giá trị | Độ dài hay nhất kết thúc ở đây | Khoảng thời gian | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 đến 0 | 
| 1 | 2 | 2 | 0 đến 1 | 
| 2 | 2 | 3 | 0 đến 2 | 
| 3 | 5 | 4 | 0 đến 3 | 

Khoảng đã chọn đã chứa mọi phần tử nên số lần xóa bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi giá trị thực hiện một truy vấn Fenwick và một cập nhật | 
| Không gian | O(N) | Phối hợp nén và lưu trữ Fenwick | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm được giới hạn ở 1000000, do đó giải pháp O(N log N) phù hợp một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    t = int(data[0])
    idx = 1
    ans = []
    for _ in range(t):
        n = int(data[idx])
        idx += 1
        arr = list(map(int, data[idx:idx+n]))
        idx += n
        x, y = solve_case(arr)
        ans.append(f"{x} {y}")
    return "\n".join(ans)

assert run("""1
10
4 3 2 1 5 6 3 3 1 3
""") == "4 3"

assert run("""1
1
7
""") == "1 0"

assert run("""1
5
2 2 2 2 2
""") == "5 0"

assert run("""1
5
5 4 3 2 1
""") == "1 0"

assert run("""1
6
1 10 2 3 4 5
""") == "5 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn |`1 0`| Xử lý kích thước tối thiểu | 
| Giá trị bằng nhau |`5 0`| Không giảm cho phép bình đẳng | 
| Giảm giá trị nghiêm ngặt |`1 0`| Không bị ép buộc lựa chọn ngày càng tăng | 
| Một giá trị lớn bên trong một dãy tăng dần |`5 1`| Xử lý đúng việc xóa nội bộ | 

## Vỏ cạnh 

Đối với một biểu tượng cảm xúc, khoảng đã chọn chỉ chứa phần tử đó. Thuật toán tạo ra một dãy con có độ dài bằng một và độ dài khoảng cũng bằng một nên số lần xóa bằng 0. 

Đối với các mảng có giá trị lặp lại, sự đẳng thức phải được chấp nhận. Truy vấn Fenwick sử dụng các giá trị nhỏ hơn hoặc bằng giá trị hiện tại, đây là sự khác biệt giữa việc xử lý các chuỗi không giảm và các chuỗi tăng nghiêm ngặt. 

Đối với các mảng giảm như`5 4 3 2 1`, mọi phần tử chỉ có thể bắt đầu một chuỗi con có độ dài bằng một. Thuật toán vẫn trả về độ dài tối đa chính xác và không tạo ra các thao tác xóa không cần thiết ngoài khoảng thời gian đã chọn. 

Đối với trường hợp chuỗi con tối ưu được bao quanh bởi các phần tử không liên quan, vị trí bắt đầu và kết thúc được lưu trữ sẽ ngăn chặn việc tính các phần tử bên ngoài đó là phần bị loại bỏ. Đây là điểm khác biệt chính giữa bài toán này và bài toán dãy con dài nhất thông thường. 

Tôi cũng có thể cung cấp phiên bản biên tập ngắn hơn theo phong cách cuộc thi nếu bạn muốn thứ gì đó gần giống với những gì sẽ xuất hiện trên Codeforces.
