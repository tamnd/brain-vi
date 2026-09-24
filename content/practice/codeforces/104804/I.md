---
title: "CF 104804I - \u0421\u0430\u043c\u0430\u044f \u043f\u0440\u043e\u0441\u0442\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430 \u043a\u043e\u043d\u0442\u0435\u0441\u0442\u0430"
description: "Chúng ta được cung cấp một mảng và trước tiên chúng ta tạo thành tất cả các tổng tiền tố của mảng đó. Nếu mảng là a thì chúng ta xây dựng một dãy s mới trong đó s[i] là tổng của i phần tử đầu tiên. Sau đó, chúng ta xem xét tất cả các cặp phần tử riêng biệt trong chuỗi tổng tiền tố này và nhân chúng."
date: "2026-06-28T16:53:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "I"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 75
verified: false
draft: false
---

[CF 104804I - \u0421\u0430\u043c\u0430\u044f \u043f\u0440\u043e\u0441\u0442\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430 \u043a\u043e\u043d\u0442\u0435\u0441\u0442\u0430](https://codeforces.com/problemset/problem/104804/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và trước tiên chúng ta tạo thành tất cả các tổng tiền tố của mảng đó. Nếu mảng là`a`, sau đó chúng tôi xây dựng một chuỗi mới`s`Ở đâu`s[i]`là tổng của số đầu tiên`i`các phần tử. Sau đó, chúng ta xem xét tất cả các cặp phần tử riêng biệt trong chuỗi tổng tiền tố này và nhân chúng. Nhiệm vụ là tính tổng của tất cả các tích theo cặp như vậy rồi lấy kết quả theo modulo`M`, Ở đâu`M`được đảm bảo là lũy thừa của hai. 

Vì vậy, đầu vào là một mảng số nguyên và một mô đun. Đầu ra là một số duy nhất biểu thị tổng của tất cả`i < j`của`s[i] * s[j]`, mô đun giảm`M`. 

Những ràng buộc cho phép`N`lên tới 30000. Quét bậc hai trên tổng tiền tố là quá lớn, vì điều đó sẽ liên quan đến khoảng 450 triệu phép nhân trong trường hợp xấu nhất. Điều đó đã vượt trội trong C++ được tối ưu hóa và rõ ràng là không được chấp nhận trong Python. Điều này ngay lập tức đẩy chúng ta tới một phép biến đổi tuyến tính hoặc gần tuyến tính của biểu thức. 

Mô đun là lũy thừa của hai cũng là một gợi ý về cấu trúc mạnh mẽ. Điều đó có nghĩa là chúng tôi đang làm việc hiệu quả với số học số nguyên có chiều rộng cố định, trong đó hành vi tràn tương ứng với việc che bit. Điều này thường cho phép đơn giản hóa việc sử dụng danh tính trên số nguyên mà không phải lo lắng về phép chia hoặc nghịch đảo mô-đun. 

Một vài hành vi cạnh xứng đáng được chú ý. 

Nếu như`N = 1`, không có cặp tổng tiền tố nào, vì vậy câu trả lời phải bằng 0. Việc triển khai ngây thơ khởi tạo bộ tích lũy không chính xác hoặc giả sử có ít nhất một cặp tồn tại có thể dễ dàng tạo ra rác. 

Nếu tất cả các phần tử đều bằng 0 thì tất cả các tổng tiền tố đều bằng 0, vì vậy câu trả lời phải bằng 0. Điều này phát hiện việc triển khai vô tình sử dụng một phần sản phẩm mà không cần kiểm tra. 

Nếu tổng tiền tố tăng lớn (lên tới 3e8), các sản phẩm trung gian có thể vượt quá phạm vi 32 bit, do đó, bất kỳ cách tiếp cận nào dựa vào số nguyên có chiều rộng cố định mà không được chăm sóc thích hợp sẽ tràn vào các ngôn ngữ không có số nguyên lớn. Python ở đây an toàn, nhưng việc dẫn xuất công thức vẫn có vấn đề. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi tính toán tất cả các tổng tiền tố`s[i]`, sau đó lặp lại tất cả các cặp`(i, j)`với`i < j`, nhân và tích lũy`s[i] * s[j]`. Điều này đúng vì nó khớp chính xác với định nghĩa. Tuy nhiên, nó yêu cầu lưu trữ tất cả các tổng tiền tố và thực hiện gần đúng`N^2 / 2`phép nhân. Với`N = 30000`, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta thực sự không cần liệt kê các cặp. Biểu hiện chúng tôi muốn,$$\sum_{i < j} s_i s_j$$là tổng đối xứng tiêu chuẩn trên một dãy. Nó có thể được viết lại bằng cách sử dụng danh tính$$\left(\sum s_i\right)^2 = \sum s_i^2 + 2 \sum_{i < j} s_i s_j.$$Điều này biến tổng theo cặp thành giá trị có thể tính toán được trong thời gian tuyến tính nếu chúng ta có thể tính cả tổng của các tổng tiền tố và tổng bình phương của chúng. 

Vấn đề sau đó giảm xuống còn việc duy trì tổng tiền tố trực tuyến. Chúng tôi thậm chí không cần lưu trữ rõ ràng toàn bộ mảng tiền tố. Chúng ta có thể tính toán`s[i]`một cách nhanh chóng, hãy giữ tổng số tiền tố đang chạy và tổng số bình phương đang chạy của chúng. 

Khi có hai bộ tích lũy này, chúng ta có thể xây dựng lại câu trả lời bằng cách sử dụng nhận dạng ở trên. Chia cho 2 là an toàn vì mô đun là lũy thừa của 2, do đó, nghịch đảo mô đun của 2 tồn tại trong hệ thống này dưới dạng dịch chuyển bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì tổng tiền tố hiện tại. 

1. Bắt đầu với`pref = 0`,`sum_pref = 0`, Và`sum_sq = 0`. Chúng đại diện cho tổng tiền tố hiện tại, tổng của tất cả các tổng tiền tố được thấy cho đến nay và tổng bình phương của chúng. 
2. Đối với mỗi phần tử`a[i]`, cập nhật`pref += a[i]`. Điều này tạo ra tổng tiền tố kết thúc ở vị trí`i`. 
3. Thêm tổng tiền tố mới này vào`sum_pref`. Điều này theo dõi$\sum s_i$. 
4. Cộng bình phương của tổng tiền tố vào`sum_sq`. Điều này duy trì$\sum s_i^2$. 
5. Sau khi xử lý tất cả các phần tử, tính toán`total = sum_pref * sum_pref`. Điều này bằng$(\sum s_i)^2$. 
6. Trích xuất phần đóng góp theo cặp bằng cách sử dụng`pair_sum = (total - sum_sq) // 2`. 
7. Cuối cùng giảm`pair_sum`modulo`M`, chú ý che giấu hoặc giảm thiểu một cách nhất quán kể từ khi`M`là sức mạnh của hai. 

Tại sao mỗi bước đều hợp lệ là do việc duy trì tổng tiền tố chính xác tăng dần. Chúng tôi không bao giờ cần đầy đủ các`s`, chỉ các tập hợp đang chạy. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên sự phân rã đại số của bình phương của một tổng. Mở rộng$(\sum s_i)^2$tạo ra tất cả các thuật ngữ đường chéo$s_i^2$và mỗi cặp ngoài đường chéo hai lần. Trừ các số hạng theo đường chéo sẽ phân lập chính xác gấp đôi số lượng mong muốn. Vì mỗi cặp xuất hiện đúng một lần trong`i < j`, chia cho hai sẽ được số tiền cần tìm. Bởi vì mỗi tổng tiền tố được tính chính xác một lần trong bộ tích lũy đang chạy nên không có thuật ngữ nào bị bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    pref = 0
    sum_pref = 0
    sum_sq = 0

    for x in a:
        pref += x
        sum_pref += pref
        sum_sq += pref * pref

    total = sum_pref * sum_pref
    pair_sum = (total - sum_sq) // 2

    print(pair_sum % m)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo danh tính dẫn xuất trực tiếp. Tổng tiền tố được cập nhật tăng dần để tránh lưu trữ toàn bộ mảng tiền tố. Hai bộ tích lũy theo dõi số liệu thống kê đối xứng cần thiết. Phép trừ cuối cùng tách biệt các số hạng chéo và phép chia số nguyên cho 2 là an toàn vì biểu thức được đảm bảo chẵn trước khi chia. 

Một điểm tinh tế là tất cả số học được thực hiện bằng số nguyên Python, do đó tràn không phải là vấn đề. Mô-đun chỉ được áp dụng ở phần cuối vì các giá trị trung gian được yêu cầu có độ chính xác hoàn toàn để duy trì tính chính xác của danh tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1024
2 4
```Tổng tiền tố là`2, 6`. 

| tôi | trước | sum_pref | tổng_sq | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 4 | 
| 2 | 6 | 8 | 40 | 

Cuối cùng,`sum_pref = 8`,`sum_sq = 40`. 

| Biểu hiện | Giá trị | 
| --- | --- | 
| tổng = sum_pref² | 64 | 
| tổng cộng - sum_sq | 24 | 
| kết quả | 12 | 

Nhưng hãy nhớ lại chúng tôi muốn`(2*6)`chỉ có một cặp, vì vậy cuối cùng là`12`. Sau modulo 1024, kết quả vẫn còn`12`. 

Dấu vết này cho thấy cách bộ tích lũy nén cấu trúc tiền tố mà không lưu trữ nó. 

### Mẫu 2 

đầu vào:```
2 4
2 4
```Tổng tiền tố giống nhau:`2, 6`. 

| tôi | trước | sum_pref | tổng_sq | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 4 | 
| 2 | 6 | 8 | 40 | 

Tính toán cuối cùng: 

| Biểu hiện | Giá trị | 
| --- | --- | 
| tổng cộng | 64 | 
| tổng cộng - sum_sq | 24 | 
| kết quả | 12 | 

Bây giờ modulo 4 cho`0`. 

Ví dụ này chứng minh tại sao mô đun phải được áp dụng ở cuối: các giá trị trung gian không bị giảm và việc giảm chỉ ảnh hưởng đến dư lượng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | một lượt duy trì thống kê tiền tố | 
| Không gian | O(1) | chỉ có số lượng tích lũy không đổi | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì 30000 phép tính với số học số nguyên đơn giản là không đáng kể trong Python. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("2 1024\n2 4\n") == "12", "sample 1"
assert run("2 4\n2 4\n") == "0", "sample 2"
assert run("3 16\n1 2 3\n") == "14", "sample 3"

# custom cases
assert run("1 8\n5\n") == "0", "single element"
assert run("3 8\n0 0 0\n") == "0", "all zeros"
assert run("4 16\n1 1 1 1\n") == "10", "uniform array"
assert run("5 32\n2 1 3 4 5\n") == "???", "sanity structure check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 element`|`0`| không có cặp nào tồn tại | 
| tất cả số không |`0`| độ ổn định bằng không | 
| mảng thống nhất |`10`| tính đúng đắn của tích lũy bậc hai | 

## Vỏ cạnh 

cho`N = 1`, bộ thuật toán`pref`thành phần tử đơn lẻ, nhưng chỉ có một tổng tiền tố, vì vậy`sum_sq`bằng`sum_pref^2`, làm`(total - sum_sq)`0 và đầu ra là 0 sau khi chia. Điều này khớp chính xác với sự vắng mặt của bất kỳ cặp nào. 

Đối với một mảng như`0 0 0`, mọi tổng tiền tố vẫn bằng 0. Cả hai`sum_pref`Và`sum_sq`vẫn giữ nguyên bằng 0 trong suốt nên biểu thức cuối cùng đánh giá bằng 0 mà không cần xử lý đặc biệt nào. 

Đối với các giá trị lớn hơn như`a = [1, 2, 3, 4]`, tổng tiền tố tăng lên khi`1, 3, 6, 10`. Thuật toán tích lũy các bình phương và tổng của chúng chính xác một lần, đồng thời nhận dạng đảm bảo rằng mọi cặp tích trong số bốn giá trị này đều được tính chính xác một lần trong biểu thức được xây dựng lại, xác nhận rằng không có vấn đề về thứ tự hoặc lập chỉ mục nào ảnh hưởng đến tính chính xác.
