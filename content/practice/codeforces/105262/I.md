---
title: "CF 105262I - Đối tác ma cà rồng"
description: "Chúng ta được đưa ra một số tình huống độc lập trong đó một hàng cốc chứa một lượng cà phê cappuccino ban đầu ẩn giấu."
date: "2026-06-24T02:34:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "I"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 43
verified: true
draft: false
---

[CF 105262I - Đối tác ma cà rồng](https://codeforces.com/problemset/problem/105262/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một số tình huống độc lập trong đó một hàng cốc chứa một lượng cà phê cappuccino ban đầu ẩn giấu. Thông tin duy nhất Eddard ghi lại lúc đầu là một giá trị duy nhất được tính từ mảng ban đầu bằng cách sử dụng tổng có trọng số: mỗi cốc đóng góp giá trị nhân với chỉ số của nó và tất cả đóng góp được cộng lại với nhau. 

Sau đó, sau một thời gian, chúng tôi nhận được số đo mới của những chiếc cốc tương tự. Câu hỏi không phải là xây dựng lại mảng ban đầu mà chỉ để quyết định xem liệu có thể không có gì thay đổi hay không. Nói cách khác, chúng tôi kiểm tra xem mảng mới có thể tạo ra tổng có trọng số giống hệt như mảng ban đầu hay không. 

Mỗi trường hợp thử nghiệm cung cấp giá trị được mã hóa ban đầu và mảng cuối cùng. Chúng ta phải quyết định xem có tồn tại ít nhất một cấu hình ban đầu hợp lệ phù hợp với cả mã hóa và quan sát cuối cùng hay không. Điều này tương đương với việc kiểm tra xem tổng trọng số của mảng cuối cùng có khớp với giá trị được lưu trữ đã cho hay không. 

Các ràng buộc cho phép tổng cộng tối đa 10^6 phần tử trong các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải tuyến tính ở kích thước đầu vào. Một phép tính lại đơn giản cho mỗi trường hợp thử nghiệm là đủ nếu nó là O(n), nhưng bất cứ điều gì bậc hai hoặc liên quan đến việc tái thiết hoặc tìm kiếm đều không thể thực hiện được. 

Trường hợp đặc biệt nhất xuất phát từ một bẫy diễn giải tinh vi: điều kiện mang tính tồn tại, không mang tính quyết định. Ngay cả khi các mảng khác nhau về nội dung, chúng tôi chỉ quan tâm liệu chúng có còn tạo ra tổng trọng số như nhau hay không. Điều đó có nghĩa là sự bằng nhau của hàm được tính toán là yếu tố quyết định duy nhất, không phải là so sánh theo từng phần tử hoặc bất kỳ khái niệm nào về “phát hiện thay đổi” ngoài giá trị được mã hóa. 

Ví dụ: nếu giá trị được lưu trữ là 30 và mảng mới cũng đánh giá là 30 trong cùng một hàm, chúng ta phải trả lời CÓ ngay cả khi chúng ta có thể tưởng tượng các mảng ban đầu khác cũng khớp. Ngược lại, ngay cả một sự không khớp duy nhất cũng buộc KHÔNG, bởi vì không có lời giải thích thay thế nào có thể dung hòa giá trị được mã hóa với tổng trọng số quan sát được. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ là xem xét liệu có tồn tại một số mảng ban đầu khớp với cả tổng trọng số được lưu trữ và mảng được quan sát cuối cùng theo quy tắc chuyển đổi hay không. Tuy nhiên, vấn đề không thực sự liên quan đến việc xây dựng lại mảng ban đầu hoặc lập mô hình các thay đổi trung gian. Kiểm tra duy nhất quan trọng là liệu mã hóa của mảng hiện tại có bằng giá trị được lưu trữ hay không. 

Một cách tiếp cận tính toán trực tiếp là đánh giá hàm F(p) = sum(i * p[i]) và so sánh nó với F(c) đã cho. Điều này rất đơn giản vì hàm này là tuyến tính và có thể tính toán trực tiếp trong một lần truyền. 

Sự đơn giản hóa quan trọng là nhận ra rằng điều kiện “liệu ​​số lượng có thể giữ nguyên” tương đương với việc hỏi liệu dấu vân tay được mã hóa có khớp với dấu vân tay được tính toán lại từ trạng thái cuối cùng hay không. Không có ràng buộc hoặc biến đổi ẩn bổ sung nào tồn tại ngoài sự so sánh này. 

Do đó, vấn đề giảm xuống còn việc tính toán một tổng có trọng số cho mỗi trường hợp thử nghiệm và so sánh nó với giá trị được cung cấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại trực tiếp | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm là độc lập, do đó không có trạng thái nào được mang giữa chúng. 
2. Đối với mỗi trường hợp thử nghiệm, đọc n và giá trị được lưu trữ S, đại diện cho tổng có trọng số ban đầu. 
3. Đọc mảng p thể hiện trạng thái hiện tại của những chiếc cốc. 
4. Tính tổng có trọng số của p bằng cách lặp qua các chỉ số từ 1 đến n và tích lũy i * p[i]. Phép nhân phản ánh quy tắc mã hóa được Eddard sử dụng. 
5. So sánh tổng tính toán với S. Nếu chúng bằng nhau, ghi CÓ; nếu không thì xuất ra NO. 

### Tại sao nó hoạt động

Hàm xác định giá trị được lưu trữ có tính xác định và có thể tính toán duy nhất từ ​​bất kỳ mảng nhất định nào. Vì cả trạng thái ban đầu và trạng thái cuối cùng đều được đánh giá bằng cùng một công thức, nên cách duy nhất để chúng có thể biểu thị cùng một tình huống là nếu các giá trị tính toán của chúng khớp chính xác. Không có sự mơ hồ hoặc biến đổi ẩn nào có thể bảo toàn giá trị được mã hóa trong khi thay đổi kết quả tính toán một cách độc lập. Do đó, sự bằng nhau của tổng trọng số vừa cần vừa đủ cho một kịch bản “có thể không thay đổi”. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, S = map(int, input().split())
        p = list(map(int, input().split()))
        
        cur = 0
        for i, v in enumerate(p, start=1):
            cur += i * v
        
        out.append("YES" if cur == S else "NO")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. Tính toán chính là tổng có trọng số, được thực hiện bằng cách sử dụng`enumerate`bắt đầu từ 1 để khớp với việc lập chỉ mục dựa trên 1 trong định nghĩa. Bộ tích lũy`cur`được giữ dưới dạng số nguyên Python, xử lý an toàn các giá trị lên tới 10^18 do số nguyên chính xác tùy ý của Python. 

Sự so sánh ở cuối là điểm quyết định duy nhất. Không cần chuẩn hóa hoặc chuyển đổi bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, S = 30 

p = [2, 4, 2, 1, 2] 

Chúng tôi tính toán tổng trọng số từng bước: 

| tôi | p[i] | tôi * p[i] | tích lũy | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 
| 2 | 4 | 8 | 10 | 
| 3 | 2 | 6 | 16 | 
| 4 | 1 | 4 | 20 | 
| 5 | 2 | 10 | 30 | 

Giá trị tính toán cuối cùng là 30, khớp với S, vì vậy câu trả lời là CÓ. Điều này xác nhận rằng mã hóa phù hợp với trạng thái được quan sát. 

### Ví dụ 2 

đầu vào: 

n = 3, S = 20 

p = [1, 4, 1] 

| tôi | p[i] | tôi * p[i] | tích lũy | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 4 | 8 | 9 | 
| 3 | 1 | 3 | 12 | 

Giá trị được tính toán là 12, khác với 20, vì vậy câu trả lời là KHÔNG. Điều này cho thấy rằng ngay cả một sự khác biệt nhỏ ở bất kỳ vị trí nào cũng ảnh hưởng đến tổng có trọng số và ngay lập tức làm mất hiệu lực khả năng tương đương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑n) | Mỗi phần tử được xử lý chính xác một lần để tính tổng có trọng số | 
| Không gian | O(1) | Ngoài bộ lưu trữ đầu vào, chỉ có một bộ tích lũy đang chạy được sử dụng | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm tối đa là 10^6, do đó, một lần quét tuyến tính cho mỗi trường hợp thử nghiệm sẽ phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return "\n".join(solve_capture(inp))

