---
title: "CF 1915B - Quảng trường không hoàn toàn Latin"
description: "Chúng ta được cung cấp một lưới 3 x 3 cố định chứa các ký tự A, B và C, có cấu trúc là hình vuông Latinh ngoại trừ một ô bị thiếu. Một hình vuông Latinh hợp lệ ở đây có nghĩa là mỗi hàng chứa chính xác một trong mỗi chữ cái A, B và C, đồng thời giữ nguyên giá trị cho mỗi cột."
date: "2026-06-08T19:55:22+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1915
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 918 (Div. 4)"
rating: 800
weight: 1915
solve_time_s: 89
verified: true
draft: false
---

[CF 1915B - Hình vuông không hoàn toàn Latinh](https://codeforces.com/problemset/problem/1915/B) 

**Đánh giá:** 800 
**Tags:** bitmasks, vũ lực, thực hiện 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới 3 x 3 cố định chứa các ký tự A, B và C, có cấu trúc là hình vuông Latinh ngoại trừ một ô bị thiếu. Một hình vuông Latinh hợp lệ ở đây có nghĩa là mỗi hàng chứa chính xác một trong mỗi chữ cái A, B và C, đồng thời giữ nguyên giá trị cho mỗi cột. 

Trong mỗi trường hợp thử nghiệm, chính xác một ô được thay thế bằng dấu chấm hỏi. Tất cả tám ô còn lại đều được điền chính xác để việc hoàn thành hình vuông Latinh sẽ xác định duy nhất ký tự còn thiếu. Nhiệm vụ chỉ đơn giản là khôi phục ký tự bị thiếu đó. 

Kích thước đầu vào lớn về số lượng ca kiểm thử, lên tới$10^8$. Mỗi trường hợp thử nghiệm là một công việc không đổi: một lưới 3 x 3. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào cũng phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm và tránh các chi phí không cần thiết như quay lui hoặc mô phỏng các hoán vị. 

Vì kích thước lưới là cố định nên không có trường hợp biên thuật toán nào có ý nghĩa liên quan đến kích thước. Điều kiện cạnh duy nhất là vị trí của ký tự bị thiếu, ký tự này có thể ở bất kỳ đâu trong lưới. Một sai lầm ngây thơ là giả định một vị trí cố định cho dấu chấm hỏi và chỉ kiểm tra vị trí đó. Ví dụ: nếu giả sử nó luôn ở hàng hoặc cột giữa, điều đó sẽ không thành công đối với các đầu vào như:```
?AB
BCA
CAB
```Câu trả lời đúng phụ thuộc vào các ràng buộc chung của hàng và cột chứ không phải vị trí cục bộ. 

Một cạm bẫy tinh vi khác là cố gắng tạo lại chữ cái bị thiếu bằng cách chỉ kiểm tra hàng hoặc chỉ cột. Chỉ riêng một trong hai là không đủ vì mỗi hàng và cột phải đồng thời thỏa mãn ràng buộc hoán vị. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là thử tất cả các chữ cái có thể có cho dấu chấm hỏi và xác minh xem lưới kết quả có thỏa mãn các ràng buộc hình vuông Latin hay không. Vì chỉ có ba chữ cái có thể, đây đã là công việc không đổi cho mỗi trường hợp thử nghiệm, nhưng chúng ta có thể mô tả nó theo nghĩa tổng quát hơn: điền A, B hoặc C vào ô bị thiếu và kiểm tra tất cả các hàng và cột. 

Việc xác minh toàn bộ lưới 3 x 3 mất nhiều thời gian vì chúng tôi chỉ kiểm tra 6 dòng. Vì vậy, ngay cả cách tiếp cận thô bạo này cũng tối ưu một cách hiệu quả cho các ràng buộc. 

Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa bằng cách khai thác một đặc tính cấu trúc: trong mỗi hàng hợp lệ của một hình vuông Latinh, ba chữ cái A, B, C xuất hiện đúng một lần. Do đó, nếu chúng ta biết hai chữ cái liên tiếp thì chữ cái thứ ba được xác định duy nhất. Điều tương tự cũng xảy ra với các cột. 

Điều này có nghĩa là chúng ta không cần phải kiểm tra tất cả các khả năng. Chúng ta có thể trực tiếp xác định vị trí hàng hoặc cột chứa dấu chấm hỏi và tính chữ cái còn thiếu là chữ cái không có trong hàng (hoặc cột) đó. Vì mỗi hàng được đảm bảo chứa chính xác một phần tử bị thiếu nên cả hai phương pháp đều hiệu quả và việc khôi phục dựa trên hàng là đơn giản nhất. 

Chúng tôi tính toán tập hợp đầy đủ {A, B, C}, loại bỏ hai chữ cái đã biết khỏi hàng chứa '?' và phần tử còn lại là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử A, B, C và xác nhận) | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| Tối ưu (đặt chênh lệch hàng/cột) | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới 3 x 3 cho mỗi trường hợp thử nghiệm. Lưới nhỏ nên chúng ta có thể xử lý trực tiếp mà không cần xử lý trước. 
2. Xác định vị trí dấu chấm hỏi. Chúng tôi làm điều này bằng cách quét tất cả 9 ô. Vì lưới có kích thước không đổi nên quá trình quét này có thời gian không đổi. 
3. Xác định hàng chứa dấu chấm hỏi. Hàng này sẽ chứa chính xác hai chữ cái đã biết và một vị trí bị thiếu. 
4. Thu thập các ký tự trong hàng đó và so sánh chúng với tập hợp {A, B, C}. 
5. Ký tự còn thiếu là ký tự không có ở hàng đó. 
6. Xuất ký tự được khôi phục. 

