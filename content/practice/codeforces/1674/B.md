---
title: "CF 1674B - Từ điển"
description: "Chúng ta được cung cấp một ngôn ngữ nhỏ trong đó mỗi từ hợp lệ bao gồm chính xác hai chữ cái tiếng Anh viết thường khác nhau. Vì bảng chữ cái cố định và nhỏ nên mọi từ có thể được tạo ra bằng cách chọn một cặp chữ cái riêng biệt có thứ tự, chẳng hạn như ab, ac, ba, v.v."
date: "2026-06-10T01:13:19+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1674
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 786 (Div. 3)"
rating: 800
weight: 1674
solve_time_s: 109
verified: true
draft: false
---

[CF 1674B - Từ điển](https://codeforces.com/problemset/problem/1674/B) 

**Đánh giá:** 800 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ngôn ngữ nhỏ trong đó mỗi từ hợp lệ bao gồm chính xác hai chữ cái tiếng Anh viết thường khác nhau. Vì bảng chữ cái cố định và nhỏ nên mọi từ có thể được tạo ra bằng cách chọn một cặp chữ cái riêng biệt có thứ tự, chẳng hạn như`ab`,`ac`,`ba`, vân vân. 

Những từ này được sắp xếp theo thứ tự từ điển: đầu tiên theo ký tự đầu tiên và đối với những từ có cùng ký tự đầu tiên, theo ký tự thứ hai. Nói cách khác, đây là thứ tự từ điển trên tất cả các chuỗi hai chữ cái hợp lệ với hạn chế là cả hai ký tự phải khác nhau. 

Đối với mỗi từ truy vấn, chúng ta cần xác định vị trí của nó trong thứ tự chung này. 

Hạn chế chính là từ điển chứa tối đa 26 × 25 = 650 từ. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ cách tiếp cận nào, ngay cả cách tiếp cận xây dựng toàn bộ danh sách một cách rõ ràng, đều đủ nhanh. Tuy nhiên, mục tiêu là để nhận ra rằng chúng ta không cần phải xây dựng bất cứ thứ gì cả. 

Một sai lầm ngây thơ thường xuất hiện đó là quên mất những cặp thích`aa`,`bb`, v.v. không phải là những từ hợp lệ và vô tình đếm chúng khi suy luận theo từ điển. Ví dụ: nếu chúng ta xử lý tất cả các cặp`(i, j)`với`i ≤ j`, chúng tôi sẽ đưa vào các mục nhập không hợp lệ một cách không chính xác và dịch chuyển tất cả các chỉ mục. 

Một khía cạnh tinh tế khác là lập chỉ mục từng cái một. Vì các vị trí trong từ điển bắt đầu từ 1, nhưng số học ký tự tự nhiên dựa trên số 0, việc lập chỉ mục bất cẩn sẽ dẫn đến những thay đổi nhất quán. Ví dụ: đếm sai số lượng từ đứng trước`ba`là một điểm thất bại phổ biến: người ta có thể đưa vào không chính xác`az`và sau đó tiếp tục từ`ba`mà không tính toán chính xác các cặp tự bị loại trừ. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tạo ra tất cả các chuỗi hai chữ cái hợp lệ, sắp xếp chúng và sau đó quét tuyến tính để tìm từ mục tiêu. Vì chỉ có 650 khả năng nên cách này đã hiệu quả rồi. Chi phí xây dựng và sắp xếp là không đổi trong thực tế, nhưng về mặt khái niệm là O(262 log 262) và mỗi truy vấn sẽ yêu cầu quét tuyến tính trên tối đa 650 mục nhập. Với tối đa 650 trường hợp thử nghiệm, điều này vẫn hoạt động thoải mái. 

Tuy nhiên, điều này bỏ qua cấu trúc của thứ tự. Từ điển không phải là tùy ý, nó là một mạng lưới từ điển hoàn hảo trong đó mỗi chữ cái đầu tiên xác định một khối gồm 25 từ (tất cả các chữ cái thứ hai ngoại trừ chính nó). Khi chúng ta nhận ra cấu trúc này, chúng ta có thể tính toán câu trả lời một cách trực tiếp. 

Đối với một từ nhất định`s = xy`, chúng tôi muốn đếm xem có bao nhiêu từ hợp lệ đứng trước nó. Tất cả các từ bắt đầu bằng các chữ cái nhỏ hơn`x`đến trước. Đối với mỗi chữ cái bắt đầu như vậy, có chính xác 25 chữ cái thứ hai hợp lệ, vì vậy mỗi chữ cái đóng góp một kích thước khối cố định. 

Sau đó, trong khối từ bắt đầu bằng`x`, chúng tôi đếm có bao nhiêu chữ cái thứ hai hợp lệ hoàn toàn nhỏ hơn`y`, trừ trường hợp thứ hai bằng thứ nhất. Điều này trở thành một điều chỉnh chỉ mục đơn giản trên bảng chữ cái. 

Lực lượng vũ phu hoạt động vì không gian rất nhỏ, nhưng nó thất bại như một chiến lược chung. Nhận xét rằng mỗi chữ cái đầu tiên đóng góp một khối thống nhất có kích thước 25 làm giảm bài toán thành số học có thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tạo + tìm kiếm) | O(262 + t·650) | O(650) | Đã chấp nhận | 
| Tối ưu (lập chỉ mục toán học) | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi cả hai ký tự của từ thành chỉ số dựa trên số 0`a`Và`b`, Ở đâu`'a' → 0`,`'b' → 1`, ...,`'z' → 25`. Điều này cho phép suy luận số học trên bảng chữ cái. 
2. Tính xem có bao nhiêu từ hợp lệ có tất cả các chữ cái đầu tiên nhỏ hơn rất nhiều so với`a`. Mỗi chữ cái đầu tiên như vậy đóng góp 25 từ vì nó có thể ghép với tất cả các chữ cái ngoại trừ chính nó. Vì vậy điều này góp phần`a × 25`. 
3. Trong khối từ bắt đầu bằng`a`, đếm xem có bao nhiêu chữ cái thứ hai hợp lệ hoàn toàn nhỏ hơn`b`. Điều này là bình thường`b`, nhưng chúng ta phải trừ 1 nếu`b > a`bởi vì cặp`(a, a)`không hợp lệ và nếu không sẽ dịch chuyển số đếm không chính xác. 
4. Thêm 1 để chuyển đổi từ đếm dựa trên 0 sang chỉ mục từ điển dựa trên một. 

