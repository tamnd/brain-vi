---
title: "CF 448A - Phần thưởng"
description: "Chúng tôi được cung cấp một số lượng kệ cố định và một bộ sưu tập phần thưởng được chia thành hai loại, cúp và huy chương. Mỗi danh mục lại được chia thành ba cấp bậc, nhưng đối với logic vị trí, các cấp bậc đó không quan trọng ngoài việc đếm tổng số mục trong mỗi danh mục."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 448
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 256 (Div. 2)"
rating: 800
weight: 448
solve_time_s: 74
verified: true
draft: false
---

[CF 448A - Phần thưởng](https://codeforces.com/problemset/problem/448/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số lượng kệ cố định và một bộ sưu tập phần thưởng được chia thành hai loại, cúp và huy chương. Mỗi danh mục lại được chia thành ba cấp bậc, nhưng đối với logic vị trí, các cấp bậc đó không quan trọng ngoài việc đếm tổng số mục trong mỗi danh mục. 

Mỗi kệ có hai ràng buộc về cấu trúc. Nó chỉ có thể lưu trữ một danh mục, nghĩa là không thể đặt cốc và huy chương trên cùng một kệ. Ngoài ra, kệ có giới hạn sức chứa tùy thuộc vào danh mục: tối đa 5 cốc trên mỗi kệ và tối đa 10 huy chương trên mỗi kệ. 

Nhiệm vụ là xác định xem liệu có thể phân phối tất cả các mặt hàng trên số kệ nhất định trong khi vẫn tôn trọng những ràng buộc này hay không. 

Điểm trừu tượng chính là chúng tôi không sắp xếp các vật phẩm riêng lẻ theo trình tự hoặc thứ tự mà đóng gói số lượng vào các thùng có sức chứa cố định trên mỗi thùng, trong đó các thùng được phân chia thành hai nhóm độc lập theo loại. 

Những hạn chế là nhỏ. Tất cả số lượng tối đa là 100 và số lượng giá cũng tối đa là 100. Điều này loại trừ mọi nhu cầu tìm kiếm hoặc quay lại cấu hình. Bất kỳ sự phân bổ theo cấp số nhân nào của các mặt hàng vào kệ sẽ vượt xa mức cần thiết, mặc dù nó vẫn khả thi về mặt tính toán ở quy mô này. 

Một sai lầm ngây thơ nhưng có lý là cố gắng giao việc một cách tham lam mà không tôn trọng cẩn thận việc phân nhóm năng lực. 

Một cách tiếp cận sai lầm là phân phát cốc và huy chương một cách độc lập mà không đảm bảo tổng số kệ được sử dụng không vượt quá n. Ví dụ: nếu cốc yêu cầu 3 kệ và huy chương yêu cầu 4 kệ nhưng các kệ giống nhau được giả định không chính xác là có thể tái sử dụng, thì việc triển khai ngây thơ có thể hợp nhất chúng không chính xác và tạo ra CÓ sai. 

Một trường hợp phức tạp khác là khi một danh mục trống. Ví dụ: nếu chỉ có cốc, câu trả lời phụ thuộc hoàn toàn vào việc liệu ceil(total_cups / 5) có nằm trong n hay không và ràng buộc huy chương không được tiêu tốn các kệ một cách giả tạo. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ là mô phỏng tất cả các cách có thể để gán từng mặt hàng riêng lẻ vào một giá, tôn trọng các hạn chế về sức chứa và đảm bảo không trộn lẫn các loại. Mỗi mục có tối đa n lựa chọn và có tổng cộng lên tới 600 mục, do đó, mục này nhanh chóng trở thành tổ hợp với một không gian trạng thái rộng lớn về mặt thiên văn. Ngay cả với việc cắt tỉa, điều này là không cần thiết vì cấu trúc của bài toán không phụ thuộc vào thứ tự hoặc sự lựa chọn bài tập riêng lẻ. 

Nhận xét quan trọng là trong mỗi danh mục, chỉ có tổng số lượng là quan trọng. Vì mỗi kệ có thể chứa một số lượng vật phẩm tối đa cố định của cùng một loại, nên số lượng kệ tối thiểu cần thiết cho cốc được xác định hoàn toàn bằng số lượng nhóm đầy đủ 5 chiếc mà chúng ta cần, và các huy chương tương tự yêu cầu nhóm 10 chiếc. Khi chúng ta tính toán riêng lẻ có bao nhiêu kệ cần cho cốc và huy chương, câu hỏi duy nhất còn lại là liệu tổng số kệ cần thiết có vừa với n hay không. 

Việc giảm này có tác dụng vì các kệ không được chia sẻ giữa các loại. Khi một kệ được chỉ định cho các cốc, giá đó sẽ được sử dụng hết cho các cốc và điều tương tự cũng áp dụng cho các huy chương. Không có sự ghép nối ngoại trừ sự cạnh tranh về tổng số kệ. 

Vì vậy, vấn đề trở thành một phép tính đóng gói đơn giản, sau đó là kiểm tra tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | Hàm mũ | O(n) | Quá chậm | 
| Đếm + Phòng Trần | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng số cốc bằng cách cộng cả ba loại cốc. Điều này thu gọn vấn đề thành một đại lượng duy nhất vì các ràng buộc về giá trị không phân biệt thứ hạng. 
2. Tính số lượng kệ cần thiết để đựng cốc bằng cách lấy trần của tổng số cốc chia cho 5. Mỗi kệ có thể chứa tối đa 5 cốc, vì vậy đây là số lượng kệ tối thiểu cần thiết. 
3. Tính tổng số huy chương tương tự bằng cách cộng tất cả các loại huy chương. 
4. Tính số ngăn cần thiết để đựng huy chương bằng cách lấy trần tổng số huy chương chia cho 10. Mỗi ngăn có thể chứa tối đa 10 huy chương. 
5. Thêm hai số kệ cần thiết. Điều này thể hiện tổng số kệ được sử dụng nếu chúng ta chỉ định cốc và huy chương một cách tối ưu trong giới hạn riêng của chúng. 
6. So sánh số tiền này với n. Nếu nó nhỏ hơn hoặc bằng n thì xuất ra CÓ, nếu không thì xuất ra KHÔNG. 

### Tại sao nó hoạt động 

Mỗi kệ chỉ dành riêng cho một danh mục duy nhất, vì vậy vấn đề sẽ được chia thành hai vấn đề đóng gói thùng độc lập. Trong mỗi danh mục, mọi kệ đều được lấp đầy một cách tối ưu, ngoại trừ có thể một kệ được lấp đầy một phần và việc sắp xếp lại các mặt hàng trên các kệ không mang lại lợi ích gì ngoài việc tối đa hóa mức sử dụng trên mỗi kệ. Do đó, các giá trị trần được tính toán là giới hạn dưới chính xác và kết cấu phía trên có thể đạt được, nghĩa là tổng của chúng chính xác là số lượng kệ tối thiểu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a1, a2, a3 = map(int, input().split())
    b1, b2, b3 = map(int, input().split())
    n = int(input())

    cups = a1 + a2 + a3
    medals = b1 + b2 + b3

    cup_shelves = (cups + 4) // 5
    medal_shelves = (medals + 9) // 10

    print("YES" if cup_shelves + medal_shelves <= n else "NO")

if __name__ == "__main__":
    solve()
```Trước tiên, giải pháp tổng hợp tất cả các mục theo loại, bỏ qua phân loại nội bộ vì nó không ảnh hưởng đến các hạn chế về năng lực. Việc chia trần được thực hiện bằng cách sử dụng số học số nguyên: thêm 4 trước khi chia cho 5 đảm bảo làm tròn chính xác cho cốc và thêm 9 trước khi chia cho 10 cũng làm như vậy đối với huy chương. 

Sự so sánh cuối cùng trực tiếp kiểm tra tính khả thi so với các kệ có sẵn. 

Một sai lầm phổ biến là quên rằng kệ không thể trộn lẫn các loại. Một người khác đang cố gắng phân phối các mặt hàng một cách tham lam mà không nhận ra rằng việc đóng gói tối ưu cho mỗi loại luôn được lấp đầy ngoại trừ một kệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1 1
1 1 1
4
```Tổng số cúp là 3, tổng số huy chương là 3. 

| Bước | Ly | Kệ cốc | Huy chương | Kệ Huy Chương | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Tính tổng | 3 | - | 3 | - | - | 
| Phân chia trần | - | 1 | - | 1 | - | 
| Kệ tổng hợp | - | 1 | - | 1 | 2 | 
| So sánh với n | - | - | - | - | 2 4 | 

Đầu ra là CÓ. 

Dấu vết này xác nhận rằng việc sử dụng một phần kệ không làm thay đổi tính khả thi, vì không gian còn sót lại trên kệ đựng cốc không thể được tái sử dụng để đựng huy chương. 

### Ví dụ 2 

đầu vào:```
5 0 0
0 0 10
2
```Tổng số cúp là 5, tổng số huy chương là 10. 

| Bước | Ly | Kệ cốc | Huy chương | Kệ Huy Chương | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Tính tổng | 5 | - | 10 | - | - | 
| Phân chia trần | - | 1 | - | 1 | - | 
| Kệ tổng hợp | - | 1 | - | 1 | 2 | 
| So sánh với n | - | - | - | - | 2 2 2 | 

Đầu ra là CÓ. 

Điều này cho thấy một trường hợp ranh giới chặt chẽ trong đó các kệ là đủ chính xác và bất kỳ mặt hàng bổ sung nào trong một trong hai danh mục sẽ buộc phải có thêm một kệ và lật ngược câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học trên đầu vào có kích thước cố định | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ ngoài các biến | 

Việc tính toán là thời gian không đổi, điều này không quan trọng trong các ràng buộc và dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a1, a2, a3 = map(int, input().split())
    b1, b2, b3 = map(int, input().split())
    n = int(input())

    cups = a1 + a2 + a3
    medals = b1 + b2 + b3

    cup_shelves = (cups + 4) // 5
    medal_shelves = (medals + 9) // 10

    return "YES" if cup_shelves + medal_shelves <= n else "NO"

# provided sample
assert run("1 1 1\n1 1 1\n4\n") == "YES"

# minimum case
assert run("0 0 0\n0 0 0\n1\n") == "YES"

# tight cup packing
assert run("5 0 0\n0 0 0\n1\n") == "YES"

# requires extra shelf for cups
assert run("6 0 0\n0 0 0\n1\n") == "NO"

# tight medals
assert run("0 0 0\n10 0 0\n1\n") == "YES"

# overflow both categories
assert run("10 10 10\n10 10 10\n3\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | CÓ | đóng gói rỗng | 
| Chính xác 5 cốc | CÓ | phù hợp chính xác cho mỗi kệ | 
| 6 cốc | KHÔNG | phát hiện tràn | 
| 10 huy chương | CÓ | ranh giới dung lượng huy chương | 
| hỗn hợp lớn | KHÔNG | lỗi ràng buộc kết hợp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi một danh mục trống. Giả sử cốc bằng 0: 

đầu vào:```
0 0 0
10 0 0
1
```Kệ cốc là 0, kệ huy chương là 1, tổng là 1, nên đầu ra là CÓ. Thuật toán xử lý việc này một cách tự nhiên vì phép chia trần bằng 0 vẫn bằng 0 và không có kệ nhân tạo nào bị lãng phí. 

Một trường hợp cạnh khác là khi cả hai loại đều phù hợp độc lập nhưng cùng nhau vượt quá n. Ví dụ: 

đầu vào:```
5 0 0
10 0 0
1
```Kệ cốc là 1, kệ huy chương là 1, tổng là 2 nên sản lượng là KHÔNG. Một sai lầm ngây thơ là cố gắng xếp các huy chương vào chỗ trống còn sót lại trên kệ đựng cốc, việc này không hợp lệ do quy định phân chia nghiêm ngặt. Thuật toán thực thi sự phân tách này theo thiết kế, do đó nó sẽ loại bỏ trường hợp đó một cách chính xác.