Quyết định quan trọng là sử dụng hàng chứa dấu chấm hỏi thay vì cố gắng kết hợp thông tin hàng và cột. Vì mỗi hàng phải chứa cả ba chữ cái chính xác một lần trong một lần hoàn thành hợp lệ, nên chỉ riêng hàng đó là đủ để tái tạo lại giá trị còn thiếu. 

### Tại sao nó hoạt động 

Mỗi hàng của hình vuông Latin là một hoán vị của {A, B, C}. Loại bỏ một mục để lại chính xác hai chữ cái riêng biệt. Chữ cái còn lại được xác định duy nhất là phần bù của cặp đó. Vì đầu vào được đảm bảo khác với hình vuông Latinh hợp lệ ở đúng một vị trí, nên hàng chứa giá trị bị thiếu vẫn giữ nguyên cấu trúc này ở dạng hoàn chỉnh. Do đó, việc xác định phần tử bị thiếu sẽ giảm xuống việc tính toán phần bù tập hợp xác định, điều này không thể mang lại sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ALL = {"A", "B", "C"}

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        grid = [input().strip() for _ in range(3)]

        ans = None

        for r in range(3):
            if "?" in grid[r]:
                s = set(grid[r])
                s.discard("?")
                ans = (ALL - s).pop()
                break

        out.append(ans)

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp quét từng hàng để tìm hàng có chứa dấu chấm hỏi. Sau khi tìm thấy, nó sẽ chuyển đổi hàng đó thành một tập hợp, loại bỏ phần giữ chỗ và tính toán ký tự bị thiếu thông qua chênh lệch tập hợp với {A, B, C}. các`.pop()`là an toàn vì chênh lệch đã đặt luôn chứa chính xác một phần tử. 

