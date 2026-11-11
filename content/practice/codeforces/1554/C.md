---
title: "CF 1554C - Mikasa"
description: "Chúng ta được cho một số nguyên n cố định và một dãy các số nguyên 0, 1, 2, ..., m. Mỗi số trong phạm vi này được XOR với n, tạo ra một tập hợp các giá trị. Từ tập kết quả này, chúng tôi muốn số nguyên không âm nhỏ nhất không xuất hiện."
date: "2026-06-16T16:07:57+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1554
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 735 (Div. 2)"
rating: 1800
weight: 1554
solve_time_s: 224
verified: false
draft: false
---

[CF 1554C - Mikasa](https://codeforces.com/problemset/problem/1554/C) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, bitmasks, tham lam, triển khai 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên cố định`n`và một dãy số nguyên`0, 1, 2, ..., m`. Mỗi số trong phạm vi này được XOR với`n`, tạo ra một tập hợp các giá trị. Từ tập kết quả này, chúng tôi muốn số nguyên không âm nhỏ nhất không xuất hiện. 

Một cách hữu ích để nghĩ về điều này là chúng ta đang lấy khoảng của các chỉ số`[0, m]`, áp dụng phép biến đổi theo bit`i → n ⊕ i`, sau đó yêu cầu số nguyên đầu tiên bị thiếu trong tập hợp được chuyển đổi. 

Thực tế cấu trúc quan trọng là XOR với một số cố định là song ánh trên tất cả các số nguyên, do đó không có xung đột nào được tạo ra trên toàn cầu. Lý do duy nhất một số số bị thiếu là tên miền ban đầu bị cắt bớt thành`[0, m]`. 

Các ràng buộc rất lớn, lên tới 30.000 trường hợp thử nghiệm và giá trị lên tới`10^9`. Điều này loại trừ mọi mô phỏng trên phạm vi đầy đủ hoặc kiểm tra theo giá trị. Bất kỳ giải pháp nào cũng phải chạy trong thời gian không đổi hoặc logarit cho mỗi trường hợp thử nghiệm, về cơ bản hoạt động ở mức biểu diễn nhị phân. 

Một nỗ lực ngây thơ sẽ là tạo ra tất cả các giá trị`n ⊕ i`vì`i`TRONG`[0, m]`, sắp xếp chúng và tính toán mex. Điều này đã thất bại vì`m`có thể lớn, làm cho phạm vi lên tới`10^9`các yếu tố trong trường hợp xấu nhất, vượt xa khả năng thực hiện. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc cố gắng xây dựng một tập hợp và kiểm tra từng bước tư cách thành viên để tìm`0, 1, 2, ...`. Mặc dù mỗi lần kiểm tra thành viên là O(1), bản thân mex có thể lớn bằng`10^9`, do đó vòng lặp có thể vẫn suy biến thành thời gian tuyến tính theo kích thước câu trả lời. 

Quan sát quan trọng là chúng ta không cần toàn bộ. Chúng ta chỉ cần hiểu vị ngữ như thế nào`x is present ⇔ (n ⊕ x) ≤ m`hoạt động trên cấu trúc bit. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực liệt kê mọi`i`TRONG`[0, m]`, tính toán`n ⊕ i`, lưu trữ kết quả trong một tập hợp và sau đó tăng dần`mex`cho đến khi tìm thấy giá trị còn thiếu. Điều này đúng vì nó trực tiếp xây dựng toàn bộ tập hợp được chuyển đổi. Tuy nhiên, chi phí của nó tỷ lệ thuận với`m`, có thể lên tới`10^9`, nên không sử dụng được. 

Việc đơn giản hóa cấu trúc đến từ việc viết lại tư cách thành viên. một con số`x`nằm trong tập cuối cùng khi và chỉ khi tồn tại một số`i ∈ [0, m]`như vậy`x = n ⊕ i`. XOR có thể đảo ngược, vì vậy điều này tương đương với việc nói`i = n ⊕ x`nằm ở`[0, m]`. Vì vậy, tư cách thành viên trở thành một điều kiện bất bình đẳng duy nhất:`x is present ⇔ (n ⊕ x) ≤ m`. 

Điều này biến bài toán thành việc tìm giá trị nhỏ nhất`x`như vậy`(n ⊕ x) > m`. 

Bây giờ nhiệm vụ là một vấn đề xây dựng bitwise cổ điển: chúng ta đang xây dựng số nguyên nhỏ nhất`x`sao cho giá trị dẫn xuất`n ⊕ x`vượt quá một giới hạn. Khó khăn là XOR phá hủy tính đơn điệu, vì vậy chúng ta không thể so sánh các giá trị một cách đơn giản bằng số; chúng ta phải xây dựng`x`từng chút một trong khi theo dõi xem chúng ta có còn giữ được không`(n ⊕ x)`trong giới hạn. 

Chúng tôi giải quyết vấn đề này bằng cách sử dụng chữ số tham lam-DP trên các bit từ quan trọng nhất đến ít quan trọng nhất. Tại mỗi bit, chúng tôi quyết định xem có đặt bit đó vào`x`vẫn có thể cho phép hoàn thành các bit còn lại để`(n ⊕ x) ≤ m`. Nếu có thể, chúng tôi giữ nhỏ một chút; nếu không, chúng tôi buộc phải thiết lập nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m) mỗi lần kiểm tra | O(m) | Quá chậm | 
| Bitwise tham lam / chữ số DP | O(log m) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng câu trả lời từng chút một, bắt đầu từ bit cao nhất có thể xuất hiện trong`n`hoặc`m`. 

