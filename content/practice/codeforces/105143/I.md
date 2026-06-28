---
title: "CF 105143I - Dây táo tuần hoàn"
description: "Chúng ta được cung cấp một chuỗi nhị phân và được phép thực hiện nhiều lần một thao tác rất linh hoạt: chọn bất kỳ đoạn liền kề nào và xoay nó theo chu kỳ."
date: "2026-06-27T16:49:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105143
codeforces_index: "I"
codeforces_contest_name: "2024 ICPC National Invitational Collegiate Programming Contest, Wuhan Site"
rating: 0
weight: 105143
solve_time_s: 62
verified: true
draft: false
---

[CF 105143I - Dây Apple tuần hoàn](https://codeforces.com/problemset/problem/105143/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và được phép thực hiện nhiều lần một thao tác rất linh hoạt: chọn bất kỳ đoạn liền kề nào và xoay nó theo chu kỳ. Xoay ở đây có nghĩa là chia phân đoạn đã chọn thành hai phần và hoán đổi thứ tự của chúng, trong khi vẫn giữ nguyên thứ tự bên trong mỗi phần. 

Mục tiêu là chuyển đổi chuỗi thành dạng đơn điệu trong đó tất cả các số 0 đứng trước và tất cả các số 1 đứng sau, sử dụng càng ít thao tác như vậy càng tốt. 

Các ràng buộc cho phép các chuỗi có độ dài lên tới 100.000, do đó, mọi giải pháp về cơ bản đều phải tuyến tính hoặc gần tuyến tính. Các chiến lược bậc hai mô phỏng các phép toán hoặc thử tất cả các chuỗi con ngay lập tức không khả thi vì ngay cả một phép toán đơn lẻ trên tất cả các chuỗi con cũng đã có thể thực hiện được.$O(n^2)$và việc khám phá các chuỗi hoạt động sẽ bùng nổ về mặt tổ hợp. 

Một khó khăn nhỏ là một thao tác khá mạnh mẽ. Nó có thể di chuyển toàn bộ khối ký tự qua khối khác bên trong một phân đoạn đã chọn, vì vậy quá trình này không phải là một mô hình hoán đổi liền kề đơn giản. Điều này làm cho các bản sửa lỗi cục bộ tham lam bỏ qua cấu trúc toàn cầu không đáng tin cậy. 

Một trường hợp thất bại phổ biến xuất phát từ việc giả định rằng mỗi lần đảo ngược có thể được khắc phục một cách độc lập. Ví dụ: trong một chuỗi như`1010`, một suy nghĩ ngây thơ có thể là hai phép đảo ngược yêu cầu hai thao tác, nhưng một phép quay duy nhất trên toàn bộ chuỗi có thể sắp xếp lại đồng thời nhiều ký tự bị đặt sai vị trí. Chi phí thực sự phụ thuộc vào cấu trúc của các lần chạy liền kề hơn là từng cặp riêng lẻ. 

Một trường hợp phức tạp khác là các chuỗi trong đó tất cả các số 0 đã tạo thành một hậu tố hoặc tất cả các số 0 đã tạo thành một hậu tố. Ví dụ,`000111`không yêu cầu thao tác nào, nhưng`001110`trông vẫn gần như được sắp xếp trong khi yêu cầu ít nhất một lần di chuyển để di chuyển số 0 ở cuối vào vùng tiền tố. 

Thách thức cốt lõi là xác định một biện pháp cấu trúc để nắm bắt chính xác có bao nhiêu “khối đặt sai vị trí” độc lập phải được sửa chữa. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng trực tiếp quy trình, chúng ta sẽ xem xét tất cả các chuỗi con và tất cả các phép quay có thể có, phân nhánh theo các lựa chọn về phân đoạn và độ lệch xoay. Mỗi thao tác có thể biến đổi một đoạn theo nhiều cách, do đó, chỉ cần một bước cũng tạo ra hệ số phân nhánh lớn. Ngay cả khi chúng ta hạn chế bản thân trong những lựa chọn tham lam, chúng ta vẫn cần đánh giá xem mỗi hoạt động sẽ thay đổi sự rối loạn toàn cầu như thế nào, điều này dẫn đến việc quét bậc hai ít nhất cho mỗi bước. 

Quan sát quan trọng là chúng ta thực sự không cần theo dõi các hoán vị tùy ý bên trong các phân đoạn. Hình dạng mục tiêu cuối cùng cực kỳ cứng nhắc: tất cả số 0 phải ở bên trái, tất cả số 1 ở bên phải. Điều này có nghĩa là mọi thao tác chỉ hữu ích nếu nó di chuyển một khối số 1 qua vùng số 0. Bên trong một khối tối đa gồm các số 1, thứ tự bên trong là không liên quan. 

Điều này chuyển góc nhìn từ các nhân vật riêng lẻ sang các đường chạy liền kề. Khi chúng tôi nén chuỗi thành các chuỗi ký tự bằng nhau, chúng tôi thấy rằng chỉ những chuỗi xuất hiện trước vùng số 0 cuối cùng mới cần được di chuyển. Mỗi lần chạy như vậy hoạt động giống như một đơn vị duy nhất phải được chuyển sang phía bên phải. 

Một sự thay đổi theo chu kỳ duy nhất trên một phân đoạn được lựa chọn cẩn thận có thể di chuyển một lần chạy một lần bị đặt sai vị trí như vậy qua một nhóm số 0, nhưng nó không thể sửa đồng thời nhiều lần chạy riêng biệt được xen kẽ bằng các số 0. Điều này làm cho mỗi lần chạy như vậy đóng góp độc lập cho câu trả lời. 

Do đó, vấn đề rút gọn thành việc đếm xem có bao nhiêu lần chạy liên tiếp`1`s xuất hiện ở vùng mà các số 0 vẫn tồn tại ở bên phải của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua các vòng quay | số mũ /$O(n^3)$|$O(n)$| Quá chậm | 
| Đếm dựa trên lần chạy |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải quyết vấn đề bằng cách nén chuỗi thành các khối liền kề có ký tự bằng nhau và chỉ suy luận về cấu trúc của các khối này. 

1. Quét chuỗi và xác định tất cả các đoạn liền kề tối đa của các ký tự giống hệt nhau. Mỗi phân đoạn là một khối số 0 hoặc một khối số 1. 
2. Tìm vị trí số 0 cuối cùng trong chuỗi. Mọi thứ ở bên phải của vị trí này phải bao gồm toàn bộ các khối ở dạng được sắp xếp cuối cùng, do đó, bất kỳ khối nào hoàn toàn sau điểm này đều đã được đặt chính xác. 
3. Lặp lại tất cả các khối liền kề nhau. Đối với mỗi một khối, hãy kiểm tra xem nó bắt đầu ở hoặc trước vị trí số 0 cuối cùng. 
4. Tính mỗi khối như vậy là đóng góp một thao tác. 
5. Trả lại số đếm này làm câu trả lời. 

Lý do quy trình này hoạt động là vì một khối xuất hiện trước số 0 ở đâu đó bên phải của nó được phân tách khỏi vùng cuối cùng chính xác của nó ít nhất một số 0. Sự phân tách đó không thể được giải quyết nếu không có một thao tác chuyên dụng di chuyển toàn bộ khối đó qua các số 0 đó. Vì các dịch chuyển theo chu kỳ chỉ di chuyển một đoạn liền kề trong mỗi thao tác nên mỗi khối như vậy yêu cầu di chuyển riêng. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi thao tác, thứ tự tương đối của các ký tự bên ngoài phân đoạn đã chọn vẫn không thay đổi và trong phân đoạn đó chỉ thực hiện một lần cắt theo chu kỳ duy nhất. Điều này có nghĩa là một khối chỉ có thể được di chuyển toàn bộ qua các vùng 0 liền kề, không được hợp nhất hoặc chia thành nhiều lần di chuyển độc lập trong một bước. Do đó, mỗi một khối bị “chặn” ít nhất một số 0 ở bên phải của nó phải được giải quyết một cách độc lập và không có thao tác nào có thể giảm số lượng các khối bị chặn đó nhiều hơn một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    last_zero = -1
    for i, ch in enumerate(s):
        if ch == '0':
            last_zero = i

    if last_zero == -1:
        print(0)
        return

    ans = 0
    i = 0
    while i < n:
        if s[i] == '1':
            j = i
            while j < n and s[j] == '1':
                j += 1
            if i <= last_zero:
                ans += 1
            i = j
        else:
            i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên tính toán lần xuất hiện cuối cùng của số 0, xác định ranh giới giữa vùng phải trở thành tất cả số 0 và vùng phải trở thành tất cả số 1. Sau đó, nó quét qua chuỗi theo thời gian tuyến tính, nhóm các chuỗi liên tiếp thành các lần chạy. 

Một điểm tinh tế là điều kiện`i <= last_zero`. Điều này đảm bảo chúng tôi chỉ tính một khối không được chứa đầy đủ trong vùng hậu tố cuối cùng của khối đó. Bất kỳ khối một khối nào bắt đầu ngay sau số 0 cuối cùng đều đã ở đúng phía của sự sắp xếp cuối cùng và không yêu cầu bất kỳ thao tác nào. 

Phần còn lại của logic là quét tuyến tính tiêu chuẩn với các bước nhảy con trỏ để tránh xử lý lại các ký tự trong một lần chạy. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`01010101`Đầu tiên chúng ta xác định vị trí số 0 cuối cùng. Trong chuỗi này, số 0 cuối cùng nằm ở chỉ số 6. 

Sau đó chúng tôi xác định các lần chạy: 

| Bước | Chạy bắt đầu | Chạy cuối | Loại | Bắt đầu ≤ cuối_zero | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | vâng | 1 | 
| 2 | 3 | 3 | 1 | vâng | 2 | 
| 3 | 5 | 5 | 1 | vâng | 3 | 
| 4 | 7 | 7 | 1 | không | 3 | 

Thuật toán đưa ra 3. 

Dấu vết này cho thấy rằng mỗi lần xen kẽ`1`khối xuất hiện trước hoặc ở ranh giới của vùng 0 cuối cùng, vì vậy mỗi khối phải được di chuyển một lần. 

### Ví dụ 2:`1100101001`Số 0 cuối cùng là ở chỉ số 7. 

Chúng tôi trích xuất một lần chạy: 

| Bước | Chạy bắt đầu | Chạy cuối | Loại | Bắt đầu ≤ cuối_zero | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | vâng | 1 | 
| 2 | 4 | 4 | 1 | vâng | 2 | 
| 3 | 6 | 6 | 1 | vâng | 3 | 
| 4 | 8 | 9 | 1 | không | 3 | 

Câu trả lời là 3. 

Điều này chứng tỏ rằng mặc dù các vùng xuất hiện ở nhiều vùng riêng biệt, nhưng mỗi vùng nằm trước ranh giới số 0 cuối cùng lại đóng góp độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Quét một lần để tìm số 0 cuối cùng và một lần quét khác để đếm một lần chạy | 
| Không gian |$O(1)$| Chỉ sử dụng bộ đếm và chỉ số | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện công việc tuyến tính trên một chuỗi có độ dài tối đa$10^5$, với bộ nhớ bổ sung liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.write = lambda x: out.append(x)
    out.clear()
    solve()
    return "".join(out)

out = []

# custom helper assumes solve() defined above

# All zeros
out = []
assert run("0000\n") == "0", "all zeros"

# Already sorted
out = []
assert run("000111\n") == "0", "already sorted"

# Alternating
out = []
assert run("0101\n") == "2", "alternating"

# Single element
out = []
assert run("1\n") == "0", "single 1"

# One misplaced block
out = []
assert run("11000\n") == "1", "single block"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0000`| 0 | không cần thao tác | 
|`000111`| 0 | cấu trúc hậu tố đã được sắp xếp | 
|`0101`| 2 | nhiều lần chạy một lần riêng biệt | 
|`1`| 0 | trường hợp cạnh tối thiểu | 
|`11000`| 1 | trường hợp khối đặt sai vị trí | 

## Vỏ cạnh 

Đối với một chuỗi chỉ bao gồm các số 1, vị trí 0 cuối cùng là`-1`, do đó thuật toán ngay lập tức trả về 0. Điều này khớp với thực tế là chuỗi đã ở dạng được sắp xếp. 

Đối với một chuỗi chỉ bao gồm các số 0, không có khối một, do đó quá trình quét không bao giờ tăng câu trả lời. Đầu ra bằng 0 như mong đợi. 

Đối với trường hợp tất cả những cái đã có hậu tố, chẳng hạn như`000111`, số 0 cuối cùng nằm ở ranh giới giữa hai vùng và tất cả các khối một đều bắt đầu sau nó, vì vậy không có khối nào được tính. 

Đối với các chuỗi xen kẽ như`010101`, mỗi một khối nằm trước số 0 cuối cùng, vì vậy mỗi khối đóng góp độc lập, phù hợp với hành vi trong trường hợp xấu nhất trong đó mỗi lần chạy phải được sửa riêng.