Việc triển khai dựa trên thực tế là chỉ có một hàng chứa ký tự bị thiếu, vì vậy chúng ta có thể ngắt ngay sau khi xử lý nó. Điều này tránh được việc kiểm tra không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ABC
C?B
BCA
```| Bước | Hàng được kiểm tra | Nội dung hàng | Những chữ cái đã biết | Thiếu tính toán | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Hàng 0 | ABC | A, B, C | đã hoàn thành | bỏ qua | 
| 2 | Hàng 1 | C?B | C, B | {A,B,C} - {B,C} | A | 

Điều này xác nhận rằng giá trị còn thiếu được xác định hoàn toàn bằng tính đầy đủ của hàng mà không cần kiểm tra cột. 

### Ví dụ 2 

đầu vào:```
BCA
CA?
ABC
```| Bước | Hàng được kiểm tra | Nội dung hàng | Những chữ cái đã biết | Thiếu tính toán | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Hàng 0 | BCA | B, C, A | hoàn thành | bỏ qua | 
| 2 | Hàng 1 | CA? | C, A | {A,B,C} - {A,C} | B | 

Điều này cho thấy ngay cả khi phần tử bị thiếu ở cuối hàng, phương thức vẫn hoạt động mà không cần giả định về vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi lưới có kích thước cố định 3 x 3, do đó các phép toán không đổi | 
| Không gian | O(1) | Chỉ sử dụng lưới cố định và tập hợp hằng số | 

Các ràng buộc cho phép lên đến$10^8$các trường hợp thử nghiệm, do đó, thời gian không đổi cho mỗi hành vi của trường hợp thử nghiệm là điều cần thiết. Bất kỳ thuật toán nào có tỷ lệ với kích thước đầu vào vượt quá hằng số cho mỗi trường hợp sẽ không khả thi, nhưng ở đây mọi thao tác đều bị giới hạn bởi một số so sánh ký tự cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline  # placeholder, replace with solve() capture

# corrected runner
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    ALL = {"A", "B", "C"}

    t = int(input())
    out = []
    for _ in range(t):
        grid = [input().strip() for _ in range(3)]
        for r in range(3):
            if "?" in grid[r]:
                s = set(grid[r])
                s.discard("?")
                out.append((ALL - s).pop())
                break
    return "\n".join(out)

# provided samples
assert run("""3
ABC
C?B
BCA
BCA
CA?
ABC
?AB
BCA
ABC
""") == "A\nB\nC"

# custom cases
assert run("""1
ABC
BCA
CA?
""") == "B"

assert run("""1
?BC
CAB
ABC
""") == "A"

assert run("""1
ABC
A?C
BAC
""") == "B"

assert run("""1
ABC
BCA
A?C
""") == "B"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hàng đơn bị thiếu ở cuối | B | thiếu ở cột cuối cùng | 
| thiếu ở trên cùng bên trái | A | vị trí độc lập | 
| hàng giữa thiếu ô giữa | B | xử lý vị trí trung tâm | 
| hàng dưới thiếu ô giữa | B | tính nhất quán tương tác cột | 

## Vỏ cạnh 

Trường hợp một cạnh là khi ký tự bị thiếu xuất hiện ở vị trí đầu tiên của hàng. Ví dụ:```
?BC
CAB
ABC
```Thuật toán xác định hàng đầu tiên có chứa dấu chấm hỏi. Các chữ cái đã biết là B và C, do đó, chênh lệch tập hợp từ {A, B, C} mang lại A. Vị trí của ô bị thiếu không ảnh hưởng đến việc tính toán, vì tư cách thành viên tập hợp hoàn toàn bỏ qua thứ tự. 

Một trường hợp khác là khi ký tự bị thiếu ở cột cuối cùng:```
ABC
BCA
CA?
```Hàng thứ hai chứa dấu chấm hỏi và các chữ cái đã biết là C và A. Phần bù là B. Thuật toán không dựa vào chỉ số số học nên vị trí cột không có tác dụng. 

Trường hợp cạnh cấu trúc cuối cùng là tất cả các hàng ngoại trừ một hàng đều là hoán vị hoàn chỉnh. Thuật toán vẫn chỉ kiểm tra hàng chưa hoàn chỉnh, do đó không có sự mơ hồ nào phát sinh từ các hàng hợp lệ đầy đủ ở nơi khác.
