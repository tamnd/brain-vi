---
title: "CF 105562A - Quý tộc theo bảng chữ cái"
description: "Chúng ta được cung cấp một tập hợp các họ được viết dưới dạng chuỗi tự do. Mỗi họ có thể chứa chữ hoa, chữ thường, dấu cách và dấu nháy đơn."
date: "2026-06-22T12:47:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105562
codeforces_index: "A"
codeforces_contest_name: "2024-2025 ICPC Northwestern European Regional Programming Contest (NWERC 2024)"
rating: 0
weight: 105562
solve_time_s: 52
verified: true
draft: false
---

[CF 105562A - Quý tộc theo bảng chữ cái](https://codeforces.com/problemset/problem/105562/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các họ được viết dưới dạng chuỗi tự do. Mỗi họ có thể chứa chữ hoa, chữ thường, dấu cách và dấu nháy đơn. Nhiệm vụ là sắp xếp những họ này bằng cách sử dụng quy tắc so sánh tùy chỉnh thay vì thứ tự từ điển tiêu chuẩn trên toàn bộ chuỗi. 

Khóa sắp xếp được xác định bằng cách bỏ qua mọi thứ trong chuỗi trước chữ cái viết hoa đầu tiên. Bắt đầu từ chữ cái viết hoa đầu tiên đó, chúng ta lấy phần còn lại của chuỗi chính xác như nó xuất hiện và sử dụng chuỗi con đó làm khóa so sánh. Các khóa này được so sánh về mặt từ điển bằng cách sử dụng thứ tự ASCII, có nghĩa là các chữ cái viết hoa đứng trước các chữ cái viết thường, dấu cách là các ký tự quan trọng và dấu nháy đơn cũng tham gia vào thứ tự. 

Đầu ra chỉ đơn giản là họ ban đầu được sắp xếp lại theo quy tắc này, với tất cả định dạng ban đầu được giữ nguyên. 

Ràng buộc n 1000 và độ dài chuỗi ≤ 50 ngụ ý rằng bất kỳ giải pháp O(n² log n) hoặc O(n log n) nào cũng dễ dàng đủ. Ngay cả khi chúng tôi tạo khóa cho từng chuỗi và sắp xếp chúng trực tiếp, chúng tôi đang làm việc với tối đa 1000 chuỗi ngắn, do đó, cả quá trình tiền xử lý và sắp xếp đều có chi phí không đáng kể. 

Một khía cạnh tinh tế là xác định chính xác chữ in hoa đầu tiên. Nó được đảm bảo rằng một chữ cái như vậy tồn tại trong mỗi chuỗi và chuỗi con bắt đầu từ đó là duy nhất trên tất cả các đầu vào. Tính duy nhất này đảm bảo thứ tự sắp xếp được xác định rõ ràng và không cần phải có các ràng buộc ổn định. 

Các trường hợp Edge quan trọng nhất chủ yếu là về việc phân tích khóa một cách chính xác. Ví dụ: một chuỗi như "van 't Hek" nên sử dụng "Hek" trở đi bắt đầu từ chữ H viết hoa, không phải từ đầu. Một trường hợp khác là "DeN bRAnD hEeK", trong đó chỉ chữ D viết hoa đầu tiên là có liên quan, do đó khóa trở thành chuỗi đầy đủ từ chữ D đó trở đi, bảo toàn cách viết hoa hỗn hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng các so sánh giữa mỗi cặp chuỗi trong quá trình sắp xếp và đối với mỗi so sánh, hãy quét từ chữ cái in hoa đầu tiên trở đi để so sánh từng ký tự. Mỗi so sánh có chi phí O(L) trong đó L tối đa là 50 và việc sắp xếp n mục có chi phí so sánh O(n log n), vì vậy tổng thể điều này trở thành O(n log n · L). Với n = 1000 và L = 50, đây là khoảng 1000 × 10 × 50 = 500.000 kiểm tra ký tự, vốn đã nhỏ nhưng việc triển khai sẽ liên tục tính toán lại chuỗi “bắt đầu của khóa” và quét lại, gây ra chi phí không cần thiết. 

Quan sát quan trọng là khóa so sánh cho mỗi họ chỉ phụ thuộc vào một chuỗi con cố định. Chúng ta có thể xử lý trước mỗi chuỗi một lần, trích xuất hậu tố bắt đầu từ ký tự viết hoa đầu tiên của chuỗi đó, sau đó sắp xếp bằng cách sử dụng khóa được tính toán trước này. Điều này biến việc tính toán lại lặp đi lặp lại bên trong các so sánh thành một bước tiền xử lý tuyến tính duy nhất. 

Khi mỗi chuỗi có khóa của nó, việc sắp xếp sẽ trở thành một cách sắp xếp từ điển tiêu chuẩn trên các khóa này trong khi vẫn xuất ra các chuỗi gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n · L) | O(1) thêm | Đã chấp nhận | 
| Tối ưu (phím tính toán trước) | O(n L + n log n) | O (n L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển mỗi họ thành một cặp gồm khóa sắp xếp và chuỗi gốc, sau đó sắp xếp theo khóa.

1. Với mỗi chuỗi đầu vào, quét từ trái sang phải cho đến khi tìm được chữ in hoa đầu tiên. Vị trí này xác định nơi bắt đầu so sánh có ý nghĩa. Điều này là cần thiết vì mọi thứ trước nó đều không liên quan theo quy tắc bài toán. 
2. Trích xuất chuỗi con bắt đầu từ ký tự viết hoa đó cho đến hết chuỗi. Chuỗi con này là khóa sắp xếp. Chúng tôi không sửa đổi cách viết hoa hoặc loại bỏ khoảng trắng vì thứ tự từ điển ASCII phụ thuộc vào các ký tự chính xác. 
3. Lưu trữ một cặp (khóa, chuỗi gốc) cho mỗi dòng đầu vào. Điều này duy trì yêu cầu đầu ra ban đầu trong khi cung cấp cho chúng tôi cơ sở so sánh rõ ràng. 
4. Sắp xếp tất cả các cặp theo khóa bằng cách sử dụng thứ tự từ điển tiêu chuẩn. 
5. Xuất các chuỗi gốc theo thứ tự được sắp xếp, bỏ qua các phím. 

Tại sao nó hoạt động dựa trên thực tế là mọi so sánh giữa hai họ chỉ phụ thuộc vào hậu tố của chúng bắt đầu từ chữ in hoa đầu tiên. Bằng cách tính toán trước hậu tố này một lần, chúng tôi đảm bảo rằng mọi so sánh trong quá trình sắp xếp đều phù hợp với quy tắc của bài toán. Vì việc sắp xếp chỉ phụ thuộc vào các khóa cố định này và các khóa giống hệt với những gì bộ so sánh thủ công sẽ tạo ra theo yêu cầu nên thứ tự kết quả giống hệt với thứ tự dự định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    items = []

    for _ in range(n):
        s = input().rstrip("\n")
        start = 0
        for i, ch in enumerate(s):
            if 'A' <= ch <= 'Z':
                start = i
                break
        key = s[start:]
        items.append((key, s))

    items.sort(key=lambda x: x[0])

    for _, s in items:
        print(s)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là vòng lặp tiền xử lý để tìm chữ in hoa đầu tiên. Đây là quá trình quét tuyến tính đơn giản trên mỗi chuỗi, an toàn vì các chuỗi này ngắn. 

Sau đó chúng ta cắt chuỗi một lần để tạo thành khóa. Việc cắt của Python tạo ra một chuỗi mới, nhưng với những hạn chế, chi phí này không đáng kể. 

Việc sắp xếp được thực hiện bằng cách sử dụng tính năng timsort tích hợp của Python với chức năng khóa, chỉ so sánh các khóa được tính toán trước. 

## Ví dụ đã hoạt động 

Hãy xem xét kịch bản đặt hàng mẫu đầu tiên với một tập hợp con nhỏ: 

đầu vào:```
Bakker
van der Steen
Groot Koerkamp
```Chúng tôi tính toán các phím: 

| Chuỗi | Vị trí viết hoa đầu tiên | Chìa khóa | 
| --- | --- | --- | 
| Thợ làm bánh | 0 | Thợ làm bánh | 
| van der Steen | 4 | Steen | 
| Groot Koerkamp | 0 | Groot Koerkamp | 

Sau khi sắp xếp theo khóa: 

| Chìa khóa | Bản gốc | 
| --- | --- | 
| Thợ làm bánh | Thợ làm bánh | 
| Groot Koerkamp | Groot Koerkamp | 
| Steen | van der Steen | 

Đầu ra trở thành:```
Bakker
Groot Koerkamp
van der Steen
```This shows how leading lowercase prefixes are ignored entirely and only the capitalized suffix governs ordering.

 Now consider a case with mixed casing:

 đầu vào:```
DeN bRAnD hEeK
Brand 'Heek
van Brand heek
```Keys:

 | Chuỗi | Chìa khóa | 
| --- | --- | 
| DeN bRAnD hEeK | DeN bRAnD hEeK |
 | Brand 'Heek | Brand 'Heek |
 | van Brand heek | Brand heek |

 Sorting lexicographically compares character by character starting from ASCII values, where space, uppercase, and lowercase differences matter. This produces a deterministic order consistent with raw ASCII comparison.

 ## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + nL) | scanning each string once plus sorting n items by key |
 | Không gian | O(nL) | storing extracted keys alongside originals |

 With n ≤ 1000 and L ≤ 50, both bounds are extremely small, and the solution runs comfortably within limits.

 ## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    input = sys.stdin.readline

    n = int(input().strip())
    items = []
    for _ in range(n):
        s = input().rstrip("\n")
        start = 0
        for i, ch in enumerate(s):
            if 'A' <= ch <= 'Z':
                start = i
                break
        items.append((s[start:], s))

    items.sort(key=lambda x: x[0])
    return "\n".join(s for _, s in items)

# provided-style samples
assert run("3\nBakker\nvan der Steen\nGroot Koerkamp\n") == "Bakker\nGroot Koerkamp\nvan der Steen"

# minimum size
assert run("1\nAaa\n") == "Aaa"

# all same key prefix but different tails
assert run("3\naA\nbA\ncA\n") == "aA\nbA\ncA"

# punctuation and spaces
assert run("2\nvan 't Hek\nvan 't Aa\n") == "van 't Aa\nvan 't Hek"

# mixed casing stability check
assert run("2\nDeN bRAnD hEeK\nden brandHeek\n") == "DeN bRAnD hEeK\nden brandHeek"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | tính đúng đắn của trường hợp cơ sở | 
| same suffix structure | sorted order | xử lý khóa từ điển | 
| trường hợp dấu câu | deterministic order | So sánh nhạy cảm ASCII | 
| mixed casing | đặt hàng nhất quán | correct key extraction |

 ## Vỏ cạnh 

A common pitfall is starting comparison from the beginning of the string instead of the first uppercase letter. Ví dụ, trong`"van der Steen"`, chìa khóa đúng là`"Steen"`, không`"van der Steen"`. Thuật toán quét rõ ràng cho đến khi`'S'`và lát cắt từ đó, đảm bảo tính chính xác. 

Một trường hợp khác là các chuỗi trong đó chữ in hoa đầu tiên không phải là ký tự đầu tiên, chẳng hạn như`"fakederSteenOfficial"`. Đây là chữ hoa đầu tiên`'S'`, vì vậy chìa khóa trở thành`"SteenOfficial"`. Vòng tiền xử lý đảm bảo vị trí này được tìm thấy trước khi cắt, do đó không có tiền tố không chính xác nào bị rò rỉ vào phần so sánh. 

Cuối cùng, các chuỗi trường hợp hỗn hợp như`"DeN bRAnD hEeK"`dựa vào thứ tự ASCII thô sau khi trích xuất. Vì chúng tôi không chuẩn hóa chữ hoa và chữ thường nên sự khác biệt giữa chữ hoa và chữ thường được giữ nguyên chính xác theo yêu cầu và việc sắp xếp vẫn trung thành với đặc điểm kỹ thuật.