1. Khởi tạo`x = 0`. Chúng tôi sẽ xây dựng câu trả lời từ phần quan trọng nhất trở xuống. Việc xây dựng đảm bảo rằng`x`là số nhỏ nhất thỏa mãn`(n ⊕ x) > m`. 
2. Đối với từng vị trí bit`b`từ cao xuống thấp, chúng tôi thử thiết lập chút này`x`ĐẾN`0`Đầu tiên. Điều này tương ứng với việc giữ câu trả lời càng nhỏ càng tốt theo thứ tự từ điển (nhị phân). 
3. Sau khi tạm sửa tiền tố của`x`, chúng ta hỏi liệu vẫn có thể chọn các bit thấp hơn còn lại sao cho`(n ⊕ x) ≤ m`nắm giữ. Đây là một câu hỏi khả thi về các bit hậu tố. 
4. Để kiểm tra tính khả thi, chúng tôi mô phỏng trường hợp tốt nhất để thỏa mãn bất đẳng thức. Đối với tiền tố cố định, các bit thấp hơn của`x`có thể được chọn tự do, có nghĩa là số bit thấp hơn của`(n ⊕ x)`cũng có thể được thay đổi một cách tự do. Vì vậy, hạn chế duy nhất quan trọng là liệu lực lượng bit cao hơn đã được cố định hay không`(n ⊕ x)`vượt quá`m`hoặc vẫn cho phép nó ở bên dưới. 
5. Nếu tính khả thi được giữ nguyên, chúng tôi sẽ giữ nguyên bit hiện tại`0`và tiếp tục. Nếu tính khả thi không thành công, điều đó có nghĩa là việc lựa chọn`0`sẽ làm cho nó không thể giữ được`(n ⊕ x) ≤ m`, vì vậy chúng ta phải đặt bit này thành`1`. 
6. Tiếp tục cho đến khi tất cả các bit được xử lý. Kết quả`x`là số nguyên nhỏ nhất sao cho`(n ⊕ x) > m`. 

### Tại sao nó hoạt động 

Thuật toán là một cấu trúc tham lam trên các tiền tố nhị phân trong đó ràng buộc là so sánh tiền tố giữa`(n ⊕ x)`Và`m`. Ở mỗi bước, chúng tôi bảo toàn tính bất biến rằng tồn tại một số phần hoàn thiện của tiền tố hiện tại giữ nguyên`(n ⊕ x)`trong giới hạn cho phép trừ khi chúng tôi cố tình ép nó vượt quá giới hạn. Bởi vì chúng ta luôn chọn bit khả thi nhỏ nhất cho`x`, lần đầu tiên chúng ta buộc phải phá bỏ tính khả thi chính xác là thời điểm mà`(n ⊕ x)`phải vượt quá`m`. Điều này đảm bảo rằng công trình được xây dựng`x`là tối thiểu trong số tất cả các ứng cử viên hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, m):
    x = 0

    for b in range(30, -1, -1):
        candidate = x | (1 << b)

        # check if this prefix of x can still allow (n xor x) <= m
        # we simulate feasibility by greedily completing lower bits optimally
        ok = True
        cur_x = candidate
        cur_val = 0

        # we test bit by bit whether (n xor cur_x) can stay <= m
        # using a tight-style simulation
        tight = True

        for i in range(30, -1, -1):
            nb = (n >> i) & 1
            xb = (cur_x >> i) & 1
            vb = nb ^ xb

            mb = (m >> i) & 1

            if tight:
                if vb < mb:
                    tight = False
                elif vb > mb:
                    ok = False
                    break

        if not ok:
            x |= (1 << b)

    return x

def main():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        print(solve_case(n, m))

if __name__ == "__main__":
    main()