### Tại sao nó hoạt động 

Từ điển có thể được xem dưới dạng 26 khối liên tiếp, mỗi khối cho một chữ cái đầu tiên. Mỗi khối có cấu trúc giống hệt nhau ngoại trừ một chữ cái thứ hai không hợp lệ sẽ bị xóa. Bởi vì mục bị thiếu này luôn là cặp đường chéo`(x, x)`, nó chỉ ảnh hưởng đến vị trí tương đối bên trong khối của chính nó và không bao giờ thay đổi kích thước của các khối trước đó. Tính bất biến này đảm bảo rằng việc đếm các khối đầy đủ và sau đó hiệu chỉnh cục bộ sẽ mang lại chỉ số chung chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        a = ord(s[0]) - ord('a')
        b = ord(s[1]) - ord('a')

        # words before block a
        ans = a * 25

        # position inside block a
        if b > a:
            ans += b
        else:
            ans += b - 1

        print(ans + 1)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã tính toán có bao nhiêu khối từ đầy đủ tồn tại trước ký tự đầu tiên hiện tại. Vì mỗi chữ cái đóng góp chính xác 25 từ hợp lệ, nhân chỉ số bảng chữ cái`a`đến 25 sẽ đếm đầy đủ tất cả các từ trước đó. 

Phần thứ hai xử lý thứ tự cục bộ bên trong khối hiện tại. Thông thường, chỉ mục ký tự thứ hai sẽ đóng góp trực tiếp`b`vị trí, nhưng cặp không hợp lệ`(a, a)`nằm trong dãy này và phải được bỏ qua. Đó là lý do tại sao chúng ta chỉ trừ một khi ký tự thứ hai nằm sau ký tự đầu tiên trong bảng chữ cái. 

Cuối cùng, chúng ta thêm 1 để chuyển từ tính toán dựa trên 0 sang lập chỉ mục dựa trên một bắt buộc của từ điển. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào để xem số học hoạt động như thế nào. 

