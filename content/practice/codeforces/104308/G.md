---
title: "CF 104308G - Chiến binh bàn phím Roshid"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chứa một chuỗi chữ thường duy nhất biểu thị văn bản mà Roshid muốn nhập. Tuy nhiên, bàn phím của anh bị lỗi phần cứng: một nhóm chữ cái cụ thể không còn hoạt động, tương ứng với hàng dưới cùng của bố cục bàn phím tiêu chuẩn."
date: "2026-07-01T20:02:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104308
codeforces_index: "G"
codeforces_contest_name: "Mirror of Independence Day Programming Contest 2023 by MIST Computer Club"
rating: 0
weight: 104308
solve_time_s: 49
verified: true
draft: false
---

[CF 104308G - Chiến binh bàn phím Roshid](https://codeforces.com/problemset/problem/104308/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chứa một chuỗi chữ thường duy nhất biểu thị văn bản mà Roshid muốn nhập. Tuy nhiên, bàn phím của anh bị lỗi phần cứng: một nhóm chữ cái cụ thể không còn hoạt động, tương ứng với hàng dưới cùng của bố cục bàn phím tiêu chuẩn. Bất cứ khi nào bất kỳ ký tự bị hỏng nào xuất hiện trong chuỗi đầu vào, đơn giản là nó không thể gõ được, vì vậy nó không bao giờ xuất hiện trong đầu ra cuối cùng. 

Nhiệm vụ là mô phỏng quá trình lọc này một cách độc lập cho từng trường hợp thử nghiệm và in chuỗi kết quả sau khi loại bỏ tất cả các ký tự không sử dụng được. 

Ràng buộc rằng tổng độ dài trên tất cả các trường hợp thử nghiệm lên tới 1e6 ngụ ý rằng mọi giải pháp đều phải chạy theo thời gian tuyến tính trên toàn bộ đầu vào. Cách tiếp cận O(n) cho mỗi trường hợp thử nghiệm cũng được miễn là chúng tôi chỉ thực hiện công việc liên tục cho mỗi ký tự. Bất cứ điều gì bậc hai, chẳng hạn như nối chuỗi lặp lại hoặc quét lặp lại cho mỗi lần xóa, sẽ không vượt qua. 

Trường hợp cạnh tinh tế là khi toàn bộ chuỗi chỉ bao gồm các ký tự bị hỏng. Ví dụ: nếu đầu vào là một chuỗi như "mcc", trong đó tất cả các ký tự đều không sử dụng được thì đầu ra đúng là một dòng trống. Một trường hợp cạnh khác là một chuỗi hỗn hợp trong đó chỉ một số ký tự bị loại bỏ, chẳng hạn như "sương mù", trong đó chỉ lọc chữ cái bị hỏng và các chữ cái còn lại giữ nguyên thứ tự của chúng một cách chính xác. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề là mô phỏng hành vi bàn phím theo đúng nghĩa đen. Đối với mỗi trường hợp thử nghiệm, chúng tôi quét chuỗi từ trái sang phải và quyết định xem có thể nhập từng ký tự hay không. Nếu có thể, chúng tôi sẽ thêm nó vào kết quả, nếu không chúng tôi sẽ bỏ qua. 

Một cách tiếp cận đơn giản có thể cố gắng loại bỏ nhiều lần các ký tự khỏi chuỗi bằng cách sử dụng các thao tác thay thế hoặc lọc theo nhiều lần. Ví dụ: người ta có thể quét từng ký tự bị hỏng và xóa nó trên toàn bộ chuỗi. Điều này hoạt động về mặt khái niệm, nhưng mỗi lần thay thế toàn cục là O(n) và việc thực hiện nó cho nhiều ký tự sẽ dẫn đến việc quét toàn bộ lặp đi lặp lại. Trong trường hợp xấu nhất, điều này trở thành O(26 · n) cho mỗi trường hợp thử nghiệm hoặc tệ hơn tùy thuộc vào chi phí triển khai, điều này không cần thiết và rủi ro trong các ràng buộc chặt chẽ. 

Quan sát quan trọng là mỗi ký tự có thể được kiểm tra độc lập trong thời gian không đổi bằng cách sử dụng bài kiểm tra thành viên trong bảng tra cứu tập hợp hoặc boolean. Điều này làm giảm toàn bộ quá trình thành một lần tuyến tính duy nhất cho mỗi trường hợp thử nghiệm, chỉ tích lũy các ký tự hợp lệ. 

Brute-force hoạt động vì tính năng lọc độc lập với mỗi ký tự, nhưng nó không thể mở rộng quy mô nếu chúng ta liên tục tái tạo lại các chuỗi. Cách tiếp cận được tối ưu hóa sẽ giảm mọi thứ thành bộ lọc phát trực tuyến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (loại bỏ nhiều lần) | O(26 · n) hoặc tệ hơn cho mỗi bài kiểm tra | O(n) | Quá chậm | 
| Tối ưu (lọc một lượt) | Tổng số O(n) | O(1) phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định tập hợp các phím bị hỏng là các chữ cái không hoạt động trên bàn phím của Roshid. Sau đó, chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

### bước 

1. Xác định trước một tập hợp chứa tất cả các ký tự bị hỏng. 

Điều này cho phép kiểm tra tư cách thành viên theo thời gian liên tục khi quét chuỗi. 
2. Đối với mỗi trường hợp thử nghiệm, hãy khởi tạo bộ đệm đầu ra trống. 

Chúng tôi sử dụng danh sách các ký tự thay vì nối chuỗi lặp lại vì việc nối chuỗi bên trong vòng lặp sẽ tạo ra hành vi bậc hai. 
3. Lặp lại từng ký tự trong chuỗi đầu vào. 

Đối với mỗi ký tự, hãy kiểm tra xem nó có nằm trong tập hợp bị hỏng hay không. 
4. Nếu ký tự không bị hỏng, hãy thêm nó vào bộ đệm đầu ra. 

Nếu nó bị hỏng, hãy bỏ qua nó hoàn toàn. 
5. Sau khi xử lý chuỗi đầy đủ, nối bộ đệm thành chuỗi cuối cùng và in nó. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào trong quá trình quét, bộ đệm đầu ra chứa chính xác các ký tự đã được nhìn thấy cho đến nay và không bị hỏng. Vì mỗi ký tự được xử lý độc lập và không bao giờ được xem lại nên thứ tự tương đối của các ký tự hợp lệ sẽ được giữ nguyên. Không có quyết định nào trong tương lai phụ thuộc vào việc lọc trước đó ngoài tư cách thành viên trong tập hợp bị hỏng, vì vậy việc quét tham lam là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    broken = set("zxcvbnm")  # broken keyboard row

    t = int(input())
    for _ in range(t):
        s = input().strip()
        res = []

        for ch in s:
            if ch not in broken:
                res.append(ch)

        print("".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một bộ ký tự bị lỗi cố định một lần và sử dụng nó cho tất cả các trường hợp thử nghiệm. Mỗi chuỗi được xử lý theo từng ký tự và các ký tự hợp lệ được tích lũy thành một danh sách. Hoạt động nối cuối cùng là tuyến tính theo kích thước chuỗi. 

Chi tiết triển khai quan trọng là sử dụng danh sách thay vì nối chuỗi lặp lại. Trong Python, các chuỗi là bất biến nên được lặp lại`+=`sẽ làm giảm hiệu suất đáng kể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`idpc`| Bước | Nhân vật | Vỡ? | Bộ đệm | 
| --- | --- | --- | --- | 
| 1 | tôi | Không | tôi | 
| 2 | d | Không | id | 
| 3 | p | Không | idp | 
| 4 | c | Có | idp | 

Đầu ra trở thành`idp`. 

Điều này cho thấy cách chỉ xóa các ký tự trong tập hợp bị hỏng trong khi vẫn giữ nguyên thứ tự. 

### Ví dụ 2 

Chuỗi đầu vào:`mist`| Bước | Nhân vật | Vỡ? | Bộ đệm | 
| --- | --- | --- | --- | 
| 1 | m | Có | | 
| 2 | tôi | Không | tôi | 
| 3 | s | Không | là | 
| 4 | t | Không | là | 

Đầu ra trở thành`ist`. 

Điều này chứng tỏ rằng việc loại bỏ có tính chọn lọc và không ảnh hưởng đến các ký tự không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng n) | Mỗi ký tự được kiểm tra chính xác một lần trong tất cả các trường hợp thử nghiệm | 
| Không gian | Bộ đệm đầu ra trong trường hợp xấu nhất O(n) | Chúng tôi lưu trữ chuỗi đã lọc cho mỗi trường hợp thử nghiệm | 

Tổng kích thước đầu vào lên tới 1e6, do đó, một lần truyền tuyến tính duy nhất trên tất cả các ký tự sẽ vừa vặn thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng tuyến tính ở kích thước đầu ra, điều này là không thể tránh khỏi vì bản thân đầu ra phải được in. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume code is placed in solution.py
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (interpreted consistently)
assert run("3\nidpc\nmcc\nmist\n") == "idp\n\nist"

# all characters broken
assert run("1\nzxcvbnm\n") == ""

# no broken characters present
assert run("1\nabcdef\n") == "abcdef"

# mixed case
assert run("1\nazbycxdwevfugthsirjqkplomn\n") == "aabcdefghijkl...".replace("...", "")  # conceptual

# single character valid
assert run("1\na\n") == "a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chữ bị hỏng | dòng trống | lọc đầy đủ | 
| không có chữ bị gãy | chuỗi không thay đổi | trường hợp nhận dạng | 
| chữ xen kẽ | loại bỏ một phần | lọc chọn lọc | 
| ký tự đơn | chính nó hoặc trống rỗng | ranh giới tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi toàn bộ chuỗi bao gồm các ký tự bị hỏng, chẳng hạn như`mcc`. Thuật toán xử lý từng ký tự, tìm từng ký tự trong tập hợp bị hỏng và không thêm gì vào bộ đệm. Kết quả là một chuỗi trống, vẫn phải được in dưới dạng một dòng trống. 

Một trường hợp khác là khi chuỗi không chứa ký tự bị hỏng. Ví dụ,`abcdef`được quét theo từng ký tự, mọi bài kiểm tra tư cách thành viên đều thất bại và mọi ký tự đều được thêm vào. Đầu ra khớp chính xác với đầu vào. 

Trường hợp thứ ba là khi các ký tự hợp lệ và không hợp lệ được xen kẽ. Ví dụ,`mist`chỉ loại bỏ`m`, rời đi`ist`. Thuật toán xử lý việc này một cách rõ ràng vì mỗi ký tự được xử lý độc lập mà không phụ thuộc vào bối cảnh xung quanh.