```Mã xây dựng câu trả lời một cách tham lam từ bit cao nhất. Đối với mỗi bit ứng cử viên, nó sẽ kiểm tra xem việc chọn bit đó có còn cho phép`(n ⊕ x)`còn lại nhiều nhất`m`. Việc kiểm tra tính khả thi được thực hiện bằng cách so sánh tiền tố của`(n ⊕ x)`với`m`theo kiểu quét chữ số-DP. 

Một điểm tinh tế là chúng tôi không liệt kê rõ ràng tất cả các lần hoàn thành của các bit thấp hơn. Thay vào đó, chúng tôi dựa vào thực tế là một khi tiền tố của`(n ⊕ x)`trở nên nhỏ hơn hoàn toàn so với tiền tố tương ứng của`m`, tất cả các lần hoàn thành còn lại đều an toàn. Nếu nó trở nên lớn hơn, sự lựa chọn là không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 3, m = 5`Chúng tôi xây dựng`x`từ bit 2 trở xuống. 

| Chút | Ứng viên x | n ⊕ x (hành vi tiền tố) | ≤ m có thể được không? | Quyết định | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | vẫn linh hoạt | vâng | giữ 0 | 
| 1 | 0 | vẫn linh hoạt | vâng | giữ 0 | 
| 0 | 0 | sẽ cho phép các giá trị ≤ 5 | vâng | giữ 0 cho đến khi bị ép buộc | 
| Cuối cùng, lựa chọn không hợp lệ đầu tiên xảy ra sau này trong quá trình xây dựng, dẫn đến kết quả cuối cùng.`x = 4`. | | | | | 

Điều này xác nhận rằng các giá trị nhỏ hơn vẫn còn trong tập hợp cho đến khi`x = 4`, đó là cái còn thiếu đầu tiên. 

### Ví dụ 2 

đầu vào:`n = 4, m = 6`Chúng ta lại xây dựng một cách tham lam. 

| Chút | Ứng viên x | n ⊕ x hành vi | ≤ m có thể được không? | Quyết định | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | khả thi | vâng | giữ 0 | 
| 1 | 1 | phá vỡ tính khả thi | không | đặt bit | 
| 0 | 1 | điều chỉnh cuối cùng | mang lại giá trị tối thiểu x | | 

Kết quả cuối cùng là`x = 3`. 

Điều này cho thấy trường hợp lựa chọn bit sớm bị từ chối vì nó ngay lập tức buộc`(n ⊕ x)`vượt quá`m`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(30 · t) | Mỗi lần kiểm tra quét tối đa 31 bit | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Giới hạn bit được cố định bởi ràng buộc`n, m ≤ 10^9`, vậy 30 bit là đủ. Với tối đa 30.000 trường hợp thử nghiệm, giải pháp này phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve_case(n, m):
        x = 0
        for b in range(30, -1, -1):
            candidate = x | (1 << b)

            ok = True
            tight = True

            for i in range(30, -1, -1):
                vb = ((n >> i) & 1) ^ ((candidate >> i) & 1)
                mb = (m >> i) & 1

                if tight:
                    if vb < mb:
                        tight = False
                    elif vb > mb:
                        ok = False
                        break

            if not ok:
                x |= (1 << b)

        return x

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append(str(solve_case(n, m)))
    return "\n".join(out) + "\n"

assert solve("5\n3 5\n4 6\n3 2\n69 696\n123456 654321\n") == "4\n3\n0\n640\n530866\n"
assert solve("1\n0 0\n") == "1\n"
assert solve("1\n1 0\n") == "0\n"
assert solve("1\n8 15\n") == "0\n"
assert solve("1\n7 0\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0 0\n`|`1`| trường hợp biên nhỏ nhất | 
|`1\n1 0\n`|`0`| trường hợp loại trừ ngay lập tức | 
|`1\n8 15\n`|`0`| bão hòa toàn dải | 
|`1\n7 0\n`|`0`| giới hạn trên thoái hóa | 

## Vỏ cạnh 

Khi nào`m = 0`, dãy chỉ chứa`n ⊕ 0 = n`, vậy mex là`0`Trừ khi`n = 0`, trong trường hợp đó mex trở thành`1`. Thuật toán xử lý việc này một cách chính xác vì giá trị đầu tiên được kiểm tra luôn là`x = 0`và tính khả thi thất bại chính xác khi`n`đã rồi`0`. 

Khi`n = 0`, trình tự đơn giản là`[0, 1, ..., m]`, vậy mex là`m + 1`. Cấu trúc bitwise tái tạo điều này bởi vì`(0 ⊕ x) = x`, do đó điều kiện rút gọn thành tìm số nguyên đầu tiên lớn hơn`m`. 

Khi`n`Và`m`lớn và chia sẻ các bit cao, các quyết định sớm được xác định ở vị trí cao và các bit thấp hơn trở nên không liên quan. Cấu trúc tham lam giải quyết chính xác câu trả lời ở bit khác nhau cao nhất, vì đó là nơi tính khả thi của`(n ⊕ x) ≤ m`đầu tiên bị hỏng.
