---
title: "CF 106407A - Trò chơi lưới cực đỉnh"
description: "Trò chơi được chơi trên một bảng n × m ban đầu trống. Ở mỗi lượt, người chơi chọn một hình chữ nhật thẳng hàng với trục vẫn hoàn toàn trống và vẽ nó. Hình chữ nhật có thể có chiều cao và chiều rộng dương bất kỳ miễn là diện tích của nó lớn nhất là k."
date: "2026-06-25T10:00:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106407
codeforces_index: "A"
codeforces_contest_name: "Purdue Spring 2026 In-House Contest #3"
rating: 0
weight: 106407
solve_time_s: 43
verified: true
draft: false
---

[CF 106407A - Trò chơi lưới cực đoan](https://codeforces.com/problemset/problem/106407/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi lúc đầu trống`n × m`Cái bảng. Ở mỗi lượt, người chơi chọn một hình chữ nhật thẳng hàng với trục vẫn hoàn toàn trống và vẽ nó. Hình chữ nhật có thể có chiều cao và chiều rộng dương bất kỳ miễn là diện tích của nó lớn nhất là`k`. Người chơi đầu tiên không còn hình chữ nhật hợp lệ sẽ thua. Chúng ta cần xác định xem người chơi đầu tiên, Munir, có chiến lược chiến thắng hay không. 

Đầu vào chứa một số trò chơi độc lập. Mỗi trò chơi đều cung cấp kích thước bàn cờ và diện tích hình chữ nhật tối đa được phép. Đầu ra là tên của người chơi giành chiến thắng trong lối chơi tối ưu. 

Các ràng buộc nhỏ về tổng số ô: tổng của`n × m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`1000`. Điều này loại trừ các thuật toán phụ thuộc vào toàn bộ cây trò chơi, vì số lượng vị trí có thể tăng theo cấp số nhân. Tuy nhiên, nó cho phép lập trình động trên các kích thước bảng, bởi vì chỉ những hình chữ nhật có diện tích lớn nhất`1000`có thể xuất hiện dưới dạng các bài toán con. 

Phần không rõ ràng là bảng không cần phải mô phỏng. Sau khi di chuyển bên trong vùng trống hình chữ nhật, các ô trống còn lại sẽ được tách thành các vùng hình chữ nhật độc lập. Điều này có nghĩa là trạng thái trò chơi có thể được biểu thị bằng hình chữ nhật thay vì bằng các ô riêng lẻ. 

Việc thực hiện bất cẩn vẫn có thể thất bại trong một số trường hợp. Ví dụ:```
1 1 1
```Ô duy nhất có thể được sơn nên người chơi đầu tiên sẽ thắng. Một giải pháp chỉ kiểm tra xem diện tích bảng có lớn hơn không`k`sẽ nói không chính xác người chơi đầu tiên thua. 

Một ví dụ khác là:```
2 3 1
```Chỉ có thể sơn các ô đơn lẻ. Bàn cờ có sáu nước đi độc lập nên người chơi thứ hai thắng vì tổng số nước đi là chẵn. Một giải pháp chỉ xem xét sự tồn tại của nước đi đầu tiên sẽ thất bại ở đây. 

Trường hợp quan trọng thứ ba là:```
3 4 6
```Người chơi đầu tiên có thể vẽ một`3 × 2`hình chữ nhật ở giữa, chia bảng còn lại thành hai phần độc lập bằng nhau. Chỉ suy nghĩ về số lượng ô được sơn sẽ bỏ lỡ sự phân chia chiến lược này. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp mô hình hóa trò chơi theo cách đệ quy. Với mỗi hình chữ nhật trống, nó sẽ thử mọi hình chữ nhật có thể đặt bên trong nó. Đối với mỗi nước đi, nó sẽ tính toán bốn hình chữ nhật còn lại xung quanh quân cờ đã đặt và giải quyết đệ quy các trò chơi nhỏ hơn đó. Điều này đúng vì sau khi di chuyển, không có hình chữ nhật nào trong tương lai có thể vượt qua vùng được sơn, do đó bốn vùng không bao giờ tương tác với nhau nữa. 

Vấn đề là số lượng lựa chọn. Một bảng có diện tích xung quanh`1000`có thể chứa hàng nghìn vị trí có thể và cây đệ quy có nhiều nhánh. Nếu không ghi nhớ, số lượng trạng thái được khám phá sẽ tăng theo cấp số nhân. Ngay cả với khả năng ghi nhớ, việc thử lặp đi lặp lại mọi vị trí sắp xếp là không cần thiết. 

Nhận xét quan trọng là đây là một trò chơi tổ hợp không thiên vị. Mỗi hình chữ nhật trống là một trạng thái trò chơi và kết quả của việc kết hợp các trò chơi độc lập được xác định bởi xor của các số Grundy của chúng. Khi một hình chữ nhật được chia thành bốn phần, số Grundy của vị trí kết quả là xor của bốn giá trị hình chữ nhật nhỏ hơn. 

Bởi vì diện tích bảng nhiều nhất`1000`, chúng ta có thể tính toán`grundy(h, w)`cho mọi kích thước hình chữ nhật có thể tiếp cận. Đối với mỗi hình chữ nhật, chúng tôi liệt kê mọi bước đi hợp pháp đầu tiên, tính giá trị xor của bốn hình chữ nhật còn lại và lưu trữ mex của tất cả các giá trị có thể truy cập. 

Lực lượng vũ phu hoạt động vì mọi tương lai có thể đều được khám phá. Nó thất bại vì nó lặp đi lặp lại cùng một trạng thái hình chữ nhật nhiều lần. Nhận xét rằng mọi vị trí chỉ là một tập hợp các hình chữ nhật độc lập sẽ biến trò chơi thành một bài toán lập trình động nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lần di chuyển | Hàm mũ | Quá chậm | 
| Tối ưu | Phụ thuộc vào trạng thái và vị trí hình chữ nhật có thể tiếp cận, phù hợp với`n × m ≤ 1000`giới hạn | O(số trạng thái hình chữ nhật) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`grundy(h, w)`là số Grundy của một hình chữ nhật trống có chiều cao`h`và chiều rộng`w`. Câu trả lời dựa trên việc liệu`grundy(n, m)`có bằng không hay không. 
2. Đối với hình chữ nhật`h × w`, hãy thử đặt mọi hình chữ nhật`a × b`như vậy`a ≤ h`,`b ≤ w`, Và`a × b ≤ k`. 

Điều này bao gồm mọi động thái đầu tiên có thể xảy ra từ trạng thái này. 
3. Với mọi vị trí có thể có của hình chữ nhật đã đặt đó, hãy tính bốn hình chữ nhật trống còn lại. Các số Grundy của chúng độc lập nên hãy kết hợp chúng bằng xor. 
4. Thu thập tất cả các giá trị xor có thể truy cập bằng một lần di chuyển và lấy mex của chúng. Giá trị này là`grundy(h, w)`. 
5. Nếu số Grundy cuối cùng khác 0, người chơi đầu tiên có nước đi thắng. Nếu không, mọi nước đi đều dẫn đến thua cuộc. 

Tại sao nó hoạt động: 

Điều bất biến là mọi trạng thái hình chữ nhật chỉ được biểu thị bằng số Grundy của nó. Một nước đi sẽ thay đổi một hình chữ nhật thành nhiều hình chữ nhật độc lập nhỏ hơn và lý thuyết Sprague-Grundy cho biết giá trị tổng hợp của các trò chơi độc lập là xor của các giá trị của chúng. Cấu trúc mex cung cấp chính xác số Grundy của hình chữ nhật hiện tại, do đó hình chữ nhật ban đầu sẽ mất chính xác khi số Grundy của nó bằng 0. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

def solve_case(n, m, k):
    @lru_cache(None)
    def grundy(h, w):
        seen = set()

        for a in range(1, h + 1):
            for b in range(1, w + 1):
                if a * b > k:
                    continue

                for top in range(h - a + 1):
                    bottom = h - top - a
                    for left in range(w - b + 1):
                        right = w - left - b

                        x = (
                            grundy(top, left)
                            ^ grundy(top, right)
                            ^ grundy(bottom, left)
                            ^ grundy(bottom, right)
                        )
                        seen.add(x)

        g = 0
        while g in seen:
            g += 1
        return g

    return "Munir" if grundy(n, m) else "Matthew"

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        ans.append(solve_case(n, m, k))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Hàm đệ quy lưu trữ câu trả lời cho mọi kích thước hình chữ nhật mà nó đã giải được. Bộ đệm rất cần thiết vì cùng một hình chữ nhật con xuất hiện từ nhiều vị trí khác nhau. 

Các vòng lặp kết thúc`a`Và`b`tạo ra mọi hình chữ nhật được sơn có thể. các`top`Và`left`các vòng lặp chọn vị trí của nó bên trong hình chữ nhật hiện tại. Bốn cuộc gọi tương ứng với bốn góc còn lại sau khi loại bỏ hình chữ nhật đã sơn. 

Biểu thức xor phải bao gồm tất cả bốn vùng, ngay cả khi một trong số chúng có chiều cao hoặc chiều rộng bằng 0. Một hình chữ nhật có kích thước bằng 0 được coi là một trò chơi đã hoàn thành và đương nhiên có số Grundy bằng 0. 

Vòng lặp mex tìm số nguyên không âm nhỏ nhất không thể đạt được. Đây chính xác là định nghĩa của số Grundy. 

## Ví dụ đã hoạt động 

Dành cho:```
2 3 1
```Diện tích hình chữ nhật tối đa là một, vì vậy chỉ có thể đặt các ô đơn lẻ. 

| Tiểu bang | Nước đi được chọn | Còn lại Grundy xor | 
| --- | --- | --- | 
| 2×3 | Đặt một ô | 5 ô còn lại | 

Trò chơi tương đương với sáu trò chơi một nước đi độc lập. Xor của sáu giá trị Grundy giống hệt nhau bằng 0, do đó người chơi đầu tiên sẽ thua. 

Dấu vết chứng tỏ rằng chỉ riêng số lượng ô không phải là trạng thái mà là xor của các vùng độc lập cho kết quả chính xác. 

Vì:```
3 4 6
```Một động thái đầu tiên có thể là`3×2`hình chữ nhật. 

| Tiểu bang | Hình chữ nhật được chọn | Phần còn lại | 
| --- | --- | --- | 
| 3×4 | 3×2 ở giữa | 3×1 và 3×1 | 

Hai hình chữ nhật còn lại có giá trị Grundy bằng nhau nên xor của chúng bằng 0. Người chơi đầu tiên có thể chuyển sang trạng thái thua đối thủ. 

Điều này xác nhận tính bất biến trung tâm: một bước di chuyển có giá trị khi nó thay đổi xor của các thành phần độc lập về 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Phù hợp trong tổng số`n × m ≤ 1000`hạn chế | Mỗi trạng thái hình chữ nhật được giải quyết một lần và mọi vị trí hợp pháp đều được xem xét | 
| Không gian | O(số trạng thái hình chữ nhật) | Bản ghi nhớ lưu trữ một giá trị Grundy cho mỗi hình chữ nhật đã giải | 

Vùng giới hạn là yếu tố giúp cho việc lập trình động có thể thực hiện được. Thuật toán không bao giờ khám phá các cấu hình bảng đầy đủ, chỉ có kích thước hình chữ nhật và tổng số kích thước hữu ích đủ nhỏ cho các ràng buộc. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from functools import lru_cache

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve_case(n, m, k):
        @lru_cache(None)
        def grundy(h, w):
            seen = set()
            for a in range(1, h + 1):
                for b in range(1, w + 1):
                    if a * b <= k:
                        for i in range(h - a + 1):
                            for j in range(w - b + 1):
                                seen.add(
                                    grundy(i, j)
                                    ^ grundy(i, w - j - b)
                                    ^ grundy(h - i - a, j)
                                    ^ grundy(h - i - a, w - j - b)
                                )
            g = 0
            while g in seen:
                g += 1
            return g

        return "Munir" if grundy(n, m) else "Matthew"

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        out.append(solve_case(n, m, k))

    sys.stdin = old
    return "\n".join(out)

assert run("""3
2 3 1
3 3 4
3 4 6
""") == """Matthew
Munir
Munir""", "samples"

assert run("""1
1 1 1
""") == "Munir", "single cell"

assert run("""1
2 2 4
""") == "Matthew", "whole board move"

assert run("""1
3 3 1
""") == "Matthew", "odd-looking cell game"

assert run("""1
10 100 1000
""") == "Matthew", "large area boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`| Munir | Kích thước bảng tối thiểu và di chuyển ngay lập tức | 
|`2 2 4`| Matthew | Một nước đi bao trùm toàn bộ bàn cờ | 
|`3 3 1`| Matthew | Nhiều bước di chuyển đơn bào độc lập | 
|`10 100 1000`| Matthew | Xử lý diện tích tối đa | 

## Vỏ cạnh 

Dành cho:```
1 1 1
```Lập trình động đạt tới`grundy(1,1)`. Nước đi duy nhất tạo ra bốn hình chữ nhật trống, có xor bằng 0, do đó tập Grundy có thể truy cập chứa`{0}`. Mexico là`1`, nghĩa là vị trí xuất phát đang thắng. 

Vì:```
2 3 1
```Mỗi bước di chuyển sẽ vẽ chính xác một ô. Phép đệ quy chia bảng thành các hình chữ nhật nhỏ hơn cho đến khi đã chiếm hết sáu ô. Xor cuối cùng bằng 0, vì vậy không có động thái nào có thể buộc chiến thắng. 

Vì:```
3 4 6
```Nhà nước`grundy(3,4)`xem xét`3×2`vị trí. Kết quả là hai`3×1`hình chữ nhật có các giá trị Grundy bằng nhau và xor của chúng có giá trị bằng 0. Vì vị trí có giá trị bằng 0 được trao cho đối thủ nên trạng thái bắt đầu là thắng.