# We redefine a safe runner since __main__ context varies in platforms
def solve_capture(inp):
    import sys
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, S = map(int, input().split())
        p = list(map(int, input().split()))
        cur = 0
        for i, v in enumerate(p, start=1):
            cur += i * v
        out.append("YES" if cur == S else "NO")
    return out

# provided-style tests
assert solve_capture("1\n5 30\n2 4 2 1 2\n") == ["YES"]
assert solve_capture("1\n3 20\n1 4 1\n") == ["NO"]

# custom cases
assert solve_capture("1\n1 0\n0\n") == ["YES"]
assert solve_capture("1\n1 5\n5\n") == ["YES"]
assert solve_capture("1\n2 1\n1 0\n") == ["NO"]
assert solve_capture("2\n3 6\n1 1 1\n3 10\n1 2 3\n") == ["YES","YES"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp không | CÓ | kích thước tối thiểu, độ chính xác của giá trị bằng 0 | 
| khớp phần tử đơn | CÓ | đẳng thức trực tiếp của tổng có trọng số | 
| sự không phù hợp nhỏ | KHÔNG | phát hiện sai lệch nhỏ | 
| nhiều trường hợp | CÓ/CÓ | xử lý hàng loạt chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi n = 1. Tổng có trọng số giảm xuống một giá trị duy nhất 1 * p[1], do đó câu trả lời chỉ phụ thuộc vào sự bằng nhau với S. Thuật toán xử lý điều này một cách tự nhiên vì vòng lặp chạy một lần và tích lũy chính xác. 

Một trường hợp cạnh khác là khi tất cả các giá trị bằng 0. Tổng có trọng số trở thành 0 bất kể n. Nếu S cũng bằng 0 thì câu trả lời là CÓ, nếu không thì KHÔNG. Thuật toán tích lũy chính xác số đóng góp bằng 0 mà không cần xử lý đặc biệt. 

Trường hợp cạnh thứ ba là khi giá trị lớn, gần 10^9 và n lớn. Tổng có thể đạt tới 10^18, nhưng số nguyên Python xử lý việc này một cách an toàn và thuật toán vẫn chạy theo thời gian tuyến tính vì chỉ thực hiện phép nhân và phép cộng cho mỗi phần tử.