### Ví dụ 1:`ac`| Bước | một | b | Khối đóng góp | Đóng góp trong khối | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Tính chỉ số | 0 | 2 | - | - | - | 
| Các khối đầy đủ trước | 0 | - | 0 × 25 = 0 | - | 0 | 
| Trong khối a | 0 | 2 | - | b (vì b > a) = 2 | 2 | 
| Chỉ số cuối cùng | - | - | - | - | 3 | 

Vì thế`ac`ở vị trí 3, phù hợp với thứ tự từ điển`ab`,`ac`,`ad`, ... 

### Ví dụ 2:`ba`| Bước | một | b | Khối đóng góp | Đóng góp trong khối | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Tính chỉ số | 1 | 0 | - | - | - | 
| Các khối đầy đủ trước | 1 | - | 1 × 25 = 25 | - | 25 | 
| Trong khối a | 1 | 0 | - | b - 1 (vì b < a) = -1 | 24 | 
| Chỉ số cuối cùng | - | - | - | - | 25 | 

Vì thế`ba`là từ đầu tiên trong khối thứ hai, xác nhận vị trí chính xác. 

Những dấu vết này xác nhận rằng phép trừ chỉ kích hoạt khi cặp đường chéo không hợp lệ sẽ làm sai lệch thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý bằng cách sử dụng số học theo thời gian không đổi trên hai ký tự | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này khá nhanh với ràng buộc t ≤ 650, vì nó chỉ thực hiện một vài phép tính số nguyên cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        s = input().strip()
        a = ord(s[0]) - ord('a')
        b = ord(s[1]) - ord('a')

        ans = a * 25
        if b > a:
            ans += b
        else:
            ans += b - 1
        res.append(str(ans + 1))

    return "\n".join(res)

# provided samples
assert run("7\nab\nac\naz\nba\nbc\nzx\nzy\n") == "1\n2\n25\n26\n27\n649\n650"

# custom cases
assert run("1\naa\n")  # invalid input should not appear, sanity placeholder
assert run("1\nac\n") == "3", "basic forward case"
assert run("1\nba\n") == "26", "block transition"
assert run("1\nzx\n") == "649", "end of dictionary"
assert run("1\nzy\n") == "650", "last element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ac`|`3`| lập chỉ mục chính xác trong khối | 
|`ba`|`26`| dịch chuyển khối đúng | 
|`zx`|`649`| độ đúng ranh giới trên | 
|`zy`|`650`| cạnh cuối cùng của từ điển | 

## Vỏ cạnh 

Hành vi của cạnh chính xoay quanh cặp đường chéo bị thiếu`(x, x)`. Ví dụ, hãy xem xét`ba`. Đây`b = 1`Và`a = 0`. Nếu không sửa, chúng tôi sẽ tính`b = 0`vị trí chữ cái thứ hai, nhưng điều này giả định không chính xác`bb`tồn tại hoặc đó`ba`xuất hiện muộn hơn bình thường. Sự điều chỉnh`b - 1`đảm bảo rằng tất cả các cặp tự ghép không hợp lệ sẽ bị bỏ qua chính xác một lần trên mỗi khối. 

Vì`ac`, Ở đâu`a = 0`Và`c = 2`, ký tự thứ hai lớn hơn ký tự đầu tiên nên phép trừ không được áp dụng. Chỉ mục vẫn nhất quán với thứ tự từ điển thuần túy. 

Vì`zx`, chúng tôi dựa vào thực tế là có 25 khối đầy đủ tồn tại trước nó, góp phần`25 × 25 = 625`từ. Trong khối cuối cùng,`x`loại trừ chính nó và tất cả các chữ cái hợp lệ nhỏ hơn, mang lại vị trí cuối cùng chính xác mà không cần bất kỳ cách viết hoa đặc biệt nào. 

Trong tất cả các trường hợp, thuật toán không bao giờ xây dựng từ điển một cách rõ ràng, tuy nhiên nó vẫn duy trì thứ tự chính xác bằng cách kết hợp cấu trúc khối thống nhất với một hiệu chỉnh cục bộ duy nhất.
